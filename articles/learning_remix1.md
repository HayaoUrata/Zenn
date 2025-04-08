---
title: "Remixのお勉強１"
emoji: "⛳"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [remix, react, typescript, javascript]
published: false
---

---
# Remixの勉強

## Remixの特徴
- ReactベースのWebフレームワーク
- サーバーサイドレンダリングとクライアントサイドをうまく連携できる
- ルーティング、データフェッチがシンプル
- エラーハンドリングがシンプル
- フォーム処理やキャッシュ処理も持ってる

## Type scriptの特徴
いきなり脱線だけどRemixと同時にType Scriptも復習したい
- 型安定性が向上する
- コンパイル時に型のチェックが動く
- 特に関数とかAPIの受け渡しの保守性が上がる
- チーム開発で他のメンバが書いた内容をわかりやすい

## Remixプロジェクトの開発
こいつらがいるか確認
```
node -v
npm -v
yarn -v
```
Remixプロジェクトの作成
```
npx create-remix@latest
```
こんなのが走るので、色々設定
```
Need to install the following packages:
create-remix@2.16.4
Ok to proceed? (y)  y

npm warn deprecated inflight@1.0.6: This module is not supported, and leaks memory. Do not use it. Check out lru-cache if you want a good and tested way to coalesce async requests by a key value, which is much more comprehensive and powerful.
npm warn deprecated glob@7.2.3: Glob versions prior to v9 are no longer supported

 remix   v2.16.4 💿 Let's build a better website...

   dir   Where should we create your new project?
         ./my-remix-app

      ◼  Using basic template See https://remix.run/guides/templates for more
      ✔  Template copied

   git   Initialize a new git repository?
         Yes

  deps   Install dependencies with npm?
         Yes

      ✔  Dependencies installed

      ✔  Git initialized

  done   That's it!

         Enter your project directory using cd ./my-remix-app
         Check out README.md for development and deploy instructions.

         Join the community at https://rmx.as/discord
```

初期ディレクトリ構成
![](/images/learning_remix1//image1.png)