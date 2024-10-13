# クラス

## クラスの定義

クラスを定義するには`class`構文を使う。 クラスの定義方法にはクラス宣言文とクラス式がある。

クラス宣言文では`class`キーワードを使い、`class クラス名{ }`のようにクラスの構造を定義できる。

クラスは必ずコンストラクタを持ち、`constructor`という名前のメソッドとして定義する。 コンストラクタとは、そのクラスからインスタンスを作成する際にインスタンスに関する状態の初期化を行うメソッドである。 `constructor`メソッドに定義した処理は、クラスをインスタンス化したときに自動的に呼び出される。

```js
class MyClass {
  constructor() {
    // コンストラクタ関数の処理
    // インスタンス化されるときに自動的に呼び出される
  }
}
```

もうひとつの定義方法であるクラス式は、クラスを値として定義する方法である。 クラス式ではクラス名を省略できる。これは関数式における無名関数と同じである。

```js
const MyClass = class MyClass {
  constructor() {}
};

const AnonymousClass = class {
  constructor() {}
};
```

コンストラクタ関数内で、何も処理がない場合はコンストラクタの記述を省略できる。 省略した場合でも自動的に空のコンストラクタが定義されるため、クラスにはコンストラクタが必ず存在する。

```js
class MyClassA {
  constructor() {
    // コンストラクタの処理が必要なら書く
  }
}
// コンストラクタの処理が不要な場合は省略できる
class MyClassB {}
```

## クラスのインスタンス化

クラスは`new`演算子でインスタンスであるオブジェクトを作成できる。 `class`構文で定義したクラスからインスタンスを作成することをインスタンス化と呼ぶ。 あるインスタンスが指定したクラスから作成されたものかを判定するには`instanceof`演算子が利用できる。

```js
class MyClass {}
// `MyClass`をインスタンス化する
const myClass = new MyClass();
// 毎回新しいインスタンス(オブジェクト)を作成する
const myClassAnother = new MyClass();
// それぞれのインスタンスは異なるオブジェクト
console.log(myClass === myClassAnother); // => false
// クラスのインスタンスかどうかは`instanceof`演算子で判定できる
console.log(myClass instanceof MyClass); // => true
console.log(myClassAnother instanceof MyClass); // => true
```

クラスではインスタンスの初期化処理をコンストラクタ関数で行う。 コンストラクタ関数は`new`演算子でインスタンス化する際に自動的に呼び出される。 コンストラクタ関数内での`this`はこれから新しく作るインスタンスオブジェクトとなる。

次のコードでは、x 座標と y 座標の値を持つ`Point`というクラスを定義している。 コンストラクタ関数（`constructor`）の中でインスタンスオブジェクト（`this`）の`x`と`y`プロパティに値を代入して初期化している。

```js
class Point {
  // コンストラクタ関数の仮引数として`x`と`y`を定義
  constructor(x, y) {
    // コンストラクタ関数における`this`はインスタンスを示すオブジェクト
    // インスタンスの`x`と`y`プロパティにそれぞれ値を設定する
    this.x = x;
    this.y = y;
  }
}
```

この`Point`クラスのインスタンスを作成するには`new`演算子を使う。 `new`演算子には関数呼び出しと同じように引数を渡すことができる。 `new`演算子の引数はクラスの`constructor`メソッド（コンストラクタ関数）の仮引数に渡される。 そして、コンストラクタの中ではインスタンスオブジェクト（`this`）の初期化処理を行う。

```js
class Point {
  // 2. コンストラクタ関数の仮引数として`x`には`3`、`y`には`4`が渡る
  constructor(x, y) {
    // 3. インスタンス(`this`)の`x`と`y`プロパティにそれぞれ値を設定する
    this.x = x;
    this.y = y;
    // コンストラクタではreturn文は書かない
  }
}

// 1. コンストラクタを`new`演算子で引数とともに呼び出す
const point = new Point(3, 4);
// 4. `Point`のインスタンスである`point`の`x`と`y`プロパティには初期化された値が入る
console.log(point.x); // => 3
console.log(point.y); // => 4
```

このようにクラスからインスタンスを作成するには必ず`new`演算子を使う。

一方、クラスは通常の関数として呼ぶことができません。 これは、クラスのコンストラクタはインスタンス（`this`）を初期化する場所であり、通常の関数とは役割が異なるためである。

```js
class MyClass {
  constructor() {}
}
// クラスは関数として呼び出すことはできない
MyClass(); // => TypeError: class constructors must be invoked with |new|
```

また、コンストラクタ関数は`return`文で任意のオブジェクトを返すことが可能だが、行うべきではない。 なぜなら、クラスを`new`演算子で呼び出し、その評価結果はクラスのインスタンスを期待するのが一般的であるためである。

次のコードのようにコンストラクタで返した値が`new`演算子で呼び出した際の返り値となる。

```js
// 非推奨の例: コンストラクタで値を返すべきではない
class Point {
  constructor(x, y) {
    // `this`の代わりにただのオブジェクトを返せる
    return { x, y };
  }
}

// `new`演算子の結果はコンストラクタ関数が返したただのオブジェクト
const point = new Point(3, 4);
console.log(point); // => { x: 3, y: 4 }
// Pointクラスのインスタンスではない
console.log(point instanceof Point); // => false
```

## [Note] クラス名は大文字ではじめる

JavaScript では慣習としてクラス名には大文字ではじまる名前をつける。これは、変数名にキャメルケースを使う慣習があるのと同じで、名前自体に特別なルールがあるわけではない。クラス名を大文字にしておき、そのインスタンスは小文字で開始すれば名前が被らないという合理的な理由で好まれている。

```js
class Thing {}
const thing = new Thing();
```

## [コラム] class 構文と関数でのクラスの違い

ES2015 より前はこれらのクラスを`class`構文ではなく、関数で表現していた。 その表現方法は人によってさまざまで、これも`class`構文という統一した記法が導入された理由の 1 つである。

次のコードは、関数でクラスを実装した 1 つの例である。 この関数でのクラス表現は、継承の仕組みなどは省かれてるが、`class`構文とよく似ている。

```js
// コンストラクタ関数
const Point = function PointConstructor(x, y) {
  // インスタンスの初期化処理
  this.x = x;
  this.y = y;
};

// `new`演算子でコンストラクタ関数から新しいインスタンスを作成
const point = new Point(3, 4);
```

大きな違いとして、`class`構文で定義したクラスは関数として呼び出すことができない。 クラスは`new`演算子でインスタンス化して使うものなので、これはクラスの誤用を防ぐ仕様である。 一方、関数でのクラス表現はただの関数なので、当然関数として呼び出せる。

```js
// 関数でのクラス表現
function MyClassLike() {}
// 関数なので関数として呼び出せる
MyClassLike();

// `class`構文でのクラス
class MyClass {}
// クラスは関数として呼び出すと例外が発生する
MyClass(); // => TypeError: class constructors must be invoked with |new|
```

このように、関数でクラスのようなものを実装した場合には、関数として呼び出せてしまう問題がある。 このような問題を避けるためにもクラスは`class`構文を使って実装する。

