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
   git clone https://github.com/candypopbeat/laravel-docker-light.git
   ```
1. htmlフォルダの中にLaravelプロジェクトを入れ込む（.gitkeepは削除する）
   1. 新規インストールする場合
      ```bash
      composer create-project laravel/laravel ./html
      ```
   1. 既存プロジェクトの場合はhtmlフォルダ内にコピーする
2. Dockerデスクトップを起動させる
3. docker-compose.ymlからコンテナを構築する
   ```bash
   docker compose up
   ```
4. webコンテナ名を調べる
   ```bash
   docker ps
   ```
5. webコンテナの調整をする
   - 上記で調べたwebコンテナ名を使う
      ```bash
      # ボリューム用（同期をとらない永続化）のディレクトリをコピーする
      docker cp ./html/vendor {コンテナ名}:/var/www/html
      docker cp ./html/node_modules {コンテナ名}:/var/www/html
      docker cp ./html/bootstrap {コンテナ名}:/var/www/html
      docker cp ./html/storage {コンテナ名}:/var/www/html

      # パーミッションを変更する
      docker exec {コンテナ名} chmod -R 777 bootstrap
      docker exec {コンテナ名} chmod -R 777 storage
      ```
6. .env と docker-compose.yml のデータベース情報を合わせる
   ```bash
   # .env
   DB_CONNECTION=mysql
   DB_HOST=db
   DB_PORT=3306
   DB_DATABASE=laravel
   DB_USERNAME=user
   DB_PASSWORD=password-user

   # docker-compose.yml
   db:
      build: ./docker/db
      ports:
         - "3307:3306"
      environment:
         MYSQL_ROOT_PASSWORD: password-root
         MYSQL_DATABASE: laravel
         MYSQL_USER: user
         MYSQL_PASSWORD: password-user
         TZ: 'Asia/Tokyo'
      volumes:
         - db-data:/var/lib/mysql
   ```
<br><br>

## Docker コマンド
- 起動中コンテナ情報
   ```bash=
   docker ps
   ```
- コンテナに入る
   ```bash=
   docker exec -it {コンテナ名} bash
   ```
- ホストからコンテナ内へコピー
   ```bash=
   docker cp {対象ファイルパス} {コンテナID}:{パス}
   ```
- コンテナを一括停止
   ```bash=
   docker stop $(docker ps -q)
   ```
<br><br>

## Docker Compose コマンド
- コンテナ起動
   ```bash=
   docker-compose up
   ```
- 再ビルドしながらのコンテナ起動
   ```bash=
   docker compose up --build
   ```
- コンテナ削除 コンポーズファイル指定なし
   ```bash=
   docker-compose down
   ```
- キャッシュを使わないでビルド
   ```bash=
   docker-compose build --no-cache
   ```
- コンテナとそれに関連したイメージとボリューム削除
   ```bash=
   docker-compose down --rmi all --volumes --remove-orphans
   ```
