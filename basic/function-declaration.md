# 関数と再宣言

## 関数の4つの要素で構成されている

- **関数名** - 利用できる名前は変数名と同じ（「変数名に使える名前のルール」を参照）
- **仮引数** - 関数の呼び出し時に渡された値が入る変数。複数ある場合は,（カンマ）で区切る
- **関数の中身** - `{`と`}`で囲んだ関数の処理を書く場所
- **関数の返り値** - 関数を呼び出したときに、呼び出し元へ返される値

## [ES2015] デフォルト引数

```javascript
function echo(x = "デフォルト値") {
    return x;
}

console.log(echo(1)); // => 1
console.log(echo()); // => "デフォルト値"
```

引数がないときにデフォルト値を設定することができる

[ES2015] デフォルト引数がない時は以下のようにOR演算子を使っていた。

```javascript
function addPrefix(text, prefix) {
    const pre = prefix || "デフォルト:";
    return pre + text;
}

console.log(addPrefix("文字列")); // => "デフォルト:文字列"
console.log(addPrefix("文字列", "カスタム:")); // => "カスタム:文字列"
```

## 呼び出し時の引数が多いとき

```javascript
function add(x, y) {
    return x + y;
}
add(1, 3); // => 4
add(1, 3, 5); // => 4
```

## 可変長引数

固定した数ではなく任意の個数の引数を受け取れることを可変長引数と呼ぶ

### 以下例

```javascript
// Math.maxは可変長引数を受け取る関数
const max = Math.max(1, 5, 10, 20);
console.log(max); // => 20
```

## [ES2015] Rest parameters

```javascript
function fn(...args) {
    // argsは、渡された引数が入った配列
    console.log(args); // => ["a", "b", "c"]
}
fn("a", "b", "c");
```

追加してかける。

```javascript
function fn(arg1, ...restArgs) {
    console.log(arg1); // => "a"
    console.log(restArgs); // => ["b", "c"]
}
fn("a", "b", "c");
```

## Spread構文

Spread構文は、配列の前に`...`をつけた構文のことで、関数には配列の値を展開したものが引数として渡される。
次のコードでは、`array`の配列を展開して`fn`関数の引数として渡している。

```javascript
function fn(x, y, z) {
    console.log(x); // => 1
    console.log(y); // => 2
    console.log(z); // => 3
}

const array = [1, 2, 3];
// Spread構文で配列を引数に展開して関数を呼び出す
fn(...array);
// 次のように書いたのと同じ意味
fn(array[0], array[1], array[2]);
```

## arguments

可変長引数を扱う方法として、`arguments`という関数の中でのみ参照できる特殊な変数がある。
`arguments`は関数に渡された引数の値がすべて入ったArray-likeなオブジェクト。
Array-likeなオブジェクトは、配列のようにインデックスで要素へアクセスできる。
しかし、Arrayではないため、実際の配列とは異なりArrayのメソッドは利用できないという特殊なオブジェクト。

次のコードでは、`fn`関数に仮引数が定義されていません。
しかし、関数の内部では`arguments`という変数で、実際に渡された引数を配列のように参照できます。

```javascript
function fn() {
    // `arguments`はインデックスを指定して各要素にアクセスできる
    console.log(arguments[0]); // => "a"
    console.log(arguments[1]); // => "b"
    console.log(arguments[2]); // => "c"
}
fn("a", "b", "c");
```

なんで配列じゃないんだろう？かなり使いづらそう

## [ES2015] 関数の引数と分割代入

関数の引数においても分割代入（Destructuring assignment）が利用できる。
分割代入はオブジェクトや配列からプロパティを取り出し、変数として定義し直す構文。
以下のコードは関数のひきすうとしてオブジェクトを受け取り、その`id`プロパティを出力している。

```javascript
function printUserId(user) {
    console.log(user.id); // => 42
}
const user = {
    id: 42
};
printUserId(user);
```

配列についても利用可能

## 関数はオブジェクト

関数は関数オブジェクトとも呼ばれ、オブジェクトの一種
関数はただのオブジェクトとは異なり、関数名に`()`をつけることで、関数としてまとめた処理を呼び出すことができる。
一方で、`()`をつけて呼び出されなければ、関数をオブジェクトとして参照できる。
また、関数はほかの値と同じように変数へ代入したり、関数の引数として渡すことが可能。

```javascript
function fn() {
    console.log("fnが呼び出されました");
}
// 関数`fn`を`myFunc`変数に代入している
const myFunc = fn;
myFunc();
```

このように関数が値として扱えることを、ファーストクラスファンクション（第一級関数）と呼びます。

