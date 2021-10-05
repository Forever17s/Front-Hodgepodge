> 从 ECMAScript 2016（ES7）开始，版本发布变得更加频繁，每年发布一个新版本，好在每次版本的更新内容并不多，本文会细说这些新特性，尽可能和旧知识相关联，帮你迅速上手这些特性。

### ES7(ES2016) 新特性

#### Array.prototype.includes()方法

在 ES6 中我们有 `String.prototype.includes()` 可以查询给定字符串是否包含一个字符，而在 ES7 中，我们在数组中也可以用 `Array.prototype.includes()` 方法来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 `true`，否则返回 `false`。

```javascript
const arr = [1, 3, 5, 2, "8", NaN, -0];
arr.includes(1); // true
arr.includes(1, 2); // false 该方法的第二个参数表示搜索的起始位置，默认为0
arr.includes("1"); // false
arr.includes(NaN); // true
arr.includes(+0); // true
```

在 ES7 之前想判断数组中是否包含一个元素，有如下两种方法,但都不如 `includes` 来得直观

- `indexOf()`

  `indexOf`方法返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回 `-1`。不过这种方法有个缺点，它内部使用严格相等运算符 `===` 进行判断，这会导致对 `NaN` 的误判。

  ```javascript
  if (arr.indexOf(el) !== -1) {
    // ...
  }

  [NaN].indexOf(NaN); // -1
  ```

- `find()` 和 `findIndex()`
  数组实例的 `find` 方法，用于找出第一个符合条件的数组成员。另外，这两个方法都可以发现 `NaN`，弥补了数组的 `indexOf` 方法的不足。

  ```javascript
  [1, 4, -5, 10].find((n) => n < 0); // -5

  [1, 5, 10, 15].findIndex((n) => n > 9); // 2

  [NaN].findIndex((y) => Object.is(NaN, y)); // 0
  ```

#### 求幂运算符『 \*\* 』

在 ES7 中引入了指数运算符，具有与 `Math.pow()` 等效的计算结果

```javascript
console.log(2 ** 10); // 1024
console.log(Math.pow(2, 10)); // 1024
```

---

### ES8(ES2017) 新特性

#### Async/Await

虽然 `Promise` 可以解决回调地狱的问题，但是链式调用太多，则会变成另一种形式的回调地狱 ——『 面条地狱 』，所以在 ES8 里则出现了 `Promise` 的语法糖 `async/await`，专门解决这个问题。

我们先看一下下面的 Promise 代码：

```javascript
fetch("coffee.jpg")
  .then((response) => response.blob())
  .then((myBlob) => {
    let objectURL = URL.createObjectURL(myBlob);
    let image = document.createElement("img");
    image.src = objectURL;
    document.body.appendChild(image);
  })
  .catch((e) => {
    console.log(
      "There has been a problem with your fetch operation: " + e.message
    );
  });
```

然后再看看 async/await 版的，这样看起来是不是更清晰了。

```javascript
async function myFetch() {
  let response = await fetch("coffee.jpg");
  let myBlob = await response.blob();

  let objectURL = URL.createObjectURL(myBlob);
  let image = document.createElement("img");
  image.src = objectURL;
  document.body.appendChild(image);
}

myFetch();
```

> :warning: 需要强调的是，`await` 不可以脱离 `async` 单独使用，`await` 后面一定是 `Promise` 对象，如果不是会自动包装成 `Promise` 对象。

#### Object.values()、Object.entries()

Object.values()方法返回一个给定对象自身的所有可枚举属性值的数组，值的顺序与使用 for...in 循环的顺序相同 ( 区别在于 for-in 循环枚举原型链中的属性 )。

```javascript
const obj = {
  a: "somestring",
  b: 42,
  c: false
};
Object.values(obj); // ["somestring", 42, false]
```

Object.entries()方法返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 for...in 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环还会枚举原型链中的属性）。

```javascript
const obj = {
  a: "somestring",
  b: 42
};

const objEntries = Object.entries(obj);
// [
//   ["a", "somestring"],
//   ["b", 42]
// ];

for (let [key, value] of objEntries) {
  console.log(`${key}: ${value}`);
}

// "a: somestring"
// "b: 42"
```

#### padStart()、padEnd()

`padStart()` 方法用另一个字符串填充当前字符串(重复，如果需要的话)，以便产生的字符串达到给定的长度。填充从当前字符串的开始(左侧)应用的。

