> 在开始讲解装饰器之前，先从经典的 [装饰器模式](./../design/设计原则-丁.md) 说起。装饰器模式是一种结构型设计模式，它允许向一个现有的对象添加新的功能，同时又不改变其结构，是作为对现有类的一个包装。在 ES7 的最新迭代版本中，提出了关于 [JavaScript 迭代器](https://github.com/wycats/javascript-decorators) 的提议，允许我们在设计时诠释和修改 class 和属性。

### 装饰器设计模式

JavaScript 的装饰器可以看作是一个函数，这个函数将目标函数的 `target`、`name` 以及 `descriptor` 作为它的参数。

- `target` 是指目标构造函数。例如，如果装饰器在 class 的构造函数中，target 指向 class 的构造函数。如果装饰器在 class 的方法中，target 指向这个方法。

- `name` 就是指向方法的 method。class 构造函数的装饰器没有 name。

- `descriptor` 描述的是数据和访问器。

JavaScript 的装饰器依赖于 `Object.defineProperty`，一般是用来装饰类、类属性、类方法。使用装饰器可以做到不直接修改代码，就实现某些功能，做到真正的面向切面编程。这在一定程度上和 `Proxy` 很相似，但使用起来比 `Proxy` 会更加简洁。

#### 类装饰器

修改类本身，或者通过修改原型，给实例增加新属性。下面是给目标类增加 speak 方法的例子：

```javascript
const withSpeak = (targetClass) => {
  const prototype = targetClass.prototype;
  prototype.speak = function () {
    console.log(`i can speak ${this.language}.`);
  };
};

@withSpeak
class Student {
  constructor(language) {
    this.language = language;
  }
}

const studentA = new Student("Chinese");
const studentB = new Student("English");
studentA.speak(); // I can speak  Chinese.
studentB.speak(); // I can speak  English.
```

利用高阶函数的属性，还可以给装饰器传参，通过参数来判断对类进行什么处理。

```javascript
const withLanguage = (language) => (targetClass) => {
  targetClass.prototype.language = language;
};

@withLanguage("Chinese")
class Student {}

const studentC = new Student();
student.language; // 'Chinese'
```

如果你经常编写 `react-redux` 的代码，那么也会遇到需要将 `store` 中的数据映射到组件中的情况。connect 是一个高阶组件，它接收了两个函数 `mapStateToProps` 和 `mapDispatchToProps` 以及一个组件 App，最终返回了一个增强版的组件。

```javascript
class App extends React.Component {}
connect(mapStateToProps, mapDispatchToProps)(App);

// 有了装饰器之后，connect 的写法可以变得更加优雅。
@connect(mapStateToProps, mapDispatchToProps)
class App extends React.Component {}
```
