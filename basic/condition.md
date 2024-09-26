# 条件分岐

この章ではif文やswitch文を使った条件分岐について学んでいる。  
条件分岐を使うことで、特定の条件を満たすかどうかで行う処理を変更できる。

## if文

```javascript
if (true) {
    console.log("この行は実行されます");
}
```

## else if文とelse文

```javascript
const version = "ES6";
if (version === "ES5") {
    console.log("ECMAScript 5");
} else if (version === "ES6") {
    console.log("ECMAScript 2015");
} else (version === "ES7") {
    console.log("ECMAScript 2016");
}
```

## switch文

switch文は、次のような構文で式の評価結果が指定した値である場合に行う処理を並べて書く。  
該当するものは全て実行させるので、`break;`を忘れないようにする。

```javascript
switch (式) {
    case ラベル1:
        // `式`の評価結果が`ラベル1`と一致する場合に実行する文
        break;
    case ラベル2:
        // `式`の評価結果が`ラベル2`と一致する場合に実行する文
        break;
    default:
        // どのcaseにも該当しない場合の処理
        break;
}
```

以下のif分と同じ

```javascript
const version = "ES6";
if (version === "ES5") {
    console.log("ECMAScript 5");
} else if (version === "ES6") {
    console.log("ECMAScript 2015");
} else if (version === "ES7") {
    console.log("ECMAScript 2016");
} else {
    console.log("しらないバージョンです"); 
}

// break; 後はここから実行される
```