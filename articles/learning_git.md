---
title: "Gitを完全に理解する"
emoji: "🐥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [github, git]
published: false
---
# Gitを完全に理解する
この記事では以下の内容をまとめておこうと思います
- gitそのものについて，
- gitのコマンド
---
## gitについて
### 1. gitはスナップショット
- 最初は全てのファイルを保存する.
- 変更が行われると，変更したファイルは差分ではなく，ファイル丸ごと保存する.
- 変更されてないものは，前回保存したものを使い回す.
->ブランチを導入するときにスナップショットだと都合がいい.差分計算だと複雑になっちゃう．歴史的にはもともと差分で開発されていたが，ブランチを切るのに時間がかかっていた．そこでリーナスさんがその問題を解決してgitを開発した．
---
### 2. バージョンをコミット(スナップショットの記録)で管理してる
- コミットnは直前のコミットn-1を記録して繋がってる
- コミットを辿っていけば古いバージョンまで遡れる
---
### 3. 言葉の定義
- リモート:オンラインのgithubとか
- コミット:スナップショットの記録，記録すること
- リモートリポジトリ:オンライン上の履歴データのデータベース(保管場所)みたいなもん
- ローカル:自分のパソコン．以下はローカルの
- ローカルリポジトリ:自分の手元の履歴データのデータベース
- ステージ:スナップショットを記録するための準備の場所．変更確定させたいファイルの変更情報だけを記録するため
- ワークツリー:自分が作業している場所
---
### 4. 流れ
(個人)
- ローカルのワークツリーで作業
- 変更があったら記録したいスナップショットをステージへ
- ステージからローカルリポジトリに記録(コミット)
- github(リモートリポジトリ)にアップロード
(チーム)
- github(リモートリポジトリ)から他の人の履歴をローカルリポジトリに取得
- 変更を自分のワークツリーに反映
- 変更したら(個人)の流れでアップロード
---
### 5. 中で起こっていること
- ユーザがステージング
- 先にリポジトリに圧縮ファイルが作られる
- ステージにはその圧縮ファイル名(ハッシュID)とファイル実態の組み合わせがインデックスとして保存される
- ユーザがコミットするとリポジトリにツリーが作らる
- その際ツリー情報や作成者，日付，メッセージなどをまとめたコミット情報を持ったファイルも作成される
->ステージにはインデックス情報が保存される
　リポジトリには圧縮ファイル，ツリー，コミット情報のファイルが保存される．実態は.git/objectsにある

- 追加の変更が起こったときは同様に圧縮ファイルが作られ，インデックスに構成が追加される
- リポジトリに新しい変更が加えられたツリーが作成される
- この時のコミット情報にはその前のコミットが親コミットとして保存される(変更履歴が辿れる)
---

## 初期設定,確認
バージョン確認．何も出ない時は公式からダウンロード
```terminal.
$ git version
```
初期設定(ユーザーネーム，email,エディターの登録，確認)
```terminal.
$ git config --global user.name [githubのユーザーネーム]
$ git config --global user.email [githubに登録したemail]
# VSCodeは [code --wait]
$ git config --global core.editor [使用するエディター名]
```
確認
```terminal.
$ git config user.name
$ git config user.email
$ git config core.editor
# 全部見たい時はこれ
$ git config --list
```
configファイルの実態
```terminal
$ cat ~/.gitconfig
```
---

