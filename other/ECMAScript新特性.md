> 从 ECMAScript 2016（ES7）开始，版本发布变得更加频繁，每年发布一个新版本，好在每次版本的更新内容并不多，本文会细说这些新特性，尽可能和旧知识相关联，帮你迅速上手这些特性。

### ES7 新特性

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
  [1, 4, -5, 10].find(n => n < 0); // -5

  [1, 5, 10, 15].findIndex(n => n > 9); // 2

  [NaN].findIndex(y => Object.is(NaN, y)); // 0
  ```

#### 求幂运算符『 \*\* 』

在 ES7 中引入了指数运算符，具有与 `Math.pow()` 等效的计算结果

```javascript
console.log(2 ** 10); // 1024
console.log(Math.pow(2, 10)); // 1024
```

---

### ES8 新特性

#### Async/Await

虽然 `Promise` 可以解决回调地狱的问题，但是链式调用太多，则会变成另一种形式的回调地狱 ——『 面条地狱 』，所以在 ES8 里则出现了 `Promise` 的语法糖 `async/await`，专门解决这个问题。

我们先看一下下面的 Promise 代码：

```javascript
fetch("coffee.jpg")
  .then(response => response.blob())
  .then(myBlob => {
    let objectURL = URL.createObjectURL(myBlob);
    let image = document.createElement("img");
    image.src = objectURL;
    document.body.appendChild(image);
  })
  .catch(e => {
    console.log("There has been a problem with your fetch operation: " + e.message);
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

### ES9 新特性

#### for await of

> `for of` 方法能够遍历具有 `Symbol.iterator` 接口的同步迭代器数据，但是不能遍历异步迭代器。ES9 新增的 `for await of` 可以用来遍历具有 `Symbol.asyncIterator` 方法的数据结构，也就是异步迭代器，且会等待前一个成员的状态改变后才会遍历到下一个成员，相当于 `async` 函数内部的 `await`。

现在我们有三个异步任务，想要实现依次输出结果，该如何实现呢？

```javascript
// for of遍历
function Gen(time) {
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      resolve(time);
    }, time);
  });
}
(async function() {
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
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      resolve(time);
    }, time);
  });
}
(async function() {
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
  .then(response => {
    console.log(response.status);
  })
  .catch(error => {
    console.log(error);
  })
  .finally(() => {
    document.querySelector("#spinner").style.display = "none";
  });
```

无论操作是否成功，当您需要在操作完成后进行一些清理时，`finally()` 方法就派上用场了。这为指定执行完 `promise` 后，无论结果是 `fulfilled` 还是 `rejected` 都需要执行的代码提供了一种方式，避免同样的语句需要在 `then()` 和 `catch()`中各写一次的情况。
