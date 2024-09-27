# オブジェクト

オブジェクトはプロパティの集合。  
プロパティとは名前（キー）と値（バリュー）が対になったもの。  
プロパティのキーには文字列または`Symbol`が利用でき、値には任意のデータを指定できる。  
また、1つのオブジェクトは複数のプロパティを持てるため、1つのオブジェクトで多種多様な値を表現できる。

## オブジェクトの作成

オブジェクトリテラルでは、初期値としてプロパティを持つオブジェクトを作成できる。  
プロパティは、オブジェクトリテラル（`{}`）の中にキーと値を`:`（コロン）で区切って記述する。

### プロパティを持つオブジェクトを定義する

```javascript
const obj = {
    // キー: 値
    "key": "value"
};
```

オブジェクトリテラルのプロパティ名（キー）はクォート（`"`や`'`）を省略できる。そのため、次のように書いても同じ。

```javascript
// プロパティ名（キー）はクォートを省略することが可能
const obj = {
    // キー: 値
    key: "value"
};
```

でも以下はNG

```javascript
const object = {
    // キー: 値
    my-prop: "value" // NG
};
```

`"`や`'`を省略しない方がいいかもしれない。

## `{}`はObjectのインスタンスオブジェクト

`Object`はJavaScriptのビルトインオブジェクト。  
オブジェクトリテラル（`{}`）は、このビルトインオブジェクトである`Object`を元にして新しいオブジェクトを作成するための構文。

オブジェクトリテラル以外の方法として、`new`演算子を使うことで、`Object`から新しいオブジェクトを作成できる。  
次のコードでは、`new Object()`でオブジェクトを作成しているが、これは空のオブジェクトリテラルと同じ意味。

```javascript
// プロパティを持たない空のオブジェクトを作成
// = `Object`からインスタンスオブジェクトを作成
const obj = new Object();
console.log(obj); // => {}
```

オブジェクトリテラルのほうが明らかに簡潔で、プロパティの初期値も指定できるため、`new Object()`を使う利点はありません。

`new Object()`でオブジェクトを作成することは、「`Object`のインスタンスオブジェクトを作成する」と言う。  
`new Object()`で作成したオブジェクトは、`Object`のインスタンスオブジェクトであるため、`Object`のプロパティやメソッドを利用できる。

## プロパティへのアクセス

```javascript
const obj = {
    key: "value"
};
// ドット記法で参照
console.log(obj.key); // => "value"
// ブラケット記法で参照
console.log(obj["key"]); // => "value"
```

ドットの方は見覚えある。ブラケットは初めて見た。  
ドット記法（`.`）では、プロパティ名が変数名と同じく識別子の命名規則を満たす必要がある。

```javascript
obj.key; // OK
// プロパティ名が数字から始まる識別子は利用できない
obj.123; // NG
// プロパティ名にハイフンを含む識別子は利用できない
obj.my-prop; // NG
// Symbol型のプロパティ名はドット記法では利用できない
```

ブラケット記法（`[]`）では、プロパティ名が変数名と同じく文字列であれば、どんな文字列でも利用できる。

## [ES2015] オブジェクトと分割代入

以下のコードでは、変数`ja`と`en`を定義し、その初期値として`languages`オブジェクトのプロパティを代入している。

```javascript
const languages = {
    ja: "日本語",
    en: "英語"
};
const ja = languages.ja;
const en = languages.en;
console.log(ja); // => "日本語"
console.log(en); // => "英語"
```

これだと書くこと多いから、分割代入を使うと簡潔に書ける。

```javascript
const languages = {
    ja: "日本語",
    en: "英語"
};
const { ja, en } = languages;
console.log(ja); // => "日本語"
console.log(en); // => "英語"
```

## プロパティの追加と削除

### 追加

```javascript
// 空のオブジェクト
const obj = {};
// `key`プロパティを追加して値を代入
obj.key = "value";
console.log(obj.key); // => "value"
```

### 削除

```javascript
const obj = {
    key1: "value1",
    key2: "value2"
};
// key1プロパティを削除
delete obj.key1;
// key1プロパティが削除されている
console.log(obj); // => { "key2": "value2" }
```

### [コラム] constで定義したオブジェクトは変更可能

```javascript
const obj = { key: "value" };
obj.key = "Hi!"; // constで定義したオブジェクト(`obj`)が変更できる
console.log(obj.key); // => "Hi!"
```

