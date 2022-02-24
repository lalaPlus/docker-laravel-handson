# docker-laravel-handson

Docker環境でLaravelを動かす想定の自分用の雛形。

参考: [【超入門】20分でLaravel開発環境を爆速構築するDockerハンズオン](https://qiita.com/ucan-lab/items/56c9dc3cf2e6762672f4)

## ディレクトリ構造
```
docker-sample(適当なものにリネーム)
├── README.md
├── infra
│   ├── mysql
│   │   ├── Dockerfile (*2)
│   │   └── my.cnf
│   ├── nginx
│   │   └── default.conf
│   └── php
│       ├── Dockerfile
│       └── php.ini
├── docker-compose.yml (*1)
└── sample (Laravelをインストールする空ディレクトリ(適当なものにリネーム))
(*1) appとwebのvolumesのパス指定をプロジェクト名に合わせて変更すること(マウント指定)
(*2) .gitignoreで除外すること
```
**\*/infra/mysql/Dockerfileは.gitignoreで必ず除外すること(https://docs.github.com/ja/get-started/getting-started-with-git/ignoring-files)**

## 使い方
- 適当なdocker-プロジェクト名(docker-sample)でクローン
```bashß
git clone git@github.com:lalaPlus/docker-laravel-handson.git docker-sample
cd ./sample
rm -rf .git
git init
```
- 適当なプロジェクト名(sample)にLaravelをインストール
```bash
# docker-プロジェクトルートにて
composer create-project --prefer-dist "laravel/laravel={Laravelのバージョン番号を指定}" ./sample
```
以下のようにvalumesにてマウントの関係性を指定する。おそらくこれで、appコンテナ側に入ってLaravelをインストールしなくても大丈夫なはず。
```yml
# /infra/docker-compose.yml
version: "3.9"
services:
  app:
    build: ./infra/php
    volumes:
      - ./sample:/work # ここ(ホスト側パス:コンテナ側パス)

  web:
    image: nginx:1.20-alpine
    ports:
      - 8080:80
    volumes:
      - ./sample:/work # ここ(ホスト側パス:コンテナ側パス)
      - ./infra/nginx/default.conf:/etc/nginx/conf.d/default.conf
    working_dir: /work

  db:
    build: ./infra/mysql
    volumes:
      - db-store:/var/lib/mysql
volumes:
  db-store:
```

- コンテナのビルド＆起動
```bash
docker compose up -d
```

以下を開いてLaravelが表示されればとりあえずおk <br />
http://localhost:8080/