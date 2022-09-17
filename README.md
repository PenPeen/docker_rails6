Rails6 +　 MySQL 環境

### 環境構築

### 必要ファイル

- Dockerfile
- docker-compose.yml
- Gemfile
- Gemfile.lock
- entrypoint.sh

#### ビルド
```
docker-compose build
```

#### Rails プロジェクトの作成

```
docker-compose run web rails new . --force --no-deps --database=mysql
```

- docker-compose run 引数で指定したコンテナ内でコマンドを実行する
- database=mysql で DB 設定を MySQL にした状態でプロジェクト作成。

#### DB 接続設定(database.yml)
##### 開発環境の設定を以下で上書き

```
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  host: localhost

development:
  <<: *default
  database: myapp_development
  host: db
  username: root
  password: password

test:
  <<: *default
  database: myapp_test
  host: db
  username: root
  password: password
```

#### DB 作成

```
docker-compose run web rails db:create
```

うまくいかない場合はマイグレーションリセット
```
docker-compose run web rails db:migrate:reset
```

#### webpackerインストール
```
docker-compose run web rails webpacker:install
```

#### Docker イメージ起動

```
docker-compose up -d
```

http://localhost:3000/ にアクセスしてページが表示されたら成功。
