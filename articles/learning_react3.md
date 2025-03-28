---
title: "Reactのお勉強3"
emoji: "👏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [react, javascript, typescript]
published: false
published_at: 2025-03-28 22:54
---

# JavaScriptによるDOM操作(古)
従来のJSのDOM操作と問題点を知ることで，Reactがそれをどう改善したのかを理解できるよ
ちなみにJQueryはJSのライブラリで，処理を略して書けるだけでやってることはおんなじっぽい

### DOMアクセス
JSでDOMを手続的に操作することができる．documentはDOMツリーのエントリーポイントで取得された要素をElementという．
index.jsからidを指定して要素を取得する方法3つ

```js
// getElementsByIdで特定のElementを取得　例えば<h1 id="title">hogehoge</h1>みたいなの
const title = document.getElementById('title');
```
```js
// querySelectorで特定のElementを取得　IDは#
const title2 = document.querySelector('#title');
// querySelectorで最初のElementを取得　クラス名は.
const title2 = document.querySelector('.container');
// querySelectorAllで全てのElementを取得
const title2 = document.querySelectorAll('.container');
```
```js
// getElementsByClassNameでHTMLCollectionに複数のElementを格納して取得
const title3 = document.getElementById('title');
```

※一応以下の違いがある
- querySelectorAll->NodeList
- getElementsByClassName->HTMLCollection



従来のDOMの操作はどのElementに対して操作していくのかを明示的に記述する必要があった
これがわけわかんなくなったり，バグったりする原因だった


### DOMの作成，追加，削除
DOMの作成．引数にHTMLのタグ名を入れて作成
```js
// <div></div>が格納される
const divElement = document.createElement("div");
```
DOMへの追加．作成したElementに要素を追加できる
```js
const divElement = document.createElement("div");
const pElement = document.createElement("p");
// これでdivの配下にpが追加される　->末尾に追加されてく．prepend()だと先頭に追加
divElement.appendChild(pElement);
```
DOMの削除.
```js
// これでdiv内のpを消せる
divElement.removeChild(pElement);

// ちなみに子要素の全削除はtextContentにnullを設定してできる
divElement.textContent = null;
```


以上から古のJSのDOM操作が結構めんどいことがわかった
それを踏まえてReactへGO!!!

↓次の記事