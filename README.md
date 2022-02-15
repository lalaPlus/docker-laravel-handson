# docker-laravel-handson

目的: Docker環境でLaravel(+React)を動かすための雛形を作りたい

参考: https://qiita.com/ucan-lab/items/56c9dc3cf2e6762672f4

<!-- 暫定ディレクトリ構造 -->
├── README.md <br />
├── infra (*1) <br />
│   ├── mysql (*1) <br />
│   │   ├── Dockerfile <br />
│   │   └── my.cnf (*1) <br />
│   ├── nginx (*1) <br />
│   │   └── default.conf (*1) <br />
│   └── php (*1) <br />
│       ├── Dockerfile <br />
│       └── php.ini (*1) <br />
├── docker-compose.yml <br />
└── backend (*1) <br />
    └── Laravelをインストールするディレクトリ <br />

このディレクトリ構成を目指します。
(*1) 任意の名前に変更する可能性あり