## 概要
Docker環境でLaravelを軽く動かす
<br><br>

## インストール方法
1. リポジトリをクローンする
   ```bash
   git clone https://github.com/candypopbeat/syokunohiroba-docker.git
   ```
1. htmlの中にLaravelプロジェクトをインストールする
   ```bash
   composer create-project laravel/laravel ./html
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
1. webコンテナの調整をする
   上記で調べたwebコンテナ名を使う
   ```bash
   # ボリューム用のディレクトリをコピーする
   docker cp bootstrap {コンテナ名}:/var/www/html 
   docker cp storage {コンテナ名}:/var/www/html

   # パーミッションを変更する
   docker exec {コンテナ名} chmod -R 777 bootstrap
   docker exec {コンテナ名} chmod -R 777 storage
   ```
2. webコンテナに入る
   ```bash
   # 上記で調べたwebコンテナ名を使う
   docker exec -it {コンテナ名} bash
   ```
<br><br>