## 概要
### Docker環境でLaravelを軽く動かす！
- ホストとゲストで同期する箇所を制限する
- Laravelのインストールは手動で行う
- 同期をとらない箇所はホストとゲスト間でコピーをする
- 同期をとらない箇所はボリュームを利用して永続化する
<br><br>

## インストール方法
1. リポジトリをクローンする
   ```bash
   git clone https://github.com/candypopbeat/syokunohiroba-docker.git
   ```
1. htmlフォルダの中にLaravelプロジェクトをインストールする
   ```bash
   composer create-project laravel/laravel ./html
   ```
1. Dockerデスクトップを起動させる
2. docker-compose.ymlからコンテナを構築する
   ```bash
   docker compose up
   ```
1. webコンテナ名を調べる
   ```bash
   docker ps
   ```
1. webコンテナの調整をする
   - 上記で調べたwebコンテナ名を使う
      ```bash
      # ボリューム用（同期をとらない永続化）のディレクトリをコピーする
      docker cp vendor {コンテナ名}:/var/www/html
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