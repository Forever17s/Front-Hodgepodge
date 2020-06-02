见名知意，`Proxy` 的功能非常类似于设计模式中的代理模式，该模式常用于三个方面：

- 拦截和监视外部对对象的访问。
- 降低函数或类的复杂度。
- 在复杂操作前对操作进行校验或对所需资源进行管理。

在支持 `ES6` 的浏览器环境下，`Proxy` 作为一个全局对象可以直接使用。`Proxy(target, handler)` 是一个构造函数，`target` 是被代理的对象，`handler` 是一个配置对象，提供了拦截方法，即使这个配置对象为空对象，返回的 Proxy 实例也不是原来的目标对象。外界每次访问被代理的对象时，都会经过 `handler` 对象，从这个流程来看，代理对象很类似 `middleware` （中间件）。

```javascript
const person = {
  name: "tom"
};
// 如果第二个参数为空对象
const proxy_A = new Proxy(person, {});
proxy_A === person; // false

// 第二个参数不为空
const proxy_B = new Proxy(person, {
  get(target, prop) {
    console.log(`${prop} is ${target[prop]}`);
    return target[prop];
  }
});
proxy_B.name; // 'name is tom'
```

`Proxy` 支持 `get`、`set`、`apply`、`construct` 等 **13** 种拦截操作，另外还提供了一个 `revoke` 方法，可以随时注销所有的代理操作。在我们正式介绍 `Proxy` 之前，建议你对 `Reflect` 有一定的了解，它也是一个 `ES6` 新增的全局对象，详细信息请参考 [MDN Reflect](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect)。

**一、get(target, prop, receiver)**：拦截对象属性的访问。

**二、set(target, prop, value, receiver)**：拦截对象属性的设置，最后返回一个布尔值。

**三、apply(target, object, args)**：用于拦截函数的调用，比如 `proxy()`。

**四、construct(target, args)**：方法用于拦截 new 操作符，比如 `new proxy()`。为了使 `new` 操作符在生成的 Proxy 对象上生效，用于初始化代理的目标对象自身必须具有 `[[Construct]]` 内部方法（即 `new target` 必须是有效的）。

**五、has(target, prop)**：拦截例如 `prop in proxy` 的操作，返回一个布尔值。

**六、deleteProperty(target, prop)**：拦截例如 `delete proxy[prop]` 的操作，返回一个布尔值。

**七、ownKeys(target)**：拦截 `Object.getOwnPropertyNames(proxy)`、
`Object.keys(proxy)`、`for in 循环`等等操作，最终会返回一个数组。

**八、getOwnPropertyDescriptor(target, prop)**：拦截 Object.- getOwnPropertyDescriptor(proxy, propKey)，返回属性的描述对象。

**九、defineProperty(target, propKey, propDesc)**：拦截 Object.
defineProperty(proxy, propKey, propDesc）、Object.
defineProperties(proxy, propDescs)，返回一个布尔值。

**十、preventExtensions(target)**：拦截 Object.preventExtensions(proxy)，返回一个布尔值。

**十一、getPrototypeOf(target)**：拦截 Object.getPrototypeOf(proxy)，返回一个对象。

**十二、isExtensible(target)**：拦截 Object.isExtensible(proxy)，返回一个布尔值。

**十三、setPrototypeOf(target, proto)**：拦截 Object.setPrototypeOf(proxy, proto)，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。

> 在 `Proxy` 出现之前，JavaScript 中就提供过`Object.defineProperty`，允许对对象的 `getter/setter` 进行拦截。既然有新的技术出现，`Object.defineProperty` 肯定有他的不足。

#### `Object.defineProperty` 的不足

##### 1.无法监听到数组的变化

Vue 官方文档说明可以监听到数组的变动，但限于 `pop`, `shift`, `unshift`, `sort`, `reverse`, `splice`, `push`方法，实际上是他们对这几种方法进行了重写。在定义变量的时候，判断其是否为数组，如果是数组，那么就修改它的 `__proto__`，将其指向 `subArrProto`，从而实现重写原型链。

```javascript
const arrayProto = Array.prototype;
const subArrProto = Object.create(arrayProto);
const methods = [
  "pop",
  "shift",
  "unshift",
  "sort",
  "reverse",
  "splice",
  "push"
];
methods.forEach((method) => {
  /* 重写原型方法 */
  subArrProto[method] = function () {
    arrayProto[method].call(this, ...arguments);
  };
  /* 监听这些方法 */
  Object.defineProperty(subArrProto, method, {
    set() {},
    get() {}
  });
});
```

##### 2.不能监听所有属性

`Object.definePreperty`监听的是对象的属性，当一个对象为深层嵌套时，只能通过递归遍历添加监听。

#### `Proxy` vs `Object.defineProperty`

##### 优点

###### `Proxy` 可以劫持整个对象，这样一来操作便利程度远远优于`Object.defineProperty`。

```javascript
let person = {
  name: "test",
  age: 22
};
// Proxy 监听整个对象
person = new Proxy(person, {
  get(target, key) {
    console.log("数据被读取");
    return Reflect.get(target, key);
  },
  set(target, key, val) {
    console.log("数据被更新");
    return Reflect.set(target, key, val);
  }
});

Object.keys(person).forEach((key) => {
  Object.definePorperty(person, key, {
    get: function () {
      console.log("数据被读取");
      return person[key];
    },
    set: function (newValue) {
      console.log("数据被更新");
      person[key] = newValue;
    }
  });
});
// Proxy 生效，Object.defineProperty 不生效
person.sex = "man";
```

###### `Proxy` 可以直接监听数组的变化，无需进行数组方法重写。

###### `Proxy` 提供了 **13** 种拦截方式。
