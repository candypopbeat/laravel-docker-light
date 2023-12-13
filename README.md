## 概要
Docker環境でLaravelを軽く動かす
<br><br>

## インストール方法
1. リポジトリをクローンする
   ```bash
   git clone https://github.com/candypopbeat/syokunohiroba-docker.git
   ```
1. Dockerデスクトップを起動させる
2. コンテナ達を構築する
   ```bash
   docker compose up
   ```
1. webコンテナ名を調べる
   ```bash
   docker ps
   ```
1. webコンテナに入る
   ```bash
   # 上記で調べたwebコンテナ名を使う
   docker exec -it {コンテナ名} bash
   ```
1. Laravelをインストールする
   ```bash
   composer create-project laravel/laravel ./
   ```
1. storageとbootstrapのパーミッションを変更する
   ```bash
   chmod -R 777 storage
   chmod -R 777 bootstrap
   ```
<br><br>