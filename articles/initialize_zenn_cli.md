---
title: "ZennのCLIの初期設定"
emoji: "🔥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [zenn, cli, github]
published: true
published_at: 2025-03-28 12:40
---

## 初期化
一番最初はローカルのディレクトリを初期化
```
$ npx zenn init
```
->articlesディレクトリが作られる
->この下に記事が追加されてく

## 記事の作成
```
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

## git-hubのリモートリポジトリの確認，設定
```
$ git remote -v
$ git remote add origin https://github.com/ユーザ名とか/githubリポジトリ名
```
## プッシュ
```
$ git push --set-upstream origin main
```
デフォルトのプッシュ先を設定してgit pushだけでプッシュできるようになる
```
$ git branch --set-upstream-to=origin/main
```

## プレビュー
```
$ npx zenn preview
```