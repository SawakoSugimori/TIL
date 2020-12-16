#Docker_Basics_03_MENTA

##基礎知識
-  /rails/tutorial:/myapp　：コロンの左側はローカルのファイル、myappはどっかーで作られる
- ボリュームはローカルOSとDockerOSリンクしてくれる
- 実行優先順（高い方から）
  - docker compose web run new ----- (自分の打ったコマンドを順々に解釈している)
  - docker-compose.ymlファイルの　commandに書かれている部分。
  - Dockerfile CMD
- イメージとコンテナを、オブジェクト指向に例えてみると、クラス（設計図）がイメージで、インスタンス（クラスの具現化）がコンテナ。

###Dockerが開発される前の環境構築から、Dockerの便利さを理解する
- Dockerファイルの内容をLinuxに一行一行打ち込んでた。それが、DockerでDockerfileにコマンドを書き込むだけで、サーバーを一から立てなくても、開発をできるようになった。- 

##Dockerfile
```
# Start the main process.
CMD ["rails", "server", "-b", "0.0.0.0"] 
#コマンドはサーバーが立ち上がって、サーバーに打ち込んでくれる
#一発目のコマンドを自動で打ってくれる
# docker compose run web でCMDが走るはずだが、自分でコマンドを打つことにより、CMDが走らず、newで上書きしていた
```

##docker-composeの何が便利か
- Railsのアプリケーションでは、postgresのコンテナと、Webのコンテナが立ち上がっている。アプリケーションを動かすためには、apiを動かすたびに、その間を通信する必要がある。
WebコンテナからDBコンテナに通信したい。
- そこで本来ならコンテナごとにRUNにしなければないが、docker-composeを使うことによって勝手に通信、接続してくれる。-pで通信先を指定Dockercompose。

##Docker-compose.ymlについて
- docker-compose build でDockerfileをイメージにする。
- 以前は、ディスクファイルに入れていて、CD-ROMからOSを入れていた。isoはCDに焼くことができる。
- ディスクにあるイメージのファイル群をイメージというとから、Dockerでもイメージと使うようになった。
```
ersion: '3'
services:
  db:
    image: postgres
    restart: always
    environment:
      POSTGRES_PASSWORD: hoge
  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    #3000番ポートにホストOSの3000　　DockerコンテナにRailsっていうウェブは3000ポートにいれてください　Railsのサーバパス
    volumes:
      - /c/Program Files/Docker Toolbox/rails/tutorial/app:/myapp
      #左ホスト:右コンテナ。同期をとってくれる.どっちをとっても同じことができる。ディレクトリ内ファイルをすべてmyappにコピーする。
    tty: true
    stdin_open: true
    ports:
      - "3000:3000"
      #左側はホストOSのポート。ホストOSの3000番に入ると、リモートOSの3000番の部屋ですよ、とどこでもドアみたいにしてくれる
      #ホストOsの3000番はlocalhost postgresは5432で立ち上がるから、ポート番号を意識しないことも多い
    depends_on:
      - db
    #指定されたDBが立ち上がってから、railsのサービスが立ち上がる。。正確には、非同期平衡に動くので、一緒に立ち上げてくれる。
```
##Dockerを使う上での注意点
- DockerのエラーはLinuxでうまくいかなかった分、処理のブラックボックス化が起こっている。つまり、なんでエラーが起こっているかが分からない（まさに私）という状態に陥る。
- 調べて、DockerfileやDocker-compose.ymlの内容をコピペしてもいいが、しっかり一文一文の意味を理解すること！
- 環境構築は個人差がある。おかれている環境が違うので、ネットに上がっている記事を参考にしてもいいが、その通りにやっても自分の環境で動くとは限らない。



