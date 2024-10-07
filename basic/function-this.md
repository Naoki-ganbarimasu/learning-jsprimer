# 関数と this

基本的にはメソッドの中で利用するが、this は読み取り専用のグローバル変数のようなものでどこにでも書ける。 加えて、this の参照先（評価結果）は条件によって異なる。

- 実行コンテキストにおける this
- コンストラクタにおける this
- 関数とメソッドにおける this
- Arrow Function における this

コンストラクタにおける this は、次の章である「クラス」で扱う。 this が実際に使われるのはメソッドにおいてです。 そのため、あらゆる条件下での this の動きをすべて覚える必要はない。

## 実行コンテキストと this

### スクリプトにおける this

実行コンテキストが"Script"である場合、トップレベルのスコープに書かれた this はグローバルオブジェクトを参照する。 グローバルオブジェクトは、実行環境ごとに異なるものが定義されている。 ブラウザのグローバルオブジェクトは window オブジェクト、Node.js のグローバルオブジェクトは global オブジェクトとなる。

```js
<script>// 実行コンテキストは"Script" console.log(this); // => window</script>
```

### スクリプトにおける this

実行コンテキストが`"Module"`である場合、そのトップレベルのスコープに書かれた `this` は常に `undefined` となる。

ブラウザで、`script` 要素に `type="module"`属性がついた場合は、実行コンテキストが"Module"として実行されれる。 この script 要素の直下に書いた `this` は `undefined` となる。

```js
<script type="module">
  //実行コンテキストは"Module" console.log(this); // => undefined
</script>
```

グローバルオブジェクトを参照したい場合は、this ではなく globalThis を使う。

```js
// ブラウザでは`window`オブジェクト、Node.jsでは`global`オブジェクトを参照する
console.log(globalThis);
```

## 関数とメソッドにおける this

関数を定義する方法として、function キーワードによる関数宣言と関数式、Arrow Function などがある。 this が参照先を決めるルールは、Arrow Function とそれ以外の関数定義の方法で異なる。

## Arrow Function 以外の関数における this

Arrow Function 以外の関数（メソッドも含む）における this は、実行時に決まる値となる。 言い方を変えると this は関数に渡される暗黙的な引数のようなもので、その渡される値は関数を実行するときに決まる。

関数の中に書かれた `this` は、関数の呼び出し元から暗黙的に渡される値を参照することになる。 このルールは `Arrow Function` 以外の関数やメソッドで共通した仕組みとなる。`Arrow Function` で定義した関数やメソッドはこのルールとは別の仕組みとなる。

```js
// 疑似的な`this`の値の仕組み
// 関数は引数として暗黙的に`this`の値を受け取るイメージ
function fn(暗黙的に渡されるthisの値, 仮引数) {
  console.log(this); // => 暗黙的に渡されるthisの値
}
// 暗黙的に`this`の値を引数として渡しているイメージ
fn(暗黙的に渡すthisの値, 引数);
```

関数における`this`の基本的な参照先（暗黙的に関数に渡す this の値）はベースオブジェクトとなる。 ベースオブジェクトとは「メソッドを呼ぶ際に、そのメソッドのドット演算子またはブラケット演算子のひとつ左にあるオブジェクト」のことをいう。 ベースオブジェクトがない場合の`this`は`undefined`となる。

```js
// `fn`関数はメソッドではないのでベースオブジェクトはない
fn();
// `obj.method`メソッドのベースオブジェクトは`obj`
obj.method();
// `obj1.obj2.method`メソッドのベースオブジェクトは`obj2`
// ドット演算子、ブラケット演算子どちらも結果は同じ
obj1.obj2.method();
obj1["obj2"]["method"]();
```

`this`は関数の定義ではなく呼び出し方で参照する値が異なる。これは、後述する「`this`が問題となるパターン」で詳しく紹介する。 `Arrow Function`以外の関数では、関数の定義だけを見て this の値が何かということは決定できない点に注意が必要。

## 関数宣言や関数式における this

