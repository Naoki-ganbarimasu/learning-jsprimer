# 例外処理

## try...catch 構文

`try...catch`構文は例外が発生しうるブロックをマークし、例外が発生したときの処理を記述するための構文である。

`try...catch`構文の`try`ブロック内で例外が発生すると、`try`ブロック内のそれ以降の処理は実行されず、`catch`節に処理が移行する。`catch`節は、`try`ブロック内で例外が発生すると、発生したエラーオブジェクトとともに呼び出される。`finally`節は、`try`ブロック内で例外が発生したかどうかには関係なく、必ず`try`文の最後に実行される。

次のコードでは、`try`ブロックで例外が発生し、`catch`節の処理が実行され、最後に`finally`節の処理が実行される。

```javascript
try {
  console.log("try節: この行は実行される");
  // 未定義の関数を呼び出してReferenceError例外が発生する
  undefinedFunction();
  // 例外が発生したため、この行は実行されません
} catch (error) {
  // 例外が発生したあとはこのブロックが実行される
  console.log("catch節: この行は実行される");
  console.log(error instanceof ReferenceError); // => true
  console.log(error.message); // => "undefinedFunction is not defined"
} finally {
  // このブロックは例外の発生に関係なく必ず実行される
  console.log("finally節: この行は実行される");
}
```

## throw 文

`throw`文を使うとユーザーが例外を投げることができる。例外として投げられたオブジェクトは、`catch`節で関数の引数のようにアクセスできる。

```javascript
try {
  // 例外を投げる
  throw new Error("例外が投げられました");
} catch (error) {
  // catch節のスコープでerrorにアクセスできる
  console.log(error.message); // => "例外が投げられました"
}
```

## エラーオブジェクト

`throw`文ではエラーオブジェクトを例外として投げることができる。ここでは、`throw`文で例外として投げられるエラーオブジェクトについて見ていく。

### Error

`Error`オブジェクトのインスタンスは`new Error("エラーメッセージ")`で作成する。

```javascript
function assertPositiveNumber(num) {
  if (num < 0) {
    throw new Error(`${num} is not positive.`);
  }
}

try {
  assertPositiveNumber(-1);
} catch (error) {
  console.log(error instanceof Error); // => true
  console.log(error.message); // => "-1 is not positive."
}
```

## ビルトインエラー

ビルトインエラーとして投げられるエラーオブジェクトは、すべて`Error`オブジェクトを継承したオブジェクトのインスタンスである。

### ReferenceError

`ReferenceError`は存在しない変数や関数などの識別子が参照された場合のエラーである。

```javascript
try {
  console.log(x);
} catch (error) {
  console.log(error instanceof ReferenceError); // => true
  console.log(error.name); // => "ReferenceError"
  console.log(error.message); // エラーメッセージが表示される
}
```

### SyntaxError

`SyntaxError`は構文的に不正なコードを解釈しようとした場合のエラーである。

```javascript
try {
  eval("foo! bar!");
} catch (error) {
  console.log(error instanceof SyntaxError); // => true
  console.log(error.name); // => "SyntaxError"
  console.log(error.message); // エラーメッセージが表示される
}
```

# TypeError

`TypeError`は、値が期待される型ではない場合のエラーである。次のコードでは、関数ではないオブジェクトを関数呼び出ししているため、`TypeError`例外が投げられる。

```javascript
try {
  // 関数ではないオブジェクトを関数として呼び出す
  const fn = {};
  fn();
} catch (error) {
  console.log(error instanceof TypeError); // => true
  console.log(error.name); // => "TypeError"
  console.log(error.message); // エラーメッセージが表示される
}
```

## ビルトインエラーを投げる

ビルトインエラーのインスタンスを作成し、そのインスタンスを例外として投げることもできる。通常の`Error`オブジェクトと同じように、それぞれのビルトインエラーオブジェクトを`new`してインスタンスを作成できる。

```javascript
// 文字列を反転する関数
function reverseString(str) {
  if (typeof str !== "string") {
    throw new TypeError(`${str} is not a string`);
  }
  return Array.from(str).reverse().join("");
}

try {
  // 数値を渡す
  reverseString(100);
} catch (error) {
  console.log(error instanceof TypeError); // => true
  console.log(error.name); // => "TypeError"
  console.log(error.message); // => "100 is not a string"
}
```

## エラーとデバッグ

JavaScript 開発において、デバッグ中に発生したエラーを理解することは非常に重要である。エラーが持つ情報を活用することで、ソースコードのどこでどのような例外が投げられたのかを知ることができる。

エラーはすべて`Error`オブジェクトを拡張したオブジェクトで宣言されています。つまり、エラーの名前を表す`name`プロパティと内容を表す`message`プロパティを持っています。この 2 つのプロパティを確認することで、多くの場面で開発に役立つ。

次のコードでは、`try...catch`文で囲っていない部分で例外が発生している。

```javascript
function fn() {
  // 存在しない変数を参照する
  x++;
}
fn();
```

### console.error とスタックトレース

`console.error`メソッドでは、メッセージと合わせてスタックトレースをコンソールへ出力できる。

```javascript
function fn() {
  console.log("メッセージ");
  console.error("エラーメッセージ");
}

fn();
```

## [ES2022] Error Cause

ES2022 で追加された`Error`の`cause`オプションを使用することで、再度エラーを投げる際にも本来のエラーのスタックトレースを保持できる。

```javascript
// 数字の文字列を二つ受け取り、合計を返す関数
function sumNumStrings(a, b) {
  try {
    const aNumber = safeParseInt(a);
    const bNumber = safeParseInt(b);
    return aNumber + bNumber;
  } catch (e) {
    throw new Error("Failed to sum a and b", { cause: e });
  }
}

try {
  // 数値にならない文字列 'string' を渡しているので例外が投げられる
  sumNumStrings("string", "2");
} catch (err) {
  console.error(err);
}
```

このスクリプトを読み込むと、`sumNumStrings`の例外に`safeParseInt`から投げられたスタックトレースが付与された状態のエラーログがコンソールに出力される。