```javascript
"5".padStart(5, "0"); // "00005"

const fullNumber = "2034399002125581";
const last4Digits = fullNumber.slice(-4);
const maskedNumber = last4Digits.padStart(fullNumber.length, "*");
console.log(maskedNumber); // "************5581"
```

`padEnd()` 方法会用一个字符串填充当前字符串（如果需要的话则重复填充），返回填充后达到指定长度的字符串。从当前字符串的末尾（右侧）开始填充。

```javascript
"Breaded Mushrooms".padEnd(25, "."); // "Breaded Mushrooms........"
// 有时候我们处理日期、金额的时候经常要格式化，这个特性就派上用场
"12".padStart(10, "YYYY-MM-DD"); // "YYYY-MM-12"
"09-12".padStart(10, "YYYY-MM-DD"); // "YYYY-09-12"
```

#### Object.getOwnPropertyDescriptors()

ES5 的 `Object.getOwnPropertyDescriptor()`方法会返回某个对象属性的描述对象（descriptor）。ES8 引入了 `Object.getOwnPropertyDescriptors()`方法，返回指定对象所有自身属性（非继承属性）的描述对象。

```javascript
const obj = {
  name: "浪里行舟",
  get bar() {
    return "abc";
  }
};
console.log(Object.getOwnPropertyDescriptors(obj));
```

该方法的引入目的，主要是为了解决 Object.assign()无法正确拷贝 get 属性和 set 属性的问题。我们来看个例子：

```javascript
const source = {
  set foo(value) {
    console.log(value);
  },
  get bar() {
    return "浪里行舟";
  }
};
const target1 = {};
Object.assign(target1, source);
console.log(Object.getOwnPropertyDescriptor(target1, "foo"));
```

> 上面代码中，`source` 对象的 `foo` 属性的值是一个赋值函数，`Object.assign` 方法将这个属性拷贝给 `target1` 对象，结果该属性的值变成了 `undefined`。这是因为 `Object.assign` 方法总是拷贝一个属性的值，而不会拷贝它背后的赋值方法或取值方法。

这时 `Object.getOwnPropertyDescriptors()`方法配合 `Object.defineProperties()`方法，就可以实现正确拷贝。

```javascript
const source = {
  set foo(value) {
    console.log(value);
  },
  get bar() {
    return "浪里行舟";
  }
};
const target2 = {};
Object.defineProperties(target2, Object.getOwnPropertyDescriptors(source));
console.log(Object.getOwnPropertyDescriptor(target2, "foo"));
```

---

### ES9(ES2018) 新特性

#### for await of

> `for of` 方法能够遍历具有 `Symbol.iterator` 接口的同步迭代器数据，但是不能遍历异步迭代器。ES9 新增的 `for await of` 可以用来遍历具有 `Symbol.asyncIterator` 方法的数据结构，也就是异步迭代器，且会等待前一个成员的状态改变后才会遍历到下一个成员，相当于 `async` 函数内部的 `await`。

现在我们有三个异步任务，想要实现依次输出结果，该如何实现呢？

```javascript
// for of遍历
function Gen(time) {
  return new Promise(function (resolve, reject) {
    setTimeout(function () {
      resolve(time);
    }, time);
  });
}
(async function () {
  let arr = [Gen(2000), Gen(100), Gen(3000)];
  for (let item of arr) {
    console.log(Date.now(), item.then(console.log));
  }
})();
// 1577096506721 Promise {<pending>}
// 1577096506750 Promise {<pending>}
// 1577096506750 Promise {<pending>}
// 100
// 2000
// 3000
```

上述代码证实了 `for of` 方法不能遍历异步迭代器，得到的结果并不是我们所期待的，于是 `for await of` 就粉墨登场啦！

```javascript
function Gen(time) {
  return new Promise(function (resolve, reject) {
    setTimeout(function () {
      resolve(time);
    }, time);
  });
}
(async function () {
  let arr = [Gen(2000), Gen(100), Gen(3000)];
  for await (let item of arr) {
    console.log(Date.now(), item);
  }
})();
// 1577096529396 2000
// 1577096529397 100
// 1577096530397 3000
```

#### 对象扩展操作符

ES6 中添加了数组的扩展操作符，让我们在操作数组时更加简便，美中不足的是并不支持对象扩展操作符，但是在 ES9 开始，这一功能也得到了支持，例如：