次の例では、関数宣言で関数 `fn1`、関数式で関数 `fn2` を定義し、それぞれの関数内で this を返す。 定義したそれぞれの関数を fn1()と fn2()のようにただの関数として呼び出す。 このとき、ベースオブジェクトはないため、`this` は `undefined` となる。

```js
"use strict";
function fn1() {
  return this;
}
const fn2 = function () {
  return this;
};
// 関数の中の`this`が参照する値は呼び出し方によって決まる
// `fn1`と`fn2`どちらもただの関数として呼び出している
// メソッドとして呼び出していないためベースオブジェクトはない
// ベースオブジェクトがない場合、`this`は`undefined`となる
console.log(fn1()); // => undefined
console.log(fn2()); // => undefined
```

```js
"use strict";
function outer() {
  console.log(this); // => undefined
  function inner() {
    console.log(this); // => undefined
  }
  // `inner`関数呼び出しのベースオブジェクトはない
  inner();
}
// `outer`関数呼び出しのベースオブジェクトはない
outer();
```

## メソッド呼び出しにおける this

メソッドの場合は、そのメソッドが何かしらのオブジェクトに所属している。JavaScript ではオブジェクトのプロパティとして指定される関数のことをメソッドと呼ぶ。

```js
const obj = {
  // 関数式をプロパティの値にしたメソッド
  method1: function () {
    return this;
  },
  // 短縮記法で定義したメソッド
  method2() {
    return this;
  }
};
// メソッド呼び出しの場合、それぞれの`this`はベースオブジェクト(`obj`)を参照する
// メソッド呼び出しの`.`の左にあるオブジェクトがベースオブジェクト
console.log(obj.method1()); // => obj
console.log(obj.method2()); // => obj
```

これを利用すれば、メソッドの中から同じオブジェクトに所属する別のプロパティを this で参照できる。
メソッドが所属するオブジェクトのプロパティを、オブジェクト名.プロパティ名の代わりに this.プロパティ名で参照できる。

```js
const person = {
  fullName: "Brendan Eich",
  sayName: function () {
    // `person.fullName`と書いているのと同じ
    return this.fullName;
  }
};
// `person.fullName`を出力する
console.log(person.sayName()); // => "Brendan Eich"
```

ネストしたオブジェクトにおいてメソッド内の this がベースオブジェクトである obj3 を参照していることがわかる。 このときのベースオブジェクトはドットでつないだ一番左の obj1 ではなく、メソッドから見てひとつ左の obj3 となる。

```js
const obj1 = {
  obj2: {
    obj3: {
      method() {
        return this;
      }
    }
  }
};
// `obj1.obj2.obj3.method`メソッドの`this`は`obj3`を参照
console.log(obj1.obj2.obj3.method() === obj1.obj2.obj3); // => true
```

## this が問題となるパターン

### 問題: this を含むメソッドを変数に代入した場合

メソッドは関数を値に持つプロパティのことで、プロパティは変数に代入し直すことができるため, JavaScript ではメソッドとして定義したものが、後からただの関数として呼び出されることがある。そのため、メソッドとして定義した関数も、別の変数に代入してただの関数として呼び出されることがある。 この場合には、メソッドとして定義した関数であっても、実行時にはただの関数であるためベースオブジェクトが変わっている。 これは this が定義した時点ではなく実行したときに決まるという性質そのものである。

```js
"use strict";
const person = {
  fullName: "Brendan Eich",
  sayName: function () {
    // `this`は呼び出し元によって異なる
    return this.fullName;
  }
};
// `sayName`メソッドは`person`オブジェクトに所属する
// `this`は`person`オブジェクトとなる
console.log(person.sayName()); // => "Brendan Eich"
// `person.sayName`を`say`変数に代入する
const say = person.sayName;
// 代入したメソッドを関数として呼ぶ
// この`say`関数はどのオブジェクトにも所属していない
// `this`はundefinedとなるため例外を投げる
say(); // => TypeError: Cannot read property 'fullName' of undefined
```

エラーは以下のような処理をしている。

