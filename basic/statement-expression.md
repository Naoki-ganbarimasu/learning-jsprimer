# 文と式

## 式

```javascript
// 1という式の評価値を表示
console.log(1); // => 1
// 1 + 1という式の評価値を表示
console.log(1 + 1); // => 2
// 式の評価値を変数に代入
const total = 1 + 1;
// 関数式の評価値(関数オブジェクト)を変数に代入
const fn = function() {
    return 1;
};
// fn() という式の評価値を表示
console.log(fn()); // => 1
```

## 文

一方、if文などは文であり式ではない。のでconstとかletで宣言できない

## 式文

一方で、式（Expression）は文（Statement）になる。文となった式のことを式文と呼ぶ。 基本的に文が書ける場所には式

## ブロック文

```javascript
{
    文;
    文;
}
```

ブロック文は単独でも書けますが、基本的にはif文やfor文など他の構文と組み合わせて書くことがほとんど。 
次のコードでは、if文とブロック文を組み合わせることで、if文の処理内容に複数の文を書いている。

```javascript
// if文とブロック文の組み合わせ
if (true) {
    console.log("文1");
    console.log("文2");
}
```

よくみるやつ。
文終わりにセミコロンをつけるのは、文の終わりを明示するため。知ってはいたけどあんまり気にして書いてなかった。

### [コラム] 単独のブロック文の活用

単独のブロック文で変数定義を囲むことで回避できる。

```javascript
// REPLでの動作。»はREPLの入力欄
» {
    const count = 1;
}
undefined // ここでブロック内で定義した変数`count`は参照できなくなる
» {
    const count = 1;
}
undefined // ここでブロック内で定義した変数`count`は参照できなくなる
```

## 関数宣言（文）と関数式

```javascript
// learn関数を宣言する関数宣言文
function learn() {
}
// 関数式をread変数へ代入
const read = function() {
};
```

以下も同様

```javascript
function fn() {}
// fn(式)の評価値を代入する変数宣言の文
const read = fn;
```