## 実際にgitを触る
### リポジトリの作成
(個人)まずはリポジトリの作成．.gitディレクトリが作成される．この中にgit関係のファイルが作成されてく
```
$ git init
```
(チーム)リモートリポジトリから他の人が作成したプロジェクトのコピーを作成する．これをやると編集対象のファイルそのものとチームの.gitディレクトリがコピーされる
```
$ git clone [リポジトリ名]
```
### ステージに変更を追加
ワークツリーからステージに変更を追加.以下のコマンドで記録したいものだけ選択して追加できる
```
$ git add [ファイル名]
$ git add [ディレクトリ名]
# 全部追加
$ git add .
```
### ローカルリポジトリにコミット
変更を記録する．メッセージをエディター使わず書きたい時は-mオプション．ファイルの変更内容まで見たい時は-vオプション
```
$ git commit
$ git commit -m "[メッセージ]"
$ git commit -v
```
※コミットメッセージの書き方:
(個人)1行で変更内容と理由を書くのが望ましい
(チーム，正式)1行目:変更内容の要約，2行目:空行, 3行目変更した理由　

### 変更状況，変更差分，履歴の確認
どのファイルが変更されたのかを確認
- ワークツリーとステージ間
- ステージとリポジトリ間
```
# 変更状況
$ git status
# 変更差分
# git addする前の変更差分(ワークツリー，ステージ間)
$ git diff [ファイル名]
# git addした後の変更差分(ステージ，リポジトリ間)
$ git diff --staged
# 変更履歴(最新の変更から表示される)
$ git log
$ git log -oneline
$ git log -p [ファイル名]
$ git log -n [表示するコミット数]
```

### ファイルの削除，移動
ファイル，ディレクトリの削除，自分のワークツリーからも消える
```
$ git rm [ファイル名]
$ git rm -r [ディレクトリ名]
```
自分のワークツリーには残して，gitの記録からは削除したい
```
$ git rm --cached [ファイル名]
```
ファイルの移動　ファイル名の変更に近い
```
$ git mv [旧ファイル] [新ファイル]

# やっていることは以下のコマンド群と同じ
$ mv [旧ファイル] [新ファイル]
$ git rm [旧ファイル]
$ git add [新ファイル]
```


### github(リモートリポジトリ)を新規追加する
originを登録すれば毎回URLを入力しなくてもoriginに登録されているものを引っ張ってくるだけで良くなる
メインのリモートリポジトリ名は慣用的にorigin
```
git remote add origin https://github.com/[ユーザー名]/[リポジトリ名].git
```
一度登録した後は以下のコマンドでOK
```
$ git push [リモートリポジトリ名] [ブランチ名]
$ git push -u origin master
# このコマンドを打つと次回以降git pushだけで良くなる
$ git push origin master
```
初回はユーザー名とパーソナルアクセストークン(github側で作成)を聞かれる

### エイリアスによるコマンドの短縮
--globalでPCのホームディレクトリ配下全てがエイリアスの対象になるよ
```
$ git config --global alias.ci commit
$ git config --global alias.st status
$ git config --global alias.br branch
$ git config --global alias.co checkout
```

### 変更取り消し
ワークツリーの変更を消す->ワークツリーの状態をステージと同じ状態に戻してる
ブランチと見分けるために--を使う
```
$ git checkout -- [ファイル名]
$ git checkout -- [ディレクトリ名]
# 前変更取り消し
$ git checkout -- .
```
ステージに追加した変更を消す->リポジトリの状態(直前のコミット)で上書きして無かったことにしている
ちなみにHEADは最新のcommit(リポジトリへの保存)のこと
```
$ git reset HEAD [ファイル名]
$ git reset HEAD [ディレクトリ名]
# 全変更取り消し
$ git reset HEAD .
```
直前のコミットの変更を取り消す(コミットの修正，コミットメッセージの修正)
->直前のステージの内容を使って上書きすることで修正
これに関しては前の状態を持ってくるというより，ステージの内容を書き直して上から塗り替える感じ．再コミットみたいな
```
$ git commit --amend
#　複数のコミットのやり直し上から3つ(絶対にpushしてないやつ)
$ git rebase -i HEAD~3
```
※pushした内容は書き換えちゃダメ

### 設定しているリモートリポジトリの状態取得
```
$ git remote
# 対応するURLを表示
$ git remote -v
```