```javascript
var obj1 = { foo: "bar", x: 42 };
var obj2 = { foo: "baz", y: 13 };

var clonedObj = { ...obj1 };
// 克隆后的对象: { foo: "bar", x: 42 }

clonedObj.x = 200;

var mergedObj = { ...obj1, ...obj2 };
// 合并后的对象: { foo: "baz", x: 42, y: 13 }
```

上面便是一个简便的『 浅拷贝 』。这里有一点小提示，就是 `Object.assign()` 函数会触发 `setters`，而展开语法则不会。所以不能替换也不能模拟 `Object.assign()`。如果存在相同的属性名，只有最后一个会生效。

#### Promise.prototype.finally()

`Promise.prototype.finally()` 方法返回一个 `Promise`，在 `promise` 执行结束时，无论结果是 `fulfilled` 或者是 `rejected`，在执行 `then()` 和 `catch()`后，都会执行 `finally` 指定的回调函数。

```javascript
fetch("https://www.google.com")
  .then((response) => {
    console.log(response.status);
  })
  .catch((error) => {
    console.log(error);
  })
  .finally(() => {
    document.querySelector("#spinner").style.display = "none";
  });
```

无论操作是否成功，当您需要在操作完成后进行一些清理时，`finally()` 方法就派上用场了。这为指定执行完 `promise` 后，无论结果是 `fulfilled` 还是 `rejected` 都需要执行的代码提供了一种方式，避免同样的语句需要在 `then()` 和 `catch()`中各写一次的情况。

#### 新的正则表达式特性

ES9 为正则表达式添加了四个新特性，进一步提高了 JavaScript 的字符串处理能力。这些特点如下:

- 反向(`lookbehind`)断言
- `Unicode` 属性转义
- `s (dotAll)` 标志
- 命名捕获组

##### 反向(lookbehind)断言

断言(Assertion)是一个对当前匹配位置之前或之后的字符的测试， 它不会实际消耗任何字符，所以断言也被称为『 非消耗性匹配 』或『 非获取匹配 』。

正则表达式的断言一共有 4 种形式：

- `(?=pattern)` 零宽正向肯定断言(zero-width positive lookahead assertion)
- `(?!pattern)` 零宽正向否定断言(zero-width negative lookahead assertion)
- `(?<=pattern)` 零宽反向肯定断言(zero-width positive lookbehind assertion)
- `(?<!pattern)` 零宽反向否定断言(zero-width negative lookbehind assertion)

在 ES9 之前，JavaScript 正则表达式，只支持正向断言。正向断言的意思是：当前位置后面的字符串应该满足断言，但是并不捕获。例子如下：

```javascript
"fishHeadfishTail".match(/fish(?=Head)/g); // ["fish"]
```

反向断言和正向断言的行为一样，只是方向相反。例子如下：

```javascript
"abc123".match(/(?<=(\d+)(\d+))$/); //  ["", "1", "23", index: 6, input: "abc123", groups: undefined]
```

##### Unicode 属性转义

正则表达式中的 [Unicode 转义符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Unicode_Property_Escapes)允许根据 Unicode 字符属性匹配 Unicode 字符。它允许区分字符类型，例如大写和小写字母，数学符号和标点符号。部分例子代码如下：

```javascript
// 匹配所有数字
const regex = /^\p{Number}+$/u;
regex.test('²³¹¼½¾') // true
regex.test('㉛㉜㉝') // true
regex.test('ⅠⅡⅢⅣⅤⅥⅦⅧⅨⅩⅪⅫ') // true

// 匹配所有空格
\p{White_Space}

// 匹配各种文字的所有字母，等同于 Unicode 版的 \w
[\p{Alphabetic}\p{Mark}\p{Decimal_Number}\p{Connector_Punctuation}\p{Join_Control}]

// 匹配各种文字的所有非字母的字符，等同于 Unicode 版的 \W
[^\p{Alphabetic}\p{Mark}\p{Decimal_Number}\p{Connector_Punctuation}\p{Join_Control}]

// 匹配 Emoji
/\p{Emoji_Modifier_Base}\p{Emoji_Modifier}?|\p{Emoji_Presentation}|\p{Emoji}\uFE0F/gu

// 匹配所有的箭头字符
const regexArrows = /^\p{Block=Arrows}+$/u;
regexArrows.test('←↑→↓↔↕↖↗↘↙⇏⇐⇑⇒⇓⇔⇕⇖⇗⇘⇙⇧⇩') // true
```

