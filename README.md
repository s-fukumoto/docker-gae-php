## GAE環境構築(php)
Google Cloud Platform の App Engine 用ローカル開発環境

## 実行環境
Docker Community Edition Version 18.03.1-ce-win65 (17513) Channel: stable 93354b3
Docker version 18.03.1-ce
Docker Compose version 1.21.1

## 構築手順

docker用のファイルを配置
```
$ git clone https://github.com/s-fukumoto/docker-gae-php.git
$ cd docker-gae-php/
```

.envを環境に合わせて修正
```
$ cp .env_sample .env
$ vi .env
```

Dockerfileを環境に合わせて修正
```
$ cp -r app_sample app
$ vi /app/Dockerfile
$ vi /app/php.ini
```

起動
```
$ docker-compose build --no-cache
$ docker-compose up -d
```

起動確認
```
$ docker ps
CONTAINER ID        IMAGE                                COMMAND                  CREATED             STATUS                     PORTS                    NAMES
a778f48d8bce        google/cloud-sdk:latest              "bash"                   7 minutes ago       Up 7 minutes                                        app-test-gcloud
e08c9cc2e8e0        gcr.io/google-appengine/php:latest   "/build-scripts/entr…"   7 minutes ago       Up 5 minutes               0.0.0.0:8080->8080/tcp   app-test
```

ページ確認
```
http://localhost:8080
```

composer install (必要時のみ)
```
$ docker exec -it app-test bash
# cd /app                            # アプリの格納先を指定
# composer install
# exit
```

GCloud初期設定
```
$ docker exec -it app-test-gcloud bash
# gcloud init
  (指定された内容をGCPの設定に合わせて設定)
# exit
```

GCloudへdeploy (GCloudにアクセス)
```
$ docker exec -it app-test-gcloud bash
# cd /app                            # アプリの格納先を指定
# gcloud app deploy                  # deploy
# exit
```

終了
```
$ docker-compose down
```