値であるオブジェクトのプロパティが変更できてる。けど、以下のようにオブジェクト自体を変更することはできない。

```javascript
const obj = { key: "value" };
obj = {}; // => TypeError
```

例外として、`Object.freeze`メソッドを使うと、オブジェクト自体を変更できないようにすることができる。  
strictモードにしないとエラーにならないので注意。

```javascript
"use strict";
const object = Object.freeze({ key: "value" });
// freezeしたオブジェクトにプロパティを追加や変更できない
object.key = "value"; // => TypeError: "key" is read-only
```

## プロパティの存在を確認する

```javascript
const obj = {};
console.log(obj.notFound); // => undefined
```

プロパティ名を間違えた場合に単に`undefined`という値を返すため、間違いに気づきにくいという問題がある。

```javascript
const widget = {
    window: {
        title: "ウィジェットのタイトル"
    }
};
// `window`を`windw`と間違えているが、例外は発生しない
console.log(widget.windw); // => undefined
// さらにネストした場合に、例外が発生する
// `undefined.title`と書いたのと同じ意味となるため
console.log(widget.windw.title); // => TypeError: widget.windw is undefined
// 例外が発生した文以降は実行されません
```

あるオブジェクトがあるプロパティを持っているかを確認する方法として、次の4つがある。

- undefinedとの比較
- in演算子
- `Object.hasOwn`静的メソッド [ES2022]
- `Object.prototype.hasOwnProperty`メソッド

### プロパティの存在確認: undefinedとの比較

```javascript
const obj = {
    key: "value"
};
// `key`プロパティが`undefined`ではないなら、プロパティが存在する?
if (obj.key !== undefined) {
    // `key`プロパティが存在するときの処理
    console.log("`key`プロパティの値は`undefined`ではない");
}
```

しかし、この方法はプロパティの値が`undefined`であった場合に、プロパティそのものが存在するかを区別できないという問題がある。

次のコードでは、`key`プロパティの値が`undefined`であるため、プロパティが存在しているにもかかわらず`if`文の中は実行されない。

```javascript
const obj = {
    key: undefined
};
// `key`プロパティの値が`undefined`である場合
if (obj.key !== undefined) {
    // この行は実行されません
}
```

このような場合、プロパティが存在するかを判定するには`in`演算子か`Object.hasOwn`静的メソッドを利用する。

### プロパティの存在確認: in演算子を使う

`in`演算子は、指定したオブジェクト上に指定したプロパティがあるかを判定し真偽値を返す。

```javascript
"プロパティ名" in オブジェクト; // true or false
```

次のコードでは`obj`に`key`プロパティが存在するかを判定している。  
`in`演算子は、プロパティの値は関係なく、プロパティが存在した場合に`true`を返す。

```javascript
const obj = { key: undefined };
// `key`プロパティを持っているならtrue
if ("key" in obj) {
    console.log("`key`プロパティは存在する");
}
```

### [ES2022] プロパティの存在確認: Object.hasOwn静的メソッド

`Object.hasOwn`静的メソッドは、対象のオブジェクトが指定したプロパティを持っているかを判定できる。  
この`Object.hasOwn`静的メソッドの引数には、オブジェクトとオブジェクトが持っているかを確認したいプロパティ名を渡す。

```javascript
const obj = {};
// objが"プロパティ名"を持っているかを確認する
Object.hasOwn(obj, "プロパティ名"); // true or false
```

次のコードでは`obj`に`key`プロパティが存在するかを判定している。  
`Object.hasOwn`静的メソッドも、プロパティの値は関係なく、オブジェクトが指定したプロパティを持っている場合に`true`を返す。

```javascript
const obj = { key: undefined };
// `obj`が`key`プロパティを持っているならtrueとなる
if (Object.hasOwn(obj, "key")) {
    console.log("`obj`は`key`プロパティを持っている");
}
```

### プロパティの存在確認: Object.prototype.hasOwnPropertyメソッド

`Object.hasOwn`静的メソッドはES2022で導入されたメソッドで、ES2022より前では、  
`Object.prototype.hasOwnProperty`メソッドというよく似たメソッドが利用されていた。  
`hasOwnProperty`メソッドは、`Object.hasOwn`静的メソッドとよく似ているが、  
オブジェクトのインスタンスから呼び出す点が異なる。

