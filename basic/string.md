## 文字列を作成する

文字列リテラルには `"（ダブルクォート）`、`'（シングルクォート）`、`` `（バッククォート）`` の 3 種類がある。

### ダブルクォートとシングルクォート

`"（ダブルクォート）` と `'（シングルクォート）` に意味的な違いはない。

```js
const double = "文字列";
console.log(double); // => "文字列"
const single = "文字列";
console.log(single); // => '文字列'
// どちらも同じ文字列
console.log(double === single); // => true
```

### テンプレートリテラル

`` `（バッククォート）`` を利用することで文字列を作成できる点は、他の文字列リテラルと同じ。

これに加えて、テンプレートリテラルでは文字列中に改行を入力できる。次のコードでは、テンプレートリテラルを使って複数行の文字列を見た目どおりに定義している。

```js
const multiline = `1行目
2行目
3行目`;
// \n は改行を意味する
console.log(multiline); // => "1行目\n2行目\n3行目"
```

### 文字列リテラルのエスケープ

どの文字列リテラルでも共通だが、文字列リテラルは同じ記号が対になる。そのため、文字列の中にリテラルと同じ記号が出現した場合は、`\（バックスラッシュ）` を使いエスケープする必要がある。

```js
const str = 'This book is "js-primer"';
console.log(str); // => 'This book is "js-primer"'
```

## エスケープシーケンス

文字列リテラル中にはそのままでは入力できない特殊な文字もある。改行もそのひとつで、`"（ダブルクォート）` と `'（シングルクォート）` の文字列リテラルには改行をそのまま入力できません（テンプレートリテラル中には例外的に改行をそのまま入力できる）。

```js
// JavaScriptエンジンが構文として解釈できないため、SyntaxErrorとなる
const invalidString = "1行目
2行目
3行目";
```

