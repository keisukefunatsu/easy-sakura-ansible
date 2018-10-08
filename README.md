# さくらのレンタルサーバーに WordPress 環境を作成します

初心者が、簡単にさくらのレンタルサーバーに WordPress 環境を構築できるようにすることを目標とします。

ステージング環境、本番環境の設定までをセットで行えるようにします。

sステージング環境と本番環境は一台のレンタルサーバー上に作成するものとします。

## 参考

- https://github.com/youcune/wordpress-ansible-sakura
- https://github.com/wate/tools/tree/master/sakura_rentalserver

## 方針

- 初心者がつまづかないように
- できる限り簡潔な記述に
- 実案件で使えるように

## 環境

macOS での動作を想定しています。
windows 版はこちら

- https://docs.ansible.com/ansible/latest/user_guide/windows.html

## 準備

homebrew で ansible をインストールしてください

- `brew install ansible`

## 手順
- `ansible-playbook playbook.yml`

## 環境変数
DB情報やログイン情報は環境変数に入っているもののみ扱えます。
direnv の利用を推奨します。
.envrc のあるディレクトリでのみ環境変数が有効となり、ディレクトリを移動すると環境変数を参照できなくなるもので、センシティブな情報を管理するのに適しています。

### 使い方

`dirnv edit` で.envrc を新規作成します。
`direnv allow`で.envrc の記述を反映させる。
.envrc の中身は以下のように記述します。

```
# ステージング環境の環境変数
export STAGING_DB_HOST=""
export STAGING_DB_USER=""
export STAGING_DB_NAME=""
export STAGING_DB_PASS=""
export STAGING_DIRECTORY="~/www/staging"
export STAGING_ADMIN_NAME=""
export STAGING_ADMIN_PASS=""
export STAGING_ADMIN_MAIL="test@example.com"

# 本番環境の環境変数
export PRODUCTION_DB_HOST=""
export PRODUCTION_DB_USER=""
export PRODUCTION_DB_NAME=""
export PRODUCTION_DB_PASS=""
export PRODUCTION_DIRECTORY="~/www/production"
export PRODUCTION_ADMIN_NAME=""
export PRODUCTION_ADMIN_PASS=""
export PRODUCTION_ADMIN_MAIL="test@example.com"

export SAKURA_SSH_PASS=""
export SAKURA_HOST_NAME=""
export SAKURA_USER_NAME=""

```
