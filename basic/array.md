# 配列

配列は JavaScript の中でもよく使われるオブジェクト。
配列とは値に順序をつけて格納できるオブジェクト。 配列に格納したそれぞれの値のことを要素、それぞれの要素の位置のことをインデックス（index）と呼ぶ。 インデックスは先頭の要素から 0、1、2 のように 0 からはじまる連番となる。
また JavaScript における配列は可変長で、配列を作成後に配列へ要素を追加したり、配列から要素を削除できる。

## 配列の作成とアクセス

配列の作成には配列リテラルを使う。 配列リテラル（`[` と `]`）の中に要素をカンマ（`,`）区切りで記述する。

```js
const emptyArray = [];
const numbers = [1, 2, 3];
// 2 次元配列（配列の配列）
const matrix = [
  ["a", "b"],
  ["c", "d"]
];
```

作成した配列の要素のインデックスとなる数値を、`配列[インデックス]` と記述することで、 そのインデックスの要素を配列から読み取れる。
配列の先頭要素のインデックスは `0` となります。配列のインデックスは、`0` 以上 `2^32 - 1` 未満の整数となる。

```js
const array = ["one", "two", "three"];
console.log(array[0]); // => "one"
```

2 次元配列（配列の配列）からの値の読み取りも同様に `配列[インデックス]` でアクセスでき、`配列[0][0]` は、配列の `0` 番目の要素である配列（`["a", "b"]`）の `0` 番目の要素を読み取ります。

```js
// 2 次元配列（配列の配列）
const matrix = [
  ["a", "b"],
  ["c", "d"]
];
console.log(matrix[0][0]); // => "a"
```

配列の `length` プロパティは配列の要素の数を返す。 そのため、配列の最後の要素へアクセスするには `array.length - 1` をインデックスとして利用できる。

```js
const array = ["one", "two", "three"];
console.log(array.length); // => 3
// 配列の要素数 - 1 が 最後の要素のインデックスとなる
console.log(array[array.length - 1]); // => "three"
```

JavaScript では、存在しないインデックスに対してアクセスした場合に、例外ではなく `undefined` を返す。

```js
const array = ["one", "two", "three"];
// `array`にはインデックスが 100 の要素は定義されていない
console.log(array[100]); // => undefined
```

これは、配列がオブジェクトであることを考えると、次のように存在しないプロパティへアクセスしているのと原理は同じ。 オブジェクトでも、存在しないプロパティへアクセスした場合には `undefined` が返ってくる。

```js
const obj = {
  0: "one",
  1: "two",
  2: "three",
  length: 3
};
// obj["100"]は定義されていないため、undefined が返る
console.log(obj[100]); // => undefined
```

また、配列は常に `length` の数だけ要素を持っているとは限らない。 次のように、配列リテラルでは値を省略することで、未定義の要素を含めることができる。 このような、配列の中に隙間があるものを疎な配列と呼ぶ。 一方、隙間がなくすべてのインデックスに要素がある配列を密な配列と呼ぶ。

```js
// 未定義の箇所が 1 つ含まれる疎な配列
// インデックスが 1 の値を省略しているので、カンマが 2 つ続いていることに注意
const sparseArray = [1, , 3];
console.log(sparseArray.length); // => 3
// 1 番目の要素は存在しないため undefined が返る
console.log(sparseArray[1]); // => undefined
```

## [ES2022] Array.prototype.at

配列の要素にアクセスするには `配列[インデックス]` という構文を使うことを紹介した。 その際に、配列の末尾の要素へアクセスするには、`array[array.length - 1]` という `length` プロパティを使う必要がある。 `array` を 2 回書く必要があるなど、末尾の要素へのアクセスは少し手間が必要になってきている。

この問題を解決するため ES2022 では、相対的なインデックスの値を指定して配列の要素へアクセスできる `Array.prototype.at` メソッドが追加された。 `Array` の `at` メソッドは、`配列[インデックス]` とよく似ていますが、引数には相対的なインデックスの値を引数として渡せる。 `.at(0)` や `.at(1)` などのように `0` 以上のインデックスを渡した場合は、`配列[インデックス]` と同じく指定した位置の要素へアクセスできる。 一方で、`.at(-1)` のようにマイナスのインデックスを渡した場合は、末尾から数えた位置の要素へアクセスできる。

```js
const array = ["a", "b", "c"];
//
console.log(array.at(0)); // => "a"
console.log(array.at(1)); // => "b"
// 後ろから 1 つ目の要素にアクセス
console.log(array.at(-1)); // => "c"
// -1 は、次のように書いた場合と同じ結果
console.log(array[array.length - 1]); // => "c"
```

`配列[インデックス]` のインデックスに `-1` を指定すると、配列オブジェクトの `"-1"` というプロパティ名へのアクセスとなる。 そのため `配列[-1]` と書くと、大抵の場合は `undefined` が返される。

```js
const array = ["a", "b", "c"];
console.log(array[-1]); // => undefined
```

## オブジェクトが配列かどうかを判定する

あるオブジェクトが配列かどうかを判定するには `Array.isArray` メソッドを利用する。 `Array.isArray` メソッドは引数が配列ならば `true` を返す。

```js
const obj = {};
const array = [];
console.log(Array.isArray(obj)); // => false
console.log(Array.isArray(array)); // => true
```

また、`typeof` 演算子では配列かどうかを判定することはできない。 配列もオブジェクトの一種であるため、`typeof` 演算子の結果が `"object"` となるため。

