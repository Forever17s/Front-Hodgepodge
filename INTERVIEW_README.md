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

##### vue 生命周期和钩子函数 [:cyclone:](#主要内容包含)

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

###### vue

    只要想要在 Vue 中直接操作 DOM 元素，就必须用 ref 属性进行注册.先在 DOM 中使用 ref 标签进行了注册，然后便可以通过 this.$refs 再跟注册时的名称来引用 DOM 元素了

###### react

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
