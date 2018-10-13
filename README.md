# Ansibleを使ってさくらのレンタルサーバーに WordPress 環境を作成します

Ansible, WP-CLI を用いて、実案件で使える程度の最低限の環境を作成することを目指します。

## できること（予定）

- wp-cli の導入
- WordPress の初期構築
- プラグインの導入
- バックアップ、アップデートスクリプトの設置、cron 設定

## 環境

macOS での動作を想定しています。
windows 版はこちら

- https://docs.ansible.com/ansible/latest/user_guide/windows.html

## 仕様、できること
- WP-CLIが使えるようになります。
- 本番環境、ステージング環境を作成でき、ともにディレクトリはwww以下に作られます。
- バックアップ、アップデートスクリプトの設置、cron設定が可能です。
- テストデータの挿入が可能です。
- 予め決めておいたプラグインを構築と同時に導入できます。
  - プラグインは本番環境、ステージング環境、両方に共通のものの３種類に分けて設定することができます。
- 変数はconfig.ymlを別途作成して記入します。config-sample.ymlをリネームするなどして使ってください。


## 準備

homebrew で ansible をインストールしてください

- `brew install ansible`

## 手順

- さくらのレンタルサーバーを契約
  - https://www.sakura.ne.jp/flow.html
- 仮登録が完了すると ftp 用のパスワードがメールで送られてくる
- WordPress 用の設定、データベースのパスワードをパスワード生成ソフトを用いて作成
- コントロールパネルにログインして、データベースを作成
- ドメインを取得している場合は以下の手順を先に行う
  - ネームサーバーをさくらのものに変更する
  - コントロールパネルからドメイン登録をする
  - https 化する場合は証明書を発行（30 分くらい取得完了メールが来るのを待つ）
- config-sample.yml を config.yml にリネームして、WordPress タイトルなどの変数を設定
- 環境変数を設定
- `ansible-playbook playbook.yml`で ansible を実行

## 設定ファイルの書き方

config-sample.yml を config.yml にリネームして使用してください。

### 変数の一覧
- `sakura_ssh_pass`: メールで送られてきたFTPのパスをを設定します。
- `sakura_host_name`: ホスト名を設定します。　例）"username.sakura.ne.jp"
- `sakura_user_name`: さくらレンタルサーバーのユーザ名を設定します。
- `sakura_db_host`:　作成したデータベースホストを設定します。　例）"mysqlxxx.db.sakura.ne.jp"
- `sakura_db_user`: 作成したデータベースのユーザ名を設定します。　
- `sakura_db_pass`: 作成したデータベースのパスワードを設定します。

**この設定の場合、毎日0:30に実行される**
- `cron_hour`: cronの実行時間を設定します。例）0
- `cron_minute`: cronの実行時間の分数を設定します。例）30
- `cron_enable`: cronを設定する場合はtrue 必要ない場合はfalse

- `production_directory`: 本番環境のディレクトリ名、~/www以下に作成されます　例）"production"
- `production_db_name`: 本番環境のデータベース名
- `production_db_prefix`: データベースの接頭辞（オプション）、デフォルトはwp_　例）"wp_pro_"


**以下はWordPressの設定項目**
- `production_admin_name`: 本番環境の管理者ユーザ名を設定します。
- `production_admin_pass`: 本番環境の管理者のパスワードを設定します。
- `production_admin_mail`: 本番環境の管理者のメールアドレスを設定します。　例）test@example.com
- `production_url`: 本番環境のURLを設定します。　例）"https://easy-ansible-sakura.tk/"
- `production_title`:　本番環境のサイトタイトルを設定します。　例） "WordPress　demo site production"
- `production_plugins`: 本番環境のプラグイン（複数指定可能）を設定します。

**ステージング環境の設定**
- `staging_necessary`: ステージング環境が必要な場合は true, 必要でない場合はfalse
- `staging_test_data`: true ステージング環境にテストデータをインポートする場合はtrue、いらない場合- はfalse
- `staging_directory`: ステージング環境のディレクトリ名、~/www以下に作成されます 例）"staging"
- `staging_db_name`: ステージング環境用のデータベースの名前を設定します。
- `staging_db_prefix`: データベースの接頭辞（オプション）を設定します。、デフォルトはwp_　例）"wp_stg_"

**以下はWordPressの設定項目**
- `staging_admin_name`: ステージング環境の管理者ユーザ名を設定します。
- `staging_admin_pass`: ステージング環境の管理者のパスワードを設定します。
- `staging_admin_mail`: ステージング環境の管理者のメールアドレスを設定します。　例）test@example.com
- `staging_url`: ステージング環境のサイトURLを設定します。　例）"http://xxxx.sakura.ne.jp/staging"
- `staging_title`:　ステージング環境のサイトタイトルを設定します。　例） "WordPress　demo site - staging"
- `staging_plugins`:　ステージング環境のプラグイン（複数指定可能）を設定します。

**以下は共通設定の項目**
- `common_plugins`:　本番、ステージング両方の環境に入るプラグインを設定します。（複数指定可能）



### config.ymlをプロジェクトで共有する場合
安全のために以下の手順でconfig.ymlを暗号化してから共有することをおすすめします。
- `.gitignore`の中の`config.yml`を削除
- `ansible-vault encrypt config.yml`を実行し、パスワードを入力して暗号化した後push

## 参考リポジトリ

- https://github.com/youcune/wordpress-ansible-sakura
- https://github.com/wate/tools/tree/master/sakura_rentalserver
