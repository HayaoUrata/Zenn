---
title: "Reactのお勉強"
emoji: "🙌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [react, javascript, typescript]
published: false
---

今までなんとなく触っていたReactを本格的に勉強しようと思い．その過程を記録しておくことにしました．
↓こちらの書籍を参考にさせていただきます．
https://amzn.to/42822TI

# 勉強すること
### Ⅰ.モダンJavaScript
まずReactを勉強するにあたりJavaScriptをしっかり理解しておく必要がある．Js，Reactのそれぞれの機能の範囲を正しく認識することでより学習がしやすくなる．ということでまずはモダンJsについて知ろうね．
### Ⅱ.DOM,仮想DOM
フロントエンド開発でDOM(Document Object Model)の知識は必須.実際にはあんまり意識しなくても裏で処理してくれてる．モダンなのは仮想DOM．
- DOM
    HTMLを解釈し，木構造で表現したもの．DOMを直接操作しまくって画面を書き換える　→古
- 仮想DOM
    差分を比較して，変更箇所のみDOMに反映．レンダリング効率UP　→新
### Ⅲ.パッケージマネージャ
ファイル間での読み込みとかパッケージの依存関係の管理，バージョン管理が大変なので，npmやyarnといった技術を使うと勝手に管理してくれる．
json(npm)ファイルやlock(yarn)ファイルでパッケージ情報を管理　-> node_modulesディクレトリ内に実態が展開される.
### Ⅳ.ECMAScript

