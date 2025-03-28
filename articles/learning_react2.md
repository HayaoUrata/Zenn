---
title: "Reactのお勉強2"
emoji: "🔥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [react, javascript, typescript]
published: true
published_at: 2025-03-28 18:24
---
まずはモダンJavaScriptを簡単に復習

### 変数宣言
```js
var value = "hogehoge"
```
これだと上書き可能(変更したくない値も上書きされちゃう)，再宣言可能(同じ変数名が複数箇所で宣言できてしまう)という問題がある -> 基本使わない

基本的にはconst,letを使っていく
- letは上書き可能，再宣言不可
```js
// 上書きはOK
let value = "hoge1"
value = "hoge2"

// 再宣言はだめ
let value = "hoge1"
// エラー
let value = "hoge2"
```

- constは上書きも再宣言もできない定数
```js
// 上書きもだめ
const value = "hoge1"
// エラー
value = "hoge2"
```

※プリミティブ型は変更不可だがオブジェクト型の中身は変更できる
オブジェクトの定義と配列の定義は基本的にconstで定義する.

### テンプレート文字列
`(バッククォート)で囲んで使う.バックウォート内の${}ではJSのコードを書けるので,文字列の中でJSで定義した変数を展開できる.
```js
const name = "ほげ"

const age = 20

const message = `私は${name}です．${age}歳です．`
```

### アロー関数()=>{}
従来と違いfunctionという宣言が要らなくなった
```js
const func = (value) => {
    return value;
}
```
特徴的な省略パターン
- 引数が1つの時は括弧を書かなくてもいい　
```js
const func = value => {
    return value;
}
```
-　処理が単一行で終わる時は{}とreturnは省略できる
```js
const fun = (value1, value2) => value1 + value2
```
- {}の中身が複数行のときは({複数業})とすることで単一行のようにまとめて返せる.
これはreturnも省略してる．Reactでもよく使うので覚えとく．
```js
const func = (value1, value2) => (
    {
        nam: "hoge",
        age: 20,
    }
)
```
※いろんな書き方があって混乱するけどPrettierというのを使って，コードの整形ができる．チーム開発とかでも統一感を出せる．

### 分割代入
テンプレート文字列とかでプロパティ(オブジェクトの要素)が多いときいちいち書くのは面倒なので，まとめて変数に代入しちゃおうってやつ
```js
const profile (
    {
        name: "hoge",
        age: 20,
    }
)
// いつもはprofile.nameとかでアクセスしてた
// 分割代入だとオブジェクト内で定義されてるやつを取り出しとくことができる
const { name, age } = profile
const message = `${name}です．`
```
ちなみに:をつけてプロパティ名を変更して分割代入できる
```js
const { name: onamae } = profile
```

### 配列の分割代入
配列に格納されている順に任意の配列名を設定しながら代入できる.順番は入れ替えられない.インデックスの途中までしか要らないとかもOK
```js
profile = ["hoge", 20]
const[name] = profile
```

### デフォルト値
関数の呼び出し時に値が定義されてないとundefinedになっちゃうので，あらかじめデフォルトを設定できる.
```js
const hoge = (value="hogehoge") => {
    return value
}
```
オブジェクトの分割代入時に存在しないプロパティは代入できないのでこれもデフォルト値を設定しとくと回避できる
```js
const profile = {
    age: 20
}
const{ name = "hoge"} = profile
```

### スプレッド構文
...を使って要素を展開できる.例えば関数への代入とかで役立つ
```js
const array = [1,2,3]
// 1 2 3 が格納される
console.log(...array) 
```
要素をまとめることも可能
```js
const array = [1,2,3,4,5,6,7]
// それぞれ1, 2, [4,5,6,7]が代入される
const {num1, num2, ...nums} = array
```
コピー，結合にも使える．...で複数要素を取り出してるのでそれを[]で囲めば新たな配列ができるイメージ
※=を使うと参照値(場所)もコピーしちゃうので意図しない値も書き換えられちゃう可能性がある．コピーなのか，書き換えなのかは意識する．

### オブジェクトの省略記法
プロパティと変数名が同じ時は省略できる．例えばあらかじめname,ageが定義されてて
```js
const value = { 
    name: name
    age: age,
}
```
みたいな時は，下のように省略できる
```js
const value = { 
    name,
    age,
}
```

※ESLintという静的解析ツールがあり,varでの定義とか,console.logとか使っってない変数とかをチェックできる.Prettierと合わせて使われる．

### map, filter
mapは配列のループ処理とかの記述量を削減する仕組み．
配列を順番に処理して結果を配列として受け取る．配列.map()という形で使用．
ごちゃごちゃするので記述ステップごとに分解して解説
Step.1　map関数の()中にアロー関数を書く
```js
const profile = ["hoge1", "hoge2", "hoge3"]
const func = profile.map(() => {})
```
Step.2　関数は引数を持てる．
関数の引数(ここではname)に配列の値が格納されてく
```js
const profile = ["hoge1", "hoge2", "hoge3"]
const func = profile.map((name) => {
    return name
})
```
ちなみに第2引数はindexが入ってくる
map(name, index) => みたいな書き方でindexを取得できる


filterは基本的にmapと同じだけど，returnの後に条件式を書いて，それに該当する要素だけが返される．
```js
const profile = [1, 2, 3, 4, 5, 6]
// 条件に合致する1, 3, 5だけが返される
const func = profile.filter((name) => {
    return num % 2 === 1;
})
```

### 三項演算子
条件　? 条件がTrueの時の処理 : 条件がFalseの時の処理
```js
const hoge = 0 > 1 ? "Trueです" : "Falseです"
```
