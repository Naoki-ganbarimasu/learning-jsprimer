# 暗黙な型変換について

## 厳密等価演算子

厳密等価演算子（===）では異なるデータ型を比較した場合に、その比較結果は必ずfalseとなる。 次のコードは、数値の1と文字列の"1"という異なるデータ型を比較しているので、結果はfalseとなる。

```javascript
// `===`では、異なるデータ型の比較結果はfalse
console.log(1 === "1"); // => false
```

```javascript
// 等価演算子（==）では異なるデータ型を比較した場合に、同じ型となるように暗黙的な型変換をしてから比較される
// `==`では、異なるデータ型は暗黙的な型変換をしてから比較される
// 暗黙的な型変換によって 1 == 1 のように変換されてから比較される
console.log(1 == "1"); // => true
```

## 等価演算子の暗黙的な型変換

もっとも有名な暗黙的な型変換は、先ほども出てきた等価演算子（==）。 等価演算子は、オペランド同士が同じ型となるように暗黙的な型変換をしてから、比較する。

```javascript
// 異なる型である場合に暗黙的な型変換が行われる
console.log(1 == "1"); // => true
console.log(0 == false); // => true
console.log(10 == ["10"]); // => true
```

上記のように意図しないような型変換によって結果がこのなることがあるため、あんまり等価演算子は使わない方がいいかもしれない。厳密等価演算子を使うべき。

以下の式は、真偽値のtrueが数値の1へと暗黙的に変換されてから加算処理が行われる。

```javascript
// 暗黙的な型変換が行われ、数値の加算として計算される
1 + true; // => 2
// 次のように暗黙的に変換されてから計算される
1 + 1; // => 2
```

## さまざまな暗黙的な型変換

次のコードでは、数値の1と文字列の"2"をプラス演算子で処理する。プラス演算子（+）は、数値の加算と文字列の結合を両方実行できるように多重定義されている。このケースでは、JavaScriptは文字列の結合を優先する仕様となっている。そのため、数値の1を文字列の"1"へ暗黙的に変換してから、文字列結合する。

```javascript
1 + "2"; // => "12"　マイナスも同じように暗黙的な型変換が行われる。
// 演算過程で次のように暗黙的な型変換が行われる
"1" + "2"; // => "12"
```

次のように3つ以上の値を+演算子で演算する場合に、値の型が混ざっていると、 演算する順番によっても結果が異なる。

```javascript
const x = 1, y = "2", z = 3;
console.log(x + y + z); // => "123"
console.log(y + x + z); // => "213"
console.log(x + z + y); // => "42"
```

### 任意の値 → 真偽値

```javascript
Boolean("string"); // => true
Boolean(1); // => true
Boolean({}); // => true
Boolean(0); // => false
Boolean(""); // => false
Boolean(null); // => false
```

JavaScriptでは、次の値はfalseへ変換される。

- false
- undefined
- null
- 0
- 0n
- NaN
- ""（空文字列）

### 数値 → 文字列

```javascript
String(1); // => "1"
String("str"); // => "str"
String(true); // => "true"
String(null); // => "null"
String(undefined); // => "undefined"
String(Symbol("シンボルの説明文")); // => "Symbol(シンボルの説明文)"
// プリミティブ型ではない値の場合
String([1, 2, 3]); // => "1,2,3"
String({ key: "value" }); // => "[object Object]"
String(function() {}); // "function() {}"
```

### シンボル → 文字列

次のコードでは、シンボルを文字列結合演算子（+）で文字列に変換できないというTypeErrorが発生する。

```javascript
"文字列と" + Symbol("シンボルの説明"); // => TypeError: can't convert symbol to string
```

この問題もStringコンストラクタ関数を使うことで、シンボルを明示的に文字列化することで解決できる。

```javascript
"文字列と" + String(Symbol("シンボルの説明")); // => "文字列とSymbol(シンボルの説明)"
```

### 文字列 → 数値

この式の使い所はユーザー入力として数字を受け取ることがあげられる。ユーザー入力は文字列でしか受け取ることができないため、それを数値に変換してから利用する必要がある。

```javascript
// ユーザー入力を文字列として受け取る
const input = window.prompt("数字を入力してください", "42");
// 文字列を数値に変換する
const num = Number(input);
console.log(typeof num); // => "number"
console.log(num); // 入力された文字列を数値に変換したもの
```

また、文字列から数字を取り出して変換する関数としてNumber.parseInt、Number.parseFloatも利用できる。Number.parseIntは文字列から整数を取り出し、Number.parseFloatは文字列から浮動小数点数を取り出すことができる。Number.parseInt(文字列, 基数)の第二引数には基数を指定する。たとえば、文字列をパースして10進数として数値を取り出したい場合は、第二引数に基数として10を指定する。

