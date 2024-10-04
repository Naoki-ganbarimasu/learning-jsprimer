# 文字列とUnicode

JavaScript は文字コードとして Unicode を採用し、エンコード方式として UTF-16 を採用している。 この UTF-16 を採用しているのは、あくまで JavaScript の内部で文字列を扱う際の文字コード（内部コード）である。 そのため、コードを書いたファイル自体の文字コード（外部コード）は、UTF-8 のように UTF-16 以外の文字コードであっても問題ない。

## Code Point

Unicode はすべての文字（制御文字などの画面に表示されない文字も含む）に対して ID を定義する目的で策定されている仕様で、この「文字」に対する「一意の ID」のことを Code Point（符号位置）と呼ぶ。
String の`codePointAt`メソッドや`String.fromCodePoint`静的メソッドを使うことで、文字列と Code Point を相互変換できる。

```js
// 文字列"あ"のCode Pointを取得
console.log("あ".codePointAt(0)); // => 12354
```

一方の`String.fromCodePoint`メソッドは、指定した Code Point に対応する文字を返す。

```js
// Code Pointが`12354`の文字を取得する
console.log(String.fromCodePoint(12354)); // => "あ"
// `12354`を16進数リテラルで表記しても同じ結果
console.log(String.fromCodePoint(0x3042)); // => "あ"
```

また、文字列リテラル中には Unicode エスケープシーケンスで、直接 Code Point を書くこともできる。

```js
// "あ"のCode Pointは12354
const codePointOfあ = "あ".codePointAt(0);
// 12354の16進数表現は"3042"
const hexOfあ = codePointOfあ.toString(16);
console.log(hexOfあ); // => "3042"
// Unicodeエスケープで"あ"を表現できる
console.log("\u{3042}"); // => "あ"
```

## Code Point と Code Unit の違い

JavaScript の文字列の構成要素は UTF-16 で変換された Code Unit（符号単位）である。

```js
// 文字列をCode Unit(16進数)の配列にして返す
function convertCodeUnits(str) {
  const codeUnits = [];
  for (let i = 0; i < str.length; i++) {
    codeUnits.push(str.charCodeAt(i).toString(16));
  }
  return codeUnits;
}
// 文字列をCode Point(16進数)の配列にして返す
function convertCodePoints(str) {
  return Array.from(str).map((char) => {
    return char.codePointAt(0).toString(16);
  });
}

const str = "アオイ";
const codeUnits = convertCodeUnits(str);
console.log(codeUnits); // => ["30a2", "30aa", "30a4"]
const codePoints = convertCodePoints(str);
console.log(codePoints); // => ["30a2", "30aa", "30a4"]
```

| インデックス          | 0      | 1      | 2      |
| --------------------- | ------ | ------ | ------ |
| 文字列                | ア     | オ     | イ     |
| Unicode の Code Point | 0x30A2 | 0x30AA | 0x30A4 |
| Unicode の Code Point | 0x30A2 | 0x30AA | 0x30A4 |

しかし、文字列によって Code Point と Code Unit が異なる値を取る場合がある。`リンゴ🍎（リンゴの絵文字）`とか

```js
// 文字列をCode Unit(16進数)の配列にして返す
function convertCodeUnits(str) {
  const codeUnits = [];
  for (let i = 0; i < str.length; i++) {
    codeUnits.push(str.charCodeAt(i).toString(16));
  }
  return codeUnits;
}
// 文字列をCode Point(16進数)の配列にして返す
function convertCodePoints(str) {
  return Array.from(str).map((char) => {
    return char.codePointAt(0).toString(16);
  });
}

const str = "リンゴ🍎";
const codeUnits = convertCodeUnits(str);
console.log(codeUnits); // => ["30ea", "30f3", "30b4", "d83c", "df4e"]
const codePoints = convertCodePoints(str);
console.log(codePoints); // => ["30ea", "30f3", "30b4", "1f34e"]
```

| インデックス          | 0      | 1      | 2      | 3      | 4      |
| --------------------- | ------ | ------ | ------ | ------ | ------ |
| 文字列                | リ     | ン     | ゴ     | 🍎     |        |
| Unicode の Code Point | 0x30A2 | 0x30AA | 0x30A4 | 0x30A4 |
| Unicode の Code Point | 0x30A2 | 0x30AA | 0x30A4 | 0xb83c | 0xdf4e |

ある 1 つの文字に対応する ID である Code Point を、16 ビット（2 バイト）の Code Unit で表現するのが UTF-16 というエンコード方式で、16 ビット（2 バイト）で表現できる範囲は、65536 種類（2 の 16 乗）である。 現在、Unicode に登録されている Code Point は 10 万種類を超えているため、すべての文字と Code Unit を 1 対 1 の関係で表すことができない。

## サロゲートペア