```js
const array = [];
console.log(typeof array); // => "object"
```

## [コラム] [ES2015] TypedArray

JavaScript の配列は可変長のみですが、`TypedArray` という固定長でかつ型つきの配列を扱う別のオブジェクトが存在する。 `TypedArray` はバイナリデータのバッファを示すために使われるデータ型で、WebGL やバイナリを扱う場面で利用される。 文字列や数値などのプリミティブ型の値を直接は利用できないため、通常の配列とは用途や使い勝手が異なる。

また、`TypedArray` は `Array.isArray` のメソッドの結果が `false` となることからも別物と考えてよい。

```js
// TypedArray を作成
const typedArray = new Int8Array(8);
console.log(Array.isArray(typedArray)); // => false
```

そのため、JavaScript で配列といった場合には `Array` を示す。

## [ES2015] 配列と分割代入

配列の指定したインデックスの値を変数として定義し直す場合には、分割代入（Destructuring assignment）が利用できる。
配列の分割代入では、左辺に配列リテラルのような構文で定義したい変数名を書きます。 右辺の配列から対応するインデックスの要素が、左辺で定義した変数に代入される。
次のコードでは、左辺に定義した変数に対して、右辺の配列から対応するインデックスの要素が代入されます。 `first` にはインデックスが 0 の要素、`second` にはインデックスが 1 の要素、`third` にはインデックスが 2 の要素が代入される。

```js
const array = ["one", "two", "three"];
const [first, second, third] = array;
console.log(first); // => "one"
console.log(second); // => "two"
console.log(third); // => "three"
```

## [コラム] undefined の要素と未定義の要素の違い

疎な配列で該当するインデックスに要素がない場合は undefined を返す。
しかし、undefined という値も存在するため、配列に undefined という値がある場合に区別できない。
次のコードでは、undefined という値を要素として定義した密な配列と、要素そのものがない疎な配列を定義している。 どちらも要素にアクセスした結果は undefined となり、区別できていないことがわかる。

```js
// 要素として`undefined`を持つ密な配列
const denseArray = [1, undefined, 3];
// 要素そのものがない疎な配列
const sparseArray = [1, , 3];
console.log(denseArray[1]); // => undefined
console.log(sparseArray[1]); // => undefined
```

この違いを見つける方法として利用できるのが、`Object.hasOwn` 静的メソッド。 `Object.hasOwn` 静的メソッドを使うことで、配列オブジェクトに対して指定したインデックスに要素自体が存在するかを判定できる。

```js
const denseArray = [1, undefined, 3];
const sparseArray = [1, , 3];
// 要素自体は存在し、その値が`undefined`
console.log(Object.hasOwn(denseArray, 1)); // => true
// 要素自体が存在しない
console.log(Object.hasOwn(sparseArray, 1)); // => false
```

## 配列から要素を検索

配列から指定した要素を検索する目的には、 主に次の 3 つがある。

- その要素のインデックスが欲しい場合
- その要素自体が欲しい場合
- その要素が含まれているかという真偽値が欲しい場合

配列にはそれぞれに対応したメソッドが用意されているため、目的別に見ていく。

## インデックスを取得

指定した要素が配列のどの位置にあるかを知りたい場合、Array の indexOf メソッドや findIndex メソッド[ES2015]を利用する。 要素の位置のことを**インデックス（index）**と呼ぶため、メソッド名にも index という名前が入っている。

次のコードでは、Array の indexOf メソッドを利用して、配列の中から`"JavaScript"`という文字列のインデックスを取得している。 indexOf メソッドは引数と厳密等価演算子（===）で一致する要素を先頭から検索して該当する要素のインデックスを返し、該当する要素がない場合は-1 を返す。 indexOf メソッドには対となる lastIndexOf メソッドがあり、lastIndexOf メソッドでは末尾から検索した結果が得られる。

```js
const array = ["Java", "JavaScript", "Ruby", "JavaScript"];
// 先頭から探索して最初に見つかった"JavaScript"のインデックスを取得
const indexOfJS = array.indexOf("JavaScript");
console.log(indexOfJS); // => 1
// 末尾から探索して最初に見つかった"JavaScript"のインデックスを取得
const lastIndexOfJS = array.lastIndexOf("JavaScript");
console.log(lastIndexOfJS); // => 3
console.log(array[indexOfJS]); // => "JavaScript"
console.log(array[lastIndexOfJS]); // => "JavaScript"
// "JS" という要素はないため `-1` が返される
console.log(array.indexOf("JS")); // => -1
console.log(array.lastIndexOf("JS")); // => -1
```

indexOf メソッドは配列からプリミティブな要素を発見できるが、オブジェクトは持っているプロパティが同じでも別オブジェクトだと異なるものとして扱われる。 次のコードを見ると、同じプロパティを持つ異なるオブジェクトは、indexOf メソッドでは見つけることができない。 これは、異なる参照を持つオブジェクト同士は===で比較しても一致しないため。

```js
const obj = { key: "value" };
const array = ["A", "B", obj];
console.log(array.indexOf({ key: "value" })); // => -1
// リテラルは新しいオブジェクトを作るため、異なるオブジェクトだと判定される
console.log(obj === { key: "value" }); // => false
// 等価のオブジェクトを検索してインデックスを返す
console.log(array.indexOf(obj)); // => 2
```

findIndex メソッドの引数には配列の各要素をテストする関数をコールバック関数として渡します。 indexOf メソッドとは異なり、テストする処理を自由に書けます。 これにより、プロパティの値が同じ要素を配列から見つけて、その要素のインデックスを得ることができます。

