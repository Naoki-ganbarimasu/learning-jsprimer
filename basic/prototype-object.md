# プロトタイプオブジェクト

## Objectはすべての元

`Object`には、他の`Array`、`String`、`Function`などのオブジェクトとは異なる特徴がある。  
それは、他のオブジェクトはすべて`Object`を継承しているという点。  
正確には、ほとんどすべてのオブジェクトは`Object.prototype`プロパティに定義されたプロトタイプオブジェクトを継承している。  
プロトタイプオブジェクトとは、すべてのオブジェクトの作成時に自動的に追加される特殊なオブジェクト。  
`Object`のプロトタイプオブジェクトは、すべてのオブジェクトから利用できるメソッドなどを提供するベースオブジェクトとも言える。

`toString`メソッドは、`Object`のプロトタイプオブジェクトに定義されている。  
次のように、`Object.prototype.toString`メソッドの実装自体も参照できる。

```javascript
// `Object.prototype`オブジェクトに`toString`メソッドの定義がある
console.log(typeof Object.prototype.toString); // => "function"
```

`Object`のインスタンスは、この`Object.prototype`オブジェクトに定義されたメソッドやプロパティを継承する。  
つまり、オブジェクトリテラルや`new Object`でインスタンス化したオブジェクトは、`Object.prototype`に定義されたものが利用できる。  
次のコードでは、オブジェクトリテラルで作成（インスタンス化）したオブジェクトから、`Object.prototype.toString`メソッドを参照している。  
このときに、インスタンスの`toString`メソッドと`Object.prototype.toString`は同じものとなることがわかる。

```javascript
const obj = {
    "key": "value"
};
// `obj`インスタンスは`Object.prototype`に定義されたものを継承する
// `obj.toString`は継承した`Object.prototype.toString`を参照している
console.log(obj.toString === Object.prototype.toString); // => true
// インスタンスからプロトタイプメソッドを呼び出せる
console.log(obj.toString()); // => "[object Object]"
```

## プロトタイプメソッドとインスタンスメソッドの優先順位

プロトタイプメソッドと同じ名前のメソッドがインスタンスオブジェクトに定義されている場合、インスタンスに定義したメソッドが優先して呼び出される。

```javascript
// オブジェクトのインスタンスにtoStringメソッドを定義
const customObject = {
    toString() {
        return "custom value";
    }
};
console.log(customObject.toString()); // => "custom value"
```

## `Object.hasOwn`静的メソッドと`in`演算子との違い

`Object.hasOwn`静的メソッドは、指定したオブジェクト自体が指定したプロパティを持っているかを判定する。  
一方、`in`演算子はオブジェクト自身が持っていなければ、そのオブジェクトの継承元であるプロトタイプオブジェクトまで探索して持っているかを判定する。  
つまり、`in`演算子はインスタンスに実装されたメソッドなのか、プロトタイプオブジェクトに実装されたメソッドなのかを区別しない。

```javascript
const obj = {};
// `obj`というオブジェクト自体に`toString`メソッドが定義されているわけではない
console.log(Object.hasOwn(obj, "toString")); // => false
// `in`演算子は指定されたプロパティ名が見つかるまで親をたどるため、`Object.prototype`まで見にいく
console.log("toString" in obj); // => true
```

## オブジェクトの継承元を明示する`Object.create`メソッド

これまで、オブジェクトリテラルは`Object.prototype`オブジェクトを自動的に継承したオブジェクトを作成している。  
オブジェクトリテラルで作成する新しいオブジェクトは、`Object.create`メソッドを使うことで次のように書ける。

```javascript
// const obj = {} と同じ意味
const obj = Object.create(Object.prototype);
// `obj`は`Object.prototype`を継承している
// そのため、`obj.toString`と`Object.prototype.toString`は同じとなる
console.log(obj.toString === Object.prototype.toString); // => true
```

## ArrayもObjectを継承している

```javascript
// このコードはイメージです！
// `Array`コンストラクタ自身は関数でもある
const Array = function() {};
// `Array.prototype`は`Object.prototype`を継承している
Array.prototype = Object.create(Object.prototype);
// `Array`のインスタンスは、`Array.prototype`を継承している
const array = Object.create(Array.prototype);
// `array`は`Object.prototype`を継承している
console.log(array.hasOwnProperty === Object.prototype.hasOwnProperty); // => true
```

`Array`のインスタンスも`Object.prototype`を継承しているため、`Object.prototype`に定義されているメソッドを利用できる。

```javascript
const array = [];
// `Array`のインスタンス -> `Array.prototype` -> `Object.prototype`
console.log(array.hasOwnProperty === Object.prototype.hasOwnProperty); // => true
```

**`Object.prototype`はすべてのオブジェクトの親となるオブジェクトであることを覚えておく**

`Array.prototype`などもそれぞれ独自のメソッドを定義しているため、`Array`オブジェクトや`String`オブジェクトなどはそれぞれ独自のメソッドを持っている。

```javascript
const numbers = [1, 2, 3];
// `Array.prototype.toString`が定義されているため、`Object.prototype.toString`とは異なる出力形式となる
console.log(numbers.toString()); // => "1,2,3"
```

## `Object.prototype`を継承しないオブジェクト

まったく持たない本当に空のオブジェクトのこと。

```javascript
const obj = Object.create(null);
// Object.prototypeを継承しないため、hasOwnPropertyが存在しない
console.log(obj.hasOwnProperty); // => undefined
```