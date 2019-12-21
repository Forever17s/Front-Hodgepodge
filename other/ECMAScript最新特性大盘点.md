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