##　クラスのプロトタイプメソッドの定義

クラスの動作はメソッドによって定義できる。 `constructor`メソッドは初期化時に呼ばれる特殊なメソットだが、`class`構文ではクラスに対して自由にメソッドを定義できる。 このクラスに定義したメソッドは作成したインスタンスが持つ動作となる。

次のように`class`構文ではクラスに対してメソッドを定義できる。 メソッドの中からクラスのインスタンスを参照するには、`constructor`メソッドと同じく`this`を使う。 このクラスのメソッドにおける`this`は「関数と this」の章で学んだメソッドと同じくベースオブジェクトを参照する。

```js
class クラス {
  メソッド() {
    // ここでの`this`はベースオブジェクトを参照
  }
}

const インスタンス = new クラス();
// メソッド呼び出しのベースオブジェクト(`this`)は`インスタンス`となる
インスタンス.メソッド();
```

クラスのプロトタイプメソッド定義では、オブジェクトにおけるメソッドとは異なり`key : value`のように`:`区切りでメソッドを定義できないことに注意してください。 つまり、次のような書き方は構文エラー（`SyntaxError`）となる。

```js
// クラスでは次のようにメソッドを定義できない
class クラス {
   // SyntaxError
   メソッド: () => {}
   // SyntaxError
   メソッド: function(){}
}
```

このメソッド定義の構文でクラスに対して定義したメソッドは、クラスの各インスタンスから共有されるメソッドとなる。 このインスタンス間で共有されるメソッドのことをプロトタイプメソッドと呼ぶ。

次のコードでは、`Counter`クラスに`increment`メソッドを定義している。 このときの`Counter`クラスのインスタンスは、それぞれ別々の状態（`count`プロパティ）を持つ。

```js
class Counter {
  constructor() {
    this.count = 0;
  }
  // `increment`メソッドをクラスに定義する
  increment() {
    // `this`は`Counter`のインスタンスを参照する
    this.count++;
  }
}
const counterA = new Counter();
const counterB = new Counter();
// `counterA.increment()`のベースオブジェクトは`counterA`インスタンス
counterA.increment();
// 各インスタンスの持つプロパティ(状態)は異なる
console.log(counterA.count); // => 1
console.log(counterB.count); // => 0
```

このときの`increment`メソッドはプロトタイプメソッドとして定義されている。 プロトタイプメソッドは各インスタンス間（`counterA`と`counterB`）で共有される。 そのため、次のように各インスタンスの`increment`メソッドの参照先は同じとなっていることがわかる。

```js
class Counter {
  constructor() {
    this.count = 0;
  }
  increment() {
    this.count++;
  }
}
const counterA = new Counter();
const counterB = new Counter();
// 各インスタンスオブジェクトのメソッドは共有されている(同じ関数を参照している)
console.log(counterA.increment === counterB.increment); // => true
```

プロトタイプメソッドがなぜインスタンス間で共有されているのかは、クラスの継承の仕組みと密接に関係している。 プロトタイプメソッドの仕組みについては後ほど解説する。

ここでは、次のような構文でクラスにメソッドを定義すると、各インスタンスで共有されるプロトタイプメソッドとして定義されるということが理解できていれば問題ない。

```js
class クラス {
  メソッド() {
    // このメソッドはプロトタイプメソッドとして定義される
  }
}
```

## クラスのアクセッサプロパティの定義

クラスに対してメソッドを定義できるが、メソッドは `インスタンス名.メソッド名()` のように呼び出す必要がある。 クラスでは、プロパティの参照（getter）、プロパティへの代入（setter）時に呼び出される特殊なメソッドを定義できる。 このメソッドはプロパティのように振る舞うためアクセッサプロパティと呼ばれる。

次のコードでは、プロパティの参照（getter）、プロパティへの代入（setter）に対するアクセッサプロパティを定義している。 アクセッサプロパティはメソッド名（プロパティ名）の前に `get` または `set` をつけるだけである。 `getter`（get）には仮引数はないが、必ず値を返す必要がある。 `setter`（set）の仮引数にはプロパティへ代入する値が入るが、値を返す必要はない。

```js
class クラス {
  // getter
  get プロパティ名() {
    return 値;
  }
  // setter
  set プロパティ名(仮引数) {
    // setterの処理
  }
}
const インスタンス = new クラス();
インスタンス.プロパティ名; // getterが呼び出される
インスタンス.プロパティ名 = 値; // setterが呼び出される
```

次のコードでは、`NumberWrapper`クラスの`value`プロパティをアクセッサプロパティとして定義している。 `value`プロパティへアクセスした際にそれぞれ定義した`getter`と`setter`が呼ばれているのがわかる。 このアクセッサプロパティで実際に読み書きされているのは、`NumberWrapper`インスタンスの `_value` プロパティとなる。

```js
class NumberWrapper {
  constructor(value) {
    this._value = value;
  }
  // `_value`プロパティの値を返すgetter
  get value() {
    console.log("getter");
    return this._value;
  }
  // `_value`プロパティに値を代入するsetter
  set value(newValue) {
    console.log("setter");
    this._value = newValue;
  }
}

const numberWrapper = new NumberWrapper(1);
// "getter"とコンソールに表示される
console.log(numberWrapper.value); // => 1
// "setter"とコンソールに表示される
numberWrapper.value = 42;
// "getter"とコンソールに表示される
console.log(numberWrapper.value); // => 42
```

### [コラム] \_（アンダーバー）から始まるプロパティ名

`NumberWrapper`の`value`のアクセッサプロパティで実際に読み書きしているのは、 `_value` プロパティである。 このように、外から直接読み書きしてほしくないプロパティを `_` （アンダーバー）から始まる名前にするのはただの習慣であるため、構文としての意味はない。

ECMAScript 2022 から、外から直接読み書きしてほしくないプライベートなプロパティを定義する Private クラスフィールド構文が追加されました。 Private クラスフィールド構文では `#`（ハッシュ）記号をプロパティ名の前につける。 そのため、外から直接読み書きしてほしくないプロパティを `_` からはじめるという慣習は、Private クラスフィールド構文の利用が進むにつれて使われない。

Private クラスフィールド構文については、この後に解説する

### Array.prototype.length をアクセッサプロパティで再現する

`getter` や `setter` を利用しないと実現が難しいものとして、`Array.prototype.length` プロパティがある。 `Array` の `length` プロパティへ値を代入すると、そのインデックス以降の要素は自動的に削除される仕様になっている。

次のコードでは、配列の要素数（`length` プロパティ）を小さくすると配列の要素が削除されている。

```js
const array = [1, 2, 3, 4, 5];
// 要素数を減らすと、インデックス以降の要素が削除される
array.length = 2;
console.log(array.join(", ")); // => "1, 2"
// 要素数だけを増やしても、配列の中身は空要素が増えるだけ
array.length = 5;
console.log(array.join(", ")); // => "1, 2, , , "
```

