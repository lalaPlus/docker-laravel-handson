# docker-laravel-handson

Docker環境でLaravel(+React)を動かすための雛形

参考: [【超入門】20分でLaravel開発環境を爆速構築するDockerハンズオン](https://qiita.com/ucan-lab/items/56c9dc3cf2e6762672f4)

## ディレクトリ構造
```
docker-laravel-handson (*1)
├── README.md
├── infra
│   ├── mysql
│   │   ├── Dockerfile
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
```