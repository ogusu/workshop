# Git



## 1. 概要
https://ja.wikipedia.org/wiki/Git

Gitは、プログラムのソースコードなどの変更履歴を記録・追跡するための **分散型バージョン管理システム** である。Linuxカーネルのソースコード管理に用いるためにリーナス・トーバルズによって開発され、それ以降ほかの多くのプロジェクトで採用されている。Linuxカーネルのような巨大プロジェクトにも対応できるように、動作速度に重点が置かれている。現在のメンテナは濱野純 (英語: Junio C Hamano) で、2005年7月から担当している。

Gitでは、各ユーザのワーキングディレクトリに、全履歴を含んだリポジトリの完全な複製が作られる。したがって、ネットワークにアクセスできないなどの理由で中心リポジトリにアクセスできない環境でも、履歴の調査や変更の記録といったほとんどの作業を行うことができる。これが「分散型」と呼ばれる理由である。

## 2. 基本的な作業の流れ
Gitは分散型のソースコード管理システムであるため、リモートサーバ等にある中心リポジトリの完全なコピーを手元（ローカル環境）に作成して、そのローカルリポジトリを使って作業を行う。

一般的な開発スタイルでは、大雑把に言えば、以下のようなステップの繰り返しで作業が行なわれる：（若干怪しいのでいずれ直すがまずは用語を知る程度でOK）

1. **git clone** リモートサーバ等にある中心リポジトリをローカルに複製する。
2. **git commit** ローカルでコンテンツの修正・追加・削除を行い、ローカルリポジトリに変更履歴を記録する 。必要に応じて過去の状態の閲覧や復元などを行う。場合によってはこのステップを何度か繰り返す。
3. **git push** ローカルの変更内容を中心リポジトリに反映させる。ただし他者の修正とコンフリクトした場合は以下の対応が必要。
4. **git merge** 他社の修正を確認しながらローカルで手動でマージする。
5. **git pull** 更新された中心リポジトリ（他者の作業内容も統合されている）をローカルの複製にも反映する 。これによりローカル環境のコードも最新の内容になるので、改めてステップ2の作業を行う。

リポジトリ間の通信 (clone, pull, push) では以下のプロトコルが使用できる[6]。
* ローカルファイルシステム[6]
* SSH[6]
* git（認証機能なし）[6]
* HTTP/HTTPS[6]



## 3. Gitを使ってみる



### Git Bashのインストール