```javascript
const obj = { key: undefined };
// `obj`が`key`プロパティを持っているならtrueとなる
if (obj.hasOwnProperty("key")) {
    console.log("`obj`は`key`プロパティを持っている");
}
```

## [ES2020] Optional chaining演算子（?.）

プロパティが存在するかが重要な場合は、基本的には`in`演算子または`Object.hasOwn`静的メソッドを使うべき。  
最終的にオブジェクトの値を取得したいなら`if`文で`undefined`をチェックする問題位はない。なぜなら  
値を取得したい場合には、プロパティが存在するかどうかとプロパティの値が`undefined`かどうかの違いを区別する意味はないため。

次のコードでは、`widget.window.title`プロパティに値が定義されているなら（`undefined`ではないなら）、  
そのプロパティの値をコンソールに表示している。

```javascript
function printWidgetTitle(widget) {
    // 例外を避けるために`widget`のプロパティの存在を順番に確認してから、値を表示している
    if (widget.window !== undefined && widget.window.title !== undefined) {
        console.log(`ウィジェットのタイトルは${widget.window.title}です`);
    } else {
        console.log("ウィジェットのタイトルは未定義です");
    }
}
// タイトルが定義されているwidget
printWidgetTitle({
    window: {
        title: "Book Viewer"
    }
});
// タイトルが未定義のwidget
printWidgetTitle({
    // タイトルが定義されてない空のオブジェクト
});
```

```javascript
console.log(widget?.window?.title ?? "ウィジェットのタイトルは未定義です");
```

これ使うと便利。オブジェクトが定義されていること前提

## toStringメソッド

オブジェクトの`toString`メソッドは、オブジェクト自身を文字列化するメソッド。  
`String`コンストラクタ関数を使うことでも文字列化できる。

```javascript
const obj = { key: "value" };
console.log(obj.toString()); // => "[object Object]"
// `String`コンストラクタ関数は`toString`メソッドを呼んでいる
console.log(String(obj)); // => "[object Object]"
```


## [コラム] オブジェクトのプロパティ名は文字列化される

オブジェクトのプロパティへアクセスする際に、指定したプロパティ名は暗黙的に文字列に変換される。ブラケット記法では、オブジェクトをプロパティ名に指定することもできるが、これは意図したようには動作しません。なぜなら、オブジェクトを文字列化すると`"[object Object]"`という文字列になるため。

次のコードでは、`keyObject1`と`keyObject2`をブラケット記法でプロパティ名に指定している。しかし、`keyObject1`と`keyObject2`はどちらも文字列化すると`"[object Object]"`という同じプロパティ名となる。そのため、プロパティは意図せず上書きされている。

```javascript
const obj = {};
const keyObject1 = { a: 1 };
const keyObject2 = { b: 2 };
// どちらも同じプロパティ名（"[object Object]"）に代入している
obj[keyObject1] = "1";
obj[keyObject2] = "2";
console.log(obj); //  { "[object Object]": "2" }
```

唯一の例外として、`Symbol`だけは文字列化されずにオブジェクトのプロパティ名として扱える。

```javascript
const obj = {};
// Symbolは例外的に文字列化されず扱える
const symbolKey1 = Symbol("シンボル1");
const symbolKey2 = Symbol("シンボル2");
obj[symbolKey1] = "1";
obj[symbolKey2] = "2";
console.log(obj[symbolKey1]); // => "1"
console.log(obj[symbolKey2]); // => "2"
```

## オブジェクトの静的メソッド

**静的メソッド（スタティックメソッド）**とは、インスタンスの元となるオブジェクトから呼び出せるメソッドのこと。静的メソッドはインスタンスではなく、クラス自体から直接呼び出すことができる。これにより、インスタンスに依存しない共通の機能を提供します。たとえば、ユーティリティ関数や計算処理をクラスレベルで行う際に使われる。

```javascript
class MyClass {
  static staticMethod() {
    console.log("This is a static method");
  }
}
MyClass.staticMethod(); // クラスから直接呼び出せる
```

### オブジェクトの列挙

オブジェクトのプロパティを列挙する方法として、次の3つの静的メソッドがある。