この `length` プロパティの挙動を再現する `ArrayLike` クラスを実装してみる。 `Array` の `length` プロパティは、`length` プロパティへ値を代入した際に次のようなことを行っている。

1. 現在要素数より小さな要素数が指定された場合、その要素数を変更し、配列の末尾の要素を削除する
2. 現在要素数より大きな要素数が指定された場合、その要素数だけを変更し、配列の実際の要素はそのままにする

`ArrayLike` の `length` プロパティの `setter` で要素の追加や削除を実装することで、配列のような `length` プロパティを実装できる。

```js
/**
 * 配列のようなlengthを持つクラス
 */
class ArrayLike {
  constructor(items = []) {
    this._items = items;
  }

  get items() {
    return this._items;
  }

  get length() {
    return this._items.length;
  }

  set length(newLength) {
    const currentItemLength = this.items.length;
    // 現在要素数より小さな`newLength`が指定された場合、指定した要素数となるように末尾を削除する
    if (newLength < currentItemLength) {
      this._items = this.items.slice(0, newLength);
    } else if (newLength > currentItemLength) {
      // 現在要素数より大きな`newLength`が指定された場合、指定した要素数となるように末尾に空要素を追加する
      this._items = this.items.concat(new Array(newLength - currentItemLength));
    }
  }
}

const arrayLike = new ArrayLike([1, 2, 3, 4, 5]);
// 要素数を減らすとインデックス以降の要素が削除される
arrayLike.length = 2;
console.log(arrayLike.items.join(", ")); // => "1, 2"
// 要素数を増やすと末尾に空要素が追加される
arrayLike.length = 5;
console.log(arrayLike.items.join(", ")); // => "1, 2, , , "
```

このようにアクセッサプロパティでは、プロパティのようでありながら実際にアクセスした際には他のプロパティと連動する動作を実現できる。

## [ES2022] Public クラスフィールド

クラスでは、`constructor` メソッドの中でクラスの状態であるインスタンスのプロパティを初期化することを紹介しました。 先ほども紹介した`Counter`クラスでは、`constructor` メソッドの中で `count` プロパティの初期値を 0 として定義している。

```js
class Counter {
  constructor() {
    this.count = 0;
  }
  increment() {
    this.count++;
  }
}
```

この`Counter`では`new`演算子で何も引数を渡すことなく初期化するため、`constructor`メソッドには仮引数を定義していません。 このような場合でも、`constructor`メソッドを書かないとプロパティの初期化ができないためわずらわしいという問題がありました。

ES2022 で、クラスのインスタンスが持つプロパティの初期化をわかりやすく宣言的にする構文として、クラスフィールド構文が追加されました。

クラスフィールドは、クラスのインスタンスが持つプロパティを定義する次のような構文である。

```js
class クラス {
  プロパティ名 = プロパティの初期値;
}
```

クラスフィールドを使って先ほどの Counter クラスを書き直してみると次のようになる。 count プロパティをクラスフィールドとして定義して、その初期値は 0 としている。

```js
class Counter {
  count = 0;
  increment() {
    this.count++;
  }
}
const counter = new Counter();
counter.increment();
console.log(counter.count); // => 1
```

クラスフィールドで定義するのは、クラスのインスタンスが持つプロパティである。 そのため、constructor メソッドの中で this.count = 0 のように定義した場合と結果的にはほとんど同じ意味となる。 クラスフィールドで定義したプロパティは、クラス内から他のプロパティと同じように this.プロパティ名で参照できる。

クラスフィールドは constructor メソッドでの初期化と併用が可能である。 次のコードでは、クラスフィールドと constructor メソッドでそれぞれインスタンスのプロパティを定義している。

```js
// 別々のプロパティ名がそれぞれ定義される
class MyClass {
  publicField = 1;
  constructor(arg) {
    this.property = arg;
  }
}
const myClass = new MyClass(2);
console.log(myClass.publicField); // => 1
console.log(myClass.property); // => 2
```

また、クラスフィールドでの初期化処理が行われ、そのあと constructor でのプロパティの定義という処理順となる。 そのため、同じプロパティ名への定義がある場合は、constructor メソッド内での定義でプロパティは上書きされる。

```js
// 同じプロパティ名の場合は、constructorでの代入が後となる
class OwnClass {
  publicField = 1;
  constructor(arg) {
    this.publicField = arg;
  }
}
const ownClass = new OwnClass(2);
console.log(ownClass.publicField); // => 2
```

この publicField プロパティのように、クラスの外からアクセスできるプロパティを定義するクラスフィールドを Public クラスフィールドと呼ぶ。

## クラスフィールドを使ってプロパティの存在を宣言する

クラスフィールドでは、プロパティの初期値は省略可能となっている。 そのため、次のように初期値を省略した Public クラスフィールドも定義できる。

```js
class MyClass {
  // myPropertyはundefinedで初期化される
  myProperty;
}
```

このときの`myProperty`は`undefined`で初期化される。 この初期値を省略したクラスフィールドの定義は、クラスのインスタンスが持つプロパティを明示するために利用できる。

次の`Loader`クラスは、`load`メソッドを呼び出すまでは、`loadedContent`プロパティの値は`undefined`である。 クラスフィールドを使えば、`Loader`クラスのインスタンスは、`loadedContent`というプロパティを持っていることを宣言的に表現できる。

```js
class Loader {
  loadedContent;
  load() {
    this.loadedContent = "読み込んだコンテンツ内容";
  }
}
```

JavaScript では、オブジェクトのプロパティは初期化時に存在していなくても、後から代入すれば作成できてしまう。 そのため、次のように`Loader`クラスを実装しても意味は同じである。

```js
class Loader {
  load() {
    this.loadedContent = "読み込んだコンテンツ内容";
  }
}
```

しかし、このように実装してしまうと`Loader`クラスを利用する側は、`loadedContent`プロパティの存在を`load`メソッドの中まで読まないとわからないという問題がある。 これに対して、クラスフィールドを使って「`Loader`クラスは`loadedContent`というプロパティを持っている」ということを宣言的に表現できる。 宣言的にプロパティを定義することで、エディターでのコード補完が可能になったり、コードを読む人に優しいというメリットがある。

## クラスフィールドでの`this`はクラスのインスタンスを示す

クラスフィールドの初期値には任意の式が書け、`this`も利用できる。 クラスフィールドでの`this`は、そのクラスのインスタンスを参照する。

次のコードでは、`up`フィールドの初期値に`increment`メソッドを指定している。 JavaScript では関数も値として扱えるため、`up`メソッドを呼び出すと`increment`メソッドが呼び出される。

```js
class Counter {
  count = 0;
  // upはincrementメソッドを参照している
  up = this.increment;
  increment() {
    this.count++;
  }
}
const counter = new Counter();
counter.up(); // 結果的にはincrementメソッドが呼び出される
console.log(counter.count); // => 1
```

クラスフィールドでの`this`は、Arrow Function と組み合わせると強力である。