サロゲートペアでは、2 つの Code Unit の組み合わせ（合計 4 バイト）で 1 つの文字（1 つの Code Point）を表現する。

- \uD800 ～\uDBFF：上位サロゲートの範囲
- \uDC00 ～\uDFFF：下位サロゲートの範囲
  文字列中に上位サロゲートと下位サロゲートの Code Unit が並んだ場合に、2 つの Code Unit を組み合わせて 1 文字（Code Point）として扱う。

Code Point のエスケープシーケンス（\u{XXXX}）も書けるため、1 つの Code Point で𩸽という文字を表現でき現出来るが、Code Point のエスケープシーケンスで書いた場合でも、内部的に Code Unit に変換された値で保持される。

```js
// 上位サロゲート + 下位サロゲートの組み合わせ
console.log("\uD867\uDE3D"); // => "𩸽"
// Code Pointでの表現
console.log("\u{29e3d}"); // => "𩸽"
```

また`🍎`も以下のように書けます。

```js
// Code Unit（上位サロゲート + 下位サロゲート）
console.log("\uD83C\uDF4E"); // => "🍎"
// Code Point
console.log("\u{1F34E}"); // => "🍎"
```

基本的には、文字列は Code Unit が順番に並んでいるものとして扱われるため、多くの String のメソッドは Code Unit ごとに作用する。 また、インデックスアクセスも Code Unit ごととなる。そのため、サロゲートペアで表現している文字列では、上位サロゲート（0 番目）と下位サロゲート（1 番目）へのインデックスアクセスになる。

```js
// 内部的にはCode Unitが並んでいるものとして扱われている
console.log("\uD867\uDE3D"); // => "𩸽"
// インデックスアクセスもCode Unitごととなる
console.log("𩸽"[0]); // => "\uD867"
console.log("𩸽"[1]); // => "\uDE3D"
```

たとえば、String の length プロパティは文字列における Code Unit の要素数を数えるため、"🍎".length の結果は 2 となる。

console.log("🍎".length); // => 2

## Code Point を扱う

CodePoint を名前に含むメソッド
u（Unicode）フラグが有効化されている正規表現
文字列の Iterator を扱うもの（Destructuring、for...of、Array.from メソッドなど）
これらの Code Point を扱う処理と具体的な使い方を見ている。

## 正規表現の.と Unicode

`u` フラグをつけた正規表現は、文字列を Code Point が順番
具体的に`u`フラグの有無による`.`（改行文字以外のどの 1 文字にもマッチする特殊文字）の動作の違い。

```js
const [all, fish] = "𩸽のひらき".match(/(.)のひらき/);
console.log(all); // => "\ude3dのひらき"
console.log(fish); // => "\ude3d"
```

`Array.from`メソッド[ES2015]は、引数に iterable なオブジェクトを受け取り、それを元にした新しい配列を返します。 iterable オブジェクトとは`Symbol.iterator`という特別な名前のメソッドを実装したオブジェクトの総称で、`for...of`文などで反復処理が可能なオブジェクトである。

```js
// Code Pointごとの配列にする
// Array.fromメソッドはIteratorを配列にする
const codePoints = Array.from("リンゴ🍎");
console.log(codePoints); // => ["リ", "ン", "ゴ", "🍎"]
// Code Pointの個数を数える
console.log(codePoints.length); // => 4
```

Code Point の数を数えた場合でも、直感的な結果にならない場合もある。 なぜなら、Code Point には制御文字などの視覚的に見えないものも定義されている。文字として数えたくないものは無視するなど、視覚的な文字列の長さを数えるにはさらなる工夫が必要になる。

## Code Point ごとに反復処理をする

Array.from メソッドを使えば、文字列を Code Point で区切った文字の配列へと変換できる。 配列にすれば、あとは「ループと反復処理」の章で学んだ方法を使って、Code Point ごとに反復処理ができる。
`countOfCodePoints`関数は、`Array.fromでCode` Point ごとの配列にし、配列を`codePoint`でフィルターした結果できた配列の要素数を返す。

```js
// 指定した`codePoint`の個数を数える
function countOfCodePoints(str, codePoint) {
  return Array.from(str).filter((item) => {
    return item === codePoint;
  }).length;
}
console.log(countOfCodePoints("🍎🍇🍎🥕🍒", "🍎")); // => 2
```

`for...of`による反復処理も文字列を Code Point ごとに扱えます。 これは、`for...of`文が対象を Iterator として列挙するためです。

```js
// 指定した`codePoint`の個数を数える
function countOfCodePoints(str, codePoint) {
  let count = 0;
  for (const item of str) {
    if (item === codePoint) {
      count++;
    }
  }
  return count;
}
console.log(countOfCodePoints("🍎🍇🍎🥕🍒", "🍎")); // => 2
```