##### s/dotAll 标志

```javascript
let regex = /./;

regex.test("\n"); // false
regex.test("\r"); // false
regex.test("\u{2028}"); // false
regex.test("\u{2029}"); // false

regex.test("\v"); // true
regex.test("\f"); // true
regex.test("\u{0085}"); // true

/foo.bar/.test("foo\nbar"); // false
/foo[^]bar/.test("foo\nbar"); // true
/foo[\s]bar/.test("foo\nbar"); // true
```

但是在 ES9 之后，JS 正则增加了一个新的标志 **`s`** 用来表示 `dotAll`，这可以匹配任意字符。代码如下：

```javascript
/foo.bar/s.test("foo\nbar"); // true

const re = /foo.bar/s; //  等价于 const re = new RegExp('foo.bar', 's');
re.test("foo\nbar"); // true
re.dotAll; // true
re.flags; // "s"
```

##### 命名捕获组

在以往的版本里，JS 的正则分组是无法命名的，所以容易混淆。例如下面获取年月日的例子，很容易让人搞不清哪个是月份，哪个是年份:

```javascript
const matched = /(\d{4})-(\d{2})-(\d{2})/.exec("2019-01-01");
console.log(matched[0]); // 2019-01-01
console.log(matched[1]); // 2019
console.log(matched[2]); // 01
console.log(matched[3]); // 01
```

ES9 引入了命名捕获组，允许为每一个组匹配指定一个名字，既便于阅读代码，又便于引用。代码如下：

```javascript
const RE_DATE = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;

const matchObj = RE_DATE.exec("1999-12-31");
const year = matchObj.groups.year; // 1999
const month = matchObj.groups.month; // 12
const day = matchObj.groups.day; // 31

const RE_OPT_A = /^(?<as>a+)?$/;
const matchObj = RE_OPT_A.exec("");

matchObj.groups.as; // undefined
"as" in matchObj.groups; // true
```

---

### ES10(ES2019) 新特性

#### Array.prototype.flat() / flatMap()

多维数组是一种常见的数据格式，特别是在进行数据检索的时候。将多维数组打平是个常见的需求。通常我们能够实现，但是不够优雅。

flat() 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。

```javascript
// arr.flat(depth) // depth是指定要提取嵌套数组的结构深度，默认值为 1。也可以填写Infinity，无论多少层都直接铺平
const numbers1 = [1, 2, [3, 4, [5, 6]]];
console.log(numbers1.flat()); // [1, 2, 3, 4, [5, 6]]
const numbers2 = [1, 2, [3, 4, [5, 6]]];
console.log(numbers2.flat(2)); // [1, 2, 3, 4, 5, 6]
const numbers3 = [1, 2, [3, 4, [5, 6]]];
console.log(numbers3.flat(Infinity)); // [1, 2, 3, 4, 5, 6]
```

> 有了 `flat` 方法，那自然而然就有 `Array.prototype.flatMap` 方法，`flatMap()` 方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。从方法的名字上也可以看出来它包含两部分功能一个是 `map`，一个是 `flat`。实际上 `flatMap` 是综合了 `map` 和 `flat` 的操作，所以它也**只能打平一层**。

```javascript
const arr = [1, 2, 3];
console.log(arr.map((item) => [item * 2]).flat()); // [2, 4, 6]
console.log(arr.flatMap((item) => [item * 2])); // [2, 4, 6]
```

#### Object.fromEntries()

`Object.fromEntries()` 方法把键值对列表转换为一个对象，它是 `Object.entries()`的反函数。

```javascript
// 用Object.fromEntries()配合Object.entries()来实现年龄大于16的筛选
const ageObj = {
  wang: 21,
  sun: 12,
  li: 23
};

const ageOver_16 = Object.fromEntries(
  Object.entries(ageObj).filter(([name, age]) => age > 16)
);

console.log(ageOver_16); // {wang: 21, li: 23}
```

#### Symbol.prototype.description

`description` 是一个只读属性，它会返回 `Symbol` 对象的可选描述的字符串。与 `Symbol.prototype.toString()` 不同的是它不会包含 `Symbol()`的字符串。