次のコードでは、`up`メソッドを Arrow Function として定義し、関数内では`this.increment`メソッドを呼び出している。 Arrow Function で定義した関数における`this`は、どのような呼び出し方をしても変化しません（「Arrow Function でコールバック関数を扱う」を参照）。 そのため、`up`メソッドはどのような呼び方をした場合でも`this`がクラスのインスタンスとなるため、確実に`increment`メソッドを呼び出せる。

```js
class Counter {
  count = 0;
  // クラスフィールドでの`this`はクラスのインスタンスとなる
  // upメソッドは、クラスのインスタンスに定義される
  up = () => {
    this.increment();
  };
  increment() {
    this.count++;
  }
}
const counter = new Counter();
// Arrow Functionなので、thisはクラスのインスタンスに固定されている
const up = counter.up;
up();
console.log(counter.count); // => 1
// 通常のメソッド定義では、`this`が`undefined`となってしまうため例外が発生する
const increment = counter.increment;
increment(); // Error: Uncaught TypeError: this is undefined
```

## [コラム] クラスフィールドとインスタンスのプロパティの違い

クラスフィールドで定義したプロパティやメソッドは、クラスのインスタンスにプロパティとして定義される。 そのため、クラスフィールドは、`constructor`の中で`this`に対してプロパティを追加するのと意味的にはほぼ同じで、見た目がわかりやすくなった構文と捉えることができる。

```js
class ExampleClass {
  fieldMethod = () => {
    console.log("クラスフィールドで定義されたメソッド");
  };
  constructor() {
    this.propertyMethod = () => {
      console.log("インスタンスにプロパティとして定義されたメソッド");
    };
  }
}
```

しかし、厳密にはこのふたつのプロパティ定義には異なる点はある。 次のように、クラスフィールドと`constructor`の中で`this`に追加するプロパティ名に対する`setter`を定義してみるとこの違いがわかる。

```js
class ExampleClass {
  field = "フィールド";
  constructor() {
    this.property = "コンストラクタ";
  }
  // クラスフィールド名に対応するsetter
  set field(value) {
    console.log("fieldで定義された値", value);
  }
  // thisのプロパティ名に対応するsetter
  set property(value) {
    console.log("constructorで代入された値", value);
  }
}
// set fieldは呼び出されない
// 一方で、set propertyは呼び出される
const example = new ExampleClass();
```

<!-- どういうこと？ -->

クラスフィールド名に対する`setter`は呼び出されないのに対して、`this.property`への代入に対する`setter`は呼び出されている。 これは、クラスフィールドは`=`を使った代入で定義されるのではなく、`Object.defineProperty`メソッドを使ってプロパティが定義されるという違いがある。 `Object.defineProperty`を使ったプロパティの定義では、`setter`は無視してプロパティが定義される。 `setter`は`=`での代入に反応する。そのため、`constructor`の中での`this.property`への代入に対しては`setter`が呼び出される。

同じプロパティの定義であっても、プロパティの定義の仕組みが微妙に異なる点から、このような挙動の違いが存在している。 しかし、この違いを意識するようなコードを書くことは避けたほうが安全である。 実際に見た目からこの違いを意識するのは難しく、それを意識させるようなコードは複雑性が高いためである。

## [ES2022] Private クラスフィールド

クラスフィールド構文で次のように書くと、定義したプロパティはクラスをインスタンス化した後に外からも参照できる。 そのため、Public クラスフィールドと呼ばれる。

```js
class クラス {
  プロパティ名 = プロパティの初期値;
}
```

一方で外からアクセスされたくないインスタンスのプロパティも存在する。 そのようなプライベートなプロパティを定義する構文も ES2022 で追加されている。

Private クラスフィールドは、次のように`#`をフィールド名の前につけたクラスフィールドを定義する。

```js
class クラス {
  // プライベートなプロパティは#をつける
  #フィールド名 = プロパティの初期値;
}
```

定義した Private クラスフィールドは、`this.#フィールド名`で参照できる。

```js
class PrivateExampleClass {
  #privateField = 42;
  dump() {
    // Privateクラスフィールドはクラス内からのみ参照できる
    console.log(this.#privateField); // => 42
  }
}
const privateExample = new PrivateExampleClass();
privateExample.dump();
```

もう少し具体的な Private クラスフィールドの使い方を見ていきる。 アクセッサプロパティの例でも登場した`NumberWrapper`を Private クラスフィールドを使って書き直してみる。 元々の`NumberWrapper`クラスでは、`_value`プロパティに実際の値を読み書きしていました。 この場合、`_value`プロパティは、外からもアクセスできてしまうため、定義した`getter`と`setter`が無視できてしまいる。

```js
class NumberWrapper {
  // Publicクラスフィールドなのでクラスの外からアクセスができる
  _value;
  constructor(value) {
    this._value = value;
  }
  // `_value`プロパティの値を返すgetter
  get value() {
    return this._value;
  }
  // `_value`プロパティに値を代入するsetter
  set value(newValue) {
    this._value = newValue;
  }
}
const numberWrapper = new NumberWrapper(1);
// _valueプロパティは外からもアクセスできる
console.log(numberWrapper._value); // => 1
```

Private クラスフィールドでは、外からアクセスされたくないプロパティを`#`をつけてクラスフィールドとして定義する。 次のコードでは、`#value`はプライベートプロパティとなっているため、構文エラーが発生し外からアクセスできなくなることが確認できる。 Private クラスフィールドを使うことで、クラスを利用する際は`getter`と`setter`を経由しないと`#value`を参照できなくなりました。

```js
class NumberWrapper {
  // valueはPrivateクラスフィールドとして定義
  #value;
  constructor(value) {
    this.#value = value;
  }
  // `#value`フィールドの値を返すgetter
  get value() {
    return this.#value;
  }
  // `#value`フィールドに値を代入するsetter
  set value(newValue) {
    this.#value = newValue;
  }
}

