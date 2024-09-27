# ループと反復処理

## while文

`while`文は条件式が`true`であるならば、反復処理を行う。

```javascript
while (条件式) {
    実行する文;
}
```

### while文の実行フロー

最初から条件式が`false`である場合は、何も実行せず`while`文は終了する。

1. **条件式** の評価結果が`true`なら次のステップへ、`false`なら終了
2. **実行する文**を実行
3. ステップ1へ戻る

```javascript
let x = 0;
console.log(`ループ開始前のxの値: ${x}`);
while (x < 10) {
    console.log(x);
    x += 1;
}
console.log(`ループ終了後のxの値: ${x}`);
```

`x`の値を増やして`false`にすることで`while`文を終了させる。

### [コラム] 無限ループ

あんまりやらないようにする。また、うまく使って計算を少なくできるようになりたいですね。

## do-while文

`do-while`文は`while`文とほとんど同じだが実行順序が異なる

```javascript
do {
    実行する文;
} while (条件式);
```

### do-while文の実行フロー

1. **実行する文**を実行
2. **条件式** の評価結果が`true`なら次のステップへ、`false`なら終了
3. ステップ1へ戻る

次のコードのように最初から条件式を満たさない場合でも、初回の実行する文が処理される。

```javascript
const x = 1000;
do {
    console.log(x); // => 1000
} while (x < 10);
```

## for文

`for`文は繰り返す範囲を指定した反復処理を書くことができる。

```javascript
for (初期化式; 条件式; 増分式) {
    実行する文;
}
```

### for文の実行フロー

1. **初期化式** で変数の宣言
2. **条件式** の評価結果が`true`なら次のステップへ、`false`なら終了
3. **実行する文** を実行
4. **増分式** で変数を更新
5. ステップ2へ戻る

```javascript
let total = 0; // totalの初期値は0
// for文の実行フロー
// iを0で初期化
// iが10未満（条件式を満たす）ならfor文の処理を実行
// iに1を足し、再び条件式の判定へ
for (let i = 0; i < 10; i++) {
    total += i + 1; // 1から10の値をtotalに加算している
}
console.log(total); // => 55
```

任意の数値が入った配列を受け取り、その合計値を返す `sum` 関数を実装してみる。  
`numbers`配列に含まれている要素を先頭から順番に変数`total`へ加算することで合計値を計算していく。

```javascript
function sum(numbers) {
    let total = 0;
    for (let i = 0; i < numbers.length; i++) {
        total += numbers[i];
    }
    return total;
}

console.log(sum([1, 2, 3, 4, 5])); // => 15
```

## 配列のforEachメソッド

```javascript
const array = [1, 2, 3];
array.forEach(currentValue => {
    console.log(currentValue);
});
// 1
// 2
// 3
// と順番に出力される
```

## 配列のsomeメソッド

`some`メソッドは、配列の各要素をテストする処理をコールバック関数として受け取る。コールバック関数が、一度でも`true`を返した時点で反復処理を終了し、`some`メソッドは`true`を返す。

テストに使える関数

```javascript
function isEven(num) {
    return num % 2 === 0;
}

const numbers = [1, 5, 10, 15, 20];
console.log(numbers.some(isEven)); // => true
```

## continue文

`continue`文は現在の反復処理を終了して、次の反復処理を行う。  
`continue`文は、`while`、`do-while`、`for`の中で使うことができる。

たとえば、`while`文の処理中で`continue`文が実行されると、現在の反復処理はその時点で終了され、次の反復処理で条件式を評価するところからループが再開される。

```javascript
while (条件式) {
    // 実行される処理
    continue; // `条件式` へ
    // これ以降の行は実行されません
}
```

```javascript
// `number`が偶数ならtrueを返す
function isEven(num) {
    return num % 2 === 0;
}
// `numbers`に含まれている偶数だけを取り出す
function filterEven(numbers) {
    const results = [];
    for (let i = 0; i < numbers.length; i++) {
        const num = numbers[i];
        // 偶数ではないなら、次のループへ
        if (!isEven(num)) {
            continue;
        }
        // 偶数を`results`に追加
        results.push(num);
    }
    return results;
}
const array = [1, 5, 10, 15, 20];
console.log(filterEven(array)); // => [10, 20]
```

```javascript
if (!isEven(num)) {
    continue;
}
// 偶数を`results`に追加
results.push(num);
```

この部分は

```javascript
if (isEven(num)) {
    results.push(num);
}
```

の方が簡単

## 配列のfilterメソッド

配列から特定の値だけを集めた新しい配列を作るには`filter`メソッドを利用できる。

```javascript
const array = [1, 2, 3, 4, 5];
// テストをパスしたものを集めた配列
const filteredArray = array.filter((currentValue, index, array) => {
    // テストをパスするならtrue、そうでないならfalseを返す
});
```

## for...in文

`for...in`文はオブジェクトのプロパティに対して、反復処理を行う。

```javascript
for (プロパティ in オブジェクト) {
    実行する文;
}
```

以下のコードでは`obj`のプロパティ名を`key`変数に代入して反復処理をしています。`obj`には、3つのプロパティ名があるため3回繰り返す。

```javascript
const obj = {
    "a": 1,
    "b": 2,
    "c": 3
};
// 注記: ループのたびに毎回新しいブロックに変数keyが定義されるため、再定義エラーが発生しない
for (const key in obj) {
    const value = obj[key];
    console.log(`key:${key}, value:${value}`);
}
// "key:a, value:1"
// "key:b, value:2"
// "key:c, value:3"
```

### オブジェクトのキーと値を列挙するコードはfor...in文を使わずに書ける

`Object.keys`メソッドは引数のオブジェクト自身が持つ列挙可能なプロパティ名の配列を返す。そのため`for...in`文とは違い、親オブジェクトのプロパティは列挙されない。

```javascript
const obj = {
    "a": 1,
    "b": 2,
    "c": 3
};
Object.keys(obj).forEach(key => {
    const value = obj[key];
    console.log(`key:${key}, value:${value}`);
});
// "key:a, value:1"
// "key:b, value:2"
// "key:c, value:3"
```

## [ES2015] for...of文

```javascript
for (variable of iterable) {
    実行する文;
}
```

`Symbol.iterator`という特別な名前のメソッドを実装したオブジェクトを**iterable**と呼ぶ。

### Iterableオブジェクトとは？

`Symbol.iterator` メソッドを持っており、このメソッドが呼び出されると「イテレーターオブジェクト」（次の要素に進む機能を持つオブジェクト）が返される。  
イテレーターオブジェクトは、`next()` メソッドを持っており、これを呼び出すと次の要素を取得できる。  
`for...of` ループなどを使って順番に要素を取り出せる。

#### 主なIterableオブジェクト

- 配列 (`Array`)
- 文字列 (`String`)
- マップ (`Map`)
- セット (`Set`)
- 引数オブジェクト (`arguments`)
- DOMコレクション（`NodeList` など）

### 配列から値を取り出して反復処理を行う

```javascript
const array = [1, 2, 3];
for (const value of array) {
    console.log(value);
}
// 1
// 2
// 3
```

### 文字列を1文字ずつ列挙

```javascript
const str = "𠮷野家";
for (const value of str) {
    console.log(value);
}
// "𠮷"
// "野"
// "家"
```

検索とかに使えそうです。