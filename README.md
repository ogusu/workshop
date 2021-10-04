# workshop
publicなドキュメント、手順、情報などをまとめたもの



## 1. GitHub and Git

### 1.1 まずは触ってみよう。

* Gitの初期設定。gitコマンドが動くところまで。
  [Git](https://github.com/ogusu/workshop/blob/main/git.md)の１～３を参照
* GitHubの初期設定。pushまで。
  [GitHub](https://github.com/ogusu/workshop/blob/main/GitHub.md)を参照。
* ローカルでコミット・プッシュを何度か繰り返してみよう。

### 1.2. 共用リポジトリに変更を反映してみよう。

* [リポジトリ](https://github.com/ogusu/workshop)をローカルにクローンする。
  * git clone https://github.com/ogusu/workshop.git
* 編集メンバーとして招待を受ける。
* ローカルで編集する。例）ファイルを追加。
* ステージ・コミット・プッシュする。
  * git add xxx
  * git commit
  * git push origin main
* 自分の変更が反映していることを確認する。

### 1.3. 共用リポジトリにプルリクエストを送ろう。

* ブランチを作成する。
  * git branch -b feature/xxbranch
* そのブランチ名でリモートリポジトリへプッシュする。
  * git add xxx
  * git commit
  * git pull
  * git push origin feature/xxbranch
* GitHub上でそのブランチ→mainへのプルリクエストを作成する。
* プルリクエストをマージする。
* 自分の変更が反映していることを確認する。

## 2. Docker

### 2.1. Dockerについて

* https://www.docker.com/
* [Dockerとは](https://www.pasonatech.co.jp/workstyle/column/detail.html?p=2675)
  * コンテナ化したアプリケーションを、他の環境に移行しても動作できることを目的にしたもの。ハイパーバイザ型と違いゲストOSを必要とせず、ホストOSとコンテナでカーネルを共有することで実現しているもの。
  * Dockerの仕組みが知りたい人は[こちら](https://www.gwtcenter.com/how-docker-works)
    > Dockerは、Linux上でのLinuxのプロセスの実行しかしない。Windowsではhyper-vでLinuxカーネルを動作させ、その上でLinuxコンテナを動作させる。


* ポイント
  * Infrastructure as a Code | インフラをコード化してしまおう
  * Immutable Infrastructure | 不変のインフラ環境

### 2.2. 理解の順

* ローカルの仮装環境として使う
  * CentOSの実機やローカルにVMを用意をせず、ローカル環境を汚さずに用意できる。
  * 何度でも作り直せて、同じ環境や設定を保障できる。PGなら同じコードは複数書きたくない気持ちはわかると思う。それをインフラの環境構築に応用したもの。
  * 環境構築の手続きをテキストにすることで、可搬性を高めたり、冪等性を保障している。テキストなので変更管理も用意。
* チームの共通の開発環境として使う
  * テキストになってしまえばチーム間で同一環境を保障することが容易になる。手順を教育する必要がなくなる。
  * フレームワークによっては、環境の上の、DB構築や初期データ挿入、コンテンツのデプロイまで全・半自動化可能。
  * 自動テスト用の環境構築も容易。
* 検証・本番環境の差異の少ない開発環境として使う
  * 環境構築手順が明確なため、開発環境・検証環境・本番環境の差異を減らすことができる。
* 検証・本番環境にそのまま使う
  * ローカル開発環境だけではなく、もしStagingやProduction環境もDockerで構築してそのまま動いたたとしたら、どんなメリットが？　環境差異を全く気にすることがなくなる。すでにサービスとして実現ができるようになっている。
    * [Amazon ECS（Docker コンテナを実行および管理）| AWS](https://aws.amazon.com/jp/lambda/)
    * [Kubernetes (k8s)](https://kubernetes.io/ja/docs/concepts/overview/what-is-kubernetes/)
* さらにその先
  * さらに、メリットは環境差異防止に止まらない。ハードウェア的なCPUやメモリという概念がなくコンテナ単位でスペックやコンテナの数自体も負荷によって自動スケールさせられる。
    * https://www.bit-drive.ne.jp/managed-cloud/column/column_42.html
  * コンテナを包含するサーバーレスというワードも覚えておこう。例えば以下のようにコードをもうそのまま実行できるサービスがある。環境すらいらない。
    * [AWS Lambda（イベント発生時にコードを実行）](https://aws.amazon.com/jp/lambda/)

### 2.3. Dockerのインストール

Dockerのインストール。
* 基本的に影響範囲はHyper-v上だが、VMWareなど他の仮装環境と同居できるかは社内ではおそらく未確認なので調べてからの方が良い。また、Windows10の高速スタートアップとの相性が悪いことがあった。心配ならWSL2にLinux入れるか、Macならストレスなく使える。
* [公式サイト(https://www.docker.com/)のGet Startedから、Docker Desctopをダウンロードしてインストールする。
* 公式サイトの[Windows に Docker Desktop をインストール](https://docs.docker.jp/docker-for-windows/install.html)を参考にインストールします。

### 2.4. Dockerを動かしてみる。

Webサーバー[nginx](https://hub.docker.com/_/nginx/)コンテナをサクッと入れてみよう。Dockerが起動していることを確認して以下のコマンドを実行する。

~~~
$ docker run -d -p 8080:80 --name webserver nginx
~~~
公開されているnginxコンテナ（debianベースにnginxと最低限のパッケージのみがインストールされたコンテナ）が起動する。http://localhost:8080/ でアクセスするとコンテナ内の80ポートで起動するnginxからレスポンスが返ってくる。

以下のコマンドで、コンテナやダウンロードしたイメージの状態を確認できる。
~~~
$ docker container ls
$ docker images
~~~

Linux自体も動かすことができる。最新のCentOS8を入れてみよう。
~~~
$ docker pull centos
$ docker images
  REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
  centos       latest    300e315adb2f   8 months ago   209MB
~~~
以下のコマンドで対話型シェルに接続できる（CentOSの中に入れる）
~~~
$ docker run -it centos /bin/bash
~~~
CentOSの最低限のパッケージがインストールされた状態が確認できる。通常のLinuxなので、mysqlやapacheをインストールすることもできるので色々試してみよう。抜けるときはexit。
~~~
$ cat /etc/redhat-release
CentOS Linux release 8.3.2011
~~~

あとは適当に動かしてみよう。参考）[docker.md](https://github.com/ogusu/workshop/blob/main/git.md)

### 2.5. Dockerを使いこなす

コンテナをコマンドで毎回作成起動するのは大変。yaml形式の設定ファイルを一度作ってしまえば次回から起動が楽になる。
* Dockerfileを書いてみよう。
* docker-composeで複数コンテナを同時に設定と起動しよう。

Microsoft公式の[Windows Nanoコンテナ](https://qiita.com/anikundesu/items/90a7706b434daed5e266)や[IISコンテナ](https://hub.docker.com/_/microsoft-windows-servercore-iis)もあるよ。例えばIISコンテナに起動時のシェル（PowerShell）とを組み合わせると環境構築の自動化も可能。


