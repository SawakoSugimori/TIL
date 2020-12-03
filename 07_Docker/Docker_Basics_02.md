##Docker_Basics_02

##Docker Hub
  - Dockerの公式リポジトリサービス
  - コンテナから独自のイメージを作ることができる

##dockerイメージを作る
  - 開発環境の構築のため。
1. Dockerfileの生成
2. イメージの生成　docker image build 
3. 作成したイメージの動作確認

##Dockerfile の基本構造
- 構文：コマンド名　パラメータ
- コマンド名はアルファベットの大文字で記述

##Dockerhubへイメージをアップロードまでの処理の流れ
- Dockerイメージのタグづけ。docker image tag [image_name] [user_name]/[image_name]:[tag_name]
- DocerHubへのログイン docker login
- Docerイメージのアップロード docker image push [user_name]/[image_name]:[tag_name]
- DocerHubからログアウト 

###example
- docker image pull centos:latest 
- docker container run --name centos_ex -it -d centos:latest
- (echo "Hello, docker container cp,
- docker exec centos_ex(check file)
- docker container commit -a "kowasa" centosex kowasa/entos-ex-img:1.0 、コンテナからイメージを作成する
- docker login
- docker image push kowasa 

##イメージの取得
- docker image pull kowasa/centos-ex-img:1.0(パブリックに公開されているものと同じ）
- docker container run --name centos_con -it -d kowasa/centos-ex-img:1.0
- docker exec centos-con cat /root/centos.txt

##ローカルファイルへの保存と読み込み
- docker image save -o [name].tar [image name]　、-oはファイルに書き込む場合のオプション
- docker image load -i [name].tar
- tarファイル形式での保存と読み込み。tarファイル形式はdockerにおいてよく使われる

##コンテナのエキスポート
- docker container export: ローカルにコンテナを保存
- docker container export [container name] > [filename.tar]。はじめにコンテナを起動させる。保存はリダイレクトして使う
- docker container import: 保存したコンテナをイメージとして読み込む
- cat [filename.tar] | docker image import - [image_name] 
- save, loadはイメージの保存読み込み
- export, importはコンテナの保存と、そこからのイメージ生成

###Dockerの新旧コマンド
- 現在は併用可能
- コンテナ系のコマンドでは、基本的にcontainerが間に追加されたのが新コマンド。例外は、docker container ls (旧：docker ps)
- prune は新コマンドでできた
- イメージ系は、docker image rm (旧：docker rmi)が例外

###Docker Toolboxとは
- Docker環境を簡単に構築するためのインストーラ
- Dockerが動作するLinux仮想マシンを利用する
- 中身は、Docker Client, Docker Machine, Docker Compose, Docker Kitematic, Docker Quickstart Terminal, VirtualBox


###Docker machine
- docker-machine ls
- docker-machine create --driver [driver name][machine name]
- docker-machine env [machine name]。環境変数の確認。マシン名は省略可能
- docker-machine stop [machine name]
- docker-machine status [machine name]。Running, Stopped, Savedの三種類の状態がある
- docker-machine start [machine name]。IPアドレスが変わることがある
- docker-machine restart [machine name]。Saved状態のときに使う
- docker-machine rm [machine name]
- docker-machine ssh [machine name]。指定した名前んマシンにssh接続

###
- Creted ~/Document/docker/example1
- cd ~/Document/docker/example1
- docker build -t webap_sample1 . (webap_sample1がイメージ名）
- docker container run --name websample -it -d -p 80:80 webap_sample1

###Dockerイメージのレイヤー構造

1．ベースイメージ（Ubuntu）
2．nginzのインストール
3．ファイルのコピー
4．80番ポートの解放
5．nginxの起動
  - Docker fileの命令ごとにレイヤーを作る
  - 作成したイメージは他のイメージとも共有されるので、ストレージ容量を効率的に利用できるようになっている。
  - ただ、イメージの定期的な削除が必要

###Dockerfileとは
- Docker上で動作させるコンテナの構成情報を記述するファイル
- docker buildコマンドでイメージを作成する際のもとになる
- 命令　引数　で基本的に書く。命令は大文字で統一する
- FROM <image>:<tag>。ベースイメージの指定
- ENV　環境変数
- RUN <command>。コマンドの実行
- EXPOSE <port_number>
- CMD <command>。デーモンの実行をする。
- ADD ファイル・ディレクトリの追加

###パッケージ管理システム
- YUM (Yellowdog Updater Modified)。CentOSなどRedHad系
- APT (Advanced Pakaging Tool)。Ubuntuなど

###ビルド完了後実行される命令
- ONBUILD命令
- ONBUILDで開発環境をデプロイする命令を入れると。すでに作成済みのベースイメージをもとに新規イメージを作成できる。

###tarコマンド
- 複数のファイルを1つのアーカイブファイルにまとめたり、逆に展開したりするコマンド。
- アーカイブ $ tar cvf tarファイル名 アーカイブ対象ディレクトリ
- 圧縮してアーカイブ $ tar cvzf tgzファイル名 圧縮対象ディレクトリ
- 展開 $ tar xvf tarファイル名
- 解凍して展開 $ tar xvzf tgzファイル名


###Dockerの運用について
- Dockerを運用する際には、複数コンテナが存在する(Linux, Webサーバー,MySQL)
- コンテナ同士は連動して動くため、コンテナ同士がネットワークで結び付けられる必要がある。
- 映像データを扱う必要がある。永続データとは、システム運用時に生成・蓄積されるデータのこと。
永続データを扱うには、コンテナを使うか、ボリュームを使う

###ボリュームとは
- データを永続化できる場所
- 外部HDDのようなイメージ
- コンテナ本体にマウント（外部ストレージ等をディレクトリとして接続）して使う。
- ボリュームを使うメリットは、コンテナが死亡したときでもデータが残ること。ボリュームがコンテナから切り離されているため。
- ボリュームを「バックアップ」として使う方法も。
- マウントされたボリュームはディレクトリとして扱える。
- ファイル書き込み・読み込みが可能
- 保存したボリュームを他のコンテナで使用することが可能

###ボリューム関連のコマンド
- docker volume create [volume_name]
- docker volume ls
- docker volume inspect [volume_name]。詳細情報が出る。jsonファイル形式
- docker volume rm
- docker volume prune。使っていないボリュームの掃除

##コンテナにボリュームをマウントする
- 方法は2つある
1. Dockerコマンドで作成したボリュームをマウントする。
   1. docker volume create [volume_name]
   2. docker run -v [volume_name]:[directory_name]  --rm -it [OSimage]。指定したボリュームをディレクトリにマウントできる
   3. exitで終了するとコンテナが消えるが、ボリュームは残っている

###Dockerfileからボリュームを作る
1．イメージの作成（OS起動、OS内のディレクトリをボリュームに指定）
2．コンテナの生成・起動・削除
3．ボリュームが残る（別のコンテナに接続し、中身を確認）


###Dockerのネットワーク
- コンテナ間のネットワークはDockerのリソースとして管理される。
- デフォルトのネットワークと生成できるネットワークがあり、システムに応じて使い分ける

###NIC 
- ネットワークカードのこと。今は、ネットワークに接続するもの全般のことをいう。
- コンピュータなどの機器を通信ネットワーク（LAN）に接続するためのカード型の拡張装置
- Linuxではeth0というデバイスで認識されている

###ブリッジ接続
- 異なるネットワークセグメント同士を接続すること
- ソフトウェア的方法とハードウェア的な方法がある

###BusyBoxについて
- ひとつのコマンド
- アプレット（コマンド機能に相当するもの）と、libbb（アプレット間で使う関数を定義したもの）に分かれる
- とても軽い。すごく小さいLinuxという感じ
- docker run -it --rm busybox (--rmは終了時に自動的にコンテナが消えるオプション）
- 使われ方は、1)ボリュームの作成、2)ネットワーク、3)その他Linuxの機能など
- docker run -it --rm busyboxでイメージを作成。Linuxの操作ができる。