```js
"use strict";
// const say = person.sayName; は次のようなイメージ
const say = function () {
  return this.fullName;
};
// `this`は`undefined`となるため例外を投げる
say(); // => TypeError: Cannot read property 'fullName' of undefined
```

Arrow Function 以外の関数において、this は定義したときではなく実行したときに決定される。 そのため、関数に this を含んでいる場合、その関数は意図した呼ばれ方がされないと間違った結果が発生するという問題がある。

この問題の対処法としてはメソッドとして定義されている関数はメソッドとして呼ぶことと this の値を指定して関数を呼べるメソッドで関数を実行する方法の 2 つ。

### 対処法: call、apply、bind メソッド

関数やメソッドの`this`を明示的に指定して関数を実行する方法もある。 Function（関数オブジェクト）には`call、apply、bind`といった明示的に this を指定して関数を実行するメソッドが用意されている。

```js
関数.call(thisの値, ...関数の引数);
```

```js
"use strict";
function say(message) {
  return `${message} ${this.fullName}！`;
}
const person = {
  fullName: "Brendan Eich"
};
// `this`を`person`にして`say`関数を呼びだす
console.log(say.call(person, "こんにちは")); // => "こんにちは Brendan Eich！"
// `say`関数をそのまま呼び出すと`this`は`undefined`となるため例外が発生
say("こんにちは"); // => TypeError: Cannot read property 'fullName' of undefined
```

`apply`メソッドは第一引数に this とする値を指定し、第二引数に関数の引数を配列として渡す。

```js
関数.apply(thisの値, [関数の引数1, 関数の引数2]);
```

次の例では this に person オブジェクトを指定した状態で say 関数を呼び出している。 apply メソッドの第二引数で指定した配列は、自動的に展開されて say 関数の仮引数 message に入る。

```js
"use strict";
function say(message) {
  return `${message} ${this.fullName}！`;
}
const person = {
  fullName: "Brendan Eich"
};
// `this`を`person`にして`say`関数を呼びだす
// callとは異なり引数を配列として渡す
console.log(say.apply(person, ["こんにちは"])); // => "こんにちは Brendan Eich！"
// `say`関数をそのまま呼び出すと`this`は`undefined`となるため例外が発生
say("こんにちは"); // => TypeError: Cannot read property 'fullName' of undefined
```

`this`に person オブジェクトを指定した状態で `say` 関数を呼び出しています。 `apply` メソッドの第二引数で指定した配列は、自動的に展開されて say 関数の仮引数`message`に入ります。

```js
"use strict";
function say(message) {
  return `${message} ${this.fullName}！`;
}
const person = {
  fullName: "Brendan Eich"
};
// `this`を`person`にして`say`関数を呼びだす
// callとは異なり引数を配列として渡す
console.log(say.apply(person, ["こんにちは"])); // => "こんにちは Brendan Eich！"
// `say`関数をそのまま呼び出すと`this`は`undefined`となるため例外が発生
say("こんにちは"); // => TypeError: Cannot read property 'fullName' of undefined
```

```js
call メソッドと apply メソッドの違いは、関数の引数への値の渡し方が異なり、どちらのメソッドも this の値が不要な場合は null を渡す。

function add(x, y) {
    return x + y;
}
// `this`が不要な場合は、nullを渡す
console.log(add.call(null, 1, 2)); // => 3
console.log(add.apply(null, [1, 2])); // => 3
```

bind メソッドは、名前のとおり this の値を束縛（bind）した新しい関数を作成する。

```js
関数.bind(thisの値, ...関数の引数); // => thisや引数がbindされた関数
```

以下のように bind メソッドの第二引数以降に値を渡すことで、束縛した関数の引数も束縛できる。

```js
function say(message) {
  return `${message} ${this.fullName}！`;
}
const person = {
  fullName: "Brendan Eich"
};
// `this`を`person`に束縛した`say`関数をラップした関数を作る
const sayPerson = say.bind(person, "こんにちは");
console.log(sayPerson()); // => "こんにちは Brendan Eich！"
```

