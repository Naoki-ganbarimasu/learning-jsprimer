# 関数と this

基本的にはメソッドの中で利用しますが、this は読み取り専用のグローバル変数のようなものでどこにでも書けます。 加えて、this の参照先（評価結果）は条件によって異なります。

- 実行コンテキストにおける this
- コンストラクタにおける this
- 関数とメソッドにおける this
- Arrow Function における this

コンストラクタにおける this は、次の章である「クラス」で扱う。 this が実際に使われるのはメソッドにおいてです。 そのため、あらゆる条件下での this の動きをすべて覚える必要はない。

## 実行コンテキストと this

### スクリプトにおける this

実行コンテキストが`"Module"`である場合、そのトップレベルのスコープに書かれた `this` は常に `undefined` となる。

ブラウザで、`script` 要素に `type="module"`属性がついた場合は、実行コンテキストが"Module"として実行されれる。 この script 要素の直下に書いた `this` は `undefined` となる。
```js
<script type="module">
// 実行コンテキストは"Module"
console.log(this); // => undefined
</script>
```