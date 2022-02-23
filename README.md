# docker-laravel-handson

Docker環境でLaravelを動かす想定の雛形

参考: [【超入門】20分でLaravel開発環境を爆速構築するDockerハンズオン](https://qiita.com/ucan-lab/items/56c9dc3cf2e6762672f4)

自前環境にクローンして使う

## ディレクトリ構造
```
docker-project (*1)
├── README.md
├── infra
│   ├── mysql
│   │   ├── Dockerfile (*3)
│   │   └── my.cnf
│   ├── nginx
│   │   └── default.conf
│   └── php
│       ├── Dockerfile
│       └── php.ini
├── docker-compose.yml (*2)
└── origin-project (*1)
    └── Laravelをインストールするディレクトリ
(*1) 任意のプロジェクト名に変更すること
(*2) appとwebのvolumesのパス指定をプロジェクト名に合わせて変更すること
(*3) リモートリポジトリにpushする場合、.gitignoreで除外すること
```

**\*/infra/mysql/Dockerfileは.gitignoreで必ず除外すること(https://docs.github.com/ja/get-started/getting-started-with-git/ignoring-files)**