`bind`は this や引数を束縛した関数を作るメソッドである。

```js
function say(message) {
  return `${message} ${this.fullName}！`;
}
const person = {
  fullName: "Brendan Eich"
};
// `this`を`person`に束縛した`say`関数をラップした関数を作る
//  say.bind(person, "こんにちは"); は次のようなラップ関数を作る
const sayPerson = () => {
  return say.call(person, "こんにちは");
};
console.log(sayPerson()); // => "こんにちは Brendan Eich！"
```

call、apply、bind メソッドを使うことで this を明示的に指定した状態で関数を呼び出せるが、毎回関数を呼び出すたびにこれらのメソッドを使うのは、関数を呼び出すための関数が必要になってしまい手間がかかる。 そのため、基本的には「メソッドとして定義されている関数はメソッドとして呼ぶこと」でこの問題を回避するのがよい。その中で、どうしても this を固定したい場合には call、apply、bind メソッドを利用する。

## 問題: コールバック関数と this

コールバック関数の中で this を参照すると問題となる場合がある。メソッドの中で Array の map メソッドなどのコールバック関数を扱う場合に発生しやすい。

次のコードでは prefixArray メソッドの中で map メソッドを使っています。 このとき、map メソッドのコールバック関数の中で、Prefixer オブジェクトを参照するつもりで this を参照しています。

```js
"use strict";
// strict modeを明示しているのは、thisがグローバルオブジェクトに暗黙的に変換されるのを防止するため
const Prefixer = {
  prefix: "pre",
  /**
   * `strings`配列の各要素にprefixをつける
   */
  prefixArray(strings) {
    return strings.map(function (str) {
      // コールバック関数における`this`は`undefined`となる(strict mode)
      // そのため`this.prefix`は`undefined.prefix`となり例外が発生する
      return this.prefix + "-" + str;
    });
  }
};
// `prefixArray`メソッドにおける`this`は`Prefixer`
Prefixer.prefixArray(["a", "b", "c"]); // => TypeError: Cannot read property 'prefix' of undefined
```

Array の map メソッドにはコールバック関数として、その場で定義した無名関数を渡していて、Array の map メソッドに渡しているコールバック関数は callback()のようにただの関数として呼び出される。コールバック関数として呼び出すとき、この関数にはベースオブジェクトはない。 そのため callback 関数の this は undefined となる。

```js
// ...
    prefixArray(strings) {
        // 無名関数をコールバック関数として渡している
        return strings.map(function(str) {
            return this.prefix + "-" + str;
        });
    }
// ...
```

### 対処法: this を一時変数へ代入する

this は関数の呼び出し元で変化し、その参照先は呼び出し元におけるベースオブジェクトである。 prefixArray メソッドの呼び出しにおいては、this は Prefixer オブジェクトであるが、コールバック関数は改めて関数として呼び出されるため this が undefined となってしまうのが問題だった。

そのため、最初の prefixArray メソッド呼び出しにおける this の参照先を一時変数として保存することでこの問題を回避できる。 次のコードでは、prefixArray メソッドの this を that 変数に保持してている。 コールバック関数からは this の代わりに that 変数を参照することで、コールバック関数からも prefixArray メソッド呼び出しと同じ this を参照できる。

```js
"use strict";
const Prefixer = {
  prefix: "pre",
  prefixArray(strings) {
    // `that`は`prefixArray`メソッド呼び出しにおける`this`となる
    // つまり`that`は`Prefixer`オブジェクトを参照する
    const that = this;
    return strings.map(function (str) {
      // `this`ではなく`that`を参照する
      return that.prefix + "-" + str;
    });
  }
};
// `prefixArray`メソッドにおける`this`は`Prefixer`
const prefixedStrings = Prefixer.prefixArray(["a", "b", "c"]);
console.log(prefixedStrings); // => ["pre-a", "pre-b", "pre-c"]
```

Function の call メソッドなどで明示的に this を渡して関数を呼び出すこともできる。 また、Array の map メソッドなどは this となる値を引数として渡せる仕組みを持っているため第二引数に　 this となる値を返すことで解決する。

