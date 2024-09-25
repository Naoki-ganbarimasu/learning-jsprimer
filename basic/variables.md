# 変数と宣言


## const
```javascript
const 変数名 = 初期値;
// const は再宣言できない
```

## let

変数の再宣言できる。再定義できない。

```javascript
let bookTitle;
// `bookTitle`は自動的に`undefined`という値になる ＜＝初めて知った
```

### letで宣言できるもの

```javascript
let $; // OK: $が利用できる
let _title; // OK: _が利用できる
let jquery; // OK: 小文字のアルファベットが利用できる
let TITLE; // OK: 大文字のアルファベットが利用できる
let es2015; // OK: 数字は先頭以外なら利用できる
let 日本語の変数名; // OK: 一部の漢字や日本語も利用できる
```

### letで宣言できないもの

```javascript
let 1st; // NG: 数字から始まっている
let 123; // NG: 数字のみで構成されている
let let; // NG: `let`は変数宣言のために予約されているので利用できない
let if; // NG: `if`はif文のために予約されているので利用できない
```

## var

再定義、変数の再宣言できる。