const numberWrapper = new NumberWrapper(1);
// クラスの外からPrivateクラスフィールドには直接はアクセスできない
console.log(numberWrapper.#value); // => SyntaxError: reference to undeclared private field or method #value
```

Private クラスフィールドを使うことで、クラスの外からアクセスさせたくないプロパティを宣言できる。 これは、実装したクラスの意図しない使われ方を防いだり、クラスの外からプロパティの状態を直接書き換えるといった行為を防げる。

また、Private クラスフィールドでは、途中から値が入る場合でもフィールドの宣言が必須となっている。 次のコードでは、`#loadedContent`に実際に値が入るのは`load`メソッドが呼び出されたときである。 Public クラスフィールドではフィールドの定義は省略可能でしたが、Private クラスフィールドでは`#loadedContent`フィールドの定義が必須となっている。 言い換えると、Private クラスフィールドでは、クラスを定義した段階でクラスに存在するすべての Private クラスフィールドを明示する必要がある。

```js
class PrivateLoader {
  // 途中で値が入る場合でも最初に`undefined`で初期化されるフィールドの定義が必須
  #loadedContent;
  load() {
    this.#loadedContent = "読み込んだコンテンツ内容";
  }
}
```

## 静的メソッド

インスタンスメソッドは、クラスをインスタンス化して利用する。 一方、クラスをインスタンス化せずに利用できる静的メソッド（クラスメソッド）もある。

静的メソッドの定義方法はメソッド名の前に、`static`をつけるだけである。

```js
class クラス {
  static メソッド() {
    // 静的メソッドの処理
  }
}
// 静的メソッドの呼び出し
クラス.メソッド();
```

次のコードでは、配列をラップする`ArrayWrapper`というクラスを定義している。 `ArrayWrapper`はコンストラクタの引数として配列を受け取って初期化している。 このクラスに配列ではなく要素そのものを引数に受け取ってインスタンス化できる`ArrayWrapper.of`という静的メソッドを定義する。

```js
class ArrayWrapper {
  // new演算子で引数が渡されなかった場合の初期値は空配列
  constructor(array = []) {
    this.array = array;
  }

  // rest parametersとして要素を受けつける
  static of(...items) {
    return new ArrayWrapper(items);
  }

  get length() {
    return this.array.length;
  }
}

// 配列を引数として渡している
const arrayWrapperA = new ArrayWrapper([1, 2, 3]);
// 要素を引数として渡している
const arrayWrapperB = ArrayWrapper.of(1, 2, 3);
console.log(arrayWrapperA.length); // => 3
console.log(arrayWrapperB.length); // => 3
```

クラスの静的メソッドにおける`this`は、そのクラス自身を参照する。 そのため、先ほどのコードは`new ArrayWrapper`の代わりに`new this`と書くこともできる。

```js
class ArrayWrapper {
  constructor(array = []) {
    this.array = array;
  }

  static of(...items) {
    // `this`は`ArrayWrapper`を参照する
    return new this(items);
  }

  get length() {
    return this.array.length;
  }
}

const arrayWrapper = ArrayWrapper.of(1, 2, 3);
console.log(arrayWrapper.length); // => 3
```

このように静的メソッドでの`this`はクラス自身を参照するため、クラスのインスタンスは参照できません。 そのため静的メソッドは、クラスのインスタンスを作成する処理やクラスに関係する処理を書くために利用される。

## [ES2022] 静的クラスフィールド

ES2022 で追加されたクラスフィールドでは、インスタンスではなくクラス自体に定義する静的クラスフィールドも利用できる。

静的クラスフィールドは、フィールドの前に`static`をつけるだけである。 静的クラスフィールドで定義したプロパティは、クラス自体のプロパティとして定義される。 次のコードでは、Public 静的クラスフィールドを使って`Colors`クラス自体にプロパティを定義している。

```js
class Colors {
  static GREEN = "緑";
  static RED = "赤";
  static BLUE = "青";
}
// クラスのプロパティとして参照できる
console.log(Colors.GREEN); // => "緑"
```

また、Private クラスフィールドも静的に利用できる。 Private 静的クラスフィールドは、クラス自体にプロパティを定義したいが、そのプロパティを外から参照されたくない場合に利用する。 Private 静的クラスフィールドはフィールドの前に、`static`をつけるだけである。

```js
class MyClass {
  static #privateClassProp = "This is private";
  static outputPrivate() {
    // クラス内からはPrivate静的クラスフィールドで定義したプロパティを参照できる
    console.log(this.#privateClassProp);
  }
}
MyClass.outputPrivate();
```

## プロトタイプに定義したメソッドとインスタンスに定義したメソッドの違い

ここまでで、プロトタイプメソッドの定義とクラスフィールドを使ったインスタンスに対するメソッドの定義の 2 種類を見てきました。 プロトタイプメソッドの定義方法は、メソッドをプロトタイプオブジェクトという特殊なオブジェクトに定義する。 一方で、クラスフィールドで定義したメソッドは、クラスのインスタンスに対してメソッドを定義する。

どちらのメソッド定義方法でも、`new`演算子でインスタンス化したオブジェクトからメソッドを呼び出すことができる点は同じである。

```js
class ExampleClass {
  // クラスフィールドを使い、インスタンスにメソッドを定義
  instanceMethod = () => {
    console.log("インスタンスメソッド");
  };
  // メソッド構文を使い、プロトタイプオブジェクトにメソッドを定義
  prototypeMethod() {
    console.log("プロトタイプメソッド");
  }
}
const example = new ExampleClass();
// どちらのメソッドもインスタンスから呼び出せる
example.instanceMethod();
example.prototypeMethod();
```

しかしこの 2 つのメソッドの定義方法は、メソッドの定義先となるオブジェクトが異なる。

まず、この 2 種類のメソッドがそれぞれ別の場所へと定義されていることを見ていきる。 次のコードでは、`ConflictClass`クラスに`method`という同じ名前のメソッドをプロトタイプメソッドとインスタンスに対してそれぞれ定義している。

```js
class ConflictClass {
  // インスタンスオブジェクトに`method`を定義
  method = () => {
    console.log("インスタンスオブジェクトのメソッド");
  };

  // クラスのプロトタイプメソッドとして`method`を定義
  method() {
    console.log("プロトタイプのメソッド");
  }
}

const conflict = new ConflictClass();
conflict.method(); // どちらの`method`が呼び出される？
```

結論から述べると、この場合はインスタンスオブジェクトに定義した`method`が呼び出される。 このとき、インスタンスの`method`プロパティを`delete`演算子で削除すると、今度はプロトタイプメソッドの`method`が呼び出される。

```js
class ConflictClass {
  // インスタンスオブジェクトに`method`を定義
  method = () => {
    console.log("インスタンスオブジェクトのメソッド");
  };

  method() {
    console.log("プロトタイプメソッド");
  }
}

const conflict = new ConflictClass();
conflict.method(); // "インスタンスオブジェクトのメソッド"
// インスタンスの`method`プロパティを削除
delete conflict.method;
conflict.method(); // "プロトタイプメソッド"
```

- プロトタイプメソッドとインスタンスオブジェクトのメソッドは上書きされずにどちらも定義されている
- インスタンスオブジェクトのメソッドがプロトタイプオブジェクトのメソッドよりも優先して呼ばれている

どちらも注意深く意識しないと気づきにくいが、この挙動は JavaScript の重要な仕組みであるため理解することは重要である。

この挙動は**プロトタイプオブジェクト**と呼ばれる特殊なオブジェクトと、**プロトタイプチェーン**と呼ばれる仕組みで成り立っている。 どちらもプロトタイプとついていることからわかるように、2 つで 1 組のような仕組みである。

次のセクションでは、プロトタイプオブジェクトとプロトタイプチェーンとはどのような仕組みなのかを見ていきる。

## プロトタイプオブジェクト

プロトタイプメソッドとインスタンスオブジェクトのメソッドを同時に定義しても、互いのメソッドは上書きされるわけではない。 なぜなら、プロトタイプメソッドはプロトタイプオブジェクトへ、インスタンスオブジェクトのメソッドはインスタンスオブジェクトへそれぞれ定義されるためである。

プロトタイプオブジェクトとは、JavaScript の関数オブジェクトの`prototype`プロパティに自動的に作成される特殊なオブジェクトである。 クラスも一種の関数オブジェクトであるため、自動的に`prototype`プロパティにプロトタイプオブジェクトが作成されている。

次のコードでは、関数やクラス自身の`prototype`プロパティに、プロトタイプオブジェクトが自動的に作成されていることがわかる。

```js
function fn() {}
// `prototype`プロパティにプロトタイプオブジェクトが存在する
console.log(typeof fn.prototype === "object"); // => true

class MyClass {}
// `prototype`プロパティにプロトタイプオブジェクトが存在する
console.log(typeof MyClass.prototype === "object"); // => true
```

`class`構文のメソッド定義は、このプロトタイプオブジェクトのプロパティとして定義される。

次のコードでは、クラスのメソッドがプロトタイプオブジェクトに定義されていることを確認できる。 また、クラスには`constructor`メソッド（コンストラクタ）が必ず定義される。 この`constructor`メソッドもプロトタイプオブジェクトに定義されており、この`constructor`プロパティはクラス自身を参照する
。

```js
class MyClass {
  method() {}
}

console.log(typeof MyClass.prototype.method === "function"); // => true
// クラスのconstructorはクラス自身を参照する
console.log(MyClass.prototype.constructor === MyClass); // => true
```

このように、プロトタイプメソッドはプロトタイプオブジェクトに定義され、インスタンスオブジェクトのメソッドとは異なるオブジェクトに定義されている。そのため、それぞれの方法でメソッドを定義しても、上書きされることはない。

## プロトタイプチェーン

`class`構文で定義したプロトタイプメソッドはプロトタイプオブジェクトに定義される。 しかし、インスタンス（オブジェクト）にはメソッドが定義されていないのに、インスタンスからクラスのプロトタイプメソッドを呼び出せる。

```js
class MyClass {
  method() {
    console.log("プロトタイプのメソッド");
  }
}
const instance = new MyClass();
instance.method(); // "プロトタイプのメソッド"
```

インスタンスからプロトタイプメソッドを呼び出せるのは**プロトタイプチェーン**と呼ばれる仕組みによるものである。 プロトタイプチェーンは 2 つの処理から成り立つ。

1. インスタンス作成時に、インスタンスの`[[Prototype]]`内部プロパティへプロトタイプオブジェクトの参照を保存する処理
2. インスタンスからプロパティ（またはメソッド）を参照するときに、`[[Prototype]]`内部プロパティまで探索する処理

### インスタンス作成とプロトタイプチェーン

クラスから`new`演算子によってインスタンスを作成する際に、インスタンスにはクラスのプロトタイプオブジェクトへの参照が保存される。 このとき、インスタンスからクラスのプロトタイプオブジェクトへの参照は、インスタンスオブジェクトの`[[Prototype]]`という内部プロパティに保存される。

`[[Prototype]]`内部プロパティは ECMAScript の仕様で定められた内部的な表現であるため、通常のプロパティのようにはアクセスできません。 ここでは説明のために、`[[プロパティ名]]`という書式で ECMAScript の仕様上に存在する内部プロパティを表現している。

`[[Prototype]]`内部プロパティへプロパティのようにはアクセスできませんが、`Object.getPrototypeOf`メソッドで`[[Prototype]]`内部プロパティを参照できる。

次のコードでは、`instance`オブジェクトの`[[Prototype]]`内部プロパティを取得している。 その取得した結果がクラスのプロトタイプオブジェクトを参照していることを確認できる。

```js
class MyClass {
  method() {
    console.log("プロトタイプのメソッド");
  }
}
const instance = new MyClass();
// `instance`の`[[Prototype]]`内部プロパティは`MyClass.prototype`と一致する
const MyClassPrototype = Object.getPrototypeOf(instance);
console.log(MyClassPrototype === MyClass.prototype); // => true
```

ここで重要なのは、インスタンスはどのクラスから作られたかやそのクラスのプロトタイプオブジェクトを知っているということである。

### [Note] `[[Prototype]]`内部プロパティを読み書きする

- `Object.getPrototypeOf(オブジェクト)`でオブジェクトの`[[Prototype]]`を読み取ることができる。
- 一方、`Object.setPrototypeOf(オブジェクト, プロトタイプオブジェクト)`でオブジェクトの`[[Prototype]]`にプロトタイプオブジェクトを設定できる。
- また、`[[Prototype]]`内部プロパティを通常のプロパティのように扱える`__proto__`という特殊なアクセッサプロパティが存在する。

しかし、これらの`[[Prototype]]`内部プロパティを直接読み書きすることは通常の用途では行いません。 また、既存のビルトインオブジェクトの動作なども変更できるため、不用意に扱うべきではないでしょう。

## プロパティの参照とプロトタイプチェーン

プロトタイプオブジェクトのプロパティがどのようにインスタンスから参照されるかを見ていきる。

オブジェクトのプロパティを参照するときに、オブジェクト自身がプロパティを持っていない場合でも、そこで探索が終わるわけではない。 オブジェクトの`[[Prototype]]`内部プロパティ（仕様上の内部的なプロパティ）の参照先であるプロトタイプオブジェクトに対しても探索を続ける。 これは、スコープに指定した識別子の変数がなかった場合に外側のスコープへと探索する**スコープチェーン**と良く似た仕組みである。

つまり、オブジェクトがプロパティを探索するときは次のような順番で、それぞれのオブジェクトを調べる。すべてのオブジェクトにおいて見つからなかった場合の結果は`undefined`を返す。

1. `instance`オブジェクト自身
2. `instance`オブジェクトの`[[Prototype]]`の参照先（プロトタイプオブジェクト）
3. どこにもなかった場合は`undefined`

次のコードでは、インスタンスオブジェクト自身は`method`プロパティを持っていません。そのため、実際に参照しているのはクラスのプロトタイプオブジェクトの`method`プロパティである。

```js
class MyClass {
  method() {
    console.log("プロトタイプのメソッド");
  }
}
const instance = new MyClass();
// インスタンスには`method`プロパティがないため、プロトタイプオブジェクトの`method`が参照される
instance.method(); // "プロトタイプのメソッド"
// `instance.method`の参照はプロトタイプオブジェクトの`method`と一致する
const Prototype = Object.getPrototypeOf(instance);
console.log(instance.method === Prototype.method); // => true
```

このように、インスタンスオブジェクトに`method`が定義されていなくても、クラスのプロトタイプオブジェクトの`method`を呼び出すことができる。 このプロパティを参照する際に、オブジェクト自身から`[[Prototype]]`内部プロパティへと順番に探す仕組みのことを**プロトタイプチェーン**と呼ぶ。

プロトタイプチェーンの仕組みを疑似的なコードとして表現すると、次のような動きをしている。

```js
// プロトタイプチェーンの動作の疑似的なコード
class MyClass {
  method() {
    console.log("プロトタイプのメソッド");
  }
}
const instance = new MyClass();
// `instance.method()`を実行する場合
// 次のような呼び出し処理が行われている
// インスタンスが`method`プロパティを持っている場合
if (Object.hasOwn(instance, "method")) {
  instance.method();
} else {
  // インスタンスの`[[Prototype]]`の参照先（`MyClass`のプロトタイプオブジェクト）を取り出す
  const prototypeObject = Object.getPrototypeOf(instance);
  // プロトタイプオブジェクトが`method`プロパティを持っている場合
  if (Object.hasOwn(prototypeObject, "method")) {
    // `this`はインスタンス自身を指定して呼び出す
    prototypeObject.method.call(instance);
  }
}
```

プロトタイプチェーンの仕組みによって、プロトタイプオブジェクトに定義したプロトタイプメソッドをインスタンスから呼び出せる。

普段は、プロトタイプオブジェクトやプロトタイプチェーンといった仕組みを意識する必要はない。 `class`構文はこのようなプロトタイプを意識せずにクラスを利用できるように導入された構文である。

## 継承

`extends`キーワードを使うことで既存のクラスを継承できる。 継承とは、クラスの構造や機能を引き継いだ新しいクラスを定義することである。

### 継承したクラスの定義

`extends`キーワードを使って既存のクラスを継承した新しいクラスを定義してみる。 `class`構文の右辺に`extends`キーワードで継承元となる親クラス（基底クラス）を指定することで、 親クラスを継承した子クラス（派生クラス）を定義できる。

```js
class 子クラス extends 親クラス {}
```

次のコードでは、`Parent`クラスを継承した`Child`クラスを定義している。 子クラスである`Child`クラスのインスタンス化は通常のクラスと同じく`new`演算子を使って行いる。

```js
class Parent {}
class Child extends Parent {}
const instance = new Child();
```

## `super`

`extends`を使って定義した子クラスから親クラスを参照するには`super`というキーワードを利用する。 もっともシンプルな`super`を使う例としてコンストラクタの処理を見ていきる。

`class`構文でも紹介しましたが、クラスは必ず`constructor`メソッド（コンストラクタ）を持つ。 これは、継承した子クラスでも同じである。

次のコードでは、`Parent`クラスを継承した`Child`クラスのコンストラクタで、`super()`を呼び出している。 `super()`は子クラスから親クラスの`constructor`メソッドを呼び出する。

```js
// 親クラス
class Parent {
  constructor(...args) {
    console.log("Parentコンストラクタの処理", ...args);
  }
}
// Parentを継承したChildクラスの定義
class Child extends Parent {
  constructor(...args) {
    // Parentのコンストラクタ処理を呼び出す
    super(...args);
    console.log("Childコンストラクタの処理", ...args);
  }
}
const child = new Child("引数1", "引数2");
// "Parentコンストラクタの処理", "引数1", "引数2"
// "Childコンストラクタの処理", "引数1", "引数2"
```

`class`構文でのクラス定義では、`constructor`メソッド（コンストラクタ）で何も処理しない場合は省略できることを紹介しました。 これは、継承した子クラスでも同じである。

次のコードの`Child`クラスのコンストラクタでは、何も処理を行っていません。 そのため、`Child`クラスの`constructor`メソッドの定義を省略できる。

```js
class Parent {}
class Child extends Parent {}
```

このように子クラスで`constructor`を省略した場合は次のように書いた場合と同じ意味になる。 `constructor`メソッドの引数をすべて受け取り、そのまま`super`へ引数の順番を維持して渡する。

```js
class Parent {}
class Child extends Parent {
  constructor(...args) {
    super(...args); // 親クラスに引数をそのまま渡す
  }
}
```

## コンストラクタの処理順は親クラスから子クラスへ

コンストラクタの処理順は、親クラスから子クラスへと順番が決まっている。

`class`構文では必ず親クラスのコンストラクタ処理（`super()`の呼び出し）を先に行い、その次に子クラスのコンストラクタ処理を行いる。 子クラスのコンストラクタでは、`this`を触る前に`super()`で親クラスのコンストラクタ処理を呼び出さないと`ReferenceError`となるためである。

次のコードでは、`Parent`と`Child`でそれぞれインスタンス（`this`）の`name`プロパティに値を書き込んでいる。 子クラスでは先に`super()`を呼び出してからでないと`this`を参照できません。 そのため、コンストラクタの処理順は Parent から Child という順番に限定される。

```js
class Parent {
  constructor() {
    this.name = "Parent";
  }
}
class Child extends Parent {
  constructor() {
    // 子クラスでは`super()`を`this`に触る前に呼び出さなければならない
    super();
    // 子クラスのコンストラクタ処理
    // 親クラスで書き込まれた`name`は上書きされる
    this.name = "Child";
  }
}
const parent = new Parent();
console.log(parent.name); // => "Parent"
const child = new Child();
console.log(child.name); // => "Child"
```

## クラスフィールドの継承

Public クラスフィールドもコンストラクタの処理順と同じく、親クラスのフィールドが初期化された後に子クラスのフィールドが初期化される。 Public クラスフィールドはインスタンスオブジェクトに対してプロパティを定義する構文である。 そのため、親クラスで定義されていたフィールドも、実際にインスタンス化したオブジェクトのプロパティとして定義される。

```js
class Parent {
  parentField = "親クラスで定義したフィールド";
}
// `Parent`を継承した`Child`を定義
class Child extends Parent {
  childField = "子クラスで定義したフィールド";
}
const instance = new Child();
console.log(instance.parentField); // => "親クラスで定義したフィールド"
console.log(instance.childField); // => "子クラスで定義したフィールド"
```

同じ名前のフィールドが定義されている場合は、子クラスのフィールド定義で上書きされる。

```js
class Parent {
  field = "親クラスで定義したフィールド";
}
// `Parent`を継承した`Child`を定義
class Child extends Parent {
  field = "子クラスで定義したフィールド";
}
const instance = new Child();
console.log(instance.field); // => "子クラスで定義したフィールド"
```

Public クラスフィールドは、このように親クラスで定義したフィールドも子クラスに定義される。 一方で、Private クラスフィールドは、このように親クラスで定義したフィールドは子クラスに定義されません。

次のコードでは、親クラスで定義した Private クラスフィールドを子クラスから参照しようとしている。 しかし、`#parentField`は参照できずに構文エラーとなることがわかる。

```js
class Parent {
  #parentField = "親クラスで定義したPrivateフィールド";
}
// `Parent`を継承した`Child`を定義
class Child extends Parent {
  dump() {
    console.log(this.#parentField); // => SyntaxError: reference to undeclared private field or method #parentFeild
  }
}
const instance = new Child();
instance.dump();
```

これは、Private クラスフィールドの「Private」とは各クラスごとの Private を守る目的であるためである。 継承したクラスから Private クラスフィールドが利用できてしまうと、Private な情報が子クラスに漏れてしまうためである。 JavaScript では、クラスの外に公開したくないが、子クラスからは利用できるようにしたいというような中間の制限を持ったプロパティを定義する構文はない。

このように子クラスも含むクラスの外からアクセスを厳密に拒否する Private を**hard private**と呼ぶ。 JavaScript での Private クラスフィールドは**hard private**となっている。

一方で、子クラスからのアクセスは許可したり、クラス外からのアクセスが可能となるような特例を持つような Private を**soft private**と呼ぶ。 JavaScript での soft private は、`WeakMap`や`WeakSet`を使ってユーザー自身で実装する必要がある（「Map/Set」の章を参照）。

## プロトタイプ継承

次のコードでは`extends`キーワードを使って`Parent`クラスを継承した`Child`クラスを定義している。 `Parent`クラスでは`method`を定義しているため、これを継承している`Child`クラスのインスタンスからも呼び出せる。

```js
class Parent {
  method() {
    console.log("Parent.prototype.method");
  }
}
// `Parent`を継承した`Child`を定義
class Child extends Parent {
  // methodの定義はない
}
// `Child`のインスタンスは`Parent`のプロトタイプメソッドを継承している
const instance = new Child();
instance.method(); // "Parent.prototype.method"
```

このように、子クラスのインスタンスから親クラスのプロトタイプメソッドもプロトタイプチェーンの仕組みによって呼び出せる。

`extends`によって継承した場合、子クラスのプロトタイプオブジェクトの`[[Prototype]]`内部プロパティには親クラスのプロトタイプオブジェクトが設定される。 このコードでは、`Child.prototype`オブジェクトの`[[Prototype]]`内部プロパティには`Parent.prototype`が設定される。

これにより、プロパティを参照する場合には次のような順番でオブジェクトを探索している。

1. `instance`オブジェクト自身
2. `Child.prototype`（`instance`オブジェクトの`[[Prototype]]`の参照先）
3. `Parent.prototype`（`Child.prototype`オブジェクトの`[[Prototype]]`の参照先）

このプロトタイプチェーンの仕組みにより、`method`プロパティは`Parent.prototype`オブジェクトに定義されたものを参照する。

このように JavaScript では`class`構文と`extends`キーワードを使うことでクラスの機能を継承できる。 `class`構文ではプロトタイプオブジェクトを参照する仕組みによって継承が行われている。 そのため、この継承の仕組みを**プロトタイプ継承**と呼ぶ。

## 静的メソッドの継承

インスタンスとクラスのプロトタイプオブジェクトとの間にはプロトタイプチェーンがある。 クラス自身（クラスのコンストラクタ）も親クラス自身（親クラスのコンストラクタ）との間にプロトタイプチェーンがある。つまり、静的メソッドも継承されるということである。

```js
class Parent {
  static hello() {
    return "Hello";
  }
}
class Child extends Parent {}
console.log(Child.hello()); // => "Hello"
```

`extends`によって継承した場合、子クラスのコンストラクタの`[[Prototype]]`内部プロパティには親クラスのコンストラクタが設定される。 このコードでは、`Child`コンストラクタの`[[Prototype]]`内部プロパティに`Parent`コンストラクタが設定される。

つまり、先ほどのコードでは`Child.hello`プロパティを参照した場合には、次のような順番でオブジェクトを探索している。

1. `Child`コンストラクタ
2. `Parent`コンストラクタ（`Child`コンストラクタの`[[Prototype]]`の参照先）

クラスのコンストラクタ同士にもプロトタイプチェーンの仕組みがあるため、子クラスは親クラスの静的メソッドを呼び出せる。

## `super`プロパティ

子クラスから親クラスのコンストラクタ処理を呼び出すには`super()`を使いる。 同じように、子クラスのプロトタイプメソッドからは、`super.プロパティ名`で親クラスのプロトタイプメソッドを参照できる。

次のコードでは、`Child.prototype.method`の中で`super.method()`と書くことで`Parent.prototype.method`を呼び出している。 このように、子クラスから継承元の親クラスのプロトタイプメソッドは`super.プロパティ名`で参照できる。

```js
class Parent {
  method() {
    console.log("Parent.prototype.method");
  }
}
class Child extends Parent {
  method() {
    console.log("Child.prototype.method");
    // `this.method()`だと自分(`this`)のmethodを呼び出して無限ループする
    // そのため明示的に`super.method()`を呼ぶことで、Parent.prototype.methodを呼び出す
    super.method();
  }
}
const child = new Child();
child.method();
// コンソールには次のように出力される
// "Child.prototype.method"
// "Parent.prototype.method"
```

プロトタイプチェーンでは、インスタンスからクラス、さらに親のクラスと継承関係をさかのぼるようにメソッドを探索すると紹介しました。 このコードでは`Child.prototype.method`が定義されているため、`child.method`は`Child.prototype.method`を呼び出する。 そして`Child.prototype.method`は`super.method`を呼び出しているため、`Parent.prototype.method`が呼び出される。

クラスの静的メソッド同士も同じように`super.method()`と書くことで呼び出せる。 次のコードでは、`Parent`を継承した`Child`から親クラスの静的メソッドを呼び出している。

```js
class Parent {
  static method() {
    console.log("Parent.method");
  }
}
class Child extends Parent {
  static method() {
    console.log("Child.method");
    // `super.method()`で`Parent.method`を呼びだす
    super.method();
  }
}
Child.method();
// コンソールには次のように出力される
// "Child.method"
// "Parent.method"
```

## 継承の判定

あるクラスが指定したクラスをプロトタイプ継承しているかは`instanceof`演算子を使って判定できる。

次のコードでは、`Child`のインスタンスは`Child`クラスと`Parent`クラスを継承したオブジェクトであることを確認している。

```js
class Parent {}
class Child extends Parent {}

const parent = new Parent();
const child = new Child();
// `Parent`のインスタンスは`Parent`のみを継承したインスタンス
console.log(parent instanceof Parent); // => true
console.log(parent instanceof Child); // => false
// `Child`のインスタンスは`Child`と`Parent`を継承したインスタンス
console.log(child instanceof Parent); // => true
console.log(child instanceof Child); // => true
```

## ビルトインオブジェクトの継承

ここまで自身が定義したクラスを継承してきたが、ビルトインオブジェクトのコンストラクタも継承できる。 ビルトインオブジェクトには`Array`、`String`、`Object`、`Number`、`Error`、`Date`などのコンストラクタがある。 `class`構文ではこれらのビルトインオブジェクトを継承できる。

次のコードでは、ビルトインオブジェクトである`Array`を継承して独自のメソッドを加えた`MyArray`クラスを定義している。 継承した`MyArray`は`Array`の性質であるメソッドや状態管理についての仕組みを継承している。 継承した性質に加えて、`MyArray`クラスへ`first`や`last`といったアクセッサプロパティを追加している。

```js
class MyArray extends Array {
  get first() {
    return this.at(0);
  }

  get last() {
    return this.at(-1);
  }
}

// Arrayを継承しているのでArray.fromも継承している
// Array.fromはIterableなオブジェクトから配列インスタンスを作成する
const array = MyArray.from([1, 2, 3, 4, 5]);
console.log(array.length); // => 5
console.log(array.first); // => 1
console.log(array.last); // => 5
```

`Array`を継承した`MyArray`は、`Array`が元々持つ`length`プロパティや`Array.from`メソッドなどを継承しているので利用できる。