```js
// colorプロパティを持つオブジェクトの配列
const colors = [{ color: "red" }, { color: "green" }, { color: "blue" }];
// `color`プロパティが"blue"のオブジェクトのインデックスを取得
const indexOfBlue = colors.findIndex((obj) => {
  return obj.color === "blue";
});
console.log(indexOfBlue); // => 2
console.log(colors[indexOfBlue]); // => { "color": "blue" }
```

Array の findIndex メソッドにも対となる findLastIndex メソッド[ES2023]があり、findLastIndex メソッドは末尾から検索した結果が得られる。

```js
// dateとcountプロパティを持つオブジェクトの配列
const records = [
  { date: "2020/12/1", count: 5 },
  { date: "2020/12/2", count: 11 },
  { date: "2020/12/3", count: 9 },
  { date: "2020/12/4", count: 12 },
  { date: "2020/12/5", count: 3 }
];
// 10より大きい`count`プロパティを持つ最初のオブジェクトのインデックスを取得
const firstRecordIndex = records.findIndex((record) => {
  return record.count > 10;
});
// 10より大きい`count`プロパティを持つ最後のオブジェクトのインデックスを取得
const lastRecordIndex = records.findLastIndex((record) => {
  return record.count > 10;
});
console.log(firstRecordIndex); // => 1
console.log(records[firstRecordIndex]); // => { date: "2020/12/2", count: 11 }
console.log(lastRecordIndex); // => 3
console.log(records[lastRecordIndex]); // => { date: "2020/12/4", count: 12 }
```

## 条件に一致する要素を取得

より明確に要素自体が欲しいということを表現するには、Array の `find メソッド[ES2015]`が使える。 find メソッドには、findIndex メソッドと同様にテストする関数をコールバック関数として渡す。 find メソッドの返り値は、要素そのものとなり、要素が存在しない場合は undefined を返す。

```js
// colorプロパティを持つオブジェクトの配列
const colors = [{ color: "red" }, { color: "green" }, { color: "blue" }];
// `color`プロパティが"blue"のオブジェクトを取得
const blueColor = colors.find((obj) => {
  return obj.color === "blue";
});
console.log(blueColor); // => { "color": "blue" }
// 該当する要素がない場合は`undefined`を返す
const whiteColor = colors.find((obj) => {
  return obj.color === "white";
});
console.log(whiteColor); // => undefined
```

find メソッドにも対となる findLast メソッド[ES2023]があり、findLast メソッドは末尾から検索した結果が得られる。以下に、find は条件に一致した最初の要素を返すが、`findLast`は最後の要素を返す。

```js
// dateとcountプロパティを持つオブジェクトの配列
const records = [
  { date: "2020/12/1", count: 5 },
  { date: "2020/12/2", count: 11 },
  { date: "2020/12/3", count: 9 },
  { date: "2020/12/4", count: 12 },
  { date: "2020/12/5", count: 3 }
];
// 10より大きい`count`プロパティを持つ最初のオブジェクトを取得
const firstRecord = records.find((record) => {
  return record.count > 10;
});
// 10より大きい`count`プロパティを持つ最後のオブジェクトを取得
const lastRecord = records.findLast((record) => {
  return record.count > 10;
});
console.log(firstRecord); // => { date: "2020/12/2", count: 11 }
console.log(lastRecord); // => { date: "2020/12/4", count: 12 }
```

## 指定範囲の要素を取得

配列から指定範囲の要素を取り出す方法として Array の`slice`メソッドが利用できる。 `slice`メソッドは、第一引数の開始位置から第二引数の終了位置（終了位置の要素は含まない）までの範囲を取り出した新しい配列を返す。 第二引数は省略でき、省略した場合は配列の末尾の要素まで含んだ新しい配列を返す。

```js
const array = ["A", "B", "C", "D", "E"];
// インデックス1から4まで(4の要素は含まない)の範囲を取り出す
console.log(array.slice(1, 4)); // => ["B", "C", "D"]
// 第二引数を省略した場合は、第一引数から末尾の要素までを取り出す
console.log(array.slice(1)); // => ["B", "C", "D", "E"]
// マイナスを指定すると後ろから数えた位置となる
console.log(array.slice(-1)); // => ["E"]
// 第一引数と第二引数が同じ場合は、空の配列を返す
console.log(array.slice(1, 1)); // => []
// 第一引数 > 第二引数の場合、常に空配列を返す
console.log(array.slice(4, 1)); // => []
```

## 真偽値を取得

インデックスや要素が取得できれば、その要素は配列に含まれているということはわかる。

しかし、指定した要素が含まれているかだけを知りたい場合に、 Array の findIndex メソッドや find メソッドは過剰な機能を持っています。 そのコードを読んだ人には、取得したインデックスや要素を何に使うのかが明確ではない。

以下のコードは、Array の indexOf メソッドを利用し、該当する要素が含まれているかを判定している。 indexOf メソッドの結果を indexOfJS に代入しているが、含まれているかを判定する以外には利用していない。 コードを隅々まで読まないといけないため、意図が明確ではなくコードの読みづらさにつながる。

```js
const array = ["Java", "JavaScript", "Ruby"];
// `indexOf`メソッドは含まれていないときのみ`-1`を返すことを利用
const indexOfJS = array.indexOf("JavaScript");
if (indexOfJS !== -1) {
  console.log("配列にJavaScriptが含まれている");
  // ... いろいろな処理 ...
  // `indexOfJS`は、含まれているのかの判定以外には利用してない
}
```