### リモートリポジトリの複数登録
チーム開発で自分のリポジトリを持っておきたいとき
```
$ git remote add [リモート名] [リモートURL]
# mainの内容を新しく作ったリモートリポジトリにもpush
$ git push -u [新しいリモート名] main
```

### リモートから情報を取得(やり方は2つfetchとpull)
fetch->自分のリポジトリにリモートリポジトリの内容を持ってくる
remote/リモート/ブランチ　に保存される(ローカルのリモート専用のところ)
```
$ git fetch [リモート名]
$ git fetch origin
# 自分のワークツリーの方に持ってくる
$ git marge
# ブランチの全部の情報を見る->今いるブランチ名と持ってきた変更のブランチ(remote/)が見れる
$ git branch -a
# 自分のワークツリーを切り替える
$ git checkout remotes/origin/main
```

pull->マージまでを一つのコマンドでできる
ワークツリーへの反映までを一つのコマンドで実行できる
```
$ git pull [リモート名] [ブランチ名]
$ git pull origin main
```
※git pullすると今自分がいるワークツリーにマージされちゃう　間違って統合される可能性があるので注意

リモートの情報を取得
```
$ git remote show [リモート名]
$ git remote show origin
```
### リモート名の変更，削除
```
# リモート名変更
$ git remote -rename [旧リモート名] [新リモート名]
# リモート削除
$ git remote rm [リモート名]
$ git remote rm tutorial
```

### ブランチ
- ブランチ:コミットを指し示したポインタ
- ヘッダ:今自分が作業しているブランチを指し示したポインタ
どちらもリポジトリ内に保存される
mainとmain2があったとして，これらがcommitを指してるとする
mainでコミットするとmainが指すのはcommit2になる
この状態でmain2でコミットするとmain2が指すのはcommit2'
->ブランチが指すコミットが分かれるので，開発が分岐できる

ブランチの追加
```
#ブランチを作成するだけでブランチの切り替えはしない
$ git branch [ブランチ名]
$ git branch feature
```
ブランチの表示
```
$ git branch
$ git branch -a
```
ブランチの切り替え
```
$ git checkout [既存ブランチ名]
$ git checkout feature
# 新しく作って切り替え
$ git checkout -b [新規ブランチ名]
# featureをgithubにプッシュ
$ git push origin feature
$ git checkout main
# mainをgithubにプッシュ
$ git push origin main

```
変更履歴のマージ．他のブランチの変更点を自分の作業中のブランチにマージする
```
$ git merge [ブランチ名]
$ git merge [リモート名]/[ブランチ名]
$ git merge origin/master
```
3種類のマージ
- Fast Forward:mainのブランチを新しく作ったブランチを参照するように変更
    枝分かれはせず参照するコミットが一個進むだけ
    バグをすぐに修正したい時とか
- Auto merge:基本的なマージ
    別のブランチの内容をmainとかからmergeする
    分岐しているのを統合した新しいコミットができてそれを参照するようになる
    親を2つ持っている

コンフリクト:同じファイルの同じ箇所を変更すると衝突が起きる．どっちを優先すればいいかわからない
```
<<<<<<< HEAD
HEADの変更
========
他のブランチの変更
>>>>>>> feature
```
解決方法
- ファイルの内容を書き換えて<<,==,>>を消す
予防
- 複数人で同じファイルを触らない
- merge，pullのとき変更中の状態をなくす(commitやstash)
- pullするときはpullするブランチに移動してからpull ->　自分が今いるブランチにマージされちゃうから　fetchのほうが安心

ブランチ名の変更，ブランチの削除
```
# 自分が今いるブランチ名を変更
$ git branch -m [ブランチ名]
$ git branch -m new_branch
```
```
# 削除(安全:まだマージされていない変更がある場合は削除されない)
$ git branch -d [ブランチ名]
$ git branch -d feature
# 強制削除
$ git branch D [ブランチ名]
```

