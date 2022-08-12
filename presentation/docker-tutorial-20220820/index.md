# オトナのDocker入門<br>(2022年版)
2022年8月20日

オトナのプログラミング勉強会<br>
協力 **XOSS POINT**

[connpass](https://otona.connpass.com)<!--- .element target="_blank" rel="noopener" -->

---

# 自己紹介

- 村上　卓
- ソーイ株式会社
- フロントエンド/サーバサイド

---

オトナのプログラミング勉強会<br>
[http://otona.pro](http://otona.pro)<!--- .element target="_blank" rel="noopener" -->

- 2016年8月から開始
- 月1回（第3土曜）
- いつでも講師募集中
    - プログラム言語、機械学習、Web系...
- [YouTube Live](https://www.youtube.com/channel/UCrXf76sF5RUKcGpMpZASqow)<!--- .element target="_blank" rel="noopener" -->でリモート参加も振り返りも可(?)

---

# 注意#1

資料をMac環境で作成しているため、基本的にパス表示はMacOSXです  
WindowsやLinux環境の方は読み替えていただくか、不明な場合は質問してください

また、Docker使用中は電源アダプタの利用をおすすめします

---

# 注意#2

Windowsの方は今回作業するドライブの共有を有効にしてください

![docker-for-windows-shared-drives](../../image/20190807/docker-for-windows-shared-drives.png)<!--- .element width="50%" height="50%" -->

セキュリティソフトのファイアーウォールで警告がでる場合 `10.0.75.0(デフォルト設定)` へのアクセスを許可してください

---

# 注意#3

2017年1月リリースのDockerでサブコマンドが導入  
本資料はサブコマンドを利用しています

```
# 旧コマンド
$ docker images

# サブコマンド
$ docker image ls
```

---

# Dockerをはじめよう

![horizontal-logo-monochromatic-white](../../image/20190807/horizontal-logo-monochromatic-white.png)<!--- .element width="50%" height="50%" style="box-shadow: none; border: none;" -->

---

# Dockerとは

- コンテナ技術を利用して仮想環境を提供するもの
- コンテナの作成、配布、実行が行える
- コンテナはホストOSのカーネルを利用するため仮想マシンより軽量かつ高速
- コード化して環境を管理できるため誰でも同じ環境をつくれる

---

# 仮想マシンとの比較

![docker-vm](../../image/20190807/docker-vm.png)

引用: [https://www.docker.com/resources/what-container](https://www.docker.com/resources/what-container)

---

# 有料化について

Docker社が提供するDocker Desktopの利用が有料化

以下の条件の場合無料で利用可能

- 従業員数250名未満かつ年間売上高1000万ドル未満
- 個人利用
- 教育機関
- 非商用のオープンソースプロジェクト

参考: [https://www.publickey1.jp/blog/21/docker_desktop250100011.html](https://www.publickey1.jp/blog/21/docker_desktop250100011.html)

---

# Dockerを使ってみよう#1

最初にDockerデーモン(Docker for WindowsまたはMacアプリ)を立ち上げてください

デーモンが起動していないと以下のようなエラーになります

```
$ docker info
Client:
 Debug Mode: false

Server:
ERROR: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

---

# Dockerを使ってみよう#2

[hello-world](https://hub.docker.com/_/hello-world/)というイメージを実行します

`docker container run` はイメージからコンテナを作成して実行するコマンドです

```
$ docker container run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete
Digest: sha256:6540fc08ee6e6b7b63468dc3317e3303aae178cb8a45ed3123180328bcc1d20f
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

---

# Dockerを使ってみよう#3

`docker container run hello-world` 実行説明が出力されます

1. DockerクライアントがDockerデーモンに命令
2. DockerデーモンがDocker Hubから `hello-world` イメージを取得
3. Dockerデーモンがイメージからコンテナを作成し、実行
4. Dockerデーモンからクライアントにストリーミング

---

# Dockerを使ってみよう#4

`docker image ls` でダウンロード済みのイメージを確認できます

```
$ docker image ls
REPOSITORY          TAG                  IMAGE ID            CREATED             SIZE
hello-world         latest               fce289e99eb9        7 months ago        1.84kB
```

---

# Dockerを使ってみよう#5

`docker container ls -a` でコンテナ（イメージから発生）の一覧を確認できます

```
$ docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
1c47470d6396        hello-world         "/hello"            17 minutes ago      Exited (0) 17 minutes ago                       interesting_goldwasser
```

---

# PHPを実行してみる#1

hello-worldは出力するだけのイメージのため、次はPHPのイメージを使用してみましょう  
PHPはオフィシャルでバージョンや環境別のイメージを公開しています  

[https://hub.docker.com/_/php](https://hub.docker.com/_/php)

どんなバージョンがあるかはGithubを見ましょう

[https://github.com/docker-library/repo-info/tree/master/repos/php/remote](https://github.com/docker-library/repo-info/tree/master/repos/php/remote)

---

# PHPを実行してみる#2

`docker image pull` を使ってイメージをDocker Hubからダウンロードします  
[イメージ名:タグ]とすることでバージョンを指定できます

```
$ docker image pull php:8-cli-alpine
8-cli-alpine: Pulling from library/php
213ec9aee27d: Pull complete
a600fdbc30cc: Pull complete
...
```

---

# PHPを実行してみる#3

ダウンロードしたイメージでPHPのバージョンを表示してみます

```
$ docker container run php:8-cli-alpine php -v
PHP 8.1.9 (cli) (built: Aug  9 2022 21:47:49) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.1.9, Copyright (c) Zend Technologies
```

---

# PHPを実行してみる#4

PHPのビルドインサーバを使って、 ホストのphpファイルを表示してみます

任意の場所にフォルダを作成し`index.php`を配置しましょう  
あとでフォルダの絶対パスを指定するためパスを控えててください

```
# 例
$ mkdir ~/docker
$ touch ~/docker/index.php

# PowerShell (以降省略)
> New-Item ~/docker/index.php
```

```
// index.php
<?php
phpinfo();
```

---

# PHPを実行してみる#5

dockerを通してビルドインサーバを起動します

```
$ docker container run -it -p 8080:8080 -v /Users/[USERNAME]/docker:/public php:8-cli-alpine php -S 0.0.0.0:8080 -t /public

# Ctrl+c で停止

# Windowsの場合(以降省略)
$ docker container run -it -p 8080:8080 -v C:\Users\[USERNAME]\docker:/public php:8-cli-alpine php -S 0.0.0.0:8080 -t /public
```

起動後は以下のURLでphpinfoが表示されます  
[http://localhost:8080](http://localhost:8080)<!--- .element target="_blank" rel="noopener" -->

---

# PHPを実行してみる#6

パラメータの説明です

```
-it: 標準入力を開き、TTYを割り当てます
-p: "ホスト:コンテナ"でポート番号を指定することでポートフォワードします
-v: "ホスト:コンテナ"でパスを指定することでボリュームを割り当てします
```

---

# -itについて

ほとんどのdockerコマンド例では-itがつけられます  
dockerは起動時に指定されたプロセスが終了するとコンテナも終了します  
itをつけることによってホストとdockerコンテナをつなげプロセス終了を防ぐことができます

```
$ docker container run php:8-cli-alpine
# 終了

$ docker container run -it php:8-cli-alpine
Interactive shell

> # インタプリタが利用可能
> exit # 終了コマンドを打つ
```

---

# イメージを作ろう#1

先程のビルドインサーバの実行まで1セットになるようにオリジナルのイメージを作成します

---

# イメージを作ろう#2

DockerはDockerfile元にイメージ作成ができます
最初に `Dockerfile` という名前のファイルを作成します

```
$ cd ~/docker
$ touch Dockerfile
```

---

# イメージを作ろう#3

```
# Dockerfile
# ベースとなるイメージ名
FROM php:8-cli-alpine

# 環境変数の宣言
ENV PUBLIC_DIR /public

# 作業ディレクトリを指定
WORKDIR $PUBLIC_DIR

# ビルドするホストからイメージにファイルをコピー
COPY . $PUBLIC_DIR

# 使用するポートを宣言
EXPOSE 8080

# コンテナ作成時にコマンド実行
CMD [ "php", "-S", "0.0.0.0:8080", "-t", "/public" ]
```

---

# イメージを作ろう#4

Dockerfileを元にイメージをビルドします  
buildコマンドでファイル名を指定しない場合Dockerfileをデフォルトで利用します

```
 $ ls
Dockerfile index.php

$ docker image build --tag=phpserver .
[+] Building 0.2s (8/8) FINISHED
 => [internal] load build definition from Dockerfile
...
 => => naming to docker.io/library/phpserver
```

```
# 確認
$ docker image ls
REPOSITORY                           TAG               IMAGE ID       CREATED         SIZE
phpserver                            latest            28973baad4eb   4 seconds ago   92.2MB
```

---

# イメージを作ろう#5

作成したイメージでコンテナを起動します

```
$ docker container run -p 8080:8080 phpserver
# access to http://localhost:8080
# Ctrl+c で停止
```

**補足**

```
# Windowsの場合Ctrl+cでコンテナが停止しないためstopコマンドを使用
# コマンド例
$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
1c47470d6396        hello-world         "/hello"            17 minutes ago      Exited (0) 17 minutes ago                       interesting_goldwasser

$ docker container stop 1c47470d6396
```

---

# イメージを作ろう#6

作成したイメージはイメージ内に実行するPHPファイルをコピーしています  
新規で作成したファイルや既存ファイルの変更を反映させる場合は再度ビルドする必要があります

```
$ echo "hello" >> echo.php
$ docker run -p 8080:8080 phpserver
# http://localhost:8080/echo.php is Not Found

$ docker container build --tag=phpserver .
$ docker container run -p 8080:8080 phpserver
# http://localhost:8080/echo.php is OK
```

---

# イメージを作ろう#7

開発中は `-v` を使ってホストのディレクトをマウントすることもできます  
この場合は再ビドルは必要ありません  
用途に合わせて使いこなしましょう

```
$ docker container run -p 8080:8080 -v /Users/[USERNAME]/docker:/public phpserver
```

---

# none？

docker buildやpullをすると `<none>` というイメージが作成される場合があります  
`<none>` は同じタグ名で新しいものができた場合に古いイメージが差し替えられてできます  
今回は2回 `phpserver` というタグ名で作成したので `<none>` ができています

```
$ docker image ls
REPOSITORY          TAG                  IMAGE ID            CREATED              SIZE
phpserver           latest               c186cf469288        About a minute ago   79.3MB
<none>              <none>               a3b003757c76        5 minutes ago        79.3MB
```

`image prune`を使用することで削除できます  
(イメージに紐付いたコンテナがある場合削除されません)

```
$ docker image prune
WARNING! This will remove all dangling images.
Are you sure you want to continue? [y/N]
```

---

# 休憩(質問あれば)

---

# LAMP環境を作ろう

今回はLaravelというPHPフレームワークが実行できる環境を作ります  
LAMPは以下の実行環境です

- Linux
- Apache
- MySQL
- PHP

---

# Docker Composeを使おう#1

dockerコマンドを繰り返し使用することで、複数のコンテナを連携して使用することはできます  
しかし、それらを毎回入力したり依存関係がある場合は管理が大変です

Docker Composeを使用して複数のコンテナを宣言的に扱うようにできます

---

# Docker Composeを使おう#2

新しく作業ディレクトリを作成し、 `docker-compose.yml` を作成します

```
$ mkdir ~/lamp && cd ~/lamp
$ touch docker-compose.yml
```

---

# Docker Composeを使おう#3

Docker Composerではイメージ名を指定して利用する方法と、Dockerfileを指定して利用する方法があります  
まずはイメージ名を指定してMySQL実行してみましょう  

MySQLのバージョンや環境変数の設定などはURLから確認できます  
[https://hub.docker.com/_/mysql](https://hub.docker.com/_/mysql)<!--- .element target="_blank" rel="noopener" -->

また、MySQLのWebクライアントとしてAdminerを入れて同時に実行します

---

# Docker Composeを使おう#4

```
# docker-compose.yml
version: '3.9'
services:
  db:
    image: mysql:8.0.23
    restart: always
    command: --default-authentication-plugin=mysql_native_password
    environment:
      - MYSQL_ROOT_PASSWORD=password
      - MYSQL_DATABASE=sample
      - TZ='Asia/Tokyo'
    ports:
      - 3306:3306
  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
```

---

# Docker Composeを使おう#5

`docker compose up` コマンドを使用してイメージをダウンロードしてコンテナを起動します  
`-d` オプションを渡すことでバックグランドで起動します

```
$ docker compose up -d
Creating network "lamp_default" with the default driver
Pulling db (mysql:8.0.17)...
8.0.17: Pulling from library/mysql
0a4690c5d889: Already exists
98aa2fc6cbeb: Pull complete
0777e6eb0e6f: Pull complete
...
```

---

# Docker Composeを使おう#6

Adminerを通してMySQLの接続を試します  
パスワードは環境変数(environment)で渡した `password` です

[http://localhost:8080](http://localhost:8080)<!--- .element target="_blank" rel="noopener" -->

![adminer1](../../image/20190807/adminer1.png)<!--- .element style="box-shadow: none; border: none;" -->


---

# Docker Composeを使おう#7

ログインできました

![adminer2](../../image/20190807/adminer2.png)<!--- .element style="box-shadow: none; border: none;" -->

---

# Docker Composeを使おう#9

`compose down` コマンドを使用することで起動中のコンテナを破棄できます  
仮に現在の設定でMySQLにデータを保存していた場合、全て **破棄** されます

```
$ docker compose down
```

* コンテナの起動/停止を行うstart/stopコマンドもあります

---

# Docker Composeを使おう#10

永続化を行いたい場合2つの方法があります

- Docker Volume
- ホストOSのディレクトリ

Docker Volumeはコンテナとは別に管理されるもので、コンテナが削除されても影響されません  
Docker Volumeは `volume ls` コマンドで見ることができます

```
$ docker volume ls
DRIVER              VOLUME NAME
local               3e7fe090d4ca063...
```

---

# Docker Composeを使おう#11

今回はDocker Volumeを使ってみましょう  
`docker-compose.yml` を編集します

```yml
# docker-compose.yml
version: '3.7'
services:
  db:
    image: mysql:8.0.17
    # 省略
    ports:
      - 3306:3306
    # 下2行追加
    volumes:
      - data:/var/lib/mysql
  adminer:
    # 省略
# volumesから2行追加
volumes:
  data:
```

---

# Docker Composeを使おう#12

再度 `up` コマンドを実行するとVolumeが作成されて永続化されます

```
$ docker compose up -d
Creating network "lamp_default" with the default driver
Creating volume "lamp_data" with default driver
```

---

# Docker Composeを使おう#12

現在コンテナ起動時にDocker ComposerのcommandでMySQLの設定を変更しています  
`my.cnf` のようなファイルに記述するためcommandから差し替えましょう

docker用の設定フォルダを作成します

```
$ mkdir -p docker/db/conf.d
$ touch docker/db/conf.d/my.cnf
```

```
# my.cnf
[mysqld]
default_authentication_plugin=mysql_native_password
```

---

# Docker Composeを使おう#13

dbサービスからcommandを削除して、volumeで設定を読み込めるようにします

```yml
# docker-compose.yml
db:
    image: mysql:8.0.17
    restart: always
    # command行を削除
    environment:
      - MYSQL_ROOT_PASSWORD=password
      - MYSQL_DATABASE=sample
      - TZ='Asia/Tokyo'
    ports:
      - 3306:3306
    volumes: # 下1行追加
      - ./docker/db/conf.d/my.cnf:/etc/mysql/conf.d/my.cnf
      - data:/var/lib/mysql
```

---

# Docker Composeを使おう#14

コンテナを再起動して動作に問題ないか確認してください

```
$ docker compose down
$ docker compose up -d
```

---

# LAMP環境を作ろう#1

Docker Composeを使ってMySQLを立ち上げることができました  
次はApacheとPHPの実行環境を作成します  

Laravelを動かすには素のPHPイメージだけでは足りないので、Dockerfileで環境を整える必要があります  

先程のdockerコマンドでは `php:8-cli-alpine` のイメージを使用しました  
今回はApacheとPHPがセットになった `php:8.1-apache-bullseye` を使用します

---

# LAMP環境を作ろう#2

まずはDockerfileを元にコンテナを立ち上げできるようにします

```
$ mkdir docker/web
$ touch docker/web/Dockerfile
```

```
# docker/web/Dockerfile
FROM php:8.1-apache-bullseye

EXPOSE 80
```

---

# LAMP環境を作ろう#3

`docker-compose.yml` にwebサービスを追加します

```yml
# docker-compose
services:
  web:
    build:
      context: .
      dockerfile: ./docker/web/Dockerfile
    ports:
      - 8000:80
    volumes:
      - .:/var/www/html
  db:
  ...
```

---

# LAMP環境を作ろう#4

起動するとDockerfileをビルドして実行します

```
$ docker compose down
$ docker compose up -d
Creating network "lamp_default" with the default driver
Building api
Step 1/2 : FROM php:7.3.8-apache-stretch
7.3.8-apache-stretch: Pulling from library/php
0a4690c5d889: Already exists
b07fddb037ca: Pull complete
...
Application key set successfully.
```

---

# LAMP環境を作ろう#5

Apacheが実行できているか確認します

[http://localhost:8000](http://localhost:8000)<!--- .element target="_blank" rel="noopener" -->

![apache-forbidden](../../image/20190807/apache-forbidden.png)

---

# LAMP環境を作ろう#6

何か表示させたい場合は、HTMLを作成しましょう  
ホストのカレントディレクトリが公開フォルダにマウントされていることがわかります

```
$ echo "<h1>Hello</h1>" >> index.html
```

![apache-hello](../../image/20190807/apache-hello.png)

---

# LAMP環境を作ろう#7

DockerfileでLaravel実行に必要なパッケージを追加します  

```
# docker/web/Dockerfile
FROM php:8.1-apache-bullseye

# nodeとnpmのインストール
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
# composerのインストール
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# composerで必要なものとmysql、gd利用のためのパッケージ
RUN apt-get update \
&& apt-get install -y git gnupg unzip nodejs \
libfreetype6-dev libjpeg62-turbo-dev libpng-dev libonig-dev \
&& docker-php-ext-install pdo_mysql mysqli mbstring \
&& docker-php-ext-install -j$(nproc) iconv \
&& docker-php-ext-configure gd \
--with-jpeg=/usr/include/ \
--with-freetype=/usr/include/ \
&& docker-php-ext-install -j$(nproc) gd \
&& apt-get clean && rm -rf /var/lib/apt/lists/* \
&& a2enmod rewrite

EXPOSE 80
```

---

# LAMP環境を作ろう#8

再度イメージをビルドをします

```
$ docker compose build web
...
# コンテナを再作成
$ docker compose down
$ docker compose up -d
```

---

# LAMP環境を作ろう#9

composerが使えるようになったので、Laravelのプロジェクトを作成します

```
$ docker compose exec web composer create-project --prefer-dist laravel/laravel sample
Creating a "laravel/laravel" project at "./sample"
Info from https://repo.packagist.org: #StandWithUkraine
Installing laravel/laravel (v9.3.3)
...
```

---

# LAMP環境を作ろう#10

composerのインストールが終わるとパスを直接叩くことでLaravelの初期画面が見えます

[http://localhost:8000/sample/public/](http://localhost:8000/sample/public/)<!--- .element target="_blank" rel="noopener" -->


![welcome-laravel](../../image/20220820/welcome-laravel.png)<!--- .element width="50%" height="50%" -->


---

# LAMP環境を作ろう#11

ApacheとPHPの設定をできるようにします

```
$ mkdir docker/web/sites-enabled
$ mkdir docker/web/php
$ touch docker/web/sites-enabled/000-default.conf
$ touch docker/web/php/php.ini
```

今回は `php.ini` は編集しませんが、触る機会は多いので入れています

---

# LAMP環境を作ろう#12

DocumentRootをLaravelのpublicフォルダにします  
また `docker-compose.yml` でマウントするフォルダにsampleを指定します

```
# docker/web/sites-enabled/000-default.conf
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html/public
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

```yml
# docker-compose.yml
web:
  volumes: # ./:/var/www/htmlから書き換え
      - ./sample:/var/www/html
```

---

# LAMP環境を作ろう#13

ビルド時に各設定ファイルをコピーします

```
# docker/web/Dockerfile
# 省略
RUN apt-get update \
...

# 3行追加
COPY ./docker/web/php/php.ini /usr/local/etc/php/
COPY ./docker/web/sites-enabled/*.conf /etc/apache2/sites-enabled/
WORKDIR /var/www/html

EXPOSE 80
```

* `000-default.conf` は元ファイルを上書きしています

---

# LAMP環境を作ろう#14

ビルドして再起動します

```
$ docker compose build web
$ docker compose down
$ docker compose up -d
```

ルートでアクセスできるようになります

[http://localhost:8000](http://localhost:8000)<!--- .element target="_blank" rel="noopener" -->

---

# LAMP環境を作ろう#14

最後にLaravelのマイグレーションを行いMySQLが使えることを確認します  
Laravelの `.env` を書き換えます

```
# sample/.env
# DB_HOST, DB_DATABASE, DB_USERNAME, DB_PASSWORD を書き換え
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=sample
DB_USERNAME=root
DB_PASSWORD=password
```

---

# LAMP環境を作ろう#15

マイグレーションをします

```
$ docker compose exec web php artisan migrate
Migration table created successfully.
Migrating: 2014_10_12_000000_create_users_table
Migrated:  2014_10_12_000000_create_users_table (0.05 seconds)
Migrating: 2014_10_12_100000_create_password_resets_table
Migrated:  2014_10_12_100000_create_password_resets_table (0.03 seconds)
```

---

# LAMP環境を作ろう#16

Adminerで確認してみてください

![adminer2](../../image/20190807/adminer3.png)<!--- .element style="box-shadow: none; border: none;" --3

---

# 終わりに

今回の環境構築はあくまで一例です  
php-fpmとapacheイメージを使って構築するものもあれば、nginxを利用したものもあります  

イメージを小さくするための方法や、マルチステージビルドといった機能もあります  
ぜひ今回登場していない命令や機能を使って試してみてください

---

# 後始末

不要なコンテナやイメージ、ボリュームができていると思うので、まるごとリセットしてしまうか削除コマンドを使用するとすると良いです  

```
# コンテナ一覧
$ docker container ls -a
# コンテナ削除
$ docker container rm [NAMES or CONTAINER_ID]

# イメージ一覧
$ docker image ls
# イメージ削除
$ docker image rm [IMAGE_ID]

# ボリューム一覧
$ docker volume ls
# ボリューム削除
$ docker volume rm [VOLUME_NAME]
```

---

# 資料

- [Get Started](https://docs.docker.com/get-started/)<!--- .element target="_blank" rel="noopener" -->
- [Dockerfileを改善するためのBest Practice 2019年版](https://www.slideshare.net/zembutsu/dockerfile-bestpractices-19-and-advice)<!--- .element target="_blank" rel="noopener" -->

---

# おつかれさまでした🍵
