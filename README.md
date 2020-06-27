# Cake2.xローカル開発環境

## 事前準備

### 1. Docker for macのインストール

公式サイトからDockerのアカウントを作ってログインし、DockerHubからダウンロードしてインストール  
https://hub.docker.com/editions/community/docker-ce-desktop-mac

### 2. このリポジトリのクローン

ファイルを展開したいディレクトリで  
`git clone https://github.com/oaxisstudio/cake-env.git`

### 3. Cake2.xのクローン

`cake-env` ディレクトリ直下で  
`git clone -b 2.x https://github.com/cakephp/cakephp.git`

## とりあえず動かしてみる

1. `cake-env`ディレクトリで `docker-compose build`
2. `docker-compose up`
3. `localhost:8080` にアクセス：CakePHPのトップ画面が表示（まだエラーメッセージが多々出るはず）

## 必要なファイルの作成・書き換え（CakePHPの初期設定）

1. `cakephp/app/Config/database.php` の作成  
  `cakephp/app/Config/database.php default`をコピーしてファイル名からdefauleを削除。ファイルを以下のように編集  
  （データベース名等は`docker-compose.yml`と一致していれば任意に編集してよい)

    ```php
    class DATABASE_CONFIG
      {

        public $default = array(
          'datasource' => 'Database/Mysql',
          'persistent' => false,
          'host' => 'db',
          'login' => 'root',
          'password' => 'rootpassword',
          'database' => 'cakephp',
          'prefix' => '',
          'encoding' => 'utf8'
        );

      …以下略
    ```

2. `cakephp/app/Config/core.php` の編集  
`Security.salt` `Security.cipherSeed` を検索、ランダムな英数字や数字の文字列を任意の値に変更する

3. (任意)DebbugKitのインストール

4. `cake-env` というディレクトリの名前は好きなものに変えても問題ありません。

### エラー解消を確認

1. `localhost:8080`にアクセス 赤いエラーメッセージはなくなりましたか？
2. `localhost:3000` にアクセス：phpmyadminのトップ画面が表示されますか？

## フォルダ構成

最終的にこのようになります

```text
.
├── README.md
├── cakephp（任意の名前に変更可・変更の場合はdocker-compose.ymlも修正する）
│   ├── app
│   │   ├── Config
│   │   ~中略
│   │   ├── tmp
│   │   └── webroot
│  （省略 CakePHPのファイル一式）
├── db（*Docker-compose up後自動生成)
├── docker
│   ├── mysql
│   │   └── my.conf
│   └── php
│       ├── Dockerfile
│       ├── base.conf
│       └── php.ini
├── docker-compose.yml
└── phpmyadmin（*Docker-compose up後自動生成)
```

## 開発時の手順

1. 開発環境の起動  
  `docker-comppose up`

2. 開発終了  
  `docker-compose down`

3. サーバー設定やDockerfileを書き換えたときは再ビルドが必要です  
  `docker-compose build`

## 参考URL

- Dockerfileの設定内容
   https://github.com/onoya/dockerized-cakephp-app
- CakePHP初期設定
  