1. `Object.keys`メソッド: オブジェクトのプロパティ名の配列にして返す
2. `Object.values`メソッド [ES2017]: オブジェクトの値の配列にして返す
3. `Object.entries`メソッド [ES2017]: オブジェクトのプロパティ名と値の配列の配列を返す

```javascript
const obj = {
    "one": 1,
    "two": 2,
    "three": 3
};
// `Object.keys`はキーを列挙した配列を返す
console.log(Object.keys(obj)); // => ["one", "two", "three"]
// `Object.values`は値を列挙した配列を返す
console.log(Object.values(obj)); // => [1, 2, 3]
// `Object.entries`は[キー, 値]の配列を返す
console.log(Object.entries(obj)); // => [["one", 1], ["two", 2], ["three", 3]]
```

`forEach`メソッドを使って、`Object.keys`メソッドで取得したプロパティ名の配列を反復処理することで、オブジェクトのプロパティを列挙できる。

```javascript
const keys = Object.keys(obj);
keys.forEach(key => {
    console.log(key);
});
// 次の値が順番に出力される
// "one"
// "two"
// "three"
```

### オブジェクトのマージと複製

`Object.assign`メソッド [ES2015] は、あるオブジェクトを別のオブジェクトに代入（assign）できる。このメソッドを使うことで、オブジェクトの複製やオブジェクト同士のマージができる。

`Object.assign`メソッドは、`target`オブジェクトに対して、1つ以上の`sources`オブジェクトを指定する。`sources`オブジェクト自身が持つ列挙可能なプロパティを第一引数の`target`オブジェクトに対してコピーする。`Object.assign`メソッドの返り値は、`target`オブジェクトになる。

```javascript
const obj = Object.assign(target, ...sources);
```

### オブジェクトのマージ

空のオブジェクト（`target`）に`objectA`と`objectB`をマージしたものが、`Object.assign`メソッドの返り値となる。

```javascript
const objectA = { a: "a" };
const objectB = { b: "b" };
const merged = Object.assign({}, objectA, objectB);
console.log(merged); // => { a: "a", b: "b" }
```

空のオブジェクトを`target`にすることで、既存のオブジェクトには影響を与えずマージしたオブジェクトを作ることができる。このとき、プロパティ名が重複した場合は、後ろのオブジェクトのプロパティにより上書きされる。

```javascript
// `version`のプロパティ名が被っている
const objectA = { version: "a" };
const objectB = { version: "b" };
const merged = Object.assign({}, objectA, objectB);
// 後ろにある`objectB`のプロパティで上書きされる
console.log(merged); // => { version: "b" }
```

## [ES2018] オブジェクトのspread構文でのマージ

```javascript
const objectA = { a: "a" };
const objectB = { b: "b" };
const merged = {
    ...objectA,
    ...objectB
};
console.log(merged); // => { a: "a", b: "b" }
```

プロパティ名が被った場合の優先順位は、後ろにあるオブジェクトが優先される。

```javascript
// `version`のプロパティ名が被っている
const objectA = { version: "a" };
const objectB = { version: "b" };
const merged = {
    ...objectA,
    ...objectB,
    other: "other"
};
// 後ろにある`objectB`のプロパティで上書きされる
console.log(merged); // => { version: "b", other: "other" }
```

### オブジェクトの複製

新しく空のオブジェクトを作成し、そこへ既存のオブジェクトのプロパティをコピーすれば、それはオブジェクトの複製をしていると言える。

```javascript
// 引数の`obj`を浅く複製したオブジェクトを返す
const shallowClone = (obj) => {
    return Object.assign({}, obj);
};
const obj = { a: "a" };
const cloneObj = shallowClone(obj);
console.log(cloneObj); // => { a: "a" }
// オブジェクトを複製しているので、異なるオブジェクトとなる
console.log(obj === cloneObj); // => false
```

注意点として、`Object.assign`メソッドは`sources`オブジェクトのプロパティを浅くコピー（shallow copy）する点。**shallow copy**とは、`sources`オブジェクトの直下にあるプロパティだけをコピーする。ネストした先のオブジェクトまでも複製するわけではない。

```javascript
const shallowClone = (obj) => {
    return Object.assign({}, obj);
};
const obj = {
    level: 1,
    nest: {
        level: 2
    },
};
const cloneObj = shallowClone(obj);
// `nest`プロパティのオブジェクトは同じオブジェクトのままになる 
console.log(cloneObj.nest === obj.nest); // => true
```