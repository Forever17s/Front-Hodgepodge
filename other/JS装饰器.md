> 在开始讲解装饰器之前，先从经典的 [装饰器模式](./../design/设计原则-丁.md) 说起。装饰器模式是一种结构型设计模式，它允许向一个现有的对象添加新的功能，同时又不改变其结构，是作为对现有类的一个包装。在 ES7 的最新迭代版本中，提出了关于 [JavaScript 迭代器](https://github.com/wycats/javascript-decorators) 的提议，允许我们在设计时诠释和修改 class 和属性。

### 装饰器设计模式

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

#### 类属性装饰器

类属性装饰器可以用在`类的属性`、`方法`、`get/set 函数`中，一般会接收三个参数：

- `target` 是指目标构造函数。例如，如果装饰器在 class 的构造函数中，target 指向 class 的构造函数。如果装饰器在 class 的方法中，target 指向这个方法。

- `name` 就是指向方法的 method。class 构造函数的装饰器没有 name。

- `descriptor` 描述的是数据和访问器。

###### 举个:chestnut:：只读的装饰器

```javascript
function readonly(target, name, description) {
  description.writable = false;
  return description;
}

class Person {
  @readonly name = "person";
}
const person = new Person();
person.name = "tom";
```

> 在这个例子中，装饰器使得方法只读，这意味着我们不能修改这个方法。我们要做的就是设置 `writable` 描述符属性为 `false`。

###### 再举个:chestnut:：时间日志装饰器

```javascript
function time(target, name, description) {
  const fn = description.value;

  const decoratedFn = function () {
    console.time(name);
    const result = fn.apply(target, arguments);
    console.timeEnd(name);
    return result;
  };

  description.value = decoratedFn;

  return description;
}

class Test {
  @time
  readNumber() {
    let number = 7e9;
    while (number--) {}

    return "test over.";
  }
}

const test = new Test();
console.log(test.readNumber());
// readNumber: 7021ms
// test over.
```

#### 混合装饰器

利用混合装饰器，我们可以为 class 添加或混合更多的行为。这里创建了一个新的 Mixin 类，来将 mixins 和 targetClass 上面的所有属性都拷贝过去。

```javascript
function mixin(...mixins) {
  return (target, name, descriptor) => {
    mixins.forEach((mix) => {
      for (const key in mix) {
        const desc = Object.getOwnPropertyDescriptor(min, key);

        Object.defineProperty(target.prototype, key, desc);
      }
    });

    return description;
  };
}

const ThinkMixin = {
  think() {
    return "i can think.";
  }
};

const RunMixin = {
  run() {
    return "i can run.";
  }
};

@mixin(ThinkMixin, RunMixin)
class Person {
  eat() {
    return "i can eat.";
  }
}

const person = new Person();
console.log(person.eat()); // i can eat.
console.log(person.think()); // i can think.
console.log(person.run()); // i can run.
```

#### 常用场景

##### 1.防抖节流

以往我们在频繁触发的场景下，为了优化性能，经常会使用到节流函数。下面以 React 组件绑定滚动事件为例子：

```javascript
import throttle from "lodash/throttle";

class App extends React.Component {
  constructor(props) {
    super(props);
    this.handleScroll = throttle(this.scroll, 500);
  }

  componentDidMount() {
    window.addEveneListener("scroll", this.handleScroll);
  }
  componentWillUnmount() {
    window.removeEveneListener("scroll", this.handleScroll);
  }
  scroll() {}
}
```

在组件中绑定事件需要注意应当在组件销毁的时候进行解绑。而由于节流函数返回了一个新的匿名函数，所以为了之后能够有效解绑，不得不将这个匿名函数存起来，以便于之后使用。但是在有了装饰器之后，我们就不必在每个绑定事件的地方都手动设置 `throttle` 方法，只需要在 `scroll` 函数添加一个 `throttle` 的装饰器就行了。

```javascript
// untils/decorators.js
export const Deounce = (wait = 500) => {
  let timer;
  return (target, name, description) => {
    const fn = description.value;
    if (typeof fn === "function") {
      description.value = function (...args) {
        if (timer) clearTimeout(timer);
        timer = setTimeout(() => {
          fn.apply(this, args);
        }, wait);
      };
    }
  };
};

export const Throttle = (wait = 500) => {
  let prev = new Date();
  return (target, name, description) => {
    const fn = description.value;
    if (typeof fn === "function") {
      description.value = function (...args) {
        const now = new Date();
        if (now - prev > wait) {
          fn.apply(this, args);
          prev = now;
        }
      };
    }
  };
};
```

```javascript
// 引入定义好的防抖、节流
import { Deounce, Throttle } from "untils/decorators.js";

class App extends React.Component {
  constructor(props) {
    super(props);
  }

  componentDidMount() {
    window.addEventListener("resize", this.resize);
    window.addEveneListener("scroll", this.scroll);
  }

  componentWillUnmount() {
    window.removeEventListener("resize", this.resize);
    window.removeEventListener("scroll", this.scroll);
  }

  @Deounce(400)
  resize() {}

  @Throttle(400)
  scroll() {}
}
```

##### 2.表单校验

通过类属性装饰器来对类的属性进行类型的校验。

```javascript
const validate = (type) => (target, name, description) => {
  if (typeof target[name] !== type) {
    throw new Error(`attribute ${name} must be ${type} type`);
  }
};

class Form {
  @validate("string")
  name = 233; // Error: attribute name must be ${type} type
}
```

还可以升级一下，通过编写校验规则，来对整个类进行校验。

```javascript
const validator = (rules) => (targetClass) => {
  return new Proxy(targetClass, {
    construct(target, args) {
      const obj = new target(...args);
      for (let [name, type] of Object.entries(rules)) {
        if (typeof obj[name] !== type) {
          throw new Error(`attribute ${name} must be ${type} type`);
        }
      }
      return obj;
    }
  });
};

const rules = {
  name: "string",
  password: "string",
  age: "number",
  sex: "boolean"
};

@validator(rules)
class Person {
  name = "John";
  password = "123";
  age = "12";
  sex = "male";
}

const person = new Person();
```
