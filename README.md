# さくらのレンタルサーバーに WordPress 環境を作成します

ansible,wp-cli を用いて、実案件で使える最低限の環境を作成することを目指します。

## できること（予定）

- wp-cli の導入
- WordPress の初期構築
- プラグインの導入
- バックアップ、アップデートスクリプトの設置、cron 設定

## 環境

macOS での動作を想定しています。
windows 版はこちら

- https://docs.ansible.com/ansible/latest/user_guide/windows.html

## 準備

homebrew で ansible をインストールしてください

- `brew install ansible`

## 手順

- さくらのレンタルサーバーを契約
  　-　https://www.sakura.ne.jp/flow.html
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


### config.ymlをプロジェクトで共有する場合
安全のために以下の手順でconfig.ymlを暗号化してから共有することをおすすめします。
- `.gitignore`の中の`config.yml`を削除
- `ansible-vault encrypt config.yml`を実行し、パスワードを入力して暗号化した後push

## 参考ディレクトリ

- https://github.com/youcune/wordpress-ansible-sakura
- https://github.com/wate/tools/tree/master/sakura_rentalserver