---
## 開発の流れ
前提
開発時にはmainブランチはリリース専用にして，開発用はトピックごとにブランチを切ってく
mainは今リリースされているものと同じ状態になる

レビューのためにpullリクエスト
1. mainブランチを最新に更新
```
$ git pull origin main
```
2. ブランチを作成
```
$ git checkout -b pull_request
```
3. (変更後に)変更をステージング
```
$ git add index.html
```

4. コミット(ここまではローカル)
```
$ git commit -m "hogehoge"
```
5. githubへプッシュ
```
$ git push origin/pull_request
```
6. プルリクエスト
githubでプルリクエスと作成
new pull request -> base:main(元のやつ) compare:pull_request(ブランチきったやつ) -> 本文書く
7. チームメンバーがコードレビュー
reviewerを選択
reviewerはメールが来るので，pull_requestタブからpull_equestの内容を見る -> files changed
行の横の+ボタンでレビュー内容をかける
review chagesボタンでapprove
8. プルリクエストをマージ
github上でマージボタンを使ってマージ
9. ブランチを削除
```
$ git branch -d pull_request
```

### github flow
mainはつでにデプロイできる状態に保っておく
新しい開発はmainブランチから新しいブランチを切って作る
この新しいブランチで開発，コミット，定期的にプッシュ
完了したらプルリクエスト
必ずレビューを受ける
```
# マスターブランチを確認
$ git branch
# 変更されてないか確認
$ git status
# マスタープランチを最新の状態にする
$ git pull origin master
# 新しいブランチを作成
$ git checkout -b github_flow
$ git add [変更ファイル名]
$ git commit -V
# ブランチをプッシュ
$ git push origin github_flow
# githubでpull_request作成
# reviewers追加
# reviewerはreview changedボタンからapprove
# marge pull requestボタン->mergeボタン->delete branchボタン
# ブランチの削除
$ git branch -d [すでにマージされたブランチ名]
```

---
## その他
### リベース
変更を統合の際に，履歴をきれいに整えるために使用
ブランチの起点となるコミットを別のコミットに移動->コミットが一直線になる
```
$ git rebase [ブランチ名(ここで指定したブランチが親コミットになる)]
```
以下の流れで自分のブランチにmainの変更を統合，親コミットとすることができる
```
$ git checkout feature(自分のブランチ)
$ git rebase main(親コミットにするブランチ)
```
その後mainでファストフォワードしてfeatureの内容をマージできる
```
$ git merge feature
```
※githubにプッシュした内容をrebaseするのだけはだめ
```
git push -f　は絶対にダメ
```

マージとリベース
--rebaseをつけるとマージコミットが残らない
```
$ git pull [リモ] [ブラ]
$ git pull --rebase [リモ] [ブラ] 
```

### タグ
リリースポイント(日付とか)をつけてコミットを見つけやすくできる
```
$ git tag
$ git tag -l "202505"
```
タグの作成．注釈付きと軽量版の2種類のタグがある
まあそういうのがあるとわかってればいいかな
```
# アノテーション
$ git tag -a [タグ名] -m [メッセージ]
#軽量
$ git tag [タグ名]
```
タグの表示　これでタグに紐づいたコミットの情報を表示できる
```
$ git show [タグ名]
```
タグはgit pushで送信されないので別途
```
$ git push　[リモート名] [タグ名]
# 一斉送信
$ git push origin --tags
```
### 作業の一時避難 stash
急遽作業中に別の作業(バグ修正とか)をする必要が出た時に今の作業をstash
```
$ git stash
$ git stash save
#　確認
$ git stash list
# 復元　特定の作業の場合はlstの[スタッシュ名]をつける
$ git stash apply
#ステージ情報も復元
$ git stash apply --index
#　避難したやつを削除 特定の作業の場合はlstの[スタッシュ名]をつける
$ git stash drop
#全部消す
$ git stash clear
```