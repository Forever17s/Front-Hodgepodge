<h2 align="center">
前端杂烩，记录我脱离低级趣味的点点滴滴
</h2>

## 设计模式

在程序设计中有很多使用的设计模式，而其中大部分语言的实现都是基于"类"

### 设计原则

##### 一、单一设计原则

    一个对象或方法只做一件事情。如果一个方法成单率过多的职责，那么在需求的变迁过程中，需要改写这个方法的可能性就越大

##### 二、最少知识原则

     一个软件实体应当尽可能少的与其他实体发生相互作用，尽量减少对象之间的交互，如果两个对象之间不必彼此直接通信，那么这两个对象就不要发生直接的相互联系，可以交给第三方进行处理

##### 一、开放-封闭原则

    软件实体（类、模块、函数）等应该是可以扩展的，但是不可修改。当需要改变一个程序的功能或者给这个程序增加新功能的时候，可以使用增加代码的方式，尽量避免改动程序的代码，防止影响原系统的稳定

### 什么是设计模式

> 假设有一个空房间，我们要日复一日地往里 面放一些东西。最简单的办法当然是把这些东西 直接扔进去，但是时间久了，就会发现很难从这 个房子里找到自己想要的东西，要调整某几样东 西的位置也不容易。所以在房间里做一些柜子也 许是个更好的选择，虽然柜子会增加我们的成 本，但它可以在维护阶段为我们带来好处。使用 这些柜子存放东西的规则，或许就是一种模式

### 模式介绍

- [单例模式](#单例模式)
- [策略模式](#策略模式)
- [代理模式](#代理模式)
- [迭代器模式](#迭代器模式)
- [发布-订阅模式](#发布-订阅模式)
- [命令模式](#命令模式)
- [组合模式](#组合模式)
- [模块方法模式](#模块方法模式)
- [享元模式](#享元模式)
- [职责链模式](#职责链模式)
- [中介模式](#中介模式)
- [装饰者模式](#装饰者模式)
- [状态模式](#状态模式)
- [适配器模式](#适配器模式)
- [外观模式](#外观模式)

#### 一、单例模式

##### 1. 定义

保证一个类仅有一个实例，并提供一个访问它的全局访问点

##### 2. 核心

确保只有一个实例，并提供全局访问

##### 3. 实现

假设要设置一个职位，多次调用也仅仅设置一次，我们可以使用闭包的方式缓存一个内部变量来实现这个单例

```javascript
// 提取出通用的单例
function getSingleton(fn) {
  var instance = null;

  return function() {
    if (!instance) {
      instance = fn.apply(this, arguments);
    }

    return instance;
  };
}

// 设置管理员
function SetManager(manager) {
  this.manager = manager;
}

SetManager.prototype.getManager = function() {
  console.log(this.manager);
};

var SingletonSetManager = getSingleton(function(manager) {
  var manager = new SetManager(manager);
  return manager;
});

SingletonSetManager("a").getManager(); // a
SingletonSetManager("b").getManager(); // a

// 由于单例我们提取出来了就可以复用设置管理员的方式去设置别的职位
// 比如设置Hr
function SetHr(hr) {
  this.hr = hr;
}

SetHr.prototype.getHr = function() {
  console.log(this.hr);
};

var SingletonSetHr = getSingleton(function(hr) {
  var hr = new SetHr(hr);
  return hr;
});
SingletonSetHr("hr1").getHr(); // hr1
SingletonSetHr("hr2").getHr(); // hr1
```

#### 二、策略模式

##### 1. 定义

定义一系列的算法，把它们一个个封装起来，并且使他们可以互相替换

##### 2. 核心

将算方法的使用规则和实现逻辑分离开来。
一个基于策略模式的程序至少由两部分组成：
第一部分：环境类，接受客户的请求，随后把请求委托给策略类
第二部分：策略类，封装了具体负责计算过程的算法

环境类和策略类之间的映射关系多用来来处理 `if` 、`else if` 的判断逻辑

##### 3. 实现

策略模式可以用于组合一系列算法，也可用于组合一系列业务规则。

假设需要通过成绩等级来计算学生的最终得分，每个成绩等级有对应的加权值。我们可以利用对象字面量的形式直接定义这个组策略

```javascript
// 加权映射关系
var levelMap = {
  S: 10,
  A: 8,
  B: 6,
  C: 4
};

// 组策略
var scoreLevel = {
  basicScore: 80,

  S: function() {
    return this.basicScore + levelMap["S"];
  },

  A: function() {
    return this.basicScore + levelMap["A"];
  },

  B: function() {
    return this.basicScore + levelMap["B"];
  },

  C: function() {
    return this.basicScore + levelMap["C"];
  }
};

// 调用
function getScore(level) {
  return scoreLevel[level] ? scoreLevel[level]() : 0;
}

console.log(getScore("S"), getScore("A"), getScore("B"), getScore("C"), getScore("D")); // 90 88 86 84 0
```

在组合业务规则方面，比较经典的是表单的验证方法。这里列出比较关键的部分

```javascript
// 错误提示
var errorMsgs = {
  default: "输入数据格式不正确",
  minLength: "输入数据长度不足",
  isNumber: "请输入数字",
  required: "内容不为空"
};

// 规则集
var rules = {
  minLength: function(value, length, errorMsg) {
    if (value.length < length) {
      return errorMsg || errorMsgs["minLength"];
    }
  },
  isNumber: function(value, errorMsg) {
    if (!/\d+/.test(value)) {
      return errorMsg || errorMsgs["isNumber"];
    }
  },
  required: function(value, errorMsg) {
    if (value === "") {
      return errorMsg || errorMsgs["required"];
    }
  }
};

// 校验器
function Validator() {
  this.items = [];
}

Validator.prototype = {
  constructor: Validator,

  // 添加校验规则
  add: function(value, rule, errorMsg) {
    var arg = [value];

    if (rule.indexOf("minLength") !== -1) {
      var temp = rule.split(":");
      arg.push(temp[1]);
      rule = temp[0];
    }

    arg.push(errorMsg);

    this.items.push(function() {
      // 进行校验
      return rules[rule].apply(this, arg);
    });
  },

  // 开始校验
  start: function() {
    for (var i = 0; i < this.items.length; ++i) {
      var ret = this.items[i]();

      if (ret) {
        console.log(ret);
        // return ret;
      }
    }
  }
};

// 测试数据
function testTel(val) {
  return val;
}

var validate = new Validator();

validate.add(testTel("ccc"), "isNumber", "只能为数字"); // 只能为数字
validate.add(testTel(""), "required"); // 内容不为空
validate.add(testTel("123"), "minLength:5", "最少5位"); // 最少5位
validate.add(testTel("12345"), "minLength:5", "最少5位");

var ret = validate.start();

console.log(ret);
```