ES2016 で導入された Array の includes メソッドは配列に指定要素が含まれているかを判定し、includes メソッドは真偽値を返すので、indexOf メソッドを使った場合に比べて意図が明確になる。 前述のコードでは次のように includes メソッドを使うと良い。

```js
const array = ["Java", "JavaScript", "Ruby"];
// `includes`は含まれているなら`true`を返す
if (array.includes("JavaScript")) {
  console.log("配列にJavaScriptが含まれている");
}
```

includes メソッドは、indexOf メソッドと同様、異なるオブジェクトだが値が同じものを見つけたい場合には利用できない。 Array の find メソッドのようにテストするコールバック関数を利用して真偽値を得るには、Array の some メソッドを利用する。

```js
// colorプロパティを持つオブジェクトの配列
const colors = [{ color: "red" }, { color: "green" }, { color: "blue" }];
// `color`プロパティが"blue"のオブジェクトがあるかどうか
const isIncludedBlueColor = colors.some((obj) => {
  return obj.color === "blue";
});
console.log(isIncludedBlueColor); // => true
```

## 追加と削除

配列は可変長であるため、作成後の配列に対して要素を追加、削除できる。
要素を配列の末尾へ追加するには Array の push が利用できます。 一方、末尾から要素を削除するには Array の pop が利用できます。

```js
const array = ["A", "B", "C"];
array.push("D"); // "D"を末尾に追加
console.log(array); // => ["A", "B", "C", "D"]
const poppedItem = array.pop(); // 最末尾の要素を削除し、その要素を返す
console.log(poppedItem); // => "D"
console.log(array); // => ["A", "B", "C"]
```

要素を配列の先頭へ追加するには Array の unshift が利用できる。 一方、配列の先頭から要素を削除するには Array の shift が利用する。

```js
const array = ["A", "B", "C"];
array.unshift("S"); // "S"を先頭に追加
console.log(array); // => ["S", "A", "B", "C"]
const shiftedItem = array.shift(); // 先頭の要素を削除
console.log(shiftedItem); // => "S"
console.log(array); // => ["A", "B", "C"]
```

## 配列同士を結合

```js
const array = ["A", "B", "C"];
const newArray = array.concat(["D", "E"]);
console.log(newArray); // => ["A", "B", "C", "D", "E"]
```

配列だけではなく任意の値を要素として結合できる。

```js
const array = ["A", "B", "C"];
const newArray = array.concat("新しい要素");
console.log(newArray); // => ["A", "B", "C", "新しい要素"]
```

## [ES2015] 配列の展開

...(Spread 構文)を使うことで、配列リテラル中に既存の配列を展開できます。

次のコードでは、配列リテラルの末尾に配列を展開している。 これは、Array の concat メソッドで配列同士を結合するのと同じ結果になる。

```js
const array = ["A", "B", "C"];
// Spread構文を使った場合
const newArray = ["X", "Y", "Z", ...array];
// concatメソッドの場合
const newArrayConcat = ["X", "Y", "Z"].concat(array);
console.log(newArray); // => ["X", "Y", "Z", "A", "B", "C"]
console.log(newArrayConcat); // => ["X", "Y", "Z", "A", "B", "C"]
```

## [ES2019] 配列をフラット化

Array の flat メソッド[ES2019]を使うことで、多次元配列をフラットな配列に変換できる。 引数を指定しなかった場合は 1 段階のみのフラット化だが、引数に渡す数値でフラット化する深さを指定できる。 配列をすべてフラット化する場合には、無限を意味する Infinity を値として渡すことで実現できる。

```js
const array = [[["A"], "B"], "C"];
// 引数なしは1を指定した場合と同じ
console.log(array.flat()); // => [["A"], "B", "C"]
console.log(array.flat(1)); // => [["A"], "B", "C"]
console.log(array.flat(2)); // => ["A", "B", "C"]
// すべてをフラット化するにはInfinityを渡す
console.log(array.flat(Infinity)); // => ["A", "B", "C"]
```

また、Array の flat メソッドは必ず新しい配列を作成して返すメソッド。 そのため、これ以上フラット化できない配列をフラット化しても、同じ要素を持つ新しい配列を返す。

```js
const array = ["A", "B", "C"];
console.log(array.flat()); // => ["A", "B", "C"]
```

## 配列から要素を削除

### Array.prototype.splice

配列の先頭や末尾の要素を削除する場合は Array の shift メソッドや pop メソッドで行える。 しかし、配列の任意のインデックスの要素を削除できない。 配列の任意のインデックスの要素を削除するには Array の splice メソッドを利用する。

splice メソッドを利用すると、削除した要素を自動で詰めることができ、splice メソッドは指定したインデックスから、指定した数だけ要素を取り除き、必要ならば要素を同時に追加する。

```js
const array = [];
array.splice(インデックス, 削除する要素数);
// 削除と同時に要素の追加もできる
array.splice(インデックス, 削除する要素数, ...追加する要素);
```

たとえば、配列のインデックスが 1 の要素を削除するには、インデックス 1 から 1 つの要素を削除するという指定をする必要がある。 このとき、削除した要素は自動で詰められるため、疎な配列にはならない。

```js
const array = ["a", "b", "c"];
// 1 番目から 1 つの要素("b")を削除
array.splice(1, 1);
console.log(array); // => ["a", "c"]
console.log(array.length); // => 2
console.log(array[1]); // => "c"
// すべて削除
array.splice(0, array.length);
console.log(array.length); // => 0
```

