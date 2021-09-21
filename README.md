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