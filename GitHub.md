# GitHub

https://github.co.jp/

GitHubは、gitのリポジトリのホスティングサービスであり、開発のプロセス全般をカバーする機能を持った開発プラットフォームです。

TFSでいうサーバーにあたります。ローカルで書いたソースコードをpushしてアップロードしたり、pull requestというレビュー＆マージの依頼を送ったり、issue（チケット）の管理をしたり、もちろんソースの変更管理を行ってくれます。

GitHubを使用しない場合、ローカルでgitのコミットを繰り返すか、リポジトリサーバーを別途立てる必要があって面倒。なによりpull requestという強力なレビュープロセスが取り入れられなくなる。

無料プランと有料プランがある。いまではprivateリポジトリも無料になったので勉強用途なら無料で何も問題ない。



先にGitの設定を済ませておいてください。



## GitHubの登録

登録は無料なので、サインアップしてみましょう。

詳しくは、[【Windows】Gitの環境構築をしよう！](https://prog-8.com/docs/git-env-win)の「3. Githubの設定」を参照。

### Githubアカウントの作成
[GitHub](https://github.co.jp/)を開き、「サインアップ」のページへ。メールアドレス、ユーザー名、パスワードを入力して登録します。個人の勉強用なら自分のメールアドレスにしておきましょう。

登録が完了すると、Githubから認証メールが届きますので、 そのメール内のURLをクリックすることで完了です。サインインして以下を開いてみてください。

https://github.com/ユーザー名

ここが自分のGitHubのポータルです。

### リモートリポジトリの作成
GitHubのリポジトリは、ローカルのリポジトリと区別してリモートリポジトリと呼びます。
以下の手順でリモートリポジトリ（以下リポジトリと略）を作成して、ローカルのソースをpushできるようにしてみます。

https://github.com/ユーザー名

を開いて「Repositories」をクリックするか、右上のアイコンのドロップダウンから「Your Repositories」をクリックして、リポジトリの一覧をだします。
「New」をクリックすると、「Create a new repository」画面に飛ぶので、以下の情報を入力します。あとはデフォルトでOK
* Repository name　あとで変更はできないので慎重に。当然英語名で。
* Public/Private　理由がなければPrivateでOKです。あとでPublicに変更できます。
  * Public・・・公開。誰でも参照できる。なんならソース変更して変更いらいも出せる。
  * Private・・・限定。自分以外は参照できない。ただしメンバー指定すれば共有できる。

## リモートリポジトリの確認

リモートリポジトリの作成が完了すると、専用ページが作られます。以下はユーザー名「ogusu」、リポジトリ名「workshop」の場合

https://github.com/ogusu/workshop



このページの「Code」をドロップダウンすると、Cloneのときに指定するパラメータを確認したり、クリップボードにコピーできます。

* 「SSH」を選んで、「git@github.com:ogusu/workshop.git」の右にあるアイコンをクリックします。



## GitHubにpushする

ローカルのソースをGitHubのリモートリポジトリにpushしてみます。その前に以下が必要です。

* GitHubにリモートリポジトリを作成しておく。これは上の手順で済。
* Gitでローカルのリポジトリを作成してcommitする。
* sshでローカルからGitHubに通信できるようにしておく。
* Gitでローカルのリポジトリへ、リモートリポジトリを登録しておく。
* GitHubにpushする。



### Gitでローカルのリポジトリを作成してcommitする

まずはローカルのリポジトリ（Gitで管理するディレクトリ）を作成し、移動します。

ディレクトリの位置はどこでもOK。作成はExplolerでもいいですが、練習がてら 以下のコマンドを Git Bash上で実行してください。以下例はローカルのリポジトリ名を「workshop」としています。リモートリポジトリ名とは一致しなくてもいいですがわかりにくいので一緒にしておきましょう。

~~~
$ mkdir workshop
$ cd workshop
~~~

そのフォルダをGitで管理できるように、以下のコマンドを実行します。
~~~
$ git init
~~~

適当にファイルを一つ作って保存します。例として「sample.txt」とします。

~~~
$ echo "test" > sample.txt
~~~

変更をインデックスへ追加してからコミットします。インデックスはコミットする単位のまとまりのようなものです。インデックスに変更を追加していって、まとめて一つの変更としてコミットすることで、変更を追いやすくする仕組みです。TFSの変更セットみたいなものですね。

~~~
$ git add sample.txt
$ git commit
~~~

vimのエディターが開くので、「i」で編集モードにして、コメントを適当に入力したら、ESCを押して、「wq」で保存する。

※面倒なら以下で一気に終わらせられる。

~~~
$ git add .
$ git commit -m "first commit."
~~~

### sshでローカルからGitHubに通信できるようにしておく

sshの設定済みの場合はスキップできます。設定済みかどうかは自分のホームディレクトリ以下の.sshディレクトリがあり、id_rsaとid_rsa.pubというファイルがあれば設定済みです。以下は設定がない場合の説明です。



sshに慣れていないとちょっと難しいかもしれませんが、手順は以下です。

* 秘密鍵・公開鍵のペアを作成する。秘密鍵は大事に保管し誰にも見せない。公開鍵は見られてもOK。
* 公開鍵をGitHubに登録しておく。



まず、秘密鍵・公開鍵のペアを作成する。Git Bashでホームへ移動してコマンドをたたく。以下のように鍵の保管先のファイル名を聞かれる。デフォルトのままでOKなのでEnterキーだけ押します。パスフェーズは任意ですが、あとでVisual Studio Codeと連携いたいときはつけない方がいいです。

~~~
$ cd ~
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/ogusu.TSDOMAIN/.ssh/id_rsa):
~~~

完了すると、id_rsa（秘密鍵）と、id_rsa.pub（公開鍵）の二つのファイルができあがります。

秘密鍵は大事に保管し誰にも見せないこと。公開鍵は見られてもOK。

公開鍵の中身をクリップボードにコピーしておきます。



GitHubにサインインして、右上の自分のアイコンから、Settingsを開きます。

「SSH and GPG keys」を開き、SSH Keysの「New SSH Key」をクリックします。

Titleは例えばPC名にして、Keyに先ほどの公開鍵の内容をペーストして、Add SSH Keyで保存します。

これで、そのPCからGitHubにSSH接続できるようになったので、pushできるようになりました。



id_rsaがあるけどid_rsa.pubがない場合



### Gitでローカルのリポジトリへ、リモートリポジトリを登録しておく

デフォルトだとリモートリポジトリが未設定なので、以下でpush先のリモートリポジトリを設定しておく。

上記の「リモートリポジトリの確認」で確認しておいたリモートリポジトリを「origin」というエイリアス名で登録しています。

~~~
$ git remote add origin <リモートリポジトリ>
~~~



### GitHubにpushする

GitHubのリモートリポジトリへ、ソースをアップ（push）してみます。ソースの置いてあるディレクトリのトップで以下を実行します。以下ではoriginというリモートリポジトリのmainブランチへpushしています。

~~~
$ git push -u origin main
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
・・・
~~~



pushが成功したら、GitHubのリモートリポジトリを見てみましょう。sample.txtが反映されているはずです。



以下のような場合はsshの設定に不備があります。設定を見直します。

~~~
$ git push origin master
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
~~~

リポジトリを先に作っていて、さらにreadmeファイルを作ってコミットしている場合以下のエラーになる。
fatal: refusing to merge unrelated histories
その場合は以下で解消できる
~~~
$ git merge --allow-unrelated-histories origin/main
~~~

まずはローカルでの編集をコミットし、プッシュする練習を繰り返してみましょう。また、新しいリポジトリを作成してプッシュしてみましょう。