## length プロパティへの代入

配列のすべての要素を削除することは Array の splice で行えるが、 配列の length プロパティへの代入を利用した方法もある。

```js
const array = [1, 2, 3];
array.length = 0; // 配列を空にする
console.log(array); // => []
```

## 空の配列を代入

最後に、その配列の要素を削除するのではなく、新しい空の配列を変数へ代入する方法。 次のコードでは、array 変数に空の配列を代入することで、array に空の配列を参照させられる。

```js
let array = [1, 2, 3];
console.log(array.length); // => 3
// 新しい配列で変数を上書き
array = [];
console.log(array.length); // => 0
```

`const`で宣言した配列の場合は変数に対して再代入できないため、この手法は使えません。そのため、再代入をしたい場合は let または`var`で変数宣言をする必要がある。

```js
const array = [1, 2, 3];
console.log(array.length); // => 3
// `const`で宣言された変数には再代入できない
array = []; // TypeError: invalid assignment to const `array' が発生
```

## 破壊的なメソッドと非破壊的なメソッド

配列を変更するメソッドには、破壊的なメソッドと非破壊的メソッドがあり,この破壊的なメソッドと非破壊的メソッドの違いを知ることは、重要。
破壊的なメソッド（Mutable Method）とは、配列オブジェクトそのものを変更し、変更した配列または変更箇所を返すメソッド。 非破壊的メソッド（Immutable Method）とは、配列オブジェクトのコピーを作成してから変更し、そのコピーした配列を返すメソッド。
破壊的なメソッドの例として、配列に要素を追加する Array の push メソッドがある。 push メソッドは、myArray の配列そのものへ要素を追加している。 その結果 myArray 変数の参照する配列が変更されるため破壊的なメソッド。

```js
const myArray = ["A", "B", "C"];
const result = myArray.push("D");
// `push`の返り値は配列ではなく、追加後の配列のlength
console.log(result); // => 4
// `myArray`が参照する配列そのものが変更されている
console.log(myArray); // => ["A", "B", "C", "D"]
```

非破壊的なメソッドの例として、配列に要素を結合する Array の concat メソッドがある。 concat メソッドは、myArray をコピーした配列に対して要素を結合し、その配列を返す。 myArray 変数の参照する配列は変更されないため非破壊的なメソッドである。

```js
const myArray = ["A", "B", "C"];
// `concat`の返り値は結合済みの新しい配列
const newArray = myArray.concat("D");
console.log(newArray); // => ["A", "B", "C", "D"]
// `myArray`は変更されていない
console.log(myArray); // => ["A", "B", "C"]
// `newArray`と`myArray`は異なる配列オブジェクト
console.log(myArray === newArray); // => false
```

JavaScript において破壊的なメソッドと非破壊的メソッドを名前から見分けるのは難しいという問題がある。また、配列を返す破壊的なメソッドもあるため、返り値からも判別できません。 たとえば、Array の sort メソッドは返り値がソート済みの配列ですが破壊的メソッド。
次の表で紹介するメソッドは破壊的なメソッド。

| メソッド名                            | 返り値                     |
| ------------------------------------- | -------------------------- |
| `Array.prototype.pop`                 | 配列の末尾の値             |
| `Array.prototype.push`                | 変更後の配列の `length`    |
| `Array.prototype.splice`              | 取り除かれた要素を含む配列 |
| `Array.prototype.reverse`             | 反転した配列               |
| `Array.prototype.shift`               | 配列の先頭の値             |
| `Array.prototype.sort`                | ソートした配列             |
| `Array.prototype.unshift`             | 変更後の配列の `length`    |
| `Array.prototype.copyWithin` [ES2015] | 変更後の配列               |
| `Array.prototype.fill` [ES2015]       | 変更後の配列               |

以下のように破壊的なメソッドである Array の splice メソッドで要素を削除すると、引数として受け取った配列にも影響を与える。

```js
// `array`の`index`番目の要素を削除した配列を返す関数
// 引数の`array`は破壊的に変更される
function removeAtIndex(array, index) {
  array.splice(index, 1);
  return array;
}
const array = ["A", "B", "C"];
// `array`から1番目の要素を削除した配列を取得
const newArray = removeAtIndex(array, 1);
console.log(newArray); // => ["A", "C"]
// `array`自体にも影響を与える
console.log(array); // => ["A", "C"]
```

一方、非破壊的メソッドは配列のコピーを作成するため、元々の配列に対して影響はなく、この removeAtIndex 関数を非破壊的なものにするには、受け取った配列をコピーしてから変更を加える必要がある。

JavaScript には copy メソッドそのものは存在しませんが、配列をコピーする方法として Array の slice メソッドと concat メソッドが利用されている。 slice メソッドと concat メソッドは引数なしで呼び出すと、その配列のコピーを返す。

```js
const myArray = ["A", "B", "C"];
// `slice`は`myArray`のコピーを返す - `myArray.concat()`でも同じ
const copiedArray = myArray.slice();
myArray.push("D");
console.log(myArray); // => ["A", "B", "C", "D"]
// `array`のコピーである`copiedArray`には影響がない
console.log(copiedArray); // => ["A", "B", "C"]
// コピーであるため参照は異なる
console.log(copiedArray === myArray); // => false
```

コピーした配列に変更を加えることで、removeAtIndex 関数を非破壊的な関数として実装できます。 非破壊的であれば引数の配列への副作用がないので、注意させるようなコメントは不要です。

```js
// `array`の`index`番目の要素を削除した配列を返す関数
function removeAtIndex(array, index) {
  // コピーを作成してから変更する
  const copiedArray = array.slice();
  copiedArray.splice(index, 1);
  return copiedArray;
}
const array = ["A", "B", "C"];
// `array`から1番目の要素を削除した配列を取得
const newArray = removeAtIndex(array, 1);
console.log(newArray); // => ["A", "C"]
// 元の`array`には影響がない
console.log(array); // => ["A", "B", "C"]
```

このように JavaScript の配列には破壊的なメソッドと非破壊的メソッドが混在してる。 名前からも区別することが難しく、副作用を避けるためにコピーを作ってから破壊的メソッドを使うというパターンが利用されていた。
今まで、破壊的なメソッドしかなかった、`splice、reverse、sort` に対して、 非破壊的なバージョンである`toSpliced`、`toReversed`、`toSorted` が追加された。

これらの to から始まる非破壊的メソッドが受け取る引数は破壊的なメソッドと同じですが、非破壊的に変更した配列を返す点が異なる。 次のコードの`toSplicedメソッド`は、配列を複製してから変更するため、元々の配列である`array`には影響を与えていないことがわかる。

```js
const array = ["A", "B", "C"];
// `toSpliced`は`array`を複製してから変更する
const newArray = array.toSpliced(1, 1);
console.log(newArray); // => ["A", "C"]
// コピー元の`array`には影響がない
console.log(array); // => ["A", "B", "C"]
```

先ほど`removeAtIndex`関数の実装では、`slice`メソッドで配列をコピーしてから`splice`メソッドを呼び出していた。 次のコードでは、`toSpliced`メソッドを使うことで、より簡潔に非破壊的な`removeAtIndex関数`を実装している。

```js
// `array`の`index`番目の要素を削除した配列を返す関数
function removeAtIndex(array, index) {
  // コピーを作成してから変更する
  return array.toSpliced(index, 1);
}
const array = ["A", "B", "C"];
// `array`から1番目の要素を削除した配列を取得
const newArray = removeAtIndex(array, 1);
console.log(newArray); // => ["A", "C"]
// 元の`array`には影響がない
console.log(array); // => ["A", "B", "C"]
```

`array[index] = value`の代入処理は、元々の配列を変更する破壊的な処理で、これに対して`with`メソッドは、配列を複製してから指定したインデックスの要素を変更した配列を返す非破壊的なメソッド。

```js
const array = ["A", "B", "C"];
// `array`の1番目の要素を変更した配列を返す
const newArray = array.with(1, "B2");
console.log(newArray); // => ["A", "B2", "C"]
```

以下は破壊的な方法に適応する非破壊的な方法のまとめ

| 破壊的な方法                          | 非破壊な方法                                    |
| ------------------------------------- | ----------------------------------------------- |
| `array[index] = item`                 | `Array.prototype.with` [ES2023]                 |
| `Array.prototype.pop`                 | `array.slice(0, -1)` と `array.at(-1)` [ES2022] |
| `Array.prototype.push`                | `[...array, item]` [ES2015]                     |
| `Array.prototype.splice`              | `Array.prototype.toSpliced` [ES2023]            |
| `Array.prototype.reverse`             | `Array.prototype.toReversed` [ES2023]           |
| `Array.prototype.sort`                | `Array.prototype.toSorted` [ES2023]             |
| `Array.prototype.shift`               | `array.slice(1)` と `array.at(0)` [ES2022]      |
| `Array.prototype.unshift`             | `[item, ...array]` [ES2015]                     |
| `Array.prototype.copyWithin` [ES2015] | なし                                            |
| `Array.prototype.fill` [ES2015]       | なし                                            |

破壊的なメソッドは、シンプルだが元の配列も変更してしまうため、意図しない副作用が発生しバグの原因となる可能性がある。 非破壊的なメソッドは、使い分けが必要ですが元の配列を変更せずに新しい配列を返すため、副作用が発生することはないため、まず非破壊的な方法で書けるかを検討し、そうではない場合に破壊的な方法を利用するとよい。

## 配列を反復処理するメソッド

反復処理の中でもよく利用されるのが`Array`の`forEach、map、filter、reduce`メソッドです。 どのメソッドも共通して引数にコールバック関数を受け取るため高階関数と呼ばれます。

### Array.prototype.forEach

Array の forEach のメソットは配列の要素の先頭から順番にコールバック関数へわたし、反復処理を行うメソット。

```js
const array = [1, 2, 3];
array.forEach((currentValue, index, array) => {
  console.log(currentValue, index, array);
});
// コンソールの出力
// 1, 0, [1, 2, 3]
// 2, 1, [1, 2, 3]
// 3, 2, [1, 2, 3]
```

### Array.prototype.map

`Array`の`map`メソッドは配列の要素を順番にコールバック関数へ渡し、コールバック関数が返した値から新しい配列を返す非破壊的なメソッド。 配列の各要素を加工したい場合に利用する。

次のようにコールバック関数には要素, インデックス, 配列が引数として渡され、配列要素の先頭から順番に反復処理する。 `map`メソッドの返り値は、それぞれのコールバック関数が返した値を集めた新しい配列。

```js
const array = [1, 2, 3];
// 各要素に10を乗算した新しい配列を作成する
const newArray = array.map((currentValue, index, array) => {
  return currentValue * 10;
});
console.log(newArray); // => [10, 20, 30]
// 元の配列とは異なるインスタンス
console.log(array === newArray); // => false
```

### Array.prototype.filter

Array の filter メソッドは配列の要素を順番にコールバック関数へ渡し、コールバック関数が true を返した要素だけを集めた新しい配列を返す非破壊的なメソッド。 配列から不要な要素を取り除いた配列を作成したい場合に利用する。

```js
const array = [1, 2, 3];
// 奇数の値を持つ要素だけを集めた配列を返す
const newArray = array.filter((currentValue, index, array) => {
  return currentValue % 2 === 1;
});
console.log(newArray); // => [1, 3]
// 元の配列とは異なるインスタンス
console.log(array === newArray); // => false
```

### Array.prototype.reduce

`Array`の`reduce`メソッドは累積値（アキュムレータ）と配列の要素を順番にコールバック関数へ渡し、1 つの累積値を返す。 配列から配列以外を含む任意の値を作成したい場合に利用する。

反復処理のメソッドとは異なり、コールバック関数には累積値, 要素, インデックス, 配列を引数として渡す。 `reduce`メソッドの第二引数には累積値の初期値となる値を渡せる。

```js
const array = [1, 2, 3];
// すべての要素を加算した値を返す
// accumulatorの初期値は`0`
const totalValue = array.reduce((accumulator, currentValue, index, array) => {
  return accumulator + currentValue;
}, 0);
// 0 + 1 + 2 + 3という式の結果が返り値になる
console.log(totalValue); // => 6
```

`reduce`メソッドに渡したコールバック関数は配列の要素数である 3 回呼び出され、それぞれ次のような結果になる。
| | accumulator | currentValue | return した値 |
|-------------|--------------|--------------|--------------|
| 1 回目の呼び出し | 0 | 1 | 0 + 1 |
| 2 回目の呼び出し | 1 | 2 | 1 + 2 |
| 3 回目の呼び出し | 3 | 3 | 3 + 3 |

`reduce`メソッドは、配列から配列以外のデータ型の値を作成できる特徴がある。 また、`reduce`メソッドでは、配列から直接 Number 型の値を返せるため、totalValue という変数を再代入できない const で宣言していた。

配列の数値の合計を forEach メソッドなど反復処理で計算すると、次のコードのように`totalValue`という変数は再代入ができる`let`で宣言する必要がある。

```js
const array = [1, 2, 3];
// 初期値は`0`
let totalValue = 0;
array.forEach((currentValue) => {
  totalValue += currentValue;
});
console.log(totalValue); // => 6
```

`let` で宣言した変数は再代入が可能なため、意図しない箇所で変数の値が変更され、バグの原因となることがある。 そのため、できる限り変数を `const` で宣言したい場合には reduce メソッドは有用。 一方で、`reduce` メソッドは可読性があまりよくないため、コードの意図が伝わりにくいというデメリットもある。

`reduce` メソッドには利点と可読性のトレードオフがあるが、利用する場合は `reduce` メソッドを扱う処理を関数にするといった処理の意図がわかるように工夫をする必要がある。

```js
const array = [1, 2, 3];
function sum(array) {
  return array.reduce((accumulator, currentValue) => {
    return accumulator + currentValue;
  }, 0);
}
console.log(sum(array)); // => 6
```

### [ES2024] Object.groupBy 静的メソッド

`Array.prototype.reduce`メソッドを使うことで、配列から数値やオブジェクトなど任意の値を作成できる。

先ほどは配列の合計の数値を計算する例でしたが、配列からオブジェクトを作成することもできる。 配列からオブジェクトを作成したいユースケースとして、配列の要素を条件によってグループ分けしたいケースがある。 たとえば、数値からなる配列の要素を奇数と偶数の配列に分けたい場合など。

```js
const array = [1, 2, 3, 4, 5];
const grouped = array.reduce((accumulator, currentValue) => {
  // 2で割った余りが0なら偶数(even)、そうでないなら奇数(odd)
  const key = currentValue % 2 === 0 ? "even" : "odd";
  if (!accumulator[key]) {
    accumulator[key] = [];
  }
  // グループ分けしたキーの配列に要素を追加
  accumulator[key].push(currentValue);
  return accumulator;
}, {});
console.log(grouped.even); // => [2, 4]
console.log(grouped.odd); // => [1, 3, 5]
```

`reduce`メソッドは使い方がやや複雜であるため、可能なら避けたほうが読みやすいコードとなりやすい。 `Object.groupBy`静的メソッドが追加され、配列からグループ分けしたオブジェクトを作成できるようになっている。

`Object.groupBy`静的メソッド 1 には、第一引数に配列などの iterable オブジェクト、第二引数にグループ分けの条件を返すコールバック関数を渡します。 第二引数のコールバック関数が返した値をキーとして、配列の要素をグループ分けしたオブジェクトが作成されます。

```js
zconst array = [1, 2, 3, 4, 5];
const grouped = Object.groupBy(array, (currentValue) => {
    // currentValueが偶数なら"even"、そうでないなら"odd"の配列に追加される
    return currentValue % 2 === 0 ? "even" : "odd";
});
console.log(grouped.even); // => [2, 4]
console.log(grouped.odd); // => [1, 3, 5]
```

## [コラム] Array-like オブジェクト

配列のように扱えるが配列ではないオブジェクトのことを、Array-like オブジェクトと呼ぶ。 Array-like オブジェクトとは配列のようにインデックスにアクセスでき、配列のように length プロパティも持っている。しかし、配列のインスタンスではないため、Array のプロトタイプメソッドを持っていないオブジェクトのこと。

| 機能                                                   | Array-like オブジェクト | 配列       |
| ------------------------------------------------------ | ----------------------- | ---------- |
| インデックスアクセス（`array[0]`）                     | できる                  | できる     |
| 長さ（`array.length`）                                 | 持っている              | 持っている |
| Array のプロトタイプメソッド（`forEach` メソッドなど） | 持っていない場合もある  | 持っている |

`Array-like`オブジェクトの例として arguments がある。 `arguments`オブジェクトは、`function`で宣言した関数の中から参照できる変数。 `arguments`オブジェクトには関数の引数に渡された値が順番に格納されていて、配列のように引数へアクセスできる。

```js
function myFunc() {
  console.log(arguments[0]); // => "a"
  console.log(arguments[1]); // => "b"
  console.log(arguments[2]); // => "c"
  // 配列ではないため、配列のメソッドは持っていない
  console.log(typeof arguments.forEach); // => "undefined"
}
myFunc("a", "b", "c");
```

`Array-like`オブジェクトか配列なのかを判別するには`Array.isArray`メソッドを利用する。 `Array-like`オブジェクトは配列ではないので結果は常に false となる。

```js
function myFunc() {
  console.log(Array.isArray([1, 2, 3])); // => true
  console.log(Array.isArray(arguments)); // => false
}
myFunc("a", "b", "c");
```

`Array-like`オブジェクトは配列のようで配列ではないというもどかしさを持つオブジェクト。`Array.from`静的メソッド[ES2015]を使うことで`Array-like`オブジェクトを配列に変換して扱うことができる。一度配列に変換してしまえば`Array`メソッドも利用できる。

```js
function myFunc() {
  // Array-likeオブジェクトを配列へ変換
  const argumentsArray = Array.from(arguments);
  console.log(Array.isArray(argumentsArray)); // => true
  // 配列のメソッドを利用できる
  argumentsArray.forEach((arg) => {
    console.log(arg);
  });
}
myFunc("a", "b", "c");
```

## メソッドチェーンと高階関数

配列で頻出するパターンとしてメソッドチェーンがある。 メソッドチェーンとは、メソッドを呼び出した返り値に対してさらにメソッド呼び出しをするパターンのことを言う。

```js
const array = ["a"].concat("b").concat("c");
console.log(array); // => ["a", "b", "c"]
```

concat メソッドの返り値は結合した新しい配列で先ほどのメソッドチェーンでは、その新しい配列に対してさらに concat メソッドで値を結合している。

```js
// メソッドチェーンを分解した例
// 一時的な`abArray`という変数が増えている
const abArray = ["a"].concat("b");
console.log(abArray); // => ["a", "b"]
const abcArray = abArray.concat("c");
console.log(abcArray); // => ["a", "b", "c"]
```

メソッドチェーンを利用することで処理の見た目を簡潔にでき、メソッドチェーンを利用した場合も最終的な処理結果は同じだが、途中の一時的な変数を省略できる。abArray という一時的な変数をメソッドチェーンでは省略できる。

メソッドチェーンは配列に限ったものではありませんが、配列では頻出するパターンである。なぜなら、配列に含まれるデータを表示する際には、最終的に文字列や数値など別のデータへ加工することがほとんどである。配列には配列を返す高階関数が多く実装されているため、配列を柔軟に加工できる。

次のコードでは、ECMAScript のバージョン名と発行年数が定義された ECMAScriptVersions という配列が定義されている。この配列から 2000 年以前に発行された ECMAScript のバージョン名の一覧を取り出すことを考えてみると、目的の一覧を取り出すには「2000 年以前のデータに絞り込む」と「データから name を取り出す」という 2 つの加工処理を組み合わせる必要がある。

この 2 つの加工処理は`Array`の`filter`メソッドと`map`メソッドで実現できます。`filter`メソッドで配列から 2000 年以前というルールで絞り込み,`map`メソッドでそれぞれの要素から`name` プロパティを取り出せる。 どちらのメソッドも配列を返すのでメソッドチェーンで処理をつなげられる。

```js
// ECMAScriptのバージョン名と発行年
const ECMAScriptVersions = [
  { name: "ECMAScript 1", year: 1997 },
  { name: "ECMAScript 2", year: 1998 },
  { name: "ECMAScript 3", year: 1999 },
  { name: "ECMAScript 5", year: 2009 },
  { name: "ECMAScript 5.1", year: 2011 },
  { name: "ECMAScript 2015", year: 2015 },
  { name: "ECMAScript 2016", year: 2016 },
  { name: "ECMAScript 2017", year: 2017 }
];
// メソッドチェーンで必要な加工処理を並べている
const versionNames = ECMAScriptVersions
  // 2000年以下のデータに絞り込み
  .filter((ECMAScript) => ECMAScript.year <= 2000)
  // それぞれの要素から`name`プロパティを取り出す
  .map((ECMAScript) => ECMAScript.name);
console.log(versionNames); // => ["ECMAScript 1", "ECMAScript 2", "ECMAScript 3"]
```

メソッドチェーンを使うことで複数の処理からなるものをひとつのまとまった処理のように見せることができる。長過ぎるメソッドチェーンは長過ぎる関数と同じように読みにくくなるが、適度な単位のメソッドチェーンは処理をスッキリ見せるパターンとして利用されている。
