# docker-laravel-handson

Docker環境でLaravelを動かす想定の自分用の雛形。

参考: [【超入門】20分でLaravel開発環境を爆速構築するDockerハンズオン](https://qiita.com/ucan-lab/items/56c9dc3cf2e6762672f4)

## 使い方

- サンプルディレクトリ構造
```
docker-sample(適当にリネームしてよし)
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
└── origin-project(適当にリネームしてよし)
(*1) appとwebのvolumesのパス指定をプロジェクト名に合わせて変更すること(マウント指定)
(*2) .gitignoreで除外すること
```
**\*/infra/mysql/Dockerfileは.gitignoreで必ず除外すること(https://docs.github.com/ja/get-started/getting-started-with-git/ignoring-files)**

- 適当なdocker-プロジェクト名(docker-sample)でクローン
```bash
git clone git@github.com:lalaPlus/docker-laravel-handson.git ./docker-sample
cd ./docker-sample
rm -rf .git
git init
```
- 適当なプロジェクト名(origin-project)にLaravelをインストール。
```bash
# docker-sampleルートにて

# gitの都合上配置してある.gitkeepを消す
rm ./origin-project/.gitkeep
composer create-project --prefer-dist "laravel/laravel={Laravelのバージョン番号を指定}" ./origin-project
```
以下のようにvalumesにてマウントの関係性を指定する。これならappコンテナ側に入ってLaravelをインストールしなくても大丈夫なはず。
```yml
# /infra/docker-compose.yml
version: "3.9"
services:
  app:
    build: ./infra/php
    volumes:
      - ./origin-project:/work # ここ(ホスト側パス:コンテナ側パス)

  web:
    image: nginx:1.20-alpine
    ports:
      - 8080:80
    volumes:
      - ./origin-project:/work # ここ(ホスト側パス:コンテナ側パス)
      - ./infra/nginx/default.conf:/etc/nginx/conf.d/default.conf
    working_dir: /work

  db:
    build: ./infra/mysql
    volumes:
      - db-store:/var/lib/mysql
volumes:
  db-store:
```

- Dockerコンテナイメージのビルド＆起動
```bash
docker compose up -d --build
```

以下を開いてLaravelが表示されればとりあえずおk <br />
http://localhost:8080/

- プロジェクトファイルの変更を反映(再コンパイルを有効化)

ホスト側で"npm run watch"しても、ファイルを更新しても再コンパイルされない。

このコマンドはファイル更新イベント(ファイル保存)を検知して再コンパイルを実行してくれるコマンドだが、ホスト側とコンテナ側ではファイルシステムが違い正常に検知されず、再コンパイルが働かない。ファイル更新は自動で反映されるのにね...

もちろん、コンテナ側にログインしてコマンドを叩いてもいいのだが・・・めんどい。おまけにコンパイルがクソ遅い。

---

そこで"**npm run watch-poll**"コマンド。こっちならコンテナ側でもwebpackからファイル更新を定期的に強制確認してくれるらしい。

時間はpackage.jsonで指定できる。デフォルト値の1000だとcpu負荷が高いので**5000**に変更。

```json
// ホスト側package.json
"scripts": {
 "watch-poll": "mix watch -- --watch-options-poll=5000",
},
```

あとは通常通りホスト側で"npm run watch-poll"をすればいい。
```bash
# ホスト側で
npm run watch-poll
```