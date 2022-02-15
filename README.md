# docker-laravel-handson

目的: Docker環境でLaravel(+React)を動かすための雛形を作りたい

参考: [【超入門】20分でLaravel開発環境を爆速構築するDockerハンズオン](https://qiita.com/ucan-lab/items/56c9dc3cf2e6762672f4)

## ディレクトリ構造(暫定)
```
docker-laravel-handson
├── README.md
├── infra (*1)
│   ├── mysql (*1)
│   │   ├── Dockerfile
│   │   └── my.cnf (*1)
│   ├── nginx (*1)
│   │   └── default.conf (*1)
│   └── php (*1)
│       ├── Dockerfile
│       └── php.ini (*1)
├── docker-compose.yml
└── backend (*1)
    └── Laravelをインストールするディレクトリ <br />
(*1) 任意の名前に変更する可能性あり
```