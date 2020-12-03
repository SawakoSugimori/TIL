
#Docker

##現在のシステム開発の二大特徴

1．継続的インテグレーション
   - アプリのコードを追加・修正するごとにテストする
   - 確実に実行するコードを保持する
 
2．継続的デリバリー
  - 機能を追加するごとにアプリケーションをリリースする
  - 開発とリリースの短いサイクルを繰り返す
  - 昔は、ウォーターフォールという、要求定義、設計、実装、テストを完了してサービスを提供していた。
　サービスを提供したら終了
  - 現在は、バージョンアップを繰り返す。要求定義からテストまでを短いサイクルで回す

###ここで運用の問題が出てきた！
  - 継続的デリバリーだと、使用するOS, プログラミング言語、ミドルウェアのバージョンアップなど整合性をとるのがと手大変！
  - 従来は、インフラ構築とプログラミングをするグループは別々でもよかった。
  - でも今は、「DevOps」（開発チームと運用チームが互いに協調しあうこと）が必要になった。
  - ソースコードと環境の整合性をとる必要になる＝「Docker」が必要！

##
##Dockerとは
  - Docker社が開発した仮想環境プラットフォームのこと
  - 仮想環境を作成・配布・実行することができる

##なぜDockerが必要か？
  - 特定のOSへの依存がなく導入が容易
  - 案件ごとに異なる環境を構築でき、特定のPCに依存しない
  - ミドルウェア導入・新インフラ環境のテストが各自のPCで可能になる
  - 言語野ツールのバージョンアップテストが容易
  - チームメンバー全員が各自のPCでデバッグ可能になる

###通常のシステム環境の流れ
  - 開発環境→テスト環境→ストリーミング環境→プロダクション環境
  - 同じWebサーバーのApacheでも、環境が違うと動かない時がある

  - Dockerイメージというもので同じ環境を配ってしまう！

##
##Dockerコンテナとは
  - インフラ環境をまとめたもの（アプリケーションの実行モジュール、ミドルウェアやライブラリ群、OS・インターネットなどの環境設定）
  - Dockerイメージを基に作成する

##Dockerイメージ
  - 開発したアプリケーションの実行に必要なものを集めたもの
  - Docker Hubなどのレポジトリで共有される

##Dockerの機能
  - Build：Dockerイメージを作る機能
    - Automated Build：GitHub上でDockerfileを管理しDockerイメージを自動生成可能
    - Docker Content Trust, Docker Seurity Scanning
  - Ship：Dockerイメージを共有する機能
  - Run：Dockerコンテナを動かす機能
    - ひとつのイメージから複数のコンテナを作ることができる

##Dockerのコンポーネント
  1. Docker Engine：Dockerのコア機能。Dockerイメージの生成、Dockerコマンドの実行
  2. Docker Registry：Dockerイメージを共有・公開するための機能
  3. Docker Compose：複数コンテナを一元管理するためのツール
  4. Docker Machine：Docker実行環境を構築する
  5. Docker Swarm：クラスタ管理など

##Dockerの処理の流れ
- DockerHubからイメージをダウンロードしてきて、イメージからコンテナを生成・起動する
1. イメージのダウンロード
    - docker image pull [option] [imagename] (docker pull nginx)
3. docker conainer run --name webserver -d -p 80:80 nginx
    - -d　ディタッチモードを表す。バックグラウンドでの起動させたいとき。このオプションがないと他のサービスを同時に実行できない
    - -p ポート番号の指定。ホスト：コンテナ
    - イメージ名:タグ
4. docker stop webserver
    - -it ターミナルでコンテナを実行することができる。-i　標準入力、-t ttyを割り当てる。
    - docker image pull ubuntu:18.04
    - docker contaier run --name my-ubuntu -it -d ubuntu:18.04

##動作確認、稼働中のコンテナへの接続コマンド
  - docker exec　バックグラウンドで実行されているコンテナを呼び出す。コマンドを実行するごとに呼び出す必要がある。
  - docker exec -it my-ubuntu/bin/echo"Hello"
  - docker attach バックグラウンドのコンテナをフォアグラウンドにして実行する。exitで終わるとstopする
  - docker container attach my-ubuntu 

##コンテナとイメージの削除
  - docker stop xxx
  - docker container rm xxx 
  - codker image rm (-f) xxx
  - 無駄なイメージ・コンテナが増えすぎるとリソースを圧迫するので適宜削除が必要
  - 整合性の観点から、コンテナ→イメージの順で消すのが妥当

###docker system prune
  - docker run [image name]により生成された無名のコンテナを削除でき、イメージを削除できる
  - docker system prune 不要なイメージ・コンテナを一括削除
  - docker image prune
  - docker container prune

##DockerでOSを起動する場合
  - docker run -it ubuntuとするとイメージのダウンロード、コンテナの生成起動が一気に行われる。コンテナ名は自動で生成。
  - 起動しているOSから抜ける場合、exitではななく、ctrl+p, ctrl+qをするとコンテナが終了しない
  


