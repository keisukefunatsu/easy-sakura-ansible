# さくらのレンタルサーバーに WordPress 開発環境を作成します

初心者が、簡単にさくらのレンタルサーバーに WordPress 環境を構築できるようにすることを目標とします。
ステージング環境、本番環境の設定までをセットで行えるようにします。
ステージング環境と本番環境は一台のレンタルサーバー上に作成するものとします。

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

direnv の利用を推奨します。
.envrc のあるディレクトリでのみ環境変数が有効となり、ディレクトリを移動すると環境変数を参照できなくなるもので、センシティブな情報を管理するのに適しています。

### 使い方

`dirnv edit` で.envrc を新規作成します。
`direnv allow`で.envrc の記述を反映させる。
.envrc の中身は以下のように記述します。

```
# ステージング環境の環境変数
export STAGING_DB_HOST="staging_db_host_here"
export STAGING_DB_USER="staging_db_user_name_here"
export STAGING_DB_NAME="staging_db_name_here"
export STAGING_DB_PASS="staging_db_pass_here"
export STAGING_DIRECTORY="staging_directory_here"

# 本番環境の環境変数
export PRODUCTION_DB_HOST="production_db_host_here"
export PRODUCTION_DB_USER="production_db_user_name_here"
export PRODUCTION_DB_NAME="production_db_name_here"
export PRODUCTION_DB_PASS="production_db_pass_here"
export PRODUCTION_DIRECTORY="production_directory_here"

# ssh接続用の環境変数
export SAKURA_SSH_PASS="enter_sakura_server_ftp_pass"
export SAKURA_HOST_NAME="enter_yourhost_name"
export SAKURA_USER_NAME="enter_your_user_name"
```