```javascript
// "1"をパースして10進数として取り出す
console.log(Number.parseInt("1", 10)); // => 1
// 余計な文字は無視してパースした結果を返す
console.log(Number.parseInt("42px", 10)); // => 42
console.log(Number.parseInt("10.5", 10)); // => 10
// 文字列をパースして浮動小数点数として取り出す
console.log(Number.parseFloat("1")); // => 1
console.log(Number.parseFloat("42.5px")); // => 42.5
console.log(Number.parseFloat("10.5")); // => 10.5
```

しかし、ユーザーが数字を入力するとは限りません。Numberコンストラクタ関数、Number.parseInt、Number.parseFloatは、 数字以外の文字列を渡すとNaN（Not a Number）を返す。

```javascript
// 数字ではないため、数値へは変換できない
Number("文字列"); // => NaN
// 未定義の値はNaNになる
Number(undefined); // => NaN
```

そのため、任意の値から数値へ変換した場合には、NaNになってしまった場合の処理を書く必要がある。変換した結果がNaNであるかはNumber.isNaN(x)メソッドで判定できる。

```javascript
const userInput = "任意の文字列";
const num = Number.parseInt(userInput, 10);
if (Number.isNaN(num)) {
    console.log("パースした結果NaNになった", num);
}
```

NaNはNot a NumberだけどNumber型の値を持つ特殊な値。

NaNという値を作る方法は簡単で、Number型と互換性のない性質のデータをNumber型へ変換した結果はNaNとなる。たとえば、オブジェクトは数値とは互換性のないデータ。そのため、オブジェクトを明示的に変換したとしても結果はNaNになる。

```javascript
Number({}); // => NaN
```

また、NaNは何と演算しても結果はNaNになる特殊な値。次のように、計算の途中で値がNaNになると、最終的な結果もNaNとなる。

```javascript
const x = 10;
const y = x + NaN;
const z = y + 20;
console.log(x); // => 10
console.log(y); // => NaN
console.log(z); // => NaN
```

NaNはNumber型の一種であるという名前と矛盾したデータに見える。

```javascript
// NaNはnumber型
console.log(typeof NaN); // => "number"
```

NaNしか持っていない特殊な性質として、自分自身と一致しないというものがある。この特徴を利用することで、ある値がNaNであるかを判定できる。

```javascript
function isNaN(x) {
    // NaNは自分自身と一致しない
    return x !== x;
}
console.log(isNaN(1)); // => false
console.log(isNaN("str")); // => false
console.log(isNaN({})); // => false
console.log(isNaN([])); // => false
console.log(isNaN(NaN)); // => true
```

同様の処理をする方法としてNumber.isNaN(x)メソッドがある。実際に値がNaNかを判定する際には、Number.isNaN(x)メソッドを利用するとよい。

```javascript
Number.isNaN(NaN); // => true
```

NaNは暗黙的な型変換の中でももっとも避けたい値となる。理由として、先ほど紹介したようにNaNは何と演算しても結果がNaNとなってしまう。これにより、計算していた値がどこでNaNとなったのかがわかりにくく、デバッグが難しくなる。

```javascript
// 任意の個数の数値を受け取り、その合計値を返す関数
function sum(...values) {
    return values.reduce((total, value) => {
        return total + value;
    }, 0);
}
const x = 1, z = 10;
let y; // `y`はundefined
console.log(sum(x, y, z)); // => NaN
```

undefinedに数値を加算すると結果はNaNとなる。

NaNへの変換を避ける方法として、、、
sum関数側でNumber型を扱うようにする。
以下が例＝＞throwで例外処理をしている。

```javascript
function sum(...values) {
    return values.reduce((total, value) => {
        // 値がNumber型ではない場合に、例外を投げる
        if (typeof value !== "number") {
            throw new Error(`${value}はNumber型ではありません`);
        }
        return total + Number(value);
    }, 0);
}
const x = 1, z = 10;
let y; // `y`はundefined
console.log(x, y, z);
// Number型の値ではない`y`を渡しているため例外が発生する
console.log(sum(x, y, z)); // => Error
```

またから文字でも意図しない挙動になってしまうことがあるため以下のようにかくことで回避できる。

```javascript
// 空文字列かどうかを判定
function isEmptyString(str) {
    // String型でlengthが0の値の場合はtrueを返す
    return typeof str === "string" && str.length === 0;
}
console.log(isEmptyString("")); // => true
// falsyな値でも正しく判定できる
console.log(isEmptyString(0)); // => false
console.log(isEmptyString()); // => false
```
