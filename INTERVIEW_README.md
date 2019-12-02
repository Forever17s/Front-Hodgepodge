<h2 align="center">
 
:paw_prints: 前端杂烩，记录我脱离低级趣味的点点滴滴

</h2>

<div align="center">

[设计模式](./README.md) | 前端面试

  <img src="./resource/interview_header.png" width="500" />

</div>

<br>

> 本章内容包含『 Vue 』、『 React 』、『 ES6 』和『 前端常识 』的一些内容，均是亲身经历后总结的前端高频面试考点 。面向 **1 - 3** 年工作经验的前端工程师 🦍🦍🦍

### 主要内容包含

- [:cyclone:前端基础](#前端基础)
- [:rocket:前端攀阶](#前端攀阶)
- [:ledger:前端常识](#前端常识)

#### 前端基础

##### HTML 文档三种常见 DOM 节点 [:cyclone:](#主要内容包含)

- 元素节点（标签）
- 文本节点（文本内容）
- 属性节点（元素属性）

##### javascript 变量提升 [:cyclone:](#主要内容包含)

```
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

```
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

```
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

> 算法分析：最佳 O(nlog2 n)，最差 O(n<sup>2</sup>)，平均 O(nlog2 n)。**实际中最常用的一种排序算法，速度快，效率高**

###### 求解算法的时间复杂度的具体步骤：

```
一、找出算法中基本语句；

二、计算基本语句的执行次数的数量级；

三、用大 O 记号表示算法的时间性能；
```

##### 线性表查找 [:cyclone:](#主要内容包含)

```
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

判断方式如下

> 要是基础类型可以用 `typeOf()`来判断;  
> 引用类型可以用 `instanceOf Array`;  
> 原型链方法`.constructor == Array`;  
> 通用的方法 `Object.prototype.toString.call(数据)`;

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

##### ES6 新特性 [:cyclone:](#主要内容包含)

- Default Parameters（默认参数）
- Template Literals （模板文本）
- Multi-line Strings （多行字符串）
- Destructuring Assignment （解构赋值）
- Enhanced Object Literals （增强的对象文本）
- Arrow Functions （箭头函数）
- Promises
- Block-Scoped Constructs Let and Const（块作用域构造 `Let` and `Const`）
- Classes（类）
- Modules（模块）

##### axios 特色 [:cyclone:](#主要内容包含)

- 浏览器端发起 `XMLHttpRequests` 请求
- `node` 端发起 `http` 请求
- 支持 `Promise API`
- 拦截请求和返回
- 转化请求和返回（数据）
- 取消请求
- 自动转化 `json` 数据
- 客户端支持抵御 `XSRF`（跨站请求伪造）

##### Vue 生命周期和钩子函数 [:cyclone:](#主要内容包含)

| 生命周期      | 描述                                                                     |
| ------------- | ------------------------------------------------------------------------ |
| beforeCreate  | `$el`和`data`并未初始化 （`data`：undefined，`$el`：undefined）          |
| created       | 完成了`data`数据的初始化，el 没有（`data`：XXX 可获取，\$el：undefined） |
| beforeMount   | 完成了`$el`和`data`的初始化（`data`：XXX 可获取，`$el`：XXX 虚拟）       |
| mounted       | 完成挂载（`data`：XXX 可获取，`$el`：XXX 真实）                          |
| beforeUpdate  | 组件更新之前                                                             |
| updated       | 组件更新之后                                                             |
| \*activated   | 针对 `keep-alive`，组件被激活时调用                                      |
| \*deactivated | 针对 `keep-alive`，组件被移除时调用                                      |
| beforeDestory | 组建销毁前调用                                                           |
| destoryed     | 组件销毁后调用                                                           |
| beforecreate  | 举个栗子：可以在这加个 loading 事件                                      |
| created       | 在这结束 loading，还做一些初始化，实现函数自执行                         |
| mounted       | 在这发起后端请求，拿回数据，配合路由钩子做一些事情                       |
| beforeDestory | 你确认删除 XX 吗？ `destoryed` ：当前组件已被删除，清空相关内容          |

##### addEventListener 监听函数 [:cyclone:](#主要内容包含)

```javascript
var btnAdd = document.getElementById("btnAdd");

// 第三个参数默认为false,表示冒泡模型即是多个触发事件的时候执行顺序是从里层开始执行到外层;true，表示捕抓模型
btnAdd.addEventListener(
  "click",
  function() {
    // ...
  },
  false
);
```

##### 循环中创建闭包：一个常见的错误 [:cyclone:](#主要内容包含)

```javascript
for (var i = 0; i <= oBtn.length; i++) {
  oBtn[i].onclick = function() {
    console.log(i);
  };
}
// 会使条目一直指向最后一位(拆分作用域可以看的更清楚)
```

###### 匿名函数(var fun = function(){...})

> 匿名函数主要有两种常用的场景，一是回调函数，二是直接执行函数。  
> 匿名函数直接执行函数的基本形式为`(function(){...})()`。
> 前面的括号包含函数体，后面的括号就是给匿名函数传递参数并立即执行之。  
> 匿名函数的作用是常用来包含代码以不污染全局命名空间运行后销毁环境，或者用做闭包。

###### 解决办法：

1. 使用匿名函数

```javascript

   for(var i=0;i<=oBtn.length;i++){
   (function(i){
   oBtn[i].onclick=function(){
   console.log(i);
   }
   )(i)
   }
```

2. let 块状作用域

```javascript
for (let i = 0; i <= oBtn.length; i++) {
  oBtn[i].onclick = function() {
    console.log(i);
  };
}
```

3. 使用 foreach 迭代数组

##### 时间戳 getTime() [:cyclone:](#主要内容包含)

返回的就是格林威治时间 1970 年 1 月 1 日 0 点 0 分 0 秒 0 毫秒到现在的毫秒数

##### 截取字符串 slice/splice/substr/substring(n,m) [:cyclone:](#主要内容包含)

```javascript
/**
 * slice和substring接收的是起始位置和结束位置(不包括结束位置),而substr接收的则是起始位置和所要返回的字符串长度
 * substring是以两个参数中较小一个作为起始位置，较大的参数作为结束位置。
 * slice可以对数组操作，substring不行
 * splice方法从array中移除一个或多个数组，并用新的item替换它们。参数n是从数组array中移除元素的开始位置。参数m是要移除的元素的个数。
 */
var a = ["a", "b", "c", "d"];
var b = a.slice(1, 3); // b=['b','c'],a=['a','b','c','d'];
var b = a.splice(1, 3, "e", "f"); // a=['a','e','f'],b=['b','c','d']
var b = a.substr(1, 3); // error if a='abcd',b='bcd',a='abcd'
var b = a.substring(1, 3); // error if a='abcd',b='bc',a='abcd'
// toUpperCase() 转换为大写
// toLowerCase() 转换为小写
// split() 讲字符串切割成数组
```

##### JS 设置 cookie [:cyclone:](#主要内容包含)

```javascript
/**
 * 保存cookies函数
 * @param {*} name
 * @param {*} value
 */
function SetCookie(name, value) {
  var Days = 30 * 12; // cookie 将被保存一年
  var exp = new Date(); // 获得当前时间
  exp.setTime(exp.getTime() + Days * 24 * 60 * 60 * 1000); // 换成毫秒
  document.cookie = name + "=" + escape(value) + ";expires=" + exp.toGMTString();
}
/**
 * 获取cookies函数
 * @param {*} name
 */
function getCookie(name) {
  var arr = document.cookie.match(new RegExp("(^| )" + name + "=([^;]*)(;|$)"));
  if (arr != null) {
    return unescape(arr[2]);
  } else {
    return null;
  }
}
/**
 * 删除cookies函数
 * @param {*} name
 */
function delCookie(name) {
  var exp = new Date(); //当前时间
  exp.setTime(exp.getTime() - 1);
  var cval = getCookie(name);
  if (cval != null) document.cookie = name + "=" + cval + ";expires=" + exp.toGMTString();
}
```

##### 简述面向对象 [:cyclone:](#主要内容包含)

> 万物皆对象，把一个对象抽象成类,具体上就是把一个对象的静态特征和动态特征抽象成属性和方法,也就是把一类事物的算法和数据结构封装在一个类之中,程序就是多个对象和互相之间的通信组成的. 面向对象具有封装性,继承性,多态性。  
>  **『 封装 』**:隐蔽了对象内部不需要暴露的细节,使得内部细节的变动跟外界脱离,只依靠接口进行通信.封装性降低了编程的复杂性。  
> 通过 **『 继承 』**,使得新建一个类变得容易,一个类从派生类那里获得其非私有的方法和公用属性的繁琐工作交给了编译器。  
> 而继承和实现接口和运行时的类型绑定机制 所产生的 **『 多态 』**,使得不同的类所产生的对象能够对相同的消息作出不同的反应,极大地提高了代码的通用性. 总之,面向对象的特性提高了大型程序的重用性和可维护性。

##### Number/JSON [:cyclone:](#主要内容包含)

```javascript
Infinity; // 无穷数 一般用来描述动画的重复次数; N维数组的无限降维 arr.flat(Infinity)
Number.isFinite(obj); // isFinite()检测传入的参数是否是一个有穷数
Number.isNaN(obj); // isNaN()函数用于检查其参数是否是非数字值。
JSON.parse(); // 用于将一个 JSON 字符串转换为 JavaScript 对象。
JSON.stringify(); // 用于将 JavaScript 值转换为 JSON 字符串。
```

##### attachEvent 和 addEventListener [:cyclone:](#主要内容包含)

###### 相同点

> dom 对象的方法，可以实现一种事件绑定多个事件处理函数。

```javascript
obj = document.getElementById("testdiv");
// 点击时，三个方法都会执行
obj.attachEvent('onclick',function(){{console.log('1');});
obj.attachEvent('onclick',function(){{console.log('2');});
obj.attachEvent('onclick',function(){{console.log('3');});
obj = document.getElementById("testdiv");
// 点击时，三个方法都会执行
obj.addEventListener('click',function(){{console.log('1');},false);
obj.addEventListener('click',function(){{console.log('2');},false);
obj.addEventListener('click',function(){{console.log('3');},false);
```

###### 不同点

> 执行顺序不同

```javascript
// attachEvent
console.log("3");
console.log("2");
console.log("1");

// addEventListener
console.log("1");
console.log("2");
console.log("3");
```

1. `attachEvent` 是 IE 有的方法，它不遵循 W3C 标准，而其他的主流浏览器如 FF 等遵循 W3C 标准的浏览器都使用 `addEventListener`，所以实际开发中需分开处理。
2. 多次绑定后执行的顺序是不一样的，`attachEvent` 是后绑定先执行，addEventListener 是先绑定先执行。
3. 绑定时间时，`attachEvent` 必须带 `on`，如 `onclick`，`onmouseover` 等，而 `addEventListener` 不能带 `on`，如 `click`，`mouseover`。这个区别在以上代码中可见。
4. `attachEvent` 仅需要传递两个参数，而 `addEventListener` 需要传递三个参数，这里牵扯到『 事件流 』的概念。

##### Spread Operator 展开运算符 [:cyclone:](#主要内容包含)

Spread Operator 即 3 个点 ...，有几种不同的使用方法。

- 可用于组装数组

```javascript
const todos = ["Learn dvaA", "Learn dvaB"];

// ['Learn dvaA','Learn dvaB' 'Learn antd']
console.log([...todos, "Learn antd"]);
```

- 也可用于获取数组的部分项。

```javascript
const arr = ["a", "b", "c"];

const [first, ...rest] = arr;
console.log(rest); // ['b', 'c']

const [first, , ...rest] = arr;
console.log(rest); // ['c']
```

- 还可以收集函数参数转换成数组

```javascript
function directions(first, ...rest) {
  console.log(rest);
}
directions("a", "b", "c"); // ['b', 'c'];
```

- 代替 apply

```javascript
function foo(x, y, z) {}
const args = [1, 2, 3];

// 下面两句效果相同
foo.apply(null, args);
foo(...args);
```

- 对应对象而言，组合成新的对象

```javascript
const foo = {
  a: 1,
  b: 2
};
const bar = {
  b: 3,
  c: 2
};
const d = 4;
const ret = { ...foo, ...bar, d }; // { a:1, b:3, c:2, d:4 }
```

##### JS 部分简写 :checkered_flag: 逼格拔高 [:cyclone:](#主要内容包含)

```javascript
// 取整
parseInt(a, 10); // Before
Math.floor(a); // Before
a >> 0; // Before
~~a; // After
a | 0; // After
// --------------------------
//四舍五入
Math.round(a); // Before
(a + 0.5) | 0; // After
// --------------------------
// 内置值
undefined; // Before
void 0; // After, 快
(0)[0]; // After, 略慢
// --------------------------
// 内置值
Infinity;
1 / 0;
// --------------------------
//布尔值短写法
true; // Before
!0; // After
//布尔值短写法
false; // Before
!1; // After
```

##### 关于 Ref [:cyclone:](#主要内容包含)

###### Vue

    只要想要在 Vue 中直接操作 DOM 元素，就必须用 ref 属性进行注册.先在 DOM 中使用 ref 标签进行了注册，然后便可以通过 this.$refs 再跟注册时的名称来引用 DOM 元素了

###### React

    ref 用在处理表单空间聚焦，文本选择，媒体播放以及触发动画效果等等. ref 是一个 React 的非常特殊的属性，这个属性一定是接受一个回调函数，这个回调函数会在组件渲染时进行执行一遍，在从视图中撤出来时会执行一遍

##### 关于 Set 和 Map [:cyclone:](#主要内容包含)

###### Set

`ES6`新的数据结构，类似数组但是内容唯一

```javascript
var set = new Set([1, 2, 3, 4, 4]);

[...set]; // [1, 2, 3, 4]
```

有四个操作方法：

- add(value)：添加某个值，返回 Set 结构本身。
- delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
- has(value)：返回一个布尔值，表示该值是否为 Set 的成员。
- clear()：清除所有成员，没有返回值

```javascript
// 可以结合 Array.from 深拷贝数组
var newArr = Array.from(new Set(arr));
var obj = { name: "test", age: 19, brother: ["jane", "john", "Mary"] };

// Array.from()方法可以将类数组对象和可遍历对象(key 为字符串)转换为真正的数组
Array.from(obj); // ['test',19,['jane','john','Mary']]

// Object.keys()可以取得数组对象的 key 集合
Object.keys(obj); // ['name','age','brother']
```

###### Map

Map 结构提供了`值--值`的对应，是一种更完善的`hash`结构的实现。类似`Object`,也是键值对的集合，但是『 键 』的范围不限于字符串，各种类型的值（包括对象）都可以成为『 键 』

```javascript
var m = new Map();
var o = { p: "Hello World" };
m.set(o, "content");
m.get(o); // "content"
m.has(o); // true
m.delete(o); // true
m.has(o); // false

// 只有对同一个对象的引用，Map结构才将其视为同一个键。这一点要非常小心
m.set(["a"], 233);
m.get(["a"]); // undefined。表面键相同但是内存地址不一样
```

> 实例属性和方法：**size**、**set**、**get**、**has**、**delete**、**clear**  
> 遍历方法：`keys()`、`values()`、`entries()`、`forEach()`

##### 同源策略[:cyclone:](#主要内容包含)

```
源（origin）就是协议、域名和端口号。
绕过同源策略:
  代理、
  Cors(跨域资源共享):
    浏览器发送跨域请求时，设置Http Request Header Origin = Current URI
    服务器若不接受该网站的跨域请求，则返回错误
    服务器若接受其跨域请求，则在Response Header中设置Access-Control-Allow-Origin属性
    浏览器看到Access-Control-Allow-Origin属性，同意解析返还结果。
  Jsonp:
    HTML中的script标签可以加载并执行其他域的javascript，于是我们可以通过script标记来动态加载其他域的资源
```

##### Css 中 white-space、word-break、word-wrap [:cyclone:](#主要内容包含)

- white-space: 用来控制空白符的显示，同时控制是否自动换行。常用`normal`(正常换行)、`nowrap`(不换行)
- word-break: 控制单纯如何被拆分换行。`normal`、`break-all`、`keep-all`
- word-wrap(overflow-wrap)：控制长度超过一行的单纯是否被拆分换行，是 word-break 的一个补充。`normal`、`break-word`
  > 配合`text-overflow：ellipsis`实现[文本溢出](#单行文本溢出)

##### CSS3 动画 [:cyclone:](#主要内容包含)

```css
.animation {
  position: absolute;
  animation: move 1.2s ease-in 80ms infinite alternate;
}
/* 其中属性
  animation-name: move; // 动画名称
  animation-duration: 5s; // 完成一周期需要时间
  animation-timing-function: linear; // 速度曲线
  animation-delay: 2s; // 开始动画延迟
  animation-iteration-count: infinite; // 播放次数
  animation-direction: alternate; // 下一周播放顺序
 */
@keyframes move {
  0% {
  }
  50% {
  }
  100% {
  }
}
```

##### CSS 怎么实现单、多行文本溢出 [:cyclone:](#主要内容包含)

###### 单行文本溢出

```css
.ellipsis {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
```

###### 多行文本溢出：

```css
.ellipsis {
  display: -webkit-box;
  -webkit-line-clamp: 3; /* 行数 */
  -webkit-box-orient: vitical; /* 水平排列方式 */
  text-overflow: ellipsis;
}
```

##### BFC 布局 （块级格式化上下文） [:cyclone:](#主要内容包含)

###### 应用

- 解决 `margin-top` 和 `margin-bottom` 叠加
- 用于布局（比如两栏布局）
- 用于清除浮动
- 计算 BFC 高度

###### 触发 BFC

- 设置除 `float:none` 以外的属性值（如：left | right）就会触发 BFC
- 设置除 `overflow: visible` 以外的属性值（如： hidden | auto | scroll）就会触发 BFC
- 设置 `display` 为表格布局或者弹性布局: inline-block | flex | inline-flex | table-cell | table-caption 就会触发 BF Ｃ
- 设置 `position` 为绝对定位：absolute | fixed 就会触发 BFC
- 使用 `fieldset` 元素（可以给表单元素设置环绕边框的 html 元素）也会触发 BFC

##### CSS 实现三角形 [:cyclone:](#主要内容包含)

```css
.triangle {
  width: 0px;
  height: 0px;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
  border-bottom: 10px solid blue;
}
```

##### CSS 垂直水平居中 [:cyclone:](#主要内容包含)

```css
plan-a. {
  position: absolute;
  margin: auto;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
plan-b. {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 100%;
  transform: translate(-50%, -50%);
  text-align: center;
}
plan-c. {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

##### 浏览器渲染原理 [:cyclone:](#主要内容包含)

1. DOM Tree：浏览器将 `HTML` 解析成树形的数据结构。
2. CSS Rule Tree：浏览器将 `CSS` 解析成树形的数据结构。
3. Render Tree: `DOM` 和 `CSSOM` 合并后生成 `Render Tree`。
4. layout：有了 `Render Tree`，浏览器已经能知道网页中有哪些节点、各个节点的 `CSS` 定义以及他们的从属关系，从而去计算出每个节点在屏幕中的位置。
5. painting: 按照算出来的规则，通过显卡，把内容画到屏幕上。

###### 回流 重绘

- reflow（回流）：当浏览器发现某个部分发生了点变化影响了布局，需要倒回去重新渲染
- repaint（重绘）：改变某个元素的背景色、文字颜色、边框颜色等等不影响它周围或内部布局时，屏幕需要一部分重画，但是元素尺寸没有改变

#### 前端攀阶

##### 如何判断一个对象有环 [:rocket:](#主要内容包含)

```javascript
function cycleDetector(obj) {
  // 请添加代码
  let result = true;
  try {
    JSON.stringify(obj);
  } catch (e) {
    result = false;
  } finally {
    return result;
  }
}
```

##### ES6 数组新特性 [:rocket:](#主要内容包含)

```javascript
/**
 * Array.forEach() // forEach遍历数组，无返回值，不改变原数组，仅仅只是遍历、常用于注册组件、指令等等。
 * Array.map() //【映射】 map遍历数组，和forEach用法相似，但是会返回一个新数组，map不会改变原数组的值。
 * Array.filter() //【筛选】 filter过滤掉数组中不满足条件的值，返回一个新数组，不改变原数组的值。
 * Array.some() //【或】 遍历数组的每一项，如果有一个满足条件则返回true，停止遍历，结果返回true。不改变数组。
 * Array.every() //【且】 遍历数组每一项，每一项返回true,则最终结果为true。当任何一项返回false时，停止遍历，返回false。不改变原数组
 * Array.reduce() //【从左到右累加】 Reduce让数组的前后两项进行某种计算。然后返回其值，并继续计算。不改变原数组，返回计算的最终结果，从数组的第二项开始遍历。
 * Array.reduceRight() //【从右到左累加】
 */

//array.reduce(callbackfn[, initialValue]) // initialValue选填，用作初始值来启动累积。array 和 callbackfn必填 （数组对象，遍历元素。遍历元素可以接受最多四个参数【result,item,index,arr 】）
var arr = [1, 2, 3, 4];
arr.reduce((result, item, index, arr) => {
  console.log(result); // 1  3  6  result为上次一计算的结果
  console.log(item); // 2  3  4
  console.log(index); // 1  2  3
  return result + item; // 最终结果为10
});
```

##### React 的 JSX [:rocket:](#主要内容包含)

> JSX 允许我们在 Javascript 中写 HTML，而不是用 HTML 包含 Javascript。它能帮助我们快速开发，因为我们不用担心字符串和换行等。JSX 每一个组件都有一个 `render` 方法，这个方法产生一个`ViewModel 视图模型`–在返回 HTML 到组件之前，你可以将这个模型的信息放入视图中，意味着你的 HTML 会根据这个模型动态改变(例如一个动态列表)。一旦你完成了操作，就可以返回你想渲染(`render`)的东西

##### JSX 的注意点 [:rocket:](#主要内容包含)

1. 如果在 JSX 中给元素添加类, 需要使用 `className` 代替 `class`
   类似：label 的 `for` 属性，使用 `htmlFor` 代替
2. 在 JSX 中可以直接使用 JS 代码，直接在 JSX 中通过 `{}` 中间写 JS 代码即可
3. 在 JSX 中只能使用表达式，但是 **不能出现语句**！！！
4. 在 JSX 中注释语法：{/_ 中间是注释的内容 _/}

##### React 组件生命周期 [:rocket:](#主要内容包含)

###### 装载过程

依次调用如下过程 `constructor` 、 `getInitialState` 、 `getDefaultProps` 、 `componentWillMont` 、 `render` 、 `componentDIdMount`

1. constructor

> 就是 `ES6` 里的构造函数，创建一个组件类的实例，在这一过程中我们要进行两步操作：初始化 state，绑定成员函数的 this 环境

2. getInitialState 和 getDefaultProps

> 这个两个过程可以不去深究，只会在用到 React.createClass 方法创造的组件类才会发生作用，所以在 `ES6` 的方法定义的 React 组件中根本不会用到

3. render

> 不夸张的说 `render` 是 React 组件中最为重要的一个函数。这个 React 中唯一不可忽略的函数，在 `render` 函数中，只能有一个父元素。

> 其中需要深入分析的是：`render` 函数是一个纯函数，他并不进行实际上的渲染动作，他只是一个 JSX 描述的结构，最终是由 React 来进行渲染过程，`render` 函数中不应该有任何操作，对页面的描述完全取决于 `this.state` 和 `this.props` 的返回结果，不能在 `render` 调用 `this.setState`

有一个公式总结的非常形象 `UI = render(data)`

4. componenWillMonunt 和 componentDidMount

> 这两个函数分别在 `render` 前后执行，`componentWillMonut` 显得有些鸡肋，还没有任何东西展示，操作完全可以提前到 `constructor` 中来进行  
> 但是 `componentDidMount` 函数作用就大得多，由于这一过程通常只能在浏览器端调用，所以我们在这里获取异步数据，而且在 `componentDidMount` 调用的时候，组件已经被装载到 DOM 树上了，还有，可以在这里执行其他库的代码，比如 Jquery

###### 更新过程

简单来说就是 props 和 state 被修改的过程，依次调用 `compWillReceiveProps`、 `shouldComponentUpdate`. `componentWillUpdate`、 `render`、 `componentDidUpdate`

1. componentWillReceiveProps（nextProps）

> 并不是只有在 `props` 发生改变的时候才会被调用，实际上只要是父组件的 `render` 函数被调用，`render`里面被渲染的子组件就会被更新，不管父组件传给子组件的 `props` 有没有被改变，都会触发子组件的 `componentWillReceiveProps` 过程，但是，`this.setState` 方法的触发过程不会调用这个函数，因为这个函数适合根据新的 `props` 的值来计算出是不是要更新内部状态的 `state`。

2. shouldComponentUpdate（nextProps， nextState）

> 这个函数的重要性，仅次于 `render`，稳坐第二把交椅，`render` 函数决定了改渲染什么，而 `shouldComponentUpdate` 决定了不需要渲染什么，这两位大佬都需要返回函数，这一过程可以提高性能，忽略掉没有必要重新渲染的过程

3. componentWillUpdate 和 componentDidUpdate

> 和装载过程不同，这里的 componentDidUpdate，既可以在浏览器端执行，也可以在服务器端执行

###### 卸载过程

> 如果你准备吧组件从 DOM 移除时，这个函数将会被调用。这让我们可以在组件背后进行清理，比如移除任何我们已经绑定的事件监听器。如果我们没有在自身背后做清理，而当其中一个事件被触发时，就会尝试去计算一个没有载入的组件，React 就会抛出一个错误。

##### React 中 render 的触发方式 [:rocket:](#主要内容包含)

- 首次渲染 `initial Render`
- 调用 `this.setState` (并不是一次 `setState` 会触发一次 `render`，`React` 可能会合并操作，再统一 `render`)
- 父组件发生更新(`props` 发生改变，但是就算 `props` 没有改变或者父子组件之间没有数据交换也会重新 `render`)
- 使用 `this.forceUpdate` 强制 `render`

##### Virtual DOM 算法简述 [:rocket:](#主要内容包含)

包括几个步骤

1. 用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中。
2. 当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异。
3. 把 `2` 所记录的差异应用到步骤 `1` 所构建的真正的 DOM 树上，视图就更新了。

> Virtual DOM 本质上就是在 `JS` 和 `DOM` 之间做了一个缓存。可以类比 CPU 和 硬盘，既然硬盘这么慢，我们就在它们之间加个缓存：既然 `DOM` 这么慢，我们就在它们 `JS` 和 `DOM` 之间加个缓存。`CPU（JS）`只操作`内存（Virtual）`。

##### Angular 和 Vue 的动态刷新方式 [:rocket:](#主要内容包含)

###### Angular

> 脏检查来刷新数据显示。一次脏检查就是调用一次 `$apply()` 或者 `$digest()`,将数据中最新的值呈现在界面上。在部分情况下，脏检查是低效的, 它的效率基本上取决于你绑定的观察者数量。

###### Vue

> 通过 `getter` 和 `setter` 的方法来检测后台数据变化，一旦变化就会被 `setter` 捕捉，然后来触发界面变化。同 Angular 事件驱动的方式不同的是通过数据驱动的。

Angular 可以手动触发 `apply()`来一次更新界面,但是 Vue 一旦检测数据变化 就会触发界面更新，这是很耗费性能的事。合理的选择才能突破 dom 优化的瓶颈。

##### React、Vue 和 Angular 的性能比较 [:rocket:](#主要内容包含)

###### 初始渲染

> Virtual DOM > 脏检查 >= 依赖收集

###### 小量数据更新

> 依赖收集 >> Virtual DOM + 优化 > 脏检查（无法优化） > Virtual DOM 无优化

###### 大量数据更新

> 脏检查 + 优化 >= 依赖收集 + 优化 > Virtual DOM（无法/无需优化）>> MVVM 无优化

**Virtual DOM 真正的价值从来都不是性能**，而是它

- 为函数式的 UI 编程方式打开了大门；
- 可以渲染到 DOM 以外的 Backend，比如 [React Native](https://reactnative.cn/)。

> 使用感知上：  
> ng 相当于中高档餐厅，用餐者的关注点在于用餐礼仪、消费能力等问题上。  
> vue 相当于便利店或者快餐厅，用餐者的关注点在于用餐效率。  
> react 则相当于家庭厨房，用餐者的关注点不再局限于`吃`这一个点上，既能反馈与处理食物细节问题，又能掌控制作食物的流程......

##### React 创建组件的两种方式 [:rocket:](#主要内容包含)

- 通过 JS 函数创建（**无状态组件**）
- 通过 class 创建（**有状态组件**）

###### javascript 函数创建, 注意:

- 函数名称必须为`大写字母`开头，React 通过这个特点来判断是不是一个组件
- 函数必须有返回值，返回值可以是：`JSX 对象`或 `null`
- 返回的 JSX，必须有一个`根元素`
- 组件的返回值使用`()`包裹，避免换行问题

##### 有关 key [:rocket:](#主要内容包含)

`key` 的特殊属性主要在 虚拟 DOM 算法，在新旧 `nodes` 对比时辨识 VNdoes。如果不适用 `key`，Vue 和 React 会使用一种最大限度减少动态元素并且尽可能的尝试修复、再复用相同类型元素的算法。使用 `key`，它会基于 `key` 的变化重新排列元素顺序，并且会移除 `key` 不存在的元素。**有相同父元素的的子元素必须有独特的 key**。重复的 key 会造成渲染错误。

##### Vue 修饰符 [:rocket:](#主要内容包含)

###### 事件修饰符

```
- .stop <!-- 阻止单击事件继续传播 -->
- .prevent <!-- 提交事件不再重载页面 -->
- .capture <!-- 添加事件监听器时使用事件捕获模式 -->
- .self <!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
- .once <!-- 点击事件将只会触发一次 -->
- .passive
- .native <!-- 给自定义的组件添加原生事件 -->
```

###### 按键修饰符

```
.enter
.tab
.delete <!-- 捕获“删除”和“退格”键 -->
.esc
.space
.up
.down
.left
.right
```

###### 系统修饰符

```
.ctrl
.alt
.shift
.meta
```

###### 表单输入修饰符

```
.lazy: <input v-model.lazy="msg" ><!-- 在“change”时而非“input”时更新 -->
.number: <input v-model.number="age"><!-- 将用户的输入值转为数值类型 -->
.trim: <input v-model.trim="msg"><!-- 过滤用户输入的首尾空白字符 -->
```

##### Vue 关于 \$emit 的用法 [:rocket:](#主要内容包含)

- 父组件可以使用 `props` 把数据传给子组件。
- 子组件可以使用 `$emit` 触发父组件的自定义事件。
  > vm.\$emit( event, […args] ) // 子组件通过 **\$emit** 来触发事件，将参数传递出去。

##### javascript 工厂模式函数、构造函数、原型模式 [:rocket:](#主要内容包含)

###### 工厂函数

> 工厂模式中的函数中会创建一个对象，最后 `return` 这个对象，通过每次调用时传入的参数不同来 **解决创建多个相似对象的问题**。

```javascript
// 工厂模式
function creatPerson(name, age, job) {
  var o = {};
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function() {
    console.log(this.name);
  };
  return o;
}
var tianjiao = creatPerson("tj", 22, "fe");
console.log(tianjiao);
```

###### 构造函数

> 构造函数本身也是函数，只不过是一个 **创建对象** 的函数

```javascript
// 构造函数
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.sayName = function() {
    console.log(this.name);
  };
}
var tj2 = new Person("tj2", 23);
console.log(tj2);
```

###### 原型模式

> 每个函数都有一个 `prototype` 属性，这个属性是一个指针，指向一个对象，这个对象的好处是可以 **让所有对象实例共享他所包含的属性和方法**。

```javascript
// 原型方法
function Person() {}
Person.prototype.name = "tj3";
Person.prototype.age = 24;
Person.prototype.sayName = function() {
  alert(this.name);
};
var tj3 = new Person();
console.log(tj3);
```

##### 事件轮询机制 bind、apply、call 的区别 [:rocket:](#主要内容包含)

`call` 和 `apply` 都是对函数的直接调用，**改变 this 指针指向即改变作用域**

```javascript
// 传参方式不一样，call必须将参数从第二位一个个塞进去
call(obj, arg1, arg2, arg3);
apply(obj, [arg1, arg2, arg3]);
```

```javascript
function sum(num1, num2) {
  return num1 + num2;
}
function callFun(a, b) {
  return sum.call(this, a, b);
}
function applyFun(a, b) {
  return sum.apply(this, [a, b]);
  // 或者
  // return sum.apply(this,arguments);
}
function bindFun(a, b) {
  //bind 方法返回的仍然是一个函数
  return sum.bind(this, a, b)();
}
alert(callFun(10, 10)); //20
alert(applyFun(10, 10)); //20
alert(bindFun(10, 10)); //20
```

###### 手写 call

```javascript
Funtion.prototype.mycall = function(context, ...args) {
  const cxt = context ? Object(context) : window; // 返回 this 参数或者 window,this 参数可能为基本数据类型，通过 Object 转换一下
  cxt.fn = this;
  let res = cxt.fn(...args);
  delete cxt.fn;
  return res;
};
```

###### 手写 apply

```javascript
Function.prototype.myapply = function(context, arr) {
  const cxt = context ? Object(context) : window;
  cxt.fn = this;
  let res;
  if (arr) res = cxt.fn(...arr);
  else res = cxt.fn();
  delete cxt.fn;
  return res;
};
```

###### 手写 bind

```javascript
Function.prototype.mybind = function(ctx, ...args) {
  // return (...a)=> this.call(ctx, ...args, ...a);
  return (...a) => this.apply(ctx, [...args, ...a]);
};
```

测试脚本

```javascript
const w = { name: "wgc" };
function testW() {
  console.log(this.name);
  console.log(...arguments);
}
const bindF = testW.mybind(w, "WGC");
bindF("hello");
testW.mycall(w, "nihao", "wgc");
testW.myapply(w, ["NIHAO", "WGC"]);
```

##### 函数防抖、函数节流 [:rocket:](#主要内容包含)

- 函数防抖：(任务触发的间隔超过指定间隔的时候，任务才会执行)
  - 定义：多次触发事件后，事件处理函数只执行一次，并且是在触发操作结束时执行。
  - 原理：对处理函数进行延时操作，若设定的延时到来之前，再次触发事件，则清除上一次的延时操作定时器，重新定时。
  - 手写代码:
  ```javascript
  function debounce(fn, wait = 500) {
    let timer = null;
    return function() {
      clearTimeout(timer);
      timer = setTimeout(() => {
        fn.apply(this, arguments);
      }, wait);
    };
  }
  ```
- 函数节流：(指定时间间隔内只会执行一次任务)
  - 定义：触发函数事件后，短时间间隔内无法连续调用，只有上一次函数执行后，过了规定的时间间隔，才能进行下一次的函数调用
  - 原理：设置状态，对处理函数进行延时操作，若下一次事件触发设定的延时到来之前，状态还没更新，事件无效。
  - 手写代码：
  ```javascript
  function throttle(fn, wait = 500) {
    var timerStatus = true;
    return function() {
      if (!timerStatus) return;
      timerStatus = false;
      setTimeout(() => {
        fn.apply(this, arguments);
        timerStatus = true;
      }, wait);
    };
  }
  ```

##### Vue 父子组件通过 props 传值示例 [:rocket:](#主要内容包含)

```javascript
var childNode = {
  template:'<div>{{myMessage}}的类型是{{type}}</div>'，
  props:{'myMessage':Number},
  computed:{
    type(){
      return typeof this.myMessage;
    }
  }
}
var parentNode = {
  template :`
    <div class="parent">
      <my-child :my-message="num"></my-child>
    </div>`,
  components:{
    'myChild': childNode
  },
  data() {
    return {
      num: 1234567,
    }
  }
}

new Vue{(
  el: '#app',
  components: {
    'Home': parentNode
  }
)}
```

##### Vue/React 组件间通信 [:rocket:](#主要内容包含)

###### VUE

- 兄弟组件：创建一个事件总线 `bus`
  > 在需要传值的组件中用 `bus.$emit` 触发一个自定义事件，并传递参数  
  > 在需要接收数据的组件中用 `bus.$on` 监听自定义事件，并在回调函数中处理传递过来的参数
- 父向子传值：在组件中绑定属性，子组件中 `props` 就可以获取
- 子向父传值：通过在子组件方法中调用`$emit(方法名, 参数)`,方法名父组件中用 `on` 绑定

###### REACT

- 兄弟组件：
  > `context`（还可以很好的适用跨级传值，和 `Vue` 中的 `bus` 属性一样都应该是高作用域的参数）一个是 `Context` 生产者『 Provider 』，通常是一个父节点，另外是一个 `Context` 的消费者『 Consumer 』，通常是一个或者多个子节点。所以 `Context` 的使用基于生产者消费者模式
- 父向子传值：在组件中绑定属性，子组件中 `props` 就可以获取
- 子向父传值：通过在子组件方法中通过 `props` 获取方法名，回调的方式传值

##### Vue 源码之 :checkered_flag: VNode [:rocket:](#主要内容包含)

其实 `VNode` 是对真实 `DOM` 的一种抽象描述，它的核心定义无非就几个关键属性，标签名、数据、子节点、键值等，其它属性都是都是用来扩展 `VNode` 的灵活性以及实现一些特殊 `feature` 的。由于 `VNode` 只是用来映射到真实 `DOM` 的渲染，不需要包含操作 `DOM` 的用法，因此它是非常轻量和简单的。`VirtualDOM` 除了它的数据结构的定义，映射到真实的 `DOM` 实际上要经历 `VNode` 的 **_create_**、**_diff_**、**_patch_** 等过程。

```javascript
/**
 * @param context 表示 VNode 的上下文环境，它是 Component 类型
 * @param tag 表示标签，它可以是一个字符串，也可以是一个 Component
 * @param data 表示 VNode 的数据，它是一个 VNodeData 类型
 * @param children 表示当前 VNode 的子节点，它是任意类型的
 * @param normalizationType 表示子节点规范的类型，类型不同规范的方法也就不一样，主要是参考 render 函数是编译生成还是用户手写的
 */
createElement (
  context: Component,
  tag?: string | Class<Component> | Function | Object,
  data?: VNodeData,
  children?: any,
  normalizationType?: number
){
  // 生成 VNode 的过程...
}
```

`Vue` 的 `update` 是实例的一个私有方法，被调用的时机有两个，一个是首次渲染，一个是数据更新的时候。`update` 方法的作用是把 `VNode` 渲染成真实的 `DOM`。

update 的核心就是调用 `vm.patch` 方法，这个方法在不同平台（服务器端、浏览器端）的定义是不一样的

```javascript
update (vnode: VNode, hydrating?: boolean)
```

##### Vue 源码之 :checkered_flag: 响应式对象 [:rocket:](#主要内容包含)

**核心是利用 ES5 的 Object.defineProperty**,这也是 `Vue.js` 为什么不能兼容 `IE8` 及以下浏览器的原因。

`Object.defineProperty` 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回这个对象。

```javaScript
Object.defineProperty(
  obj, // 定义属性的对象
  prop, // 要定义或修改的属性的名称
  descriptor, // 将被定义或修改属性的描述符【核心】
)
```

> **observe** 的功能就是用来监测数据的变化。实现方式是给非 `VNode` 的对象类型数据添加一个 `Observer`,如果已经添加过则直接返回，否则在满足一定条件下去实例化一个 `Observer` 对象实例。

Observer 是一个类,它的作用是给对象属性添加 『 getter 』和『 setter 』,用于 **依赖收集** 和 **派发更新**

---

**依赖收集 getter**（重点关注以下两点）

- `*const dep = new Dep() // 实例化一个Dep实例`
- `*在 get 函数中通过 dep.depend 做依赖收集`

`Dep` 是一个 `Class`,它定义了一些属性和方法，它有一个静态属性 `target`，这是一个全局唯一 `Watcher`(同一时间内只能有一个全局的 `Watcher` 被计算)`。Dep` 实际上就是对 `Watcher` 的一种管理，`Dep` 脱离 `Watcher` 单独存在是没有意义的。`Watcher` 和 `Dep` 就是典型的 观察者设计模式，也叫做 [发布-订阅模式](./README.md#5发布-订阅模式)。

`Watcher` 是一个 `Class`,在它的构造函数中定义了一些和 `Dep` 相关的属性：

```javascript
this.deps = [];
this.newDeps = [];
this.depIds = new Set();
this.newDepIds = new Set();
```

**收集过程**：当我们实例化一个渲染 `watcher` 的时候，首先进入 `watcher` 的构造函数逻辑，然后执行他的`this.get()`方法，进入 `get` 函数把 `Dep.target` 赋值为当前渲染 `watcher` 并压栈（为了恢复用）。接着执行`vm._render()`方法，生成渲染 `VNode`,并且在这个过程对 `vm` 上的数据访问，这个时候就触发数据对象的 `getter`（在此期间执行`Dep.target.addDep(this)`方法，将 `watcher` 订阅到这个数据持有的 `dep` 的 `subs` 中，为后续数据变化时通知到哪些 `subs` 做准备）。然后递归遍历添加所有子项的 `getter。`

> `Watcher` 在构造函数中初始化两个 `Dep` `实例数组。newDeps` 代表新添加的 `Dep` 实例数组，`deps` 代表上一次添加的 `Dep` 实例数组。  
> 依赖清空：在执行清空依赖（`cleanupDeps`）函数时，会首先遍历 `deps`,移除对 `dep` 的订阅，然后把 `newDepsIds` 和 `depIds` 交换，`newDeps` 和 `deps` 交换，并把 `newDepIds` 和 `newDeps` 清空。考虑场景，在条件渲染时，及时对不需要渲染数据的订阅移除，减少性能浪费。

考虑到 Vue 是数据驱动的，所以每次数据变化都会重写 Render,那么`vm._render()`方法会再次执行，并再次触发数据。

收集依赖的目的是为了当这些响应式数据发生变化，触发它们的 setter 的时候，能知道应该通知哪些订阅者去做相应的逻辑处理【**派发更新**】

---

**派发更新 setter**（重点关注以下两点）

- `*childOb = !shallow && observe(newVal) // 如果shallow为false的情况，会对新设置的值变成一个响应式对象`
- `*dep.notify() // 通知所有订阅者`

**派发过程**：当我们组件中对响应的数据做了修改，就会触发 `setter` 的逻辑，最后调用`dep.notify()`方法，它是 `Dep` 的一个实例方法。具体做法是遍历依赖收集中建立的 `subs`，也就是 `Watcher` 的实例数组【`subs` 数组在依赖收集 `getter` 中被添加，期间通过一些逻辑处理判断保证同一数据不会被添加多次】，然后调用每一个 `watcher` 的 `update` 方法。

`update` 函数中有个`queueWatcher(this)`方法引入了队列的概念，是 `vue` 在做派发更新时优化的一个点，它并不会每次数据改变都会触发 `watcher` 回调，而是把这些 `watcher` 先添加到一个队列中，然后在 nextTick 后执行 `watcher` 的 `run` 函数
**队列排序保证：**

1. 组件的更新由父到子。父组件创建早于子组件，`watcher` 的创建也是
2. 用户自定义 `watcher` 要早于渲染 `watcher` 执行，因为用户自定义 `watcher` 是在渲染 `watcher` 前创建的
3. 如果一个组件在父组件 `watcher` 执行期间被销毁，那么它对应的 `watcher` 执行都可以被跳过，所以父组件的 `watcher` 应该先执行。

- 队列遍历：排序完成后，对队列进行遍历，拿到对应的 `watcher`,执行`watcher.run()`。

**run 函数解析**：先通过`this.get()`得到它当前的值，然后做判断，如果满足新旧值不等、新值是对象类型、deep 模式任何一个条件，则执行 `watcher` 的回调，注意回调函数执行的时候会把第一个参数和第二个参数传入新值 `value` 和旧值 `oldValue`。<u>_这就是当我们自己添加 watcher 时候可以在参数中取到新旧值的来源_</u>。对应渲染 `watcher` 而言，在执行`this.get()`方法求值的时候，会执行 `getter` 方法。因此在我们修改组件相关数据时候，会触发组件重新渲染，接着重新执行 `patch` 的过程

---

### 手写一个数据绑定：

```javascript
<input id="input" type="text" />
<div id="text"></div>

let input = document.getElementById("input");
let text = document.getElementById("text");
let data = { value: "" };
Object.defineProperty(data, "value", {
  set: function(val) {
    text.innerHTML = val;
    input.value = val;
  },
  get: function() {
    return input.value;
  }
});
input.onkeyup = function(e) {
  data.value = e.target.value;
};
```
