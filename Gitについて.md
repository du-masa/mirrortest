# Gitについて
## Gitとは？

バージョン管理システム１つです。
### バージョン管理システムの必要性
バージョン管理システムを使用しない場合を考えてみます。
ファイル管理は、バックアップフォルダなどを作り、ファイル名に日付を付与して行うことが多いと思います。その場合、最初はファイル数も少なく良いかもしれませんが、バックアップファイルが増えていくと特定のファイルがどれだかわからなくなるなど、管理体制が崩壊します。
また、同一ファイルを同時に編集してしまい、どちらかの変更を上書きしてしまうといったことも起こりかねません。
Gitはそう言った問題を解決するために在ります。

### Gitの特徴
・誰が、いつ、どのファイルを、どのように変更したかの履歴を残すことができる。
・同時編集を行った際に、差分を計算し賢くマージしてくれる。
・履歴の内容を変更したり、削除したりを柔軟に行える。
・ホスティングサービス(GitHub, BitBucketなど)と連携してファイルを共有し、共同開発がしやすくなる。

## Gitのインストール

### mac
[Homebrew](http://brew.sh/index_ja.html)から落とすのが簡単です。

#### Homebrewのインストール(既にインストールされている人は不要)
ターミナルを開いて、下記を実行します。

```bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

$ brew -v
```

`brew -v`を実行してバージョンが表示されればインストールされています。

#### Gitインストール
```bash
$ brew install git
$ git -version
```
バージョンが表示されればgitのインストール完了です。

### windows

windowsインストール方法を書いてもらう。

### GitHubにSSH接続できるように設定

新FWでは、ホスティングサービスとしてGitHubを使用します。
そのためSSHキーの公開鍵をGitHubに登録する必要があります。
こちらを実施してください。
[GitHubにSSH接続できるようにする方法](http://qiita.com/shizuma/items/2b2f873a0034839e47ce)

## Gitの基本概念

### リポジトリ
ファイルやディレクトリの状態を記録する場所です。
ファイルの変更を行い、履歴を保存するとリポジトリに状態が記録されます。

リポジトリには２種類あります。
	- ローカルリポジトリ: ユーザ一人ひとりが利用するために、自分の手元のマシン上に配置するリポジトリです。
	- リモートリポジトリ: ポスティングサービス上に配置して、複数人で共有するためのリポジトリです。(GitHubのリポジトリがこれに当たります。)

リポジトリを2種類に分けることで、各々がローカルリポジトリで作業したり、他の人が編集した内容をリモートリポジトリを通じて取得することができます。

参考:[サルでもわかるGit入門/履歴を管理するリポジトリ](http://www.backlog.jp/git-guide/intro/intro1_2.html)

### コミット
リポジトリに追加・変更を記録するためにはコミットという操作を行います。
コミットすると、日付、ユーザ名、ランダムなIDなどが記録されます。また任意のコメントを残すことができます(コメントを残さないとコミットできません。)。これらの情報を使って、どういった変更だったからの履歴を遡るのに使用します。

参考:[サルでもわかるGit入門/変更を記録するコミット](http://www.backlog.jp/git-guide/intro/intro1_3.html)

### ワークツリー(作業ディレクトリ)
実際に作業しているディレクトリのことを言います。ファイルの追加・変更などを行うと、まずワークツリーに記録されます。

### インデックス(ステージングエリア)
ワークツリーで作業したファイルをコミットする前に、一度登録するエリアです。このインデックスを挟むことで余計なファイルをコミットしたり、逆に特定ファイルのコミット忘れをといったことを起こしにくくします。
コミット前にワンクッション置くといった感じです。コミットするにはインデックスにファイルを登録してからでなければできません。

### ワークツリー、インデックス、リポジトリの関係
実際に作業をするときはワークツリー、インデックス、リポジトリの順にファイルを移動していきます。

![ワークツリー、インデックス、リポジトリ](http://www.backlog.jp/git-guide/img/post/intro/capture_intro1_4_1.png)

こちらがGitの基本の流れになります。

参考:[サルでもわかるGit入門/ワークツリーとインデックス](http://www.backlog.jp/git-guide/intro/intro1_4.html)

## Git基本操作

### リポジトリを用意する
Gitによるバージョン管理を行うには、リポジトリを用意する必要があります。

リポジトリを用意するには２つ方法があります。
	- 新規作成
	- 既存のリモートリポジトリからクローン

新規作成はリモートリポジトリでも簡単にできるので、実際はクローンで用意することが多いと思います。そのため新規作成に関しては説明を省略いたします。興味にある方はこちらを参考にしてください。[サルでもわかるGit入門/新しいリポジトリを作成する](http://www.backlog.jp/git-guide/intro/intro2_3.html)

#### リポジトリのクローン

既存のリモートリポジトリを自分のローカルにクローンします。
任意のディレクトリで下記を実行します。

```bash
$ git clone <URL>
```

`<URL>`は各リモートリポジトリから取得できます。
GitHubリポジトリの右上に「Clone or download」ボタンがあるのでそちらからURLをコピーできます。

### ファイルの変更からコミットまで
#### ファイルの状態を確認する

実際にファイルを変更した時に、どのファイルが変更されたか確認したい場合は下記を実行します。

```bash
$ git status

 On branch master
 Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   sample.html
```

例えば、`sample.html` と言うファイルを変更した場合は上記のように表示されます。
`git status`では最新のコミットから変更があったものを一覧にして表示します。

#### ファイルをインデックスに登録する

変更したファイルをインデックス(ステージングエリア)に登録します。

```bash
$ git add <file>..
```

`<file>`にはファイル名やディレクトリ名を指定します。スペース区切りで、複数ファイル・ディレクトリを指定できます。
`sample.txt`をインデックスに登録する場合は下記になります。

```bash
$ git add sample.html
```

また、変更したすべてのファイルをインデックスに登録するには下記のようにします。

```bash
$ git add .
```

インデックスに登録した状態で、`git status`をしてみましょう。

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   sample.html
```

`Changes not staged for commit:`が`Changes to be committed:`に変わります。
`git status`ではインデックスに登録されているファイルも表示してくれます。

#### ファイルをコミットしてリポジトリに保存する

インデックスにファイルを登録して問題なければ、コミットします。コミットすることでリポジトリに保存され、履歴が残ります。

インデックスに登録されているファイルをコミットします。

```bash
$ git commit -m "任意のコメント"
```

`-m`をつけて任意のコメントを残します。 **コメントがないとコミットできません。**
また、`-m`をつけないとエディタが立ち上がり、そちらでコメントを入力します。その場合、複数行のコメントを残せます。

#### コミットされたかの確認

ちゃんとコミットされたかどうか確認する時は下記を実行します。

```bash
$ git log

commit 588e10c2be0c9887b077af95fb10e26497faa40c
Author: zap-mueda <m_ueda@zappallas.com>
Date:   Tue Jul 5 15:38:01 2016 +0900

    任意のコメント
 
```

コメント以外は各コミットによって違うかと思いますが、`git log`でコミットした内容が表示されていれば問題ありません。

ついでに`git status`をしてみましょう。

```bash
$ git status

On branch master
nothing to commit, working directory clean
 
```

`sample.html`が表示から消えました。無事リポジトリに保存されたということです。
(他に変更したファイルがあり、コミットしていない場合は、そのファイルが表示されます。上記の`nothing to commit, working directory clean`は最新のコミットから変更したファイルが何もない場合に表示されます。)

参考: [サルでもわかるGit入門/ファイルをコミットする](http://www.backlog.jp/git-guide/intro/intro2_4.html)

### リポジトリの共有
#### リモートリポジトリにプッシュ
ローカルリポジトリでファイル変更をコミットしたら、リモートリポジトリにアップして、他のメンバーにも共有します。
リモートリポジトリにアップすることをプッシュと言います。

```bash
$ git push <repository> <refspec>

```

`<repository>`にはリモートリポジトリのアドレスを指定します。
ただし、既存のリモートリポジトリからクローンしてきた場合は、そのリポジトリへのアドレスを`origin`と指定することができます。
`<refspec>`にはブランチ名を指定します。ブランチについては後述します。今回はデフォルトの`master`ブランチにプッシュします。

```bash
$ git push origin master

```

これでリモートリポジトリの`master`ブランチにプッシュされました。
参考: [サルでもわかるGit入門/リモートリポジトリにプッシュする](http://www.backlog.jp/git-guide/intro/intro4_2.html)

#### リモートリポジトリから最新ファイルをプル
他のメンバーがローカルでファイル変更を行って、リモートリポジトリにプッシュした時、自分のローカルリポジトリにもその変更を取り込む必要があります。その場合はリモートリポジトリからダウンロードとマージすることをプルと言います。

```bash
$ git pull <repository> <refspec>

```

`<repository>`と`<refspec>`はプッシュの時と同じです。
`master`ブランチからプルする場合は下記になります。

```bash
$ git pull origin master

```

これで他のメンバーが変更した内容が自分のローカルリポジトリにも取り込まれました。`git log`を実行すると他のメンバーの履歴が表示されます。
参考: [サルでもわかるGit入門/リモートリポジトリからプルする](http://www.backlog.jp/git-guide/intro/intro4_5.html)

#### コンフリクトの解消
gitはマージする際に自動的に変更箇所を統合してくれます。ただし、自動で統合できない場合もあります。
リモートリポジトリとローカルリポジトリで同一ファイルの同一箇所を同時に変更した場合、コンフリクト(競合)してしまいます。プルした際に競合した場合はgitがコンフリクトしていることを教えてくれるので、修正する必要があります。

```bash
$ git pull origin master

remote: Counting objects: 5, done.
・
・
・
Auto-merging sample.html
CONFLICT (content): Merge conflict in sample.html
Automatic merge failed; fix conflicts and then commit the result.

```

上記のように`CONFLICT`と表示された場合、コンフリクトしています。

実際にファイルを開いてみると、下記のようになっています。

```html
<body>
<<<<<<<HEAD
	こんにちは
=======
	<strong>Hello</strong>
>>>>>>> 588e10c2be0c9887b077af95fb10e26497faa40c
</body>
```

`<<<<<<<HEAD`から `>>>>>>> 588e10c2be0c9887b077af95fb10e26497faa40c`の間がコンフリクトを起こしている箇所です。`=======`より上がローカルリポジトリの内容、下がリモートリポジトリの内容です。この場合、手動で修正します。

```html
<body>
	<strong>Hello</strong>
</body>
```

今回は上記のように修正しました。修正が終了したら、コミットします。
これで、コンフリクトが解消されます。

参考: [サルでもわかるGit入門/競合を解決する](http://www.backlog.jp/git-guide/intro/intro6_2.html)

## ブランチとプルリクエスト
複数人で同時に作業をする際に便利となるのがブランチです。

### ブランチとは
ブランチを作ることで、本来の履歴の流れから分岐して、別の履歴を残すことができます。ブランチを利用することで他のメンバーの作業に影響を与えたり受けたりすることなく自分の作業をできます。また、履歴の残り方もわかりやくすなり、問題が発生した際など原因調査や対策が容易になります。

ブランチはリポジトリ作成時にデフォルトで１つ作成されます。`master`ブランチです。この`master`ブランチをベースに様々なブランチを作り、最終的には`master`ブランチにマージして完成させていきます。

参考: [サルでもわかるGit入門/ブランチとは](http://www.backlog.jp/git-guide/stepup/stepup1_1.html)

### ブランチを使った操作
#### ブランチの作成
gitはデフォルトで、`master`ブランチを作成します。特に他のブランチを作らない限り、`master`ブランチで作業することになります。
違うブランチで作業するには、ブランチを作成し、作成したブランチに移動する必要があります。

```bash
$ git branch <branchname>
$ git checkout <branchname>

# または
$ git checkout -b <branchname>
```

`<branchname>`には任意のブランチ名が入ります。`git branch`でブランチを作成、`git checkout`で指定したブランチに移動(「チェックアウトする」と言います)できます。また、`git checkout -b`でブランチの作成とチェックアウトを同時に行えます。

`branch1`というブランチを作ってチェックアウトするには下記のように行います。

```bash
$ git checkout -b branch1
```

#### ブランチの確認
現在どんなブランチがあるのか、また自分が今どのブランチにチェックアウトしているのかを確認したい場合は下記のようにします。

```bash
$ git branch

* branch1
  master
```

`git branch`を打つとブランチ一覧が見れます。またアスタリスクが付いているブランチに現在チェックアウトしていることになります。
上記であれば、`branch1`にチェックアウトしている状態です。

#### ブランチでの作業
各ブランチでコミットをするとブランチ固有の履歴が残ります。

例えば下記のようにコミットしたとします。

```bash
$ git commit -m "branch1でのコミット"
```

ログを見ると、履歴が増えています。

```bash
$ git log

commit 0dd453d5a31e37797ab23732f7155dca75c00a1c
Author: zap-mueda <m_ueda@zappallas.com>
Date:   Tue Jul 6 16:20:01 2016 +0900

    branch1でのコミット

commit 588e10c2be0c9887b077af95fb10e26497faa40c
Author: zap-mueda <m_ueda@zappallas.com>
Date:   Tue Jul 5 15:38:01 2016 +0900

    任意のコメント
 
```

しかし、この履歴は他のブランチには影響していません。
`master`ブランチにチェックアウトしてログを見ると`branch1`ブランチでの履歴は残っていません。

```bash
$ git checkout master
$ git log

commit 588e10c2be0c9887b077af95fb10e26497faa40c
Author: zap-mueda <m_ueda@zappallas.com>
Date:   Tue Jul 5 15:38:01 2016 +0900

    任意のコメント
 
```

このようにブランチを作ってそのブランチで作業することで、他のブランチでの作業に影響を与えたり受けたりしなくなります。

#### masterブランチへのマージ
各ブランチで行ったコミットは、最終的に`master`ブランチにマージします。マージはローカルとリモートの両方で行えます。新FWでマージは基本的にリモートリポジトリで行います。ローカルでのマージ方法はこちらでご確認ください。[サルでもわかるGit入門/ブランチをマージする](http://www.backlog.jp/git-guide/stepup/stepup2_4.html)


リモートリポジトリでのマージは、GitHubのプルリクエストという機能を使って行います。

##### プリリクエストとは？
単なるマージではなく、他のメンバーに対して、
「作業が終わってマージしようと思うのですが、問題ないかレビューしてください」と言った通知の役割を果たします。そのレビューを持ってマージするか、修正するかなどのコミュニケーションをとる場となります。
詳しくは:[サルでもわかるGit入門/プルリクエストとは？](http://www.backlog.jp/git-guide/pull-request/pull-request1_1.html)

##### プリリクエストを使う
まずは、リモートリポジトリにプッシュします。

```bash
$ git push origin branch1
 
```

リモートリポジトリにアクセスすると下記のように表示されます。
右側の「Compare & pull request」ボタンを押すと、プルリクエストを作成画面に遷移します。
![pullrequest.png](https://qiita-image-store.s3.amazonaws.com/0/83765/ddffd3db-47d4-b16a-f6d8-a2011eab8206.png "スクリーンショット 2016-07-07 11.49.15.png")


下記の画面で、プルリクエストのタイトルとコメント(どういった変更を行ったかなど)を記入して、「Create pull request」を押します。
![スクリーンショット 2016-07-07 12.11.23.png](https://qiita-image-store.s3.amazonaws.com/0/83765/64f32e88-74bc-e692-60c6-9370fc54f9da.png "スクリーンショット 2016-07-07 12.11.23.png")

最後にこちらの画面に遷移します。この画面では、他のメンバーによるレビューを行ったり、GitHubが自動で問題なくマージできるか(コンフリクトが起きてないか)のチェック、CIによるテストなどが行われます。
問題なければ、「Merge pull request」ボタンを押して`master`ブランチへマージします。

![スクリーンショット 2016-07-07 12.15.42.png](https://qiita-image-store.s3.amazonaws.com/0/83765/966e8ba5-6998-b18b-e519-e8af7d5c5446.png "スクリーンショット 2016-07-07 12.15.42.png")

#### ブランチの削除
マージが完了したら、今まで使用していたブランチは不要になるので削除を行います。

```bash
$ git branch -D <branchname> 
```

`branch1`ブランチを削除して、ブランチ一覧を見ると消えているのがわかります。※ブランチを削除する前に対象となるブランチ以外にチェックアウトしてください。

```bash
# masterブランチにチェックアウト
$ git checkout master
# branch1ブランチを削除
$ git branch -D branch1
# ブランチ一覧
$ git branch 
* master
```

`branch1`ブランチが一覧からなくなります。

リモートリポジトリからもブランチを削除する必要があります。
下記画像の「branches」を選択します。
![スクリーンショット 2016-07-07 12.36.07.png](https://qiita-image-store.s3.amazonaws.com/0/83765/eafec9e6-4c18-f1b8-d514-e2ce7b33300c.png "スクリーンショット 2016-07-07 12.36.07.png")

ブランチ一覧のページに遷移するので、対象のブランチの右側にあるゴミ箱マークを押すとブランチが削除されます。
![スクリーンショット 2016-07-07 12.37.19.png](https://qiita-image-store.s3.amazonaws.com/0/83765/86d7847e-a82a-f8c4-f395-04dd8d7f918c.png "スクリーンショット 2016-07-07 12.37.19.png")


## 開発の流れ
新FWでのGitを使った開発の流れは、Github-flowと呼ばれるフローに準ずるかたちで行います。
参考:[Github-flowを分かりやすく図解してみた](http://b.pyar.bz/blog/2014/01/22/github-flow/)

### Github-flow
簡単に説明すると、`master`ブランチは最新でいつでもデプロイできる状態を保ちます。各作業は必ずブランチを作りそちらで行います(`master`ブランチで直接作業しない。)。各作業が完了したら、プッシュしてプルリクエストを行います。その繰り返しで作業を行っていきます。

### 基本的な流れ
基本的な流れは次のようになります。
1. `master`ブランチから最新のファイルをプル(クローン)。
2. ブランチを作成、チェックアウト。
3. 実装作業を行う。
4. 作業が一通り完了したら、コミットとプシュ。
5. リモートリポジトリでプルリクエスト。
6. レビューやCIテストを行う。
7. マージ or 修正(この場合は3に戻る)
8. ブランチの削除
9. `master`ブランチにチェックアウトし1を実行

### ブランチ名
できるだけどのような作業を行うかわかるようなブランチ名にすることをお勧めします。
また、目的別に接頭辞を付けること推奨します。`接頭辞/ユニークなブランチ名`という形で名前をつけます。

- feature

新規機能開発や機能追加の時に使用します。一番使います。
例:日運機能実装であれば`feature/dayily-result`ブランチ

- hotfix

バグフィックスの時に使用します。
例:日運の結果が出ないバグであれば`hotfix/dayily-resultText`ブランチ

- release

リリース用に使用します。(あんま使ってないのでfeatureでも可)
例:テクニカルリリース用であれば`release/technical`

## こんな時どうする？？
gitを使って開発する際によく起こる事例と対応策をいくつかご紹介します。

### 直前のコミットを書き換えたい！

直前に行ったコミットにファイル漏れがあった場合など、やり直したい時に有効です。

```bash
# 漏れたファイルなどをインデックスに登録
$ git add sample2.html

# 直前のコミットを書き換え
$ git commit --amend

```

`git commit --amend`でエディタが立ち上がり、直前のコミットのコメントが表示されます。コメント内容を編集して(そのままでも可)保存すると、新しくコミットされるのでなく、直前のコミットとして書き換えられます。

また、addせず行うとコミットコメントの変更のみの行えます。

### 作業中にリモートのmasterブランチが進行してしまった
プルリクエストを行った際に、他の誰かが先に`master`ブランチにマージしている場合があります。その変更をローカルリポジトにマージしないままでいると、プルリクエストを画面で下記のような表示になります。

![スクリーンショット 2016-07-07 15.05.33.png](https://qiita-image-store.s3.amazonaws.com/0/83765/90db2800-aa73-447a-eace-186948a2e87e.png "スクリーンショット 2016-07-07 15.05.33.png")

ここで「Update branch」ボタンを押すことで最新の`master`ブランチをマージできるのですが、コミットが複雑になります。それを避けるために下記のような手順で修正します。

```bash
# masterブランチにチェックアウト
$ git checkout master
# 最新のmasterブランチをマージ
$ git pull origin master
# 該当のブランチにチェックアウト
$ git checkout <branchname>
# masterブランチをrebase
$ git rebase master
# リモートリポジトリにpush -fオプションをつけないとエラーが出るのでつける
$ git push -f origin <branchname>
```
参考:[反則技 git push -f](http://qiita.com/ppworks/items/94c0107d98e55f903ea9)
これで最新の`master`ブランチを取り込んだ状態で、プルリクエストをすることができます。

#### rebaseについて
`git merge`と似たようなものですが、`rebase`は過去の履歴を1本化して綺麗にします。ちょっと仕組みがややこしいのですが、`git rebase <branchname>`で`<branchname>`の履歴をいい感じにマージしてくれるという感覚で大丈夫かと思います。
詳しくは: [サルでもわかるGit入門/rebaseでマージする](http://www.backlog.jp/git-guide/stepup/stepup2_8.html)

#### コンフリクトが発生した場合
`master`ブランチとマージを行いますので、場合によってはコンフリクトを起こします。その場合は、コンフリクト修正後に下記のように行います。

```bash
# コンフリクト修正後、対象のfileをadd
$ git add <file>
# rebaseの続きをする。commitは不要
$ git rebase --continue

```
`rebase`や`push`のところは同じです。これでコンフリクトも解消されます。

#### 推奨手順
毎回毎回プルリクエストを投げてから、上記作業を行うのも２度手間なので、`push`前(もしくは定期的)に`master`ブランチを`pull`して各ブランチに`rebase`しておくことをお勧めします。そうすることで`push`作業を1回で済みます。

### コミットの順番入れ替え・統合・分離・削除・編集をしたい
過去に行ったコミットをあれこれしたい時があります。
そんな時は、`git rebase -i`で行えます。

```bash
$ git rebase -i <commitId or HEAD>
```

どのコミットまでを対象とするか、コミットidか`HEAD`と呼ばれるもので示します。指定されたコミットより新しいコミットが全て対象となります。

最新のコミットと１つ前のコミットを修正したい場合の例です。

```bash
$ git log
commit 465f3869d95165e060ae17235efd4b04a663cef6
Author: zap-mueda <m_ueda@zappallas.com>
Date:   Tue Jul 6 16:20:01 2016 +0900

    branch2でのコミット

commit 0dd453d5a31e37797ab23732f7155dca75c00a1c
Author: zap-mueda <m_ueda@zappallas.com>
Date:   Tue Jul 6 16:20:01 2016 +0900

    branch1でのコミット

commit 588e10c2be0c9887b077af95fb10e26497faa40c
Author: zap-mueda <m_ueda@zappallas.com>
Date:   Tue Jul 5 15:38:01 2016 +0900

    任意のコメント

# 最新のコミットと１つ前のコミットを修正したい
$ git rebase -i 0dd453d5a31e37797ab23732f7155dca75c00a1c
```

エディタが開きます。

```vi
pick 465f386 branch2でのコミット
pick 0dd453d branch1でのコミット

# Rebase 465f386.. 0dd453d onto 465f386 (2 command(s))
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out

```

過去2回のコミットIDとメッセージが表示されます。

#### コミットの順番を入れ替える
エディタ上でコミットの順番を変更すると、logの順番が入れ替わります。

```vi
# コミットの順番を入れ替える
pick 0dd453d branch1でのコミット
pick 465f386 branch2でのコミット
```

`:wq`で保存すると適用されます。

#### コミットを削除する
エディタ上でコミットの記述を消すと、logから削除されます。

```vi
# IDが0dd453dのコミットをけす
pick 465f386 branch2でのコミット
```

#### コミットを編集・結合・分離させたい
エディタに出力される`pick`の部分を変更することで、様々な変更ができます。

コマンド一覧

|コマンド|内容|
|--------|--------|
|pick, p|何もしない|
|edit, e|分割など、コミット内容を再編集する|
|squash, s|コミットをまとめる(コメントを再設定できる)|
|fixed, f|コミットをまとめる(コメントを再設定できない)|
|exec, e|コミットとコミットの間にコマンドを追加できる|

例として、よく使うので`fixed`を紹介します。

```vi
# 465f386を0dd453dに結合したい
pick 0dd453d branch1でのコミット
fixed 465f386 branch2でのコミット
```

上記のように`pick`部分を`fixed`にすることで、その上にあるコミットと結合することができます。

参考: [gitのコミットの歴史を改変する(git rebase)](http://tkengo.github.io/blog/2013/05/16/git-rebase-reference/)

#### HEADについて
`HEAD`は今チェックアウトしているコミットを指しています。特に指定しない限り、`HEAD`はそのブランチの最新コミットを指します。つまり現在いる位置です。そこから`HEAD`を使って相対的に過去のコミットし指し示すことができます。

相対的な指定は下記の２つでできます。

- `^`オペレータを使って一回で一つの前のコミットを指定
- `~<num>`オペレータを使って任意の個数分前のコミットを指定

例えば、1つ前のコミットを指し示す場合は、`HEAD^`もしくは`HEAD~`と指定します。また`HEAD`のエイリアスとして`@`でもできます。この`HEAD`を使用して過去のコミットを参照したり、履歴を変更したりします。

##### HEADの必要性
特定のコミットを指定するのにはコミットIDでも可能です。ですがIDは長い上に、`git log`などで確認ないといけないので手間です。そのため`HEAD`という相対位置を指定するものが存在します。

### 編集途中のものを一旦どっかにやりたい
ローカル上で複数のブランチ使って開発している時に、ブランチの行き来をすることがあります。その際に編集中だけどまだコミットしたくないファイルを一時的に退避することができます。

```bash
# まだコミットしていないファイルを退避
$ git stash save

# stashの一覧表示
$ git stash list

# stashの復活
$ git stash pop <stash名>
```

`<stash名>`は`stash list`で確認できます。
参考:[変更を一時的に退避！キメろgit stash](http://qiita.com/fukajun/items/41288806e4733cb9c342)

### 過去の状態に戻したい(まだ誰にも共有していない)
自分のローカルリポジトリ上で過去の状態に戻したい場合に使います。

```bash
$ git reset <option> <commitId or HEAD>
```

`<option>`にはHEADの位置・インデックス・ワーキングツリーのどの段階まで戻すかを指定できます。

option一覧

| オプション |内容|
|--------|--------|
|--hard|「HEADの位置・インデックス・ワーキングツリー」全て        |
|--mixed もしくはオプション無し|「HEADの位置・インデックス」|
|--soft|「HEADの位置」のみ|

1つ前のコミットまで戻りたい時は下記のように行います。

```bash
# 1つ前のコミットまで戻る
$ git reset --hard HEAD~
```

#### 注意点
`git reset`は履歴自体をなかったことにしてしまうので、すでにリモートに共有されているコミットに対しては行ってはいけません。すでに公開された履歴をなくしたい場合は`git revert`を使います。

#### resetする前に戻したい
直前の`reset`をなかったことにするおまじないがあります。

```bash
# 直前のgit reset をなかったことにする
$ git reset --hard ORIG_HEAD
```

参考:[[git reset (--hard/--soft)]ワーキングツリー、インデックス、HEADを使いこなす方法](http://qiita.com/shuntaro_tamura/items/db1aef9cf9d78db50ffe)

### 過去の状態に戻したい(すでにみんなに共有してしまった)
`git reset`でも書いたように、すでに公開しているコミット(厳密には`master`ブランチにマージされたコミット)を`reset`してしまうとリモートとローカルの整合性が取れなくなります。
そのために`git revert`を使います。`revert`は過去のコミットをなかったことにするために、新たにコミットします。

```bash
# 直前のコミットをなかったことにする
$ git revert HEAD

```

参考:[サルでもわかるGit入門/revert](http://www.backlog.jp/git-guide/stepup/stepup7_2.html)

### 詳しいログが見たい
`git log`では`git reset`などのlogが確認できません。
そのため誤って`reset`した際などには、`git reflog`でより詳しいコミット履歴を見ることができます。

```bash
$ git reset --hard HEAD^^ # HEAD^と指定するつもりが間違えた!
$ git reflog
f5cb888 HEAD@{0}: head^^: updating HEAD
b0b8073 HEAD@{1}: merge @{-1}: Merge made by the 'recursive' strategy.
fe3972d HEAD@{2}: checkout: moving from fix/some-bug to master
$ git reset --hard HEAD@{1} # reset --hard HEAD^^する前に戻れる
```

ブランチを消してしまった際にも復活できます。

```bash
$ git reflog
#消してしまったブランチの最後のコミットを見つけたら
$ git branch ブランチ名 HEAD@{ログ番号}
```

参考: 
[いざという時のためのgit reflog](http://qiita.com/yaotti/items/e37c707938847aee671b)
[誤って必要だったブランチを消してしまい、涙目なあなたへ。](http://qiita.com/shimomai@github/items/50b62dd2b3d92170f1f3)

## config設定
git本体の設定ができます。


### configファイルの場所
設定はグローバルとリポジトリごとに設定ができます。
	- グローバル:`~/.gitconfig`
	- ローカルリポジトリ: `(リポジトリのルートディレクトリ)/.git/config`

参考:[git config --globalのファイルの場所](http://dackdive.hateblo.jp/entry/2015/07/28/185613)

### 設定
今回はグローバル設定です。
※必ずやっておいてほしい設定を記載いたします。

```bash
# 名前の設定
$ git config --global user.name "Githubのユーザーネーム"
# メールアドレスの設定
$ git config --global user.email "Githubに登録しているメールアドレス"
# 改行コードを変換しない
$ git config --global core.autocrlf false
# 日本語ファイルを扱う
$ git config --global core.precomposeunicode true
# 常に--no-ffでmergeする。(常にfast-forwardしない)
$ git config --global merge.ff false
# 常にgit pull --rebaseにする。(git pullでmergeコミットさせない)
$ git config --global pull.rebase true

# configの内容を確認する
$ git config --global --list
```
configではコマンドのエイリアスやビジュアルの設定などもできますので、お好みでカスタマイズしてみてください。

## 参考

### 文献

- [サルでもわかるGit入門](http://www.backlog.jp/git-guide/)
- [Git Advent Calendar 2015](http://qiita.com/advent-calendar/2015/git)
- [Git初心者に捧ぐ！Gitの「これなんで？」を解説します。](http://kray.jp/blog/git-why-explanation/)
- [Gitコマンドまとめ100選！あなたはいくつ使える！？](http://tech.pjin.jp/blog/2015/11/24/git-command-matome-list-100/)

### シミュレーター
- [learnGitBranching日本語版](http://k.swd.cc/learnGitBranching-ja/)