先ほどのコードでは、関数宣言をしてから変数へ代入していましたが、最初から関数を値として定義できる。
関数を値として定義する場合には、関数宣言と同じ`function`キーワードを使った方法とArrow Functionを使った方法がある。
どちらの方法も、関数を式（代入する値）として扱うため関数式と呼ぶ。

## 関数式

関数式とは、関数を値として変数へ代入している式のことを言う。
関数宣言は文でしたが、関数式では関数を値として扱う。
これは、文字列や数値などの変数宣言と同じ定義方法。

```javascript
// 関数式
const 変数名 = function() {
    // 関数を呼び出したときの処理
    // ...
    return 関数の返り値;
};
```

### [ES2015] Arrow Function

```javascript
// Arrow Functionを使った関数定義
const 変数名 = () => {
    // 関数を呼び出したときの処理
    // ...
    return 関数の返す値;
};
```

#### 省略した書き方

```javascript
const fnA = () => { /* 仮引数がないとき */ };
const fnB = (x) => { /* 仮引数が1つのみのとき */ };
const fnC = x => { /* 仮引数が1つのみのときは()を省略可能 */ };
const fnD = (x, y) => { /* 仮引数が複数のとき */ };
// 値の返し方
// 次の２つの定義は同じ意味となる
const mulA = x => { return x * x; }; // ブロックの中でreturn
const mulB = x => x * x;
```

アロー関数の特徴

- 名前をつけることができない（常に無名関数）
- `this`が静的に決定できる（詳細は「関数とスコープ」の章で解説します）
- `function`キーワードに比べて短く書くことができる
- `new`できない（コンストラクタ関数ではない）
- `arguments`変数を参照できない.

## [コラム] 同じ名前の関数宣言は上書きされる

`fn`という関数名を2つ定義していますが、最後に定義された`fn`関数が優先される。
また、仮引数の定義が異なっていても、関数の名前が同じなら上書きされる。

```javascript
function fn(x) {
    return `最初の関数 x: ${x}`;
}
function fn(x, y) {
    return `最後の関数 x: ${x}, y: ${y}`;
}
console.log(fn(2, 10)); // => "最後の関数 x: 2, y: 10"
```

以下のようにアロー関数で`const`や`let`を使って定義すると、同じ変数名を定義できないため、構文エラーとなりなる。いいね！

```javascript
const fn = (x) => {
    return `最初の関数 x: ${x}`;
};
// constは同じ変数名を定義できないため、構文エラーとなる
const fn = (x, y) => {
    return `最後の関数 x: ${x}, y: ${y}`;
};
```

## コールバック関数

関数はファーストクラスであるため、その場で作った無名関数を関数の引数（値）として渡すことができる。
引数として渡される関数のことをコールバック関数と呼ぶ。
一方、コールバック関数を引数として使う関数やメソッドのことを高階関数と呼ぶ。

```javascript
function 高階関数(コールバック関数) {
    コールバック関数();
}
```

```javascript
const array = [1, 2, 3];
const output = (value) => {
    console.log(value);
};
array.forEach(output);
// 次のように実行しているのと同じ
// output(1); => 1
// output(2); => 2
// output(3); => 3
```

毎回、関数を定義してその関数をコールバック関数として渡すのは、少し手間がかかる。
そこで、関数はファーストクラスであることを利用して、コールバック関数となる無名関数をその場で定義して渡せる。

```javascript
const array = [1, 2, 3];
array.forEach((value) => {
    console.log(value);
});
```

## メソッド

オブジェクトのプロパティである関数をメソッドと呼ぶ。
JavaScriptにおいて、関数とメソッドの機能的な違いはない。
しかし、呼び方を区別したほうがわかりやすいため、ここではオブジェクトのプロパティである関数をメソッドと呼ぶ。
次のコードでは、`obj`の`method1`プロパティと`method2`プロパティに関数を定義している。
この`obj.method1`プロパティと`obj.method2`プロパティがメソッド。

```javascript
const obj = {
    method1: function() {
        // `function`キーワードでのメソッド
    },
    method2: () => {
        // Arrow Functionでのメソッド
    }
};
```

メソッドを呼び出す場合は、関数呼び出しと同様にオブジェクト.メソッド名()と書くことで呼び出せる。

```javascript
const obj = {
    method: function() {
        return "this is method";
    }
};
console.log(obj.method()); // => "this is method"
```

### [ES2015] メソッドの短縮記法

メソットを関数みたいに直がき

```javascript
const obj = {
    method() {
        return "this is method";
    }
};
console.log(obj.method()); // => "this is method"
```