```js
"use strict";
const Prefixer = {
  prefix: "pre",
  prefixArray(strings) {
    // Arrayの`map`メソッドは第二引数に`this`となる値を渡せる
    return strings.map(function (str) {
      // `this`が第二引数の値と同じになる
      // つまり`prefixArray`メソッドと同じ`this`となる
      return this.prefix + "-" + str;
    }, this);
  }
};
// `prefixArray`メソッドにおける`this`は`Prefixer`
const prefixedStrings = Prefixer.prefixArray(["a", "b", "c"]);
console.log(prefixedStrings); // => ["pre-a", "pre-b", "pre-c"]
```

### 対処法: Arrow Function でコールバック関数を扱う

通常の関数やメソッドは呼び出し時に暗黙的に this の値を受け取り、関数内の this はその値を参照するが、Arrow Function はこの暗黙的な this の値を受け取らないため、Arrow Function 内の this は、スコープチェーンの仕組みと同様に外側の関数（この場合は prefixArray メソッド）を探索する。

```js
"use strict";
const Prefixer = {
  prefix: "pre",
  prefixArray(strings) {
    return strings.map((str) => {
      // Arrow Function自体は`this`を持たない
      // `this`は外側の`prefixArray`関数が持つ`this`を参照する
      // そのため`this.prefix`は"pre"となる
      return this.prefix + "-" + str;
    });
  }
};
// このとき、`prefixArray`のベースオブジェクトは`Prefixer`となる
// つまり、`prefixArray`メソッド内の`this`は`Prefixer`を参照する
const prefixedStrings = Prefixer.prefixArray(["a", "b", "c"]);
console.log(prefixedStrings); // => ["pre-a", "pre-b", "pre-c"]
```

Arrow Function でのコールバック関数における this は簡潔で、 Arrow Function を使うのがもっとも簡潔である。

## Arrow Function と this

Arrow Function とそれ以外の関数で大きく違うことは、Arrow Function は this を暗黙的な引数として受けつけない。 そのため、Arrow Function 内には this が定義されていない。このときの this は外側のスコープ（関数）の this を参照する。
変数におけるスコープチェーンの仕組みと同様で、そのスコープに this が定義されていない場合には外側のスコープを探索する。 そのため、Arrow Function 内の this の参照で、常に外側のスコープ（関数）へと this の定義を探索しに行く（詳細はスコープチェーンを参照）。 また、this は ECMAScript のキーワードであるため、ユーザーは this という変数を定義できない。

```js
// thisはキーワードであるため、ユーザーは`this`という名前の変数を定義できない
const this = "thisは読み取り専用"; // => SyntaxError: Unexpected token this
```

通常の変数のように this がどの値を参照するかは静的に設定される。Arrow Function における this は「Arrow Function 自身の外側のスコープに定義されたもっとも近い関数の this の値」となる。

```js
// Arrow Functionで定義した関数
const fn = () => {
  // この関数の外側には関数は存在しない
  // トップレベルの`this`と同じ値
  return this;
};
console.log(fn() === this); // => true
```

this の値は、実行コンテキストが"Script"ならばグローバルオブジェクトとなり、"Module"ならば undefined となる。

以下のように Arrow Function を包むように通常の関数が定義されている場合は Arrow Function における this は「自身の外側のスコープにあるもっとも近い関数の this の値」となる。

```js
"use strict";
function outer() {
  // Arrow Functionで定義した関数を返す
  return () => {
    // この関数の外側には`outer`関数が存在する
    // `outer`関数に`this`を書いた場合と同じ
    return this;
  };
}
// `outer`関数の返り値はArrow Functionにて定義された関数
const innerArrowFunction = outer();
console.log(innerArrowFunction()); // => undefined
```

以下のように、この Arrow Function における this は outer 関数で this を参照した場合と同じ値になる。