```javascript
Symbol("desc").toString(); // "Symbol(desc)"
Symbol("desc").description; // "desc"
Symbol("").description; // ""
Symbol().description; // undefined

// 具名 symbols
Symbol.iterator.toString(); // "Symbol(Symbol.iterator)"
Symbol.iterator.description; // "Symbol.iterator"

//全局 symbols
Symbol.for("foo").toString(); // "Symbol(foo)"
Symbol.for("foo").description; // "foo"
```

#### String.prototype.matchAll

`matchAll()` 方法返回一个包含所有匹配正则表达式的结果及分组捕获组的迭代器。并且返回一个不可重启的迭代器。

```javascript
var regexp = /t(e)(st(\d?))/g
var str = 'test1test2'

str.match(regexp) // ['test1', 'test2']
str.matchAll(regexp) // RegExpStringIterator {}
[...str.matchAll(regexp)] // [['test1', 'e', 'st1', '1', index: 0, input: 'test1test2', length: 4], ['test2', 'e', 'st2', '2', index: 5, input: 'test1test2', length: 4]]
```

#### Function.prototype.toString()

ES2019 中，`Function.toString()`发生了变化。之前执行这个方法时，得到的字符串是去空白符号的。而现在，得到的字符串呈现出原本源码的样子：

```javascript
function sum(a, b) {
  return a + b;
}
console.log(sum.toString());
// function sum(a, b) {
//  return a + b;
// }
```

#### try-catch

在以往的版本中，try-catch 里 catch 后面必须带异常参数，例如：

```javascript
// ES10之前
try {
  // tryCode
} catch (err) {
  // catchCode
}
```

但是在 ES10 之后，这个参数却不是必须的，如果用不到，我们可以不用传，例如：

```javascript
try {
  console.log("Foobar");
} catch {
  console.error("Bar");
}
```

#### BigInt

`BigInt` 是一种内置对象，它提供了一种方法来表示大于 `2 ** 53 - 1` 的整数。这原本是 `Javascript` 中可以用 `Number` 表示的最大数字。`BigInt` 可以表示任意大的整数。

可以用在一个整数字面量后面加 `n` 的方式定义一个 `BigInt` ，如：`10n`，或者调用函数`BigInt()`。

在以往的版本中，我们有以下的弊端：

```javascript
// 大于2的53次方的整数，无法保持精度
2 ** 53 === 2 ** 53 + 1;
// 超过2的1024次方的数值，无法表示
2 ** 1024; // Infinity
```

但是在 ES10 引入 `BigInt` 之后，这个问题便得到了解决。

以下操作符可以和 `BigInt` 一起使用： `+`、`*`、`-`、`**`、`%` 。除 `>>>` （无符号右移）之外的位操作也可以支持。因为 `BigInt` 都是有符号的， `>>>` （无符号右移）不能用于 `BigInt`。`BigInt` 不支持单目 (`+`) 运算符。

`/` 操作符对于整数的运算也没问题。可是因为这些变量是 `BigInt` 而不是 `BigDecimal` ，该操作符结果会向零取整，也就是说不会返回小数部分。

`BigInt` 和 `Number`不是严格相等的，但是宽松相等的。

所以在`BigInt`出来以后，JS 的原始类型便增加到了** 8 **个，如下：

- Boolean
- Object
- Undefined
- Number
- String
- Function
- Symbol (ES6)
- BigInt (ES10)

#### globalThis

`globalThis` 属性包含类似于全局对象 `this` 值。所以在全局环境下，我们有：

```javascript
globalThis === this; // true
```

#### import()

静态的 `import` 语句用于导入由另一个模块导出的绑定。无论是否声明了 严格模式，导入的模块都运行在严格模式下。在浏览器中，`import` 语句只能在声明了 `type="module"` 的 `script` 的标签中使用。

但是在 ES10 之后，我们有动态 `import()`，它不需要依赖 `type="module"` 的 `script` 标签。

```javascript
const main = document.querySelector("main");
for (const link of document.querySelectorAll("nav > a")) {
  link.addEventListener("click", (e) => {
    e.preventDefault();

    import("/modules/my-module.js")
      .then((module) => {
        module.loadPageInto(main);
      })
      .catch((err) => {
        main.textContent = err.message;
      });
  });
}
```

### 结语

> 一个 ECMAScript 标准的制作过程，包含了 Stage 0 到 Stage 4 五个阶段。还有一些 ES10 的新特性（例如：类的私有变量、可选链操作符）处于 Stage 3 或者 Stage 4 阶段，都是实用性很强的应用，期待美好事情的发生 :santa:
