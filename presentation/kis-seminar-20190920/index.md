# KISオープンセミナー<br>Docker入門
2019年9月20日

[connpass](https://kis-seminar.connpass.com/event/143699/)<!--- .element target="_blank" rel="noopener" -->

---

# 自己紹介

- 村上　卓
- フリーランス

普段はWebサイトの管理機能やiOS/Androidアプリの開発をしています

---

# 目次(前半)

- 構成管理
- Dockerとは
- イメージとコンテナ
- Dockerを取り巻く環境
- 利用パターン

---

# 構成管理と運用

これまでのマシンの構成管理と昨今の技術要素を一緒に振り返ってみます

---

# 構成管理

---

# 構成管理#1

アプリケーションを開発・運用する場合、それぞれ環境を用意する必要がある  
構成要素としては大きく4つになる

![structures.png](../../image/20190920/structures.png)

---

# 構成管理#2

マニュアルに沿って構成管理を行った場合、その後の運用によって再構築ができないケースになりがち

![manual_man.png](../../image/20190920/manual_man.png)

---

# Immutable Infrastructure

クラウドサービスの普及とともに提唱されるように

- 最初にサーバ構築を行ったあと変更を加えないこと
- 変更を加えたい場合は新たにサーバ構築を行う

![immutable_infrastructure.png](../../image/20190920/immutable_infrastructure.png)

---

# Infrastracture as a code

コードで構成管理をする

- 属人性の排除
- 複数マシンへの実行が容易
- バージョン管理の適用

---

# 構成管理の分類#1

- Bootstrapping
    - OSインストール
    - ネットワーク構成
- Configuration
    - ミドルウェアのインストール・設定
- Orchestration
    - アプリケーションデプロイ
    - サーバ群の管理

---

# 構成管理の分類#2

- Bootstrapping
    - KickStart, OpenStack, Vagrant, AWS
- Configuration
    - Chef, Ansible, Puppet
- Orchestration
    - Kubernetes, Apache Mesos

---

# 構成管理でのDocker

BootstrappingとConfigurationを担う  
Dockerで正しく設計し運用することで構成管理の工数を削減することが期待できる

---

# Dockerとは

---

# Dockerとは

- コンテナ技術を利用して仮想環境を提供するもの
- コンテナの作成、配布、実行が行える
- コード化して環境を管理できるためバージョン管理を行える

---

# コンテナ

ホストOS上に論理的な区画を作成し、個別のサーバのように振る舞うもの  
コンテナはホストOSとファイルやプロセス、ネットワークなどが独立してるように見える

![what_container.png](../../image/20190920/what_container.png)

---

# 仮想化方式

他の仮想化方式も見てみましょう

---

# ホスト型仮想化

ホストOS上でゲストOSが動く

![host_virtualcomputer.png](../../image/20190920/host_virtualcomputer.png)

VirtualBoxやVMware Workstationなど

---

# ハイパーバイザー型仮想化

ハイパーバイザーの上でゲストOSが動く

![hypervisor.png](../../image/20190920/hypervisor.png)

Hyper-VやXenServerなど

---

# Dockerの基本機能

Build, Ship, Runの三機能

- Dockerイメージの作成(Build)
- Dockerイメージを共有(Ship)
- Dockerイメージをコンテナとして実行(Run)

---

# Build

Dockerイメージを作成する機能

- Dockerイメージとはコンテナを作成するのに必要なファイルシステム  
- Dockerイメージがコンテナの雛形
- 読み込み専用でDockerfileという設定ファイルから作成される

![docker_build.png](../../image/20190920/docker_build.png)

---

# Ship

Dockerイメージを共有する機能

- DockerイメージをDockerレジストリで共有可能
- 公式レジストリであるDocker HubでLinuxディストリビューションやミドルウェアのイメージが公開されている
- 個人が作成したイメージも公開、利用が可能

[https://hub.docker.com/](https://hub.docker.com/)<!--- .element target="_blank" rel="noopener" -->

---

# Run

Dockerイメージからコンテナを動かす機能

- Dockerがインストールされた環境であればどこでもコンテナを実行可能  
- Dockerイメージから複数のコンテナを起動可能

![docker_run.png](../../image/20190920/docker_run.png)

---

# イメージとコンテナ

---

# イメージとコンテナ#1

イメージとコンテナは混乱しやすいので特徴をまとめます

- Dockerイメージは読み取り専用
- コンテナはDockerイメージから作成
- コンテナは同じDockerイメージから複数起動可能
- コンテナ内で変更されたファイルはイメージに影響しない
- コンテナを破棄するとイメージからの変更内容は消失する

---

# イメージとコンテナ#2

- Dockerイメージはレイヤという層を積み上げて作成される
- コンテナとは書き込み可能なレイヤを積み上げたもの

![image_and_container.png](../../image/20190920/image_and_container.png)

イメージのレイヤは `docker history` コマンドで確認できる

---

# 永続化

コンテナを削除するとコンテナで行った変更はすべて消失します  
Dockerでデータを永続化する場合2つの方法があります

- データボリュームの利用
- ホストディレクトリのマウント

---

# データボリューム

Dockerが提供する永続化機能  
データボリュームを作成しコンテナにマウントすることができる  
コンテナとは別に保存管理されるため、コンテナが削除されても影響しない

![shared-volume.png](../../image/20190920/shared-volume.png)<!--- .element width="50%" height="50%" -->

出典: [http://docs.docker.jp/engine/userguide/storagedriver/imagesandcontainers.html](http://docs.docker.jp/engine/userguide/storagedriver/imagesandcontainers.html)

---

# ホストディレクトリのマウント

ホストOSのファイルパスをコンテナにマウントすることができる  
永続化されるパスにマウントしデータをホスト上に残せる

![host_mount.png](../../image/20190920/host_mount.png)

---

# Dockerの利用サイクル

![docker_image_cycle.png](../../image/20190920/docker_image_cycle.png)

---

# Dockerコンテナの利用サイクル

![docker_container_cycle.png](../../image/20190920/docker_container_cycle.png)

---

# コンテナの終了条件

コンテナを起動した際イメージで指定された命令がフォアグラウンドで実行される  
(指定された命令: CMDまたはENTRYPOINT)

フォアグラウンドに実行されたプロセスが終了するまでコンテナは起動し続ける

stopコマンドは `SIGTERM` を送信、すぐに終了しない場合 `SIGKILL` を送信  
(killコマンドの場合は最初からSIGKILLを送信)

---

# Dockerを取り巻く環境

---

# 現在までの流れ

2013年に発表されたDockerは瞬く間にコンテナツールとしての地位を確立しました  
主だったニュースを紹介

- 2013年
    - Dockerリリース
    - dotCloudからDocker社に社名変更
- 2014年
    - LXCから独自libcontainerに
    - GoogleがオーケストレーションツールKubernetesを発表
    - VMware、MicrosoftがDockerとの協業を発表
- 2017年
    - Moby Projectの発足
    - OCI(Open Container Intiative)の発表

---

# オーケストレーションツール

コンテナ関連のニュースでKubernetesという名前をよく見かけます  
Kubernetesの前にまずオーケストレーションツールとは何かを確認します

---

# コンテナ間の依存関係

実際にアプリケーションを動かす場合はたくさんの依存関係があります  



---

# 利用パターン

---

# 資料

- [Get Started](https://docs.docker.com/get-started/)<!--- .element target="_blank" rel="noopener" -->
- [Dockerfileを改善するためのBest Practice 2019年版](https://www.slideshare.net/zembutsu/dockerfile-bestpractices-19-and-advice)<!--- .element target="_blank" rel="noopener" -->
- [Dockerイメージの理解とコンテナのライフサイクル](https://www.slideshare.net/zembutsu/docker-images-containers-and-lifecycle)<!--- .element target="_blank" rel="noopener" -->
- [入門 Docker](https://y-ohgi.com/introduction-docker/)

---

# おつかれさまでした🍵