Windows環境にはgitは入っていない。昔のCygwinのような「Git Bash」というコマンドラインツールが、[git for Windows](open sourceのアプリが使いやすいのでお勧めhttps://gitforwindows.org/)というopen sourceソフトに同梱されているのでダウンロードしてインストールする。



インストール手順は[【Windows】Gitの環境構築をしよう！](https://prog-8.com/docs/git-env-win)を参照。以下補足。

* デフォルトのエディターは、コミットの時にコメントを入力する際に使うエディターで、好みだが「Use Vim・・・」にして、この際viのコマンドを覚えましょう。
* 「Use Git from Git Bash only」「Use the OpenSSL library」「Checkout Windows-style, commit Unix-style line endings」「Use MinTTY」を選択すること。間違っても何とかなるが。

### Gitの初期設定



Git Bashのインストールが終わったら上記サイトの手順通り引き続き起動して初期設定を行います。



ユーザー名とメールアドレスを設定する。コミットしたときauthor情報として登録されます。
~~~
$ git config --global user.name "ユーザー名"
$ git config --global user.email "メールアドレス"
~~~

次にデフォルトのブランチをGitHubに合わせてmainにする。

~~~
$ git config --global init.defaultBranch main
~~~

以上で初期設定が完了です。GitHubの操作にある程度なれたら、以下の基本操作を学習しましょう。



## 4. Gitの基本的操作

以下ではGitの各コマンドの基本的な使い方を説明します。※なおデフォルトのリポジトリ名が古いmasterになっていいるのでmainに読み替えてください。



リポジトリが `sturdy-engine` の例。リポジトリのURLは以下になる。 
https://github.com/ogusu/sturdy-engine

## ローカルリポジトリの作成（init）

GitHubでリポジトリを作れるが、ローカルか作業を初めて、GitHubにPushするときに作成したい場合は以下の通り。

まず、ローカルにフォルダを作り、gitのリポジトリを作る
~~~git
mkdir Repository
cd Repository/
git init
    Initialized empty Git repository in /Users/xxx/Documents/Repository/.git/
~~~

## comitterを変更する

コミットに登録されるcomitterなどを変更しておく。
~~~git
git config --local user.name xxx
git config --local user.email xxx@gmail.com
git config --local --list
git commit --amend
~~~

## authorの変更
~~~git
git commit --amend --author="xxx xxx"
git rebase --continue
git commit --amend
~~~

## ローカルリポジトリに変更を反映（add, commit）

適当にファイルを作って、変更点を反映する。インデックスの追加と、コミットをする。  
「作業ツリー」は単純なファイルシステム上のファイルですが「インデックス」はGitが管理するバイナリファイルです。(.gitディレクトリ内にあるindexというファイルです。)	

~~~git
echo "# sturdy-engine" >> README.md
ls -a
    .  ..  .git  README.md
    
git add README.md

git status
    On branch master
    Initial commit
    Changes to be committed:
    (use "git rm --cached <file>..." to unstage)
    new file:  README.md

git commit -m "my first commit"
    [master (root-commit) 41cee5f] my first commit
    Committer: xxx <xxx@mba.local>
    Your name and email address were configured automatically based
    on your username and hostname. Please check that they are accurate.
    You can suppress this message by setting them explicitly. Run the
    following command and follow the instructions in your editor to edit
    your configuration file:
    git config --global --edit
    After doing this, you may fix the identity used for this commit with:
    git commit --amend --reset-author
    1 file changed, 1 insertion(+)
    create mode 100644 README.md

git status
    On branch master
    nothing to commit, working tree clean
~~~

## リモートリポジトリに変更を反映（push）

次に、リモートリポジトリへPushする。  
pushの前に、gitへリモートリポジトリの位置を登録。以降はエイリアス `origin` でpushできる。  
まだGitHubにリポジトリはないがpushすると作ってくれる。

~~~sh
git remote add origin https://github.com/ogusu/sturdy-engine.git

# ssh公開鍵認証の場合
git remote add origin git@github.com:ogusu/study-engine.git

git push -u origin master
    Username for 'https://github.com': xxx@gmail.com
    Password for 'https://xxx@gmail.com@github.com':
    Counting objects: 3, done.
    Writing objects: 100% (3/3), 244 bytes | 0 bytes/s, done.
    Total 3 (delta 0), reused 0 (delta 0)
    To https://github.com/ogusu/sturdy-engine.git
    * [new branch] master -> master
    Branch master set up to track remote branch master from origin.

# リポジトリを先に作っていて、さらにreadmeファイルを作ってコミットしている場合以下のエラーになる。
fatal: refusing to merge unrelated histories
その場合は以下で解消できる
git merge --allow-unrelated-histories origin/main
~~~

最後の修正を見てみる。最後のcommitはHEADと呼ばれ、以下のように表示したり、diffで対象にしたりできる。
~~~git
git show HEAD
    commit ce0afbfe8ea9390a62604d4d7eca462ed9a2291f
    Author: xxx <xxx@mba.local>
            ・・・
~~~

## 変更の反映

さらに変更してコミット、pushしてみる。  
修正はコミットだけではなく、インデックスに追加してから、コミットする必要がある。

~~~git
git add .
git commit -m "next commit"
~~~

インデックスへの追加は複数回に分けて行える。
コミットはインデックスに追加された修正をまとめて行える。
ある固まった修正を一つのコミットにしたい場合に便利な仕組み。

インデックスへの追加とコミットを同時にすることもできる。
~~~git
git commit -a aaa.html
~~~

## 差分の確認（diff）

git diff 比較元 比較先　とすることで比較が可能。

### 作業ツリーの差分

「インデックス」 → 「作業ツリー」 の差分を見る。  
比較元を指定しない場合は、デフォルトで、「インデックス」を比較元とし、 「インデックス」 → 「作業ツリー」 と比較した際の差分を表示する。  
インデックスに対して作業ツリーで行った変更を見ますので、まだgit addしていない変更内容は、このコマンドで簡単にチェックできます。
~~~git
git diff																							
diff --git a/README.md b/README.md																						
index 75382af..77c0149 100644																						
--- a/README.md																						
+++ b/README.md																						
@@ -1,3 +1,4 @@																						
# sturdy-engine																						
# ToDo																						
- Create Branch																						
+ - Show Diff																						
~~~

### コミットとの差分

「最新コミット」(HEAD)→ 「作業ツリー」 の差分を見る。
~~~git
git diff HEAD
~~~
「指定したコミット」→「作業ツリー」の差分を見る。
~~~git
git diff <commit>
~~~
「HEAD」→「インデックス」の差分を見る。
~~~git
git diff --cached
git diff --cached HEAD
~~~

|比較|コマンド|
|:---|:---|
|インデックス → 作業ツリー|git diff	|
|HEAD → 作業ツリーの比較|git diff HEAD	|
|HEAD → インデックス|git diff --cached|

## その他の差分確認

差分が生じたファイルの、ファイル名の一覧を表示します。比較元（引数）は自由に指定できます。
~~~git
git diff --name-only
~~~

比較するファイルを限定する。「パス」（ディレクトリ名など）も指定可能。
~~~git
git diff <コミット名> <コミット名> ―― <ファイル名>
git diff <コミット名>:<ファイル名> <コミット名>:<ファイル名>
~~~
特定のコミット間を比較する。
~~~git
git diff 比較元のコミット 比較先のコミット
~~~

## 直前のコミットの内容を表示
HEADの一個前 から　HEADへの変化を表示。
~~~git
git diff HEAD^ HEAD
~~~

HEADのコミット内容を表示
~~~git
git diff show
git diff show HEAD
~~~

## リモートブランチとローカルブランチの差分を比較

リモート側で修正が入っているとpushが失敗し、pullしろと言われるが、
git pull すると確認もなしにいきなりローカルリポジトリにマージされてしまう。
「git fetch & git merge」の順に行ったほうがいい。

以下のようにすると、リモートリポジトリの現在の状態との差分を見ながらマージができる。
~~~git
# origin から master ブランチの履歴と参照を取得
git fetch origin master																							
# ローカルとリモートの差分を確認
git diff master origin/master
# 問題なさそうならマージ
git merge?
~~~

## コミットの一覧
~~~git
git log
git log --decorate --graph --oneline
git log --all --decorate --graph --oneline
~~~

### 認証

$ ssh-keygen -t rsa -C GitHubに登録したメールアドレス
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/xxx/.ssh/id_rsa): /Users/xxx/.ssh/github_rsa

$ ssh-add -K ~/.ssh/github
$ ssh-add -l
公開鍵をgithubに登録し直す
$ cat ~/.ssh/github
GitHubの設定、SSHキーに登録

configファイルを作成する
$ vim ~/.ssh/config   
~/.ssh/config

を実行して、以下のコードを貼り付ける

Host github.com
  HostName github.com
  IdentityFile ~/.ssh/id_rsa
  User git

$ ssh -T git@github.com
Hi ogusu! You've successfully authenticated, but GitHub does not provide shell access.

通常はssh公開鍵・認証鍵方式で接続するが、httpsのBASIC認証でも接続可能。
~~~git
git remote set-url origin https://ユーザー名:パスワード@github.com/repogitory-name.git
~~~
ローカルに `.netrc` ファイルを配置しても可能だが推奨されない。
~~~git
machine github.com
login ユーザー名
password パスワード
~~~

### git addの取り消し
resetコマンドで取り消せる。rmでもいいが--cachedを忘れるとファイル自体が消えてしまうのでresetが安全。
~~~
git reset HEAD [ファイルパス]
git reset HEAD
git reset
git rm --cached [ファイル]
~~~

### 変更点の退避
commit, addしてない状態でブランチを切り替えたい時がある。その場合はstashすると良い。saveは省略できる。
~~~
git stash save
git stash
~~~
戻す時。いまどんな変更を退避しているかを確認
~~~
# 2つの変更を保存している状態（2回stashを行った状態）
git stash list
 stash@{0}: WIP on sub: a0d2f1b add fourth line
 stash@{1}: WIP on sub: 1a61919 add second line
 <stash名>: WIP on <stashを行ったブランチ名>: <ハッシュ> <コミットコメント>

# さらに細かい情報が見たい場合。
git stash list -p
git stash show <stash名>
~~~
### stashの復元
~~~
# 復活させたいstash名で取り出す
git stash apply stash@{0}

# 復活したらstashを削除
git stash drop <消したいstash名>

# 復元と削除を同時に
git stash pop stash@{0}

# 復元の取り消し
git stash show <適用したstash名> -p | git apply -R
~~~

### ブランチ
ブランチを作成する時
~~~
# developブランチへ移動
git checkout develop

# ブランチを作成して移動。-bオプションは作成と移動
git checkout -b feature/11


# 現在のブランチはの確認
git branch
~~~

ブランチの削除
~~~
# 消したいブランチとは異なるブランチに移動し、ブランチを消す。
git checkout develop
git checkout -d feature-creaxxx
~~~

ブランチをリモートへpush
~~~
git add .
git commit -am "commit"
git push origin feature/11
~~~

作業ブランチを親ブランチにマージ
~~~
# feature/11 を developにマージ

# まずマージ先に移動。必要ならpullしておくこと。
git checkout develop

# マージしたいブランチを指定して、マージ。
git merge --no-ff feature/11


--no-ffオプション：fast-forwardの関係であっても、必ずマージコミットを作る。機能追加が見やすい。
mergeする時、ローカルのdevelopブランチを最新にすること。
また、feature/11のマージの前にdevelopにすでに修正が入っていると失敗する。feature/11にpullして動作確認しておく。
~~~

図で分かるgit-mergeの--ff, --no-ff, --squashの違い
http://d.hatena.ne.jp/sinsoku/20111025/1319497900

他の人とpushが交互になる場合、以下のようにすればブランチが分かれず1本につながる。
~~~
git fetch origin
git rebase origin/master
~~~

git rebase 失敗した時の対処法
http://qiita.com/shuntaro_tamura/items/c505b76c1021a35ca9ff

### ログ
ログのツリーを見やすくする
~~~
# ~/.gitconfigに以下を追加。git graphで表示。
[alias]
graph = log --graph --date-order -C -M --pretty=format:¥""<%h> %ad [%an] %Cgreen%d%Creset %s¥"" --date=format:¥""%m/%d %R¥""c"
~~~
ログの確認
~~~
# リモートの状況の把握
git log origin/develop

# ローカルのチェックアウトの状況の把握
git remote show origin
~~~

### ブランチ
リモートではすでに削除されたが、ローカルに残っているブランチを削除するには以下の通り。
~~~
# 削除対象の確認
git remote prune origin --dry-run
# 削除実行
git remote prune origin
~~~

## GitHub Actions

GitHubからサーバーへSSHが通るように、公開鍵と秘密鍵を用意しておく。公開鍵をデプロイ先サーバーのユーザーのaurhorized_keysに入れておく。

### Secretsの設定

リポジトリのSettings->Secretsを開く。以下のSSHとデプロイ先の設定を入れる。
~~~
DEPLOY_HOST_SERVER　SSH接続先
DEPLOY_PORT_SERVER　SSH接続時のポート
DEPLOY_USER_SERVER　SSH接続するユーザー
DEPLOY_KEY_SERVER　上記で入れた公開鍵のペアの秘密鍵
DEPLOY_HOST_PATH　デプロイ先のパス。cd でこのパスに移動してgit pullする
~~~

Actionsを開く。NewWorkFlow、もしくはSetup up this workflowを選ぶ。シンプルなものを選んだら、そのままコミットする。ローカルにpullして、.git_ignore/workflow/以下に保存されたxxx.ymlを開く。以下の内容に書き換える。

masterブランチにpushされたら、sshで接続してサーバ上の所定フォルダでgit pullする。

~~~
name: CI

# Controls when the workflow will run
on:
  # Triggers the workflow on push request events but only for the master branch
  push:
    branches: [ master ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      - name: Deploy
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.DEPLOY_HOST_SERVER }}
          username: ${{ secrets.DEPLOY_USER_SERVER }}
          port: ${{ secrets.DEPLOY_PORT_SERVER }}
          key: ${{ secrets.DEPLOY_KEY_SERVER }}
          script: |
            cd ${{ secrets.DEPLOY_HOST_PATH }}
            git pull origin master
~~~
