# さくらのレンタルサーバーに WordPress 環境を作成します

ansible,wp-cliを用いて、実案件で使える最低限の環境を作成することを目指します。

## できること（予定）
- wp-cliの導入
- WordPressの初期構築
- プラグインの導入
- バックアップ、アップデートスクリプトの設置、cron設定


## 環境

macOS での動作を想定しています。
windows 版はこちら

- https://docs.ansible.com/ansible/latest/user_guide/windows.html

## 準備

homebrew で ansible をインストールしてください

- `brew install ansible`

## 手順
- さくらのレンタルサーバーを契約します。
　-　https://www.sakura.ne.jp/flow.html
- 仮登録が完了するとftp用のパスワードがメールで送られてきます。
- WordPress用の設定、データベースのパスワードをパスワード生成ソフトを用いて作成します。
- コントロールパネルにログインして、データベースを作成します
- 環境変数を設定します。

### 環境変数の設定

```
# 共通の環境変数
export DB_USER=""
export DB_PASS=""
export DB_HOST=""

# ステージング環境の環境変数
export STAGING_DB_NAME=""
export STAGING_DIRECTORY="~/www/staging"
export STAGING_ADMIN_NAME=""
export STAGING_ADMIN_PASS=""
export STAGING_ADMIN_MAIL="test@example.com"

# 本番環境の環境変数
export PRODUCTION_DB_NAME=""
export PRODUCTION_DIRECTORY="~/www/production"
export PRODUCTION_ADMIN_NAME=""
export PRODUCTION_ADMIN_PASS=""
export PRODUCTION_ADMIN_MAIL="test@example.com"

export SAKURA_SSH_PASS=""
export SAKURA_HOST_NAME=""
export SAKURA_USER_NAME=""

```

### 実行
- `ansible-playbook playbook.yml`

## 環境変数はdirenvをおすすめします
DB情報やログイン情報は環境変数に入っているもののみ扱えます。
direnv の利用を推奨します。
.envrc のあるディレクトリでのみ環境変数が有効となり、ディレクトリを移動すると環境変数を参照できなくなるもので、センシティブな情報を管理するのに適しています。

### 使い方

`dirnv edit .` で.envrc を新規作成します。
`direnv allow` で.envrc の記述を反映させる。
.envrc の中身は以下のように記述します。


## 参考ディレクトリ

- https://github.com/youcune/wordpress-ansible-sakura
- https://github.com/wate/tools/tree/master/sakura_rentalserver