この問題を回避するためには、改行のような特殊な文字をエスケープシーケンスとして書く必要がある。エスケープシーケンスは、`\` と特定の文字を組み合わせることで、特殊文字を表現する。

### 主なエスケープシーケンス

| エスケープシーケンス   | 意味                                       |
| ---------------------- | ------------------------------------------ |
| `\'`                   | シングルクォート                           |
| `\"`                   | ダブルクォート                             |
| `` \` ``               | バッククォート                             |
| `\\`                   | バックスラッシュ (`\` そのものを表示する)  |
| `\n`                   | 改行                                       |
| `\t`                   | タブ                                       |
| `\uXXXX`               | Code Unit (`\u` と 4 桁の HexDigit)        |
| `\u{X} ... \u{XXXXXX}` | Code Point（`\u{}` のカッコ中に HexDigit） |

```js
// 改行を \n のエスケープシーケンスとして入力している
const multiline = "1行目\n2行目\n3行目";
console.log(multiline);
/* 改行した結果が出力される
1行目
2行目
3行目
*/
```

また、`\` からはじまる文字は自動的にエスケープシーケンスとして扱われる。しかし、`\a` のように定義されていないエスケープシーケンスは、`\` が単に無視され `a` という文字列として扱われる。これにより、`\（バックスラッシュ）` そのものを入力していたつもりが、その文字がエスケープシーケンスとして扱われてしまう問題がある。

```js
console.log("¯_(ツ)_/¯");
// ¯_(ツ)_/¯ のように \ が無視されて表示される
```

`\（バックスラッシュ）` そのものを入力したい場合は、`\\` のようにエスケープする必要がある。

```js
console.log("¯\\_(ツ)_/¯");
//　¯\_(ツ)_/¯ と表示される
```

## 文字列を結合する

文字列を結合する簡単な方法は文字列結合演算子（`+`）を使う方法である。

```js
const str = "a" + "b";
console.log(str); // => "ab"
```

変数と文字列を結合したい場合も文字列結合演算子で行える。

```js
const name = "JavaScript";
console.log("Hello " + name + "!"); // => "Hello JavaScript!"
```

特定の書式に文字列を埋め込むには、テンプレートリテラルを使うとより宣言的に書ける。

```js
const name = "JavaScript";
console.log(`Hello ${name}!`); // => "Hello JavaScript!"
```

## 文字へのアクセス

文字列の特定の位置にある文字にはインデックスを指定してアクセスできる。これは、配列における要素へのアクセスにインデックスを指定するのと同じ

```js
const str = "文字列";
// 配列と同じようにインデックスでアクセスできる
console.log(str[0]); // => "文"
console.log(str[1]); // => "字"
console.log(str[2]); // => "列"
```

また、存在しないインデックスへのアクセスでは配列やオブジェクトと同じように `undefined` を返す。

```js
const str = "文字列";
// 42番目の要素は存在しない
console.log(str[42]); // => undefined
```

### [ES2022] String.prototype.at

ES2022 から `String.prototype.at` メソッドが追加されている。`String` の `at` メソッドは、`Array` の `at` メソッドと同じく、相対的なインデックスを渡してその位置の文字へアクセスできる。`at` メソッドへ `-1` のようにマイナスのインデックスを渡した場合は、末尾から数えた位置の文字へアクセスできる。

```js
const str = "文字列";
console.log(str.at(0)); // => "文"
console.log(str.at(1)); // => "字"
console.log(str.at(2)); // => "列"
console.log(str.at(-1)); // => "列"
```

## 文字列とは

文字列とはどのようなものなのか？ コンピュータのメモリ上に文字列の「ア」といった文字をそのまま保存できないため、0 と 1 からなるビット列へ変換する必要がある。この文字からビット列へ変換することを**符号化（エンコード）**という。

一方で、変換後のビット列が何の文字なのかを管理する表が必要になる。この文字に対応する ID の一覧表のことを**符号化文字集合**という。

次の表は、Unicode という文字コードにおける符号化文字集合からカタカナの一部分を取り出したもの。Unicode はすべての文字に対して ID（**Code Point**）を振ることを目的に作成されている仕様。

|      | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | A   | B   | C   | D   | E   | F   |
| ---- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 30A0 | ゠  | ァ  | ア  | ィ  | イ  | ゥ  | ウ  | ェ  | エ  | ォ  | オ  | カ  | ガ  | キ  | ギ  | ク  |
| 30B0 | グ  | ケ  | ゲ  | コ  | ゴ  | サ  | ザ  | シ  | ジ  | ス  | ズ  | セ  | ゼ  | ソ  | ゾ  | タ  |
| 30C0 | ダ  | チ  | ヂ  | ッ  | ツ  | ヅ  | テ  | デ  | ト  | ド  | ナ  | ニ  | ヌ  | ネ  | ノ  | ハ  |

JavaScript（ECMAScript）は文字コードとして Unicode を採用し、文字をエンコードする方式として**UTF-16**を採用している。UTF-16 とは、それぞれの文字を 16 ビットのビット列に変換するエンコード方式で Unicode では 1 文字を表すのに使う最小限のビットの組み合わせを**Code Unit（符号単位）**と呼び、UTF-16 では各 Code Unit のサイズが 16 ビット（2 バイト）である。

次のコードは、文字列を構成する Code Unit を hex 値（16 進数）にして表示する例であり、`String` の `charCodeAt` メソッドは、文字列の指定インデックスの Code Unit を整数として返す。その Code Unit の整数値を`Number` の `toString` メソッドで hex 値（16 進数）にしている。

```js
const str = "アオイ";
// それぞれの文字をCode Unitのhex値（16進数）に変換する
// toStringの引数に16を渡すと16進数に変換される
console.log(str.charCodeAt(0).toString(16)); // => "30a2"
console.log(str.charCodeAt(1).toString(16)); // => "30aa"
console.log(str.charCodeAt(2).toString(16)); // => "30a4"
```

逆に、Code Unit を hex 値（16 進数）から文字へと変換するには`String.fromCharCode` メソッドを使う。次のコードでは、16 進数の整数リテラルである`0x`で記述した Code Unit から文字列へと変換している。

```js
const str = String.fromCharCode(
  0x30a2, // アのCode Unit
  0x30aa, // オのCode Unit
  0x30a4 // イのCode Unit
);
console.log(str); // => "アオイ"
```

これらの結果をまとめると、この文字列と文字列を構成する UTF-16 の Code Unit との関係は次のようになる。

| インデックス                   | 0      | 1      | 2      |
| ------------------------------ | ------ | ------ | ------ |
| 文字列                         | ア     | オ     | イ     |
| UTF-16 の Code Unit（16 進数） | 0x30A2 | 0x30AA | 0x30A4 |

このように、JavaScript における文字列は 16 ビットの Code Unit が順番に並んだものとして内部的に管理されている。これは、ECMAScript の内部表現として UTF-16 を採用しているだけで、JavaScript ファイル（ソースコードを書いたファイル）のエンコーディングとは関係ない。そのため、JavaScript ファイル自体のエンコードは、UTF-16 以外の文字コードであっても問題ない。

## 文字列の分解と結合

文字列を配列へ分解するには`String` の `split` メソッドを利用できる。一方、配列の要素を結合して文字列にするには`Array` の `join` メソッドを利用できる。

### 分解と結合の例

`String` の `split` メソッドは、第一引数に指定した区切り文字で文字列を分解した配列を返す。次のコードでは、文字列を「・」で区切った配列を作成している。

```js
const strings = "赤・青・緑".split("・");
console.log(strings); // => ["赤", "青", "緑"]
```

分解してできた文字列の配列を結合して文字列を作る際に、`Array` の `join` メソッドが利用できる。`Array` の `join` メソッドの第一引数には区切り文字を指定し、その区切り文字で結合した文字列を返す。

この 2 つを合わせれば、区切り文字を「・」から「、」へ変換する処理を次のように書くことができる。

```js
const str = "赤・青・緑".split("・").join("、");
console.log(str); // => "赤、青、緑"
```

`String` の `split` メソッドの第一引数には**正規表現**も指定できる。これを利用すると、次のように文字列をスペースで区切るような処理を簡単に書ける。`/\s+/`は 1 つ以上のスペースにマッチする正規表現オブジェクトを作成する正規表現リテラルである。

```js
// 文字間に1つ以上のスペースがある
const str = "a     b    c      d";
// 1つ以上のスペースにマッチして分解する
const strings = str.split(/\s+/);
console.log(strings); // => ["a", "b", "c", "d"]
```

## 文字列の長さ

`String` の `length` プロパティは文字列の要素数を返す。文字列の構成要素は Code Unit であるため、`length` プロパティは Code Unit の個数を返す。

次の文字列は 3 つの要素（Code Unit）が並んだものであるため、`length` プロパティは 3 を返す。

```js
console.log("文字列".length); // => 3
```

また、空文字列は要素数が 0 であるため、`length` プロパティの結果も 0 となる。

```js
console.log("".length); // => 0
```

## 文字列の比較

文字列の比較には `===`（厳密比較演算子）を利用する。次の条件を満たしていれば、同じ文字列と見なされる。

- 文字列の要素である Code Unit が同じ順番で並んでいる
- 文字列の長さ（`length`）が同じである

同じ文字列同士なら、`===` の結果は `true` になる。

```js
console.log("文字列" === "文字列"); // => true
// 一致しなければ false となる
console.log("JS" === "ES"); // => false
// 文字列の長さが異なるので false となる
console.log("文字列" === "文字"); // => false
```

また、`===` 以外にも、 `>`, `<`, `>=`, `<=` などの大小の関係演算子で文字列同士を比較できる。

これらの関係演算子も、文字列の要素である Code Unit 同士を先頭から順番に比較する。文字列から Code Unit の数値を取得するには、`String` の `charCodeAt` メソッドを利用できる。

次のコードでは、`ABC` と `ABD` を比較した場合にどちらが大きいか（Code Unit の値が大きいか）を比較している。

```js
// "A" と "B" の Code Unit は 65 と 66
console.log("A".charCodeAt(0)); // => 65
console.log("B".charCodeAt(0)); // => 66
// "A"（65）は "B"（66）より Code Unit の値が小さい
console.log("A" > "B"); // => false
// 先頭から順番に比較し、C > D が false であるため
console.log("ABC" > "ABD"); // => false
```

関係演算子での文字列比較は Code Unit 同士を比較する。この結果を予測するのは難しく、直感的ではない結果が生まれることも多い。文字の順番は国や言語によっても異なるため、国際化（Internationalization）に関する知識も必要。

JavaScript においても、ECMA-402 という ECMAScript と関連する別の仕様として国際化についての取り決めがされ、この国際化に関する API を定義した `Intl` というビルトインオブジェクトもある。

Intl の活用シーン

- 数値や通貨のフォーマット(`Intl.NumberFormat`)
- 日付や時刻のローカライズ(`Intl.DateTimeFormat`)
- 文字列の並び替え（ソート）(`Intl.Collator`)
- 相対的な時間の表示(`Intl.RelativeTimeFormat`)
- リストのフォーマット(`Intl.ListFormat`)
- 数字の単数形・複数形の区別(`Intl.PluralRules`)
- 地域に対応した言語名、国名、通貨名の取得(`Intl.DisplayNames`)

## 文字列の一部を取得

文字列からその一部を取り出したい場合には、`String` の `slice` メソッドや `substring` メソッドを利用できる。

### `slice` メソッド

`slice` メソッドは、第一引数の開始位置から第二引数の終了位置（終了位置の要素は含まない）までの範囲を取り出した新しい文字列を返す。第二引数は省略可能で、省略した場合は文字列の末尾まで含んだ新しい文字列を返す。

位置にマイナスの値を指定した場合は、文字列の末尾から数えた位置となる。また、第一引数の位置が第二引数の位置より大きい場合、常に空の文字列を返す。

```js
const str = "ABCDE";
console.log(str.slice(1)); // => "BCDE"
console.log(str.slice(1, 5)); // => "BCDE"
// マイナスを指定すると後ろからの位置となる
console.log(str.slice(-1)); // => "E"
// インデックスが 1 から 4 の範囲を取り出す
console.log(str.slice(1, 4)); // => "BCD"
// 第一引数 > 第二引数の場合、常に空文字列を返す
console.log(str.slice(4, 1)); // => ""
```

### `substring` メソッド

`substring` メソッドは、`slice` メソッドと同じく第一引数に開始位置、第二引数に終了位置を指定し、その範囲を取り出して新しい文字列を返す。第二引数を省略した場合の挙動も同様で、省略した場合は文字列の末尾が終了位置となる。

`slice` メソッドとは異なる点として、位置にマイナスの値を指定した場合は常に `0` として扱われる。また、第一引数の位置が第二引数の位置より大きい場合、第一引数と第二引数が入れ替わるという予想しにくい挙動となる。

```js
const str = "ABCDE";
console.log(str.substring(1)); // => "BCDE"
console.log(str.substring(1, 5)); // => "BCDE"
// マイナスを指定すると 0 として扱われる
console.log(str.substring(-1)); // => "ABCDE"
// 位置: 1 から 4 の範囲を取り出す
console.log(str.substring(1, 4)); // => "BCD"
// 第一引数 > 第二引数の場合、引数が入れ替わる
// str.substring(1, 4) と同じ結果になる
console.log(str.substring(4, 1)); // => "BCD"
```

このように、マイナスの位置や引数が交換される挙動はわかりやすいとは言えません。そのため、`slice` メソッドと `substring` メソッドに指定する引数は、どちらとも同じ結果となる範囲に限定したほうが直感的な挙動となる。つまり、指定するインデックスは `0` 以上にして、第二引数を指定する場合は `第一引数の位置 < 第二引数の位置` にする。

`slice` メソッドは、`indexOf` メソッドなどの位置を取得するものと組み合わせて使うことが多い。

```js
const url = "https://example.com?param=1";
const indexOfQuery = url.indexOf("?");
const queryString = url.slice(indexOfQuery);
console.log(queryString); // => "?param=1"
```

また、配列とは異なりプリミティブ型の値である文字列は、`slice` メソッドと `substring` メソッド共に非破壊的である。機能的な違いがほとんどないため、どちらを利用するかは好みの問題となる。

## 文字列の検索

文字列の検索方法として、大きく分けて「文字列による検索」と「正規表現による検索」がある。

指定した文字列が文字列中に含まれているかを検索する方法として、`String` メソッドには取得したい結果ごとにメソッドが用意されている。

- マッチした箇所のインデックスを取得
- マッチした文字列の取得
- マッチしたかどうかの真偽値を取得

### 文字列による検索

`String` オブジェクトには、指定した文字列で検索するメソッドが用意されている。

#### 文字列によるインデックスの取得

`String` の `indexOf` メソッドと `lastIndexOf` メソッドは、指定した文字列で検索し、その文字列が最初に現れたインデックスを返す。これらは配列の `Array` の `indexOf` メソッドと同じで、厳密等価演算子（`===`）で文字列を検索する。一致する文字列がない場合は `-1` を返す。

- `文字列.indexOf("検索文字列")`: 先頭から検索し、指定された文字列が最初に現れたインデックスを返す
- `文字列.lastIndexOf("検索文字列")`: 末尾から検索し、指定された文字列が最初に現れたインデックスを返す

どちらのメソッドも、一致する文字列が複数個ある場合でも、指定した検索文字列を最初に見つけた時点で検索は終了する。

```js
// 検索対象となる文字列
const str = "にわにはにわにわとりがいる";
// indexOfは先頭から検索しインデックスを返す - "**にわ**にはにわにわとりがいる"
// "にわ"の先頭のインデックスを返すため0 となる
console.log(str.indexOf("にわ")); // => 0
// lastIndexOfは末尾から検索しインデックスを返す- "にわにはにわ**にわ**とりがいる"
console.log(str.lastIndexOf("にわ")); // => 6
// 指定した文字列が見つからない場合は -1 を返す
console.log(str.indexOf("未知のキーワード")); // => -1
```

## 文字列にマッチした文字列の取得

文字列を検索してマッチした文字列は、検索文字列そのものになる。

次のコードでは `"Script"` という文字列で検索しているが、その検索文字列にマッチする文字列は`"Script"` になる。

```js
const str = "JavaScript";
const searchWord = "Script";
const index = str.indexOf(searchWord);
if (index !== -1) {
  console.log(`${searchWord}が見つかりました`);
} else {
  console.log(`${searchWord}は見つかりませんでした`);
}
```

## 真偽値の取得

文字列に「検索文字列」が含まれているかどうかを検索する方法がいくつか用意されている。次の 3 つのメソッドは ES2015 で導入された。

- **`String.prototype.startsWith(検索文字列)`**: 検索文字列が先頭にあるかの真偽値を返す
- **`String.prototype.endsWith(検索文字列)`**: 検索文字列が末尾にあるかの真偽値を返す
- **`String.prototype.includes(検索文字列)`**: 検索文字列を含むかの真偽値を返す

```js
// 検索対象となる文字列
const str = "にわにはにわにわとりがいる";
// startsWith - 検索文字列が先頭なら true
console.log(str.startsWith("にわ")); // => true
console.log(str.startsWith("いる")); // => false
// endsWith - 検索文字列が末尾なら true
console.log(str.endsWith("にわ")); // => false
console.log(str.endsWith("いる")); // => true
// includes - 検索文字列が含まれるなら true
console.log(str.includes("にわ")); // => true
console.log(str.includes("いる")); // => true
```


## 正規表現オブジェクト

文字列による検索では、固定の文字列にマッチするものしか検索できない。一方で、**正規表現**による検索では、あるパターン（規則性）にマッチする柔軟な検索が可能。

正規表現は**正規表現オブジェクト（`RegExp` オブジェクト）**として表現される。正規表現オブジェクトは「マッチする範囲を決める**パターン**」と「正規表現の検索モードを指定する**フラグ**」の 2 つで構成される。正規表現のパターン内では、次の文字は特殊文字と呼ばれ、特別な意味を持つ。特殊文字として解釈されないように入力する場合には `\`（バックスラッシュ）でエスケープする必要がある。

```
\ ^ $ . * + ? ( ) [ ] { } |
```

### 正規表現オブジェクトの作成方法

正規表現オブジェクトを作成するには、**正規表現リテラル**と**`RegExp` コンストラクタ**の 2 つの方法がある。

```js
// 正規表現リテラルで正規表現オブジェクトを作成
const patternA = /パターン/フラグ;
// `RegExp` コンストラクタで正規表現オブジェクトを作成
const patternB = new RegExp("パターン文字列", "フラグ");
```

#### 正規表現リテラル

正規表現リテラルは、`/` と `/` のリテラル内に正規表現のパターンを書くことで、正規表現オブジェクトを作成できる。次のコードでは、`+` という 1 回以上の繰り返しを意味する特殊文字を使い、`a` が 1 回以上連続する文字列にマッチする正規表現オブジェクトを作成している。

```js
const pattern = /a+/;
```

#### `RegExp` コンストラクタ

正規表現オブジェクトを作成するもうひとつの方法として、`RegExp` コンストラクタがある。`RegExp` コンストラクタでは、文字列から正規表現オブジェクトを作成できる。

次のコードでは、`RegExp` コンストラクタを使って `a` が 1 文字以上連続している文字列にマッチする正規表現オブジェクトを作成している。これは先ほどの正規表現リテラルで作成した正規表現オブジェクトと同じ意味になる。

```js
const pattern = new RegExp("a+");
```

---

### 正規表現リテラルと `RegExp` コンストラクタの違い

正規表現リテラルと `RegExp` コンストラクタの違いとして、正規表現のパターンが評価される**タイミング**が異なる。

- **正規表現リテラル**は、ソースコードをロード（パース）した段階で正規表現のパターンが評価される。
- **`RegExp` コンストラクタ**は通常の関数と同じように、`RegExp` コンストラクタを呼び出すまで正規表現のパターンは評価されない。

#### 評価のタイミングの違い

`[` は対になる `]` と組み合わせて利用する特殊文字であるため、正規表現のパターンに単独で書くと構文エラー（`SyntaxError`）が発生する。

**正規表現リテラル**の場合、ソースコードのロード時に正規表現のパターンが評価されるため、次のように `main` 関数を呼び出していなくても構文エラーが発生する。

```js
// 正規表現リテラルはロード時にパターンが評価され、例外が発生する
function main() {
    // `[` は対となる `]` を組み合わせる特殊文字であるため、単独で書けない
    const invalidPattern = /[/;
}

// `main` 関数を呼び出さなくても例外が発生する
```

**`RegExp` コンストラクタ**の場合、実行時に正規表現のパターンが評価されるため、`main` 関数を呼び出すことで初めて構文エラーが発生する。

```js
// `RegExp` コンストラクタは実行時にパターンが評価され、例外が発生する
function main() {
  // `[` は対となる `]` を組み合わせる特殊文字であるため、単独で書けない
  const invalidPattern = new RegExp("[");
}

// `main` 関数を呼び出すことで初めて例外が発生する
main();
```

---

### 正規表現のパターンに変数を利用する場合

正規表現リテラルはコードを書いた時点で決まったパターンの正規表現オブジェクトを作成する構文で一方で、`RegExp` コンストラクタは変数と組み合わせるなど、実行時に変わることがあるパターンの正規表現オブジェクトを作成できる。

次のコードでは、`spaceCount` の数だけ連続するホワイトスペースにマッチする正規表現オブジェクトを `RegExp` コンストラクタで作成している。注意点として、`\`（バックスラッシュ）自体が文字列中でエスケープ文字であるため、`RegExp` コンストラクタの引数ではバックスラッシュから始まる特殊文字をエスケープする必要がある。

```js
const spaceCount = 3;
// `/\s{3}/` の正規表現を文字列から作成する
// "\" がエスケープ文字であるため、"\" 自身を文字列として書くには、"\\" のように2つ書く
const pattern = new RegExp(`\\s{${spaceCount}}`);
```

---

このように、`RegExp` コンストラクタは文字列から正規表現オブジェクトを作成できるが、特殊文字のエスケープが必要。そのため、正規表現リテラルで表現できる場合は、リテラルを利用したほうが簡潔でパフォーマンスもよい。 正規表現のパターンに変数を利用する場合などは、RegExp コンストラクタを利用する。

## 正規表現による検索

正規表現による検索は、正規表現オブジェクトと対応した `String` オブジェクトまたは `RegExp` オブジェクトのメソッドを利用する。

### 正規表現によるインデックスの取得

`String` の `indexOf` メソッドの正規表現版ともいえる `String` の `search` メソッドがある。`search` メソッドは、正規表現のパターンにマッチした箇所のインデックスを返し、マッチする文字列がない場合は `-1` を返す。

- `String.prototype.indexOf(検索文字列)`: 指定された文字列にマッチした箇所のインデックスを返す
- `String.prototype.search(/パターン/)`: 指定された正規表現のパターンにマッチした箇所のインデックスを返す

次のコードでは、数字が 3 つ連続しているかを検索し、該当した箇所のインデックスを返している。 `\d` は、1 文字の数字（0 から 9）にマッチする特殊文字。

```js
const str = "ABC123EFG";
const searchPattern = /\d{3}/;
console.log(str.search(searchPattern)); // => 3
```

---

### 正規表現によるマッチした文字列の取得

文字列による検索では、検索した文字列そのものがマッチした文字列になる。しかし、`search` メソッドの正規表現による検索は、正規表現パターンによる検索であるため、マッチした文字列の長さは固定ではない。そのため、`search` メソッドでマッチしたインデックスだけを取得しても、実際にマッチした文字列がわからない。

```js
const str = "abc123def";
// 連続した数字にマッチする正規表現
const searchPattern = /\d+/;
const index = str.search(searchPattern); // => 3
// `index` だけではマッチした文字列の長さがわからない
str.slice(index, index + "マッチした文字列の長さ"); // マッチした文字列は取得できない
```

そのため、マッチした文字列を取得するための `String` の `match` メソッドと `matchAll` メソッドが用意されている。これらのメソッドは、正規表現の `g` フラグ（global の略称）によって挙動が変わる。

### マッチした文字列の取得
`match` メソッドは、正規表現の `/パターン/` が "文字列" にマッチすると、マッチした文字列に関する情報を返すメソッド。

```js
"文字列".match(/パターン/);
```

`match` メソッドで検索した結果、正規表現にマッチする文字列がなかった場合は `null` を返す。

```js
console.log("文字列".match(/マッチしないパターン/)); // => null
```

#### g フラグなしの場合

`match` メソッドは正規表現の `g` フラグなしで検索した場合、最初にマッチしたものが見つかった時点で検索が終了する。このときの `match` メソッドの返り値は、`index` プロパティと `input` プロパティを持った特殊な配列となる。`index` プロパティにはマッチした文字列の先頭のインデックスが、`input` プロパティには検索対象となった文字列全体が含まれいる。

次のコードでは、`/[a-zA-Z]+/` という正規表現は `a` から `Z` のどれかの文字が 1 つ以上連続しているものにマッチする。

```js
const str = "ABC あいう DE えお";
const alphabetsPattern = /[a-zA-Z]+/;
// gフラグなしでは、最初の結果のみを含んだ特殊な配列を返す
const results = str.match(alphabetsPattern);
console.log(results.length); // => 1
// マッチした文字列はインデックスでアクセスできる
console.log(results[0]); // => "ABC"
// マッチした文字列の先頭のインデックス
console.log(results.index); // => 0
// 検索対象となった文字列全体
console.log(results.input); // => "ABC あいう DE えお"
```

#### g フラグありの場合

`match` メソッドは、正規表現の `g` フラグありで検索した場合、マッチしたすべての文字列を含んだ配列を返す。

次のコードでは、`/[a-zA-Z]+/g` という正規表現は `a` から `Z` のどれかの文字が 1 つ以上連続しているものに繰り返しマッチする。この正規表現にマッチする箇所は `"ABC"` と `"DE"` の 2 つとなるため、`match` メソッドの返り値である配列にも 2 つの要素が含まれている。

```js
const str = "ABC あいう DE えお";
const alphabetsPattern = /[a-zA-Z]+/g;
// gフラグありでは、すべての検索結果を含む配列を返す
const resultsWithG = str.match(alphabetsPattern);
console.log(resultsWithG.length); // => 2
console.log(resultsWithG[0]); // => "ABC"
console.log(resultsWithG[1]); // => "DE"
// indexとinputはgフラグありの場合は追加されない
console.log(resultsWithG.index); // => undefined
console.log(resultsWithG.input); // => undefined
```

#### `match` メソッドの挙動

- マッチしない場合は、`null` を返す
- マッチした場合は、マッチした文字列を含んだ特殊な配列を返す
- 正規表現の `g` フラグがある場合は、マッチしたすべての結果を含んだただの配列を返す

---

### `matchAll` メソッド

`matchAll` メソッドは、マッチした結果を **Iterator** で返す。

次のコードでは、`matchAll` メソッドでアルファベットにマッチする結果の **Iterator** オブジェクトを取得している。`Iterator` オブジェクトは `for...of` 構文で反復処理すると、`Iterator` から値を 1 つずつ取り出して処理できる。

```js
const str = "ABC あいう DE えお";
const alphabetsPattern = /[a-zA-Z]+/g;
// matchAllはIteratorを返す
const matchesIterator = str.matchAll(alphabetsPattern);
for (const match of matchesIterator) {
  // マッチした要素ごとの情報を含んでいる
  console.log(
    `match: "${match[0]}", index: ${match.index}, input: "${match.input}"`
  );
}
// 次の順番でコンソールに出力される
// match: "ABC", index: 0, input: "ABC あいう DE えお"
// match: "DE", index: 8, input: "ABC あいう DE えお"
```

そのため、正規表現の `g` フラグを使った繰り返しマッチを行う場合には、`match` メソッドではなく `matchAll` メソッドを利用する。また、`matchAll` メソッドは `g` フラグなしの正規表現はサポートしていないため、`g` フラグなしの正規表現を渡した場合は例外が発生する。

## マッチした文字列の一部を取得

`String` の `match` メソッドと `matchAll` メソッドは、どちらも正規表現の**キャプチャリング**に対応している。キャプチャリングとは、正規表現中で `/パターン1(パターン2)/` のようにカッコで囲んだ部分を取り出す。このキャプチャリングによって、正規表現でマッチした一部分だけを取り出すことができる。

`match` メソッドと `matchAll` メソッドは、どちらもマッチした結果を**配列**として返す。

そのマッチしているパターンにキャプチャが含まれている場合、返り値の配列へキャプチャした部分が追加される。配列の先頭にはマッチした文字列全体が入り、順番にキャプチャリング（`(` と `)`）で囲んだ範囲が配列に含まれる。

```js
const [マッチした全体の文字列, キャプチャ1, キャプチャ2] = 文字列.match(
  /パターン(キャプチャ1)と(キャプチャ2)/
);
```

次のコードでは、`ECMAScript 数字` の数字部分だけを取り出している。`String` の `match` メソッドとキャプチャリングによって数字（`\d+`）にマッチする部分を取得している。

```js
// "ECMAScript (数字+)"にマッチするが、欲しい文字列は数字の部分のみ
const pattern = /ECMAScript (\d+)/;
// 返り値は0番目がマッチした全体、1番目がキャプチャの1番目というように対応している
// [マッチした全体の文字列, キャプチャ1, キャプチャ2 ....]
const [all, capture1] = "ECMAScript 6".match(pattern);
console.log(all); // => "ECMAScript 6"
console.log(capture1); // => "6"
```

---

### 正規表現の `g` フラグを使った繰り返しマッチ

正規表現の `g` フラグを使って繰り返し文字列にマッチする場合は、`matchAll` メソッドを利用する。

```js
// "ES(数字+)"にマッチするが、欲しい文字列は数字の部分のみ
const pattern = /ES(\d+)/g;
// iteratorを返す
const matchesIterator = "ES2015、ES2016、ES2017".matchAll(pattern);
for (const match of matchesIterator) {
  // マッチした要素ごとの情報を含んでいる
  // 0番目はマッチした文字列全体、1番目がキャプチャの1番目である数字
  console.log(
    `match: "${match[0]}", capture1: ${match[1]}, index: ${match.index}, input: "${match.input}"`
  );
}
// 次の順番でコンソールに出力される
// match: "ES2015", capture1: 2015, index: 0, input: "ES2015、ES2016、ES2017"
// match: "ES2016", capture1: 2016, index: 7, input: "ES2015、ES2016、ES2017"
// match: "ES2017", capture1: 2017, index: 14, input: "ES2015、ES2016、ES2017"
```

---

## [コラム] `RegExp.prototype.exec` と `String.prototype.matchAll`

`String` の `matchAll` メソッドは、ES2020 で導入されたメソッド。それまでは、`RegExp` の `exec` メソッドという、`String` の `match` メソッドによく似た挙動をするメソッドを利用して、`matchAll` 相当の表現を実装した。

`RegExp` の `exec` メソッドは、引数に文字列を受け取るメソッド。

```js
/pattern/.exec("文字列");
```

`exec` メソッドは、`g` フラグなしのパターンで検索した場合、最初のマッチした結果のみを含む特殊な配列を返す。このときの `exec` メソッドの返り値である配列には `index` プロパティと `input` プロパティが追加されており、この点は `String` の `match` メソッドと同様。

```js
const str = "ABC あいう DE えお";
const alphabetsPattern = /[a-zA-Z]+/;
// gフラグなしでは、最初の結果のみを持つ配列を返す
const results = alphabetsPattern.exec(str);
console.log(results.length); // => 1
console.log(results[0]); // => "ABC"
// マッチした文字列の先頭のインデックス
console.log(results.index); // => 0
// 検索対象となった文字列全体
console.log(results.input); // => "ABC あいう DE えお"
```

`RegExp` の `exec` メソッドは `g` フラグありのパターンで検索した場合でも、最初のマッチした結果のみを含む特殊な配列を返す。この点は `String` の `match` メソッドとは異なる。また、最後にマッチした文字列末尾のインデックスを正規表現オブジェクトの `lastIndex` プロパティに記録する。そして、もう一度 `exec` メソッドを呼び出すと、最後にマッチした末尾のインデックス（`lastIndex` プロパティの位置）から検索が開始される。

```js
const str = "ABC あいう DE えお";
const alphabetsPattern = /[a-zA-Z]+/g;
// まだ一度も検索していないので、lastIndexは0となり先頭から検索が開始される
console.log(alphabetsPattern.lastIndex); // => 0
// gフラグありでも、一回目の結果は同じだが、`lastIndex`プロパティが更新される
const result1 = alphabetsPattern.exec(str);
console.log(result1[0]); // => "ABC"
console.log(alphabetsPattern.lastIndex); // => 3
// 2回目の検索が、`lastIndex`の値のインデックスから開始される
const result2 = alphabetsPattern.exec(str);
console.log(result2[0]); // => "DE"
console.log(alphabetsPattern.lastIndex); // => 10
// 検索結果が見つからない場合はnullを返し、`lastIndex`プロパティは0にリセットされる
const result3 = alphabetsPattern.exec(str);
console.log(result3); // => null
console.log(alphabetsPattern.lastIndex); // => 0
```

#### `exec` メソッドの挙動まとめ

- マッチしない場合は `null` を返す
- マッチした場合は、マッチした文字列を含んだ特殊な配列を返す
- 正規表現の `g` フラグがある場合は、マッチした文字列を含んだ特殊な配列を返し、マッチした末尾のインデックスを正規表現オブジェクトの `lastIndex` プロパティに記録する

この `g` フラグと `exec` メソッドで検索した場合に、`lastIndex` プロパティが検索ごとに更新される仕組みを利用して、マッチするすべての結果を取得できる。

```js
const str = "ABC あいう DE えお";
const alphabetsPattern = /[a-zA-Z]+/g;
let matches;
while ((matches = alphabetsPattern.exec(str))) {
  // RegExpの`exec`メソッドの返り値は`index`プロパティなどを含む特殊な配列
  console.log(
    `match: ${matches[0]}, index: ${matches.index}, lastIndex: ${alphabetsPattern.lastIndex}`
  );
}
// 次の順番でコンソールに出力される
// match: ABC, index: 0, lastIndex: 3
// match: DE, index: 8, lastIndex: 10
```

## 真偽値の取得

正規表現オブジェクトを使って、そのパターンにマッチするかをテストするには、`RegExp` の `test` メソッドを利用できる。

正規表現のパターンには、位置を指定する特殊文字があり、`String` メソッドと `RegExp` の `test` メソッドを組み合わせて検索の種類を表現できる。

- **`String` の `startsWith` 相当**: `^パターン.test(文字列)`
  - `^` は先頭に一致する特殊文字
- **`String` の `endsWith` 相当**: `/パターン$/.test(文字列)`
  - `$` は末尾に一致する特殊文字
- **`String` の `includes` 相当**: `/パターン/.test(文字列)`


```js
// 検索対象となる文字列
const str = "にわにはにわにわとりがいる";
// ^ - 検索文字列が先頭ならtrue
console.log(/^にわ/.test(str)); // => true
console.log(/^いる/.test(str)); // => false
// $ - 検索文字列が末尾ならtrue
console.log(/にわ$/.test(str)); // => false
console.log(/いる$/.test(str)); // => true
// 検索文字列が含まれるならtrue
console.log(/にわ/.test(str)); // => true
console.log(/いる/.test(str)); // => true
```

## 文字列と正規表現どちらを使うべきか

- **正規表現**は曖昧な検索に強く、特殊文字を使うことで柔軟な検索結果が得られる。
- **`String` メソッド**は、そのままのコードで意図が表現されるため、正規表現と比較して可読性が高い。

次の例では、文字列が `/` で始まり `/` で終わるかを判定しようとしている。この判定を正規表現と `String` メソッドでそれぞれ実装している。

```js
const str = "/正規表現のような文字列/";

// 正規表現で`/`からはじまり`/`で終わる文字列のパターン
const regExpLikePattern = /^\/.*\/$/;
console.log(regExpLikePattern.test(str)); // => true

// Stringメソッドで、`/`からはじまり`/`で終わる文字列かを判定する関数
const isRegExpLikeString = (str) => {
  return str.startsWith("/") && str.endsWith("/");
};
console.log(isRegExpLikeString(str)); // => true
```

正規表現のパターンそのものを見るだけでは、何を意図しているのかがわかりにくいことが多いため、`String` メソッドで表現できる場合はそちらを使用し、柔軟性が必要な場合や曖昧な検索が必要な場合に正規表現を利用することを推奨されている。

## 文字列の置換/削除

文字列の一部を置換したり削除するには、`String` の `replace` メソッドを利用する。プリミティブ型である文字列は**不変**な特性を持つため、直接文字列の一部を変更することはできない。

### 置換と削除の基本

`replace` メソッドは、文字列から第一引数の検索文字列または正規表現にマッチする部分を、第二引数の置換文字列に置き換える。

```js
文字列.replace("検索文字列", "置換文字列");
文字列.replace(/パターン/, "置換文字列");
```

次のように、`replace` メソッドで削除したい部分を空文字列に置換することで、文字列の削除が可能。

```js
const str = "文字列";
// "文字"を空文字列へ置換することで削除を表現
const newStr = str.replace("文字", "");
console.log(newStr); // => "列"
```

#### 正規表現による置換

`replace` メソッドには正規表現も指定できる。`g` フラグを有効化した正規表現を渡すと、文字列からパターンにマッチするすべての部分を置換できる。

```js
const str = "にわにはにわにわとりがいる";
// 文字列を指定した場合は、最初に一致したものだけが置換される
console.log(str.replace("にわ", "niwa")); // => "niwaにはにわにわとりがいる"
// `g` フラグなし正規表現の場合は、最初に一致したものだけが置換される
console.log(str.replace(/にわ/, "niwa")); // => "niwaにはにわにわとりがいる"
// `g` フラグあり正規表現の場合は、すべて置換
console.log(str.replace(/にわ/g, "niwa")); // => "niwaにはniwaniwaとりがいる"
```

#### `replaceAll` メソッド

`String` の `replaceAll` メソッドは、ES2021 で追加され、すべての一致した部分を置換する。`replace` メソッドでは最初に一致したものだけが置換されるが、`replaceAll` ではすべてが置換される。

```js
const str = "???";
// replaceメソッドに文字列を指定した場合は、最初に一致したものだけが置換される
console.log(str.replace("?", "!")); // => "!??"
// replaceAllメソッドに文字列を指定した場合は、一致したものがすべて置換される
console.log(str.replaceAll("?", "!")); // => "!!!"
// replaceメソッドの場合、正規表現の特殊文字はエスケープが必要となる
console.log(str.replace(/\?/g, "!")); // => "!!!"
// replaceAllメソッドに正規表現を渡す場合も、エスケープが必要
console.log(str.replaceAll(/\?/g, "!")); // => "!!!"
```

### キャプチャを利用した置換

`replace` メソッドと `replaceAll` メソッドの第二引数にはコールバック関数を渡せる。第一引数のパターンにマッチした部分がコールバック関数の返り値で置換される。

```js
const 置換した結果の文字列 = 文字列.replace(
  /(パターン)/,
  (all, ...captures) => {
    return 置換したい文字列;
  }
);
```

#### 例: 日付のフォーマット変換

次の例では、`2017-03-01` を `2017年03月01日` に置換する。`/(\d{4})-(\d{2})-(\d{2})/g` という正規表現が `2017-03-01` という文字列にマッチする。

```js
function toDateJa(dateString) {
  // パターンにマッチしたときのみ、コールバック関数で置換処理が行われる
  return dateString.replace(
    /(\d{4})-(\d{2})-(\d{2})/g,
    (all, year, month, day) => {
      // `all` には、マッチした文字列全体が入るが今回は利用しない
      // `all` が次の返す値で置換される
      return `${year}年${month}月${day}日`;
    }
  );
}
// マッチしない文字列の場合は、そのままの文字列が返る
console.log(toDateJa("本日ハ晴天ナリ")); // => "本日ハ晴天ナリ"
// マッチした場合は置換した結果を返す
console.log(toDateJa("今日は2017-03-01です")); // => "今日は2017年03月01日です"
```

## 文字列の組み立て

最後に文字列の組み立てについて見ていきましょう。最初に述べたようにこの章の目的は、「**自由に文字列を作れるようになること**」。

文字列を単純に結合したり置換することで新しい文字列を作れることができる一方、構造的な文字列の場合は単純に結合するだけでは意味が異なってしまうことがある。

構造的な文字列とは、URL文字列やファイルパス文字列といった文字列中にコンテキストを持っているものを指す。

```
"https://example.com/index.html"
 ^^^^^   ^^^^^^^^^^^ ^^^^^^^^^^
   |          |          |
 scheme      host     pathname
```

これらの文字列を作成する場合は、文字列結合演算子（`+`）で単純に結合するよりも**専用の関数**を用意するほうが安全である。

たとえば、次のように `baseURL` と `pathname` を渡し、それらを結合した URL にあるリソースを取得する `getResource` 関数があるとする。この `getResource` 関数には、ベース URL（`baseURL`）とパス（`pathname`）を引数に渡して利用する。

```js
// `baseURL` と `pathname` にあるリソースを取得する
function getResource(baseURL, pathname) {
  const url = baseURL + pathname;
  console.log(url); // => "http://example.com/resources/example.js"
  // リソースを取得する処理...
}
const baseURL = "http://example.com/resources";
const pathname = "/example.js";
getResource(baseURL, pathname);
```

しかし、人によっては、`baseURL` の末尾には `/` が含まれると考える場合もある。`getResource` 関数は、`baseURL` の末尾に `/` が含まれているケースを想定していなかったため、意図しない URL からリソースを取得するという問題が発生する。

```js
// `baseURL` と `pathname` にあるリソースを取得する
function getResource(baseURL, pathname) {
  const url = baseURL + pathname;
  // `/` と `/` が2つ重なってしまっている
  console.log(url); // => "http://example.com/resources//example.js"
  // リソースを取得する処理...
}
const baseURL = "http://example.com/resources/";
const pathname = "/example.js";
getResource(baseURL, pathname);
```

この問題が難しいところは、結合してできた `url` は文字列としては正しいため、エラーではない。一見すると問題ないように見るが、実際に動かしてみて初めてわかるような問題が生じやすい。

そのため、このような構造的な文字列を扱う場合は、**専用の関数**や**専用のオブジェクト**を作ることで安全に文字列を処理する。

先ほどのような、URL 文字列の結合を安全に行うには、入力される `baseURL` 文字列の表記揺れを吸収する仕組みを作成する。

```js
// ベースURLとパスを結合した文字列を返す
function baseJoin(baseURL, pathname) {
  // 末尾に `/` がある場合は、`/` を削除してから結合する
  const stripSlashBaseURL = baseURL.replace(/\/$/, "");
  return stripSlashBaseURL + pathname;
}
// `baseURL` と `pathname` にあるリソースを取得する
function getResource(baseURL, pathname) {
  const url = baseJoin(baseURL, pathname);
  // baseURL の末尾に `/` があってもなくても同じ結果となる
  console.log(url); // => "http://example.com/resources/example.js"
  // リソースを取得する処理...
}
const baseURL = "http://example.com/resources/";
const pathname = "/example.js";
getResource(baseURL, pathname);
```

ECMAScript の範囲ではありませんが、URL やファイルパスといった典型的なものに対してはすでに専用のものがある。URL を扱うものとしてウェブ標準 API である **URL オブジェクト**、ファイルパスを扱うものとしては Node.js のコアモジュールである **Path モジュール**などがある。専用の仕組みがある場合は、直接 `+` 演算子で結合するような文字列処理は避けるべき。

### [ES2015] タグつきテンプレート関数

JavaScript では、テンプレートとなる文字列に対して一部分だけを変更する処理を行う方法として、**タグつきテンプレート関数**がある。タグつきテンプレート関数とは、`関数名`テンプレート`` という形式で記述する関数とテンプレートリテラルを合わせた表現である。

通常の関数として呼び出した場合、関数の引数にはただの文字列が渡ってくる。

```js
function tag(str) {
  // 引数 `str` にはただの文字列が渡ってくる
  console.log(str); // => "template 0 literal 1"
}
// () をつけて関数を呼び出す
tag(`template ${0} literal ${1}`);
```

しかし、`()` をつけずに `関数名`テンプレート``と記述することで、関数が受け取る引数にはタグつきテンプレート向けの値が渡ってくる。このとき、関数の第一引数にはテンプレートの中身が`${}` で区切られた文字列の配列、第二引数以降は `${}` の中に書いた式の評価結果が順番に渡される。

```js
// 呼び出し方によって受け取る引数の形式が変わる
function tag(strings, ...values) {
  // strings は文字列のパーツが `${}` で区切られた配列となる
  console.log(strings); // => ["template ", " literal ", ""]
  // values には `${}` の評価値が順番に入る
  console.log(values); // => [0, 1]
}
// () をつけずにテンプレートを呼び出す
tag`template ${0} literal ${1}`;
```

どちらも同じ関数だが、`関数名`テンプレート`` という書式で呼び出すと渡される引数が特殊な形になる。

```js
// テンプレートを順番どおりに結合した文字列を返すタグ関数
function stringRaw(strings, ...values) {
  // 配列から文字列を返すために reduce メソッドを利用する
  // result の初期値は strings[0] の値となる
  return strings.reduce((result, str, i) => {
    console.log([result, values[i - 1], str]);
    // それぞれループで次のような出力となる
    // 1度目: ["template ", 0, " literal "]
    // 2度目: ["template 0 literal ", 1, ""]
    return result + values[i - 1] + str;
  });
}
// 関数`テンプレートリテラル` という形で呼び出す
console.log(stringRaw`template ${0} literal ${1}`); // => "template 0 literal 1"
```

ここで実装した `stringRaw` タグ関数と同様のものが、`String.raw` メソッド[ES2015]として提供されている。

```js
console.log(String.raw`template ${0} literal ${1}`); // => "template 0 literal 1"
```

タグつきテンプレート関数を利用することで、テンプレートとなる文字列に対して特定の形式に変換したデータを埋め込むといったテンプレート処理が行える。

次のコードでは、テンプレート中の変数を URL エスケープしてから埋め込むタグつきテンプレート関数を定義している。`encodeURIComponent` 関数は引数の値を URL エスケープする関数。`escapeURL` では受け取った変数を `encodeURIComponent` 関数で URL エスケープしてから埋め込んでいる。

```js
// 変数をURLエスケープするタグ関数
function escapeURL(strings, ...values) {
  return strings.reduce((result, str, i) => {
    return result + encodeURIComponent(values[i - 1]) + str;
  });
}

const input = "A&B";
// escapeURL タグ関数を使ったタグつきテンプレート
const escapedURL = escapeURL`https://example.com/search?q=${input}&sort=desc`;
console.log(escapedURL); // => "https://example.com/search?q=A%26B&sort=desc"
```

タグつきテンプレートリテラルを使うことで、コンテキストに応じた処理をつけ加えることができる。この機能は JavaScript 内に HTML などの別の言語や DSL（ドメイン固有言語）を埋め込む際に利用されることが多い。
