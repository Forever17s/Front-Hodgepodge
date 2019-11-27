<h2 align="center">
 
:paw_prints: 前端杂烩，记录我脱离低级趣味的点点滴滴

</h2>

<div align="center">

[设计模式](./README.md) | 前端面试

  <img src="./resource/interview_header.png" width="500" />

</div>

<br>

> 本章内容包含『 Vue 』、『 React 』、『 ES6 』和『 前端常识 』的一些内容，均是亲身经历后总结的前端高频面试考点 。面向 1 - 3 年工作经验的前端工程师 🦍🦍🦍

### 主要内容包含

- [:cyclone:JS 基础](#js-基础)
- [:rocket:JS 攀阶](#JS-攀阶)
- [:ledger:前端常识](#前端常识)

#### JS 基础

##### HTML 文档三种常见 DOM 节点 [:cyclone:](#主要内容包含)

- 元素节点（标签）
- 文本节点（文本内容）
- 属性节点（元素属性）

##### javascript 变量提升 [:cyclone:](#主要内容包含)

```bash
对于变量 var x = 1; foo();

等同var x; foo(); x = 1;（提升变量声明，但不提升赋值）

对于函数 function fun(){};

函数本身也是一种变量，也会存在变量上升的现象，但是它是上升了整个函数

分析这种问题的时候，有意识拆分变量声明和赋值，提升函数位置会看得更明显
```

##### 运算符 `&` 与 `&&` 和 `|` 与 `||` [:cyclone:](#主要内容包含)

- `&`和 `|` 既是逻辑运算符也是位运算符，而`&&`和`||`只是逻辑运算符
- `&`和`|`都是先看左边成不成立，左边不成立直接输出
- `&&`和`||`两边都要看，看完之后再输出
- `&&`左真看右，左假为假；`||`左真为真，左假看右

##### `String` 字符串对象属性 [:cyclone:](#主要内容包含)

```bash
length、concat(String)、indexOf(String)、substr(num1,[num2])、toLowerCase()、toUpperCase()、replace(str1,str2)

Date日期对象：getYear()/getFullYear()/getMonth()/getDate()...

Math数学对象：ceil(数值)向下取整、floor(数值)向上取整、min(num1,num2)/max(num1,num2)返回最大最小值、pow(x,n)返回x的n次方、random()、round(数值)四舍五入、sqrt(数值)开平方
```

##### `Window` 对象 [:cyclone:](#主要内容包含)

- alert
- confirm
- prompt
- close
- print
- setintval
- clearintval
- setTimeout
- clearTimeout
- window 对象的子对象：

```bash
  navigator(浏览器信息对象)

  location(地址栏对象)：host 主机、port 端口、href 地址

  history(历史记录)：length 历史记录数目、back()/forward()/go()

  screen(屏幕对象)：height/width

  document HTML 文档对象
```

##### 数组去重四种方式 [:cyclone:](#主要内容包含)

1. 通过新建数组，遍历查找方法去重存入，第二个循环可以用`indexOf`替换)

```javascript
Array.prototype.unique1 = function(arr) {
  var newArr = [];
  for (var i in arr) {
    var repeat = false;
    for (var j in newArr) {
      if (newArr[j] == arr[i]) {
        repeat = true;
        break;
      }
    }
    if (!repeat) {
      newArr.push(arr[i]);
    }
  }
  return newArr;
};
var arr = [1, "a", "a", "b", "d", "e", "e", 1, 0];
console.log(arr.unique1());
```

2. 先排序，再向新数组最后一位查重存入

```javascript
Array.prototype.unique2 = function() {
  this.sort(); //先排序
  var newArr = [this[0]];
  for (var i = 1; i < this.length; i++) {
    if (this[i] !== newArr[newArr.length - 1]) {
      newArr.push(this[i]);
    }
  }
  return newArr;
};
var arr = [1, "a", "a", "b", "d", "e", "e", 1, 0];
console.log(arr.unique2());
```

3. 能通过`json`对象的方式判断去重并计算重复次数，缺点是只支持`Numbe`r 数据类型，解决方式可以在`key`值上下功夫，`typeof`+`key`的形式

```javascript
Array.prototype.unique3 = function() {
  var obj = {};
  for (var i = 0; i < this.length; i++) {
    if (!obj[this[i]]) {
      obj[this[i]] = 1;
    } else {
      obj[this[i]]++;
    }
  }
  return obj;
};
var arr = [112, 112, 34, "34", "你好", 112, 112, 34, "你好", "str", "str1"];
console.log(arr.unique3());
```

4. `Array.from(new Set(arr))`大简至极

```javascript
// 平铺并排序
Array.from(new Set(arr.flat(Infinity))).sort((a, b) => !!a - b);
```

##### CSS 元素隐藏显示 [:cyclone:](#主要内容包含)

- `display:none/block`

- `visibility:hidden/visible`

- `opacity:0/1`

##### 排序 [:cyclone:](#主要内容包含)

- 交换排序：冒泡排序、快速排序
- 插入排序：直接插入排序、希尔排序
- 选择排序：简单选择排序、堆排序
- 归并排序

###### 1）冒泡排序

```javascript
function bubbleSort(arr) {
  var len = arr.length;
  for (var i = 0; i < len; i++) {
    for (var j = 0; j < len - 1 - i; j++) {
      if (arr[j] > arr[j + 1]) {
        //相邻元素两两对比
        var temp = arr[j + 1]; //元素交换
        arr[j + 1] = arr[j];
        arr[j] = temp;
      }
    }
  }
  return arr;
}
```

> 算法分析：最佳 O(n)，最差 O(n<sup>2</sup>)，平均 O(n<sup>2</sup>)

###### 2）简单选择排序

```javascript
function selectionSort(arr) {
  var len = arr.length;
  var minIndex, temp;
  for (var i = 0; i < len; i++) {
    minIndex = i;
    for (var j = i + 1; j < len; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }
    temp = arr[minIndex];
    arr[minIndex] = arr[i];
    arr[i] = temp;
  }
  return arr;
}
```

> 算法分析：最佳 O(n<sup>2</sup>)，最差 O(n<sup>2</sup>)，平均 O(n<sup>2</sup>)，十分稳定

###### 3）插入排序

```javascript
function insertionSort(arr) {
  var len = arr.length;
  var preIndex, current;
  for (var i = 1; i < len; i++) {
    current = arr[i]; // 记录当前位置变量
    preIndex = i - 1; // 当前位置前一个索引值
    while (preIndex >= 0 && arr[preIndex] > current) {
      // 索引大于0并且前一个元素比变量大
      arr[preIndex + 1] = arr[preIndex]; // 将该索引位置元素向后移动一位
      preIndex--; // 索引再指向前一个
    }
    arr[preIndex + 1] = current; // 将之前记录的那个变量放到索引后一个位置
  }
  return arr;
}
```

> 算法分析：最佳 O(n)，最差 O(n<sup>2</sup>)，平均 O(n<sup>2</sup>)，类似扑克牌正牌

###### 4）希尔排序

```javascript
function shellSort(arr) {
  var len = arr.length,
    temp,
    gap = 1;
  while (gap < len / 3) {
    // 动态定义间隔序列，既可以提前设定好间隔序列，也可以动态的定义间隔序列
    gap = gap * 3 + 1;
  }
  for (gap; gap > 0; gap = Math.floor(gap / 3)) {
    for (var i = gap; i < len; i++) {
      temp = arr[i];
      for (var j = i - gap; j > 0 && arr[j] > temp; j -= gap) {
        arr[j + gap] = arr[j];
      }
      arr[j + gap] = temp;
    }
  }
  return arr;
}
```

> 算法分析：最佳 O(n)，最差 O(n<sup>2</sup>)，平均 O(nlog2 n)

###### 5）归并排序

```javascript
function mergeSort(arr) {
  // 采用自上而下的递归方法
  var len = arr.length;
  if (len < 2) {
    return arr;
  }
  var middle = Math.floor(len / 2),
    left = arr.slice(0, middle),
    right = arr.slice(middle);
  return merge(mergeSort(left), mergeSort(right));
}
function merge(left, right) {
  var result = [];
  console.time("归并排序耗时");
  while (left.length && right.length) {
    if (left[0] <= right[0]) {
      result.push(left.shift());
    } else {
      result.push(right.shift());
    }
  }
  while (left.length) result.push(left.shift());
  while (right.length) result.push(right.shift());
  console.timeEnd("归并排序耗时");
  return result;
}
```

> 算法分析：O(nlog2n)。分解：折半划分，合并：两两合并后排序

###### 6）快速排序

```javascript
function quickSort(arr) {
  let len = arr.length;
  if (len <= 1) return arr; // 如果数组长度小于等于1直接返回（很必要）
  let midIndex = Math.floor(len / 2); // 长度一半向上取整获取作为对比的下标
  let midNum = arr.splice(midIndex, 1)[0]; // 在数组中取出对比下标的那个数
  let left = []; // 声明左边（小于对比数）的集合
  let right = []; // 声明右边（大于对比数）的集合
  len = arr.length;
  for (let i = 0; i < len; i++) {
    if (arr[i] < midNum) left.push(arr[i]);
    else right.push(arr[i]);
  } // 遍历数组，并向左右两边集合存入值
  return quickSort(left).concat([midNum], quickSort(right)); // 返回递归的组合集合
}
```

> 算法分析：最佳 O(nlog2 n)，最差 O(n<sup>2</sup>)，平均 O(nlog2 n)。实际中最常用的一种排序算法，速度快，效率高

###### 求解算法的时间复杂度的具体步骤：

```bash
一、找出算法中基本语句；

二、计算基本语句的执行次数的数量级；

三、用大 O 记号表示算法的时间性能；
```

##### 线性表查找 [:cyclone:](#主要内容包含)

```bash
顺序查找(ASL = (N + N-1 + ... + 2 + 1) / N = (N+1) / 2),时间复杂度为 O(N)

二分查找，过程可看成遍历一个二叉树，平均查找长度实际就是二叉树高度 O(log2N)

分块查找又称索引顺序查找。它是一种性能介于顺序查找和二分查找之间的查找方法。
```

##### 迪杰斯特拉和弗洛伊德算法 [:cyclone:](#主要内容包含)

###### 1）迪杰斯特拉算法

求从源点到其余各点的最短路径(『 贪心算法 』代表，权值不能存在负值)

###### 2）弗洛伊德算法

求每对顶点之间的最短路径(『 动态规划 』，权值能出现负值)

##### 鼠标事件 [:cyclone:](#主要内容包含)

- onmousemove 鼠标在元素上移动触发事件,子元素会冒泡给父元素
- onmouseenter 鼠标在进入元素时触发事件，不冒泡；
- onmouseover 鼠标在进入元素时触发事件, 冒泡。

- onmouseover、onmouseout：鼠标移动到自身时候会触发事件，同时移动到其子元素身上也会触发事件
- onmouseenter、onmouseleave：鼠标移动到自身是会触发事件，但是移动到其子元素身上不会触发事件

##### 六大类数据类型 [:cyclone:](#主要内容包含)

- undefined
- object
- boolean
- string
- number
- function

> // 判断方式如下
> 要是基础类型可以用 `typeOf()`来判断
> 引用类型可以用 `instanceOf Array`
> 原型链方法`.constructor == Array`
> 通用的方法 `Object.prototype.toString.call(数据)`

##### Object.assign() [:cyclone:](#主要内容包含)

###### 1）实现拷贝

用于将所有可枚举属性的值从一个或多个源对象复制到目标对象（浅拷贝）
JSON 解析反解析`JSON.parse(JSON.stringify(obj))`以实现深拷贝
ES6 扩展运算符实现数组的深拷贝:

```javascript
var arr = [1, 2, 3, 4, 5];
var [...arr2] = arr;
arr[2] = 5;
console.log(arr);
console.log(arr2);
```

```javascript
// 手写深拷贝
 function deepClone(obj) {
      if (typeof obj !== 'object') return obj;
      if (type window !== 'undefined' && window.JSON) {
        return JSON.parse(JSON.stringify(obj))
      } else {
        let newObj = obj.constructor === Array ? [] : {};
        for (let key in obj) {
          newObj[key] = typeof obj[key] === 'object' ? deepClone(obj[key]) : obj[key];
        }
        return newObj;
      }
    }
```

###### 2）合并对象的功能

`Object.assign` 方法的第一个参数是目标对象，后面的参数都是源对象

```javascript
const target = { a: 1, b: 1 };
const source1 = { b: 2, c: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target; // {a:1, b:2, c:3}
```
