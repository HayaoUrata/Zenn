---
title: "Zennの記事執筆をCLIでやりたい"
emoji: "🔥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [zenn, cli, github]
published: true
published_at: 2025-03-28 12:40
---

Zennの記事執筆にCLIを使いたい
-----
zennの記事をCLIを使ってローカル環境のエディタ(VScodeとか)から書けることがわかったので，そのための初期設定などを忘れないように書いておきます．公式の記事を参考にしています．https://zenn.dev/zenn/articles/zenn-cli-guide

## 1．CLIをインストール
最初にローカルのディレクトリを初期化
```
$ npm init --yes
```
インストール
```
npm install zenn-cli
```

## 2．setup
```
$ npx zenn init
```
->articlesディレクトリが作られる
->この下に記事が追加されてく
->一旦booksは無視

## 3.記事の作成
```
articles配下に記事を作成
$ npx zenn new:article
$ npx zenn new:article --slug 記事のスラッグ --title タイトル --type idea --emoji ✨
```
->slugはIDみたいなもん
->記事の設定はyaml形式で以下のようになる
```
        ---
        title: "" # 記事のタイトル
        emoji: "😸" # アイキャッチとして使われる絵文字（1文字だけ）
        type: "tech" # tech: 技術記事 / idea: アイデア記事
        topics: [] # タグ。["markdown", "rust", "aws"]のように指定する
        published: true # 公開設定（falseにすると下書き）
        ---
        ここから本文を書く
```

## 4.git-hubとzennの連携
githubとzennを連携させる必要がある．下のリンクから連携する．連携できるリポジトリは最大2個らしいので，all repositoriesを選ぶとできない．
https://zenn.dev/dashboard/deploys


## 5.git-hubのリモートリポジトリの確認，設定
```
$ git remote -v
$ git remote add origin https://github.com/ユーザ名とか/githubリポジトリ名
```

## 6.push
```
$ git push --set-upstream origin main
```
デフォルトのプッシュ先を設定してgit pushだけでプッシュできるようになる
```
$ git branch --set-upstream-to=origin/main
```

## 7.preview
```
$ npx zenn preview
```
ローカルホストでプレビューできる．やったね！