```js
"use strict";
function outer() {
  // `outer`関数直下の`this`
  const that = this;
  // Arrow Functionで定義した関数を返す
  return () => {
    // Arrow Function自身は`this`を持たない
    // `outer`関数に`this`を書いた場合と同じ
    return that;
  };
}
// `outer()`と呼び出したときの`this`は`undefined`(strict mode)
const innerArrowFunction = outer();
console.log(innerArrowFunction()); // => undefined
```

## メソットとコールバック関すと Arrow Function

メソッド内におけるコールバック関数は Arrow Function をより活用できる。 function キーワードでコールバック関数を定義すると、this の値はコールバック関数の呼ばれ方を意識する必要がある。 それは function キーワードで定義した関数における this は呼び出し方によって変わるためである。

コールバック関数側から見ると、どのように呼ばれるかによって変わってしまう this を使うことはできない。 そのため、コールバック関数の外側のスコープで this を一時変数に代入し、それを使うという回避方法を取っていた。

```js
// `callback`関数を受け取り呼び出す関数
const callCallback = (callback) => {
  // `callback`を呼び出す実装
};

const obj = {
  method() {
    callCallback(function () {
      // ここでの `this` は`callCallback`の実装に依存する
      // `callback()`のように単純に呼び出されるなら`this`は`undefined`になる
      // Functionの`call`メソッドなどを使って特定のオブジェクトを指定するかもしれない
      // この問題を回避するために`const that = this`のような一時変数を使う
    });
  }
};
```

しかし、Arrow Function でコールバック関数を定義した場合は、1 つ外側の関数の this を参照する。 このときの Arrow Function で定義したコールバック関数における this は呼び出し方によって変化しない。 そのため、this を一時変数に代入するなどの回避方法は必要はない。

```js
// `callback`関数を受け取り呼び出す関数
const callCallback = (callback) => {
  // `callback`を呼び出す実装
};

const obj = {
  method() {
    callCallback(() => {
      // ここでの`this`は1つ外側の関数における`this`と同じ
    });
  }
};
```

この Arrow Function における this は呼び出し方の影響を受けない。 したがって、コールバック関数がどのように呼ばれるかという実装についてを考えることなく this を扱える。

```js
const Prefixer = {
  prefix: "pre",
  prefixArray(strings) {
    return strings.map((str) => {
      // `Prefixer.prefixArray()` と呼び出されたとき
      // `this`は常に`Prefixer`を参照する
      return this.prefix + "-" + str;
    });
  }
};
const prefixedStrings = Prefixer.prefixArray(["a", "b", "c"]);
console.log(prefixedStrings); // => ["pre-a", "pre-b", "pre-c"]
```

## Arrow Function は this を bind できない

Arrow Function で定義した関数では call、apply、bind を使った this の指定は単に無視される。 これは、Arrow Function は this を持てないため。

次のように Arrow Function で定義した関数に対して call で this を指定しても、this の参照先が代わっていないことがわかる。 同様に apply や bind メソッドを使った場合も this の参照先は変わらない。

```js
const fn = () => {
  return this;
};
// Scriptコンテキストの場合、スクリプト直下のArrow Functionの`this`はグローバルオブジェクト
console.log(fn()); // グローバルオブジェクト
// callで`this`を`{}`にしようとしても、`this`は変わらない
console.log(fn.call({})); // グローバルオブジェクト
```

最初に述べたように function キーワードで定義した関数では呼び出し時に、ベースオブジェクトが this の値として暗黙的な引数のように渡されるが、Arrow Function の関数は呼び出し時に this を受け取らず、this の参照先は定義時に静的に決定される。

また、this が変わらないのはあくまで Arrow Function で定義した関数だけで、Arrow Function の this が参照する「自身の外側のスコープにあるもっとも近い関数の this の値」は call メソッドで変更できる。

```js
const obj = {
  method() {
    const arrowFunction = () => {
      return this;
    };
    return arrowFunction();
  }
};
// 通常の`this`は`obj.method`の`this`と同じ
console.log(obj.method()); // => obj
// `obj.method`の`this`を変更すれば、Arrow Functionの`this`も変更される
console.log(obj.method.call("THAT")); // => "THAT"
```
