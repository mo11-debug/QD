### ES6.11对象的扩展方法

1. Object.is()

   同值相等算法，严格比较两个值是否相等。

   ```js
   // true
   Object.is({}, {})
   // false
   ```

   **不同之处**只有两个：一是`+0`不等于`-0`，二是`NaN`等于自身。

   ```
   +0 === -0 //true
   NaN === NaN // false
   
   Object.is(+0, -0) // false
   Object.is(NaN, NaN) // true
   ```

   ES5部署：

   ```
   Object.defineProperty(Object, 'is', {
     value: function(x, y) {
       if (x === y) {
         // 针对+0 不等于 -0的情况
         return x !== 0 || 1 / x === 1 / y;
       }
       // 针对NaN的情况
       return x !== x && y !== y;
     },
     configurable: true,
     enumerable: false,
     writable: true
   });
   ```

2. Object.assign()

   用于对象的合并，将源对象的所有可枚举属性，复制到目标对象

   ```js
   const target = { a: 1 };
   
   const source1 = { b: 2 };
   const source2 = { c: 3 };
   
   Object.assign(target, source1, source2);
   target // {a:1, b:2, c:3}
   ```

   注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

   ```
   const target = { a: 1, b: 1 };
   
   const source1 = { b: 2, c: 2 };
   const source2 = { c: 3 };
   
   Object.assign(target, source1, source2);
   target // {a:1, b:2, c:3}
   ```

   如果只有一个参数，`Object.assign()`会直接返回该参数。

   ```
   const obj = {a: 1};
   Object.assign(obj) === obj // true
   ```

   如果该参数不是对象，则会先转成对象，然后返回。

   ```
   typeof Object.assign(2) // "object"
   ```

   由于`undefined`和`null`无法转成对象，所以如果它们作为参数，就会报错。

   ```
   Object.assign(undefined) // 报错
   Object.assign(null) // 报错
   ```

   `Object.assign()`拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（`enumerable: false`）。

   ```
   Object.assign({b: 'c'},
     Object.defineProperty({}, 'invisible', {
       enumerable: false,
       value: 'hello'
     })
   )
   // { b: 'c' }
   ```

   如果`undefined`和`null`不在首参数，就不会报错。 其他类型的值（即数值、字符串和布尔值）不在首参数，也不会报错。但是，除了字符串会以数组形式，拷贝入目标对象，其他值都不会产生效果。 

   属性名为 Symbol 值的属性，也会被`Object.assign()`拷贝。

   ```
   Object.assign({ a: 'b' }, { [Symbol('c')]: 'd' })
   // { a: 'b', Symbol(c): 'd' }
   ```

   ##### 注意点

   **（1）浅拷贝** 

   ```
   const obj1 = {a: {b: 1}};
   const obj2 = Object.assign({}, obj1);
   
   obj1.a.b = 2;
   obj2.a.b // 2
   ```

   **（2）同名属性的替换** 

   ```
   const target = { a: { b: 'c', d: 'e' } }
   const source = { a: { b: 'hello' } }
   Object.assign(target, source)
   // { a: { b: 'hello' } }
   ```

   **（3）数组的处理**

   `Object.assign()`可以用来处理数组，但是会把数组视为对象。

   ```
   Object.assign([1, 2, 3], [4, 5])
   // [4, 5, 3]
   ```

   **（4）取值函数的处理**

   `Object.assign()`只能进行值的复制，如果要复制的值是一个取值函数，那么将求值后再复制。

   ```
   const source = {
     get foo() { return 1 }
   };
   const target = {};
   
   Object.assign(target, source)
   // { foo: 1 }
   ```

   ### 常见用途

   **（1）为对象添加属性**

   ```
   class Point {
     constructor(x, y) {
       Object.assign(this, {x, y});
     }
   }
   ```

   上面方法通过`Object.assign()`方法，将`x`属性和`y`属性添加到`Point`类的对象实例。

   **（2）为对象添加方法**

   ```
   Object.assign(SomeClass.prototype, {
     someMethod(arg1, arg2) {
       ···
     },
     anotherMethod() {
       ···
     }
   });
   
   // 等同于下面的写法
   SomeClass.prototype.someMethod = function (arg1, arg2) {
     ···
   };
   SomeClass.prototype.anotherMethod = function () {
     ···
   };
   ```

   上面代码使用了对象属性的简洁表示法，直接将两个函数放在大括号中，再使用`assign()`方法添加到`SomeClass.prototype`之中。

   **（3）克隆对象**

   ```
   function clone(origin) {
     return Object.assign({}, origin);
   }
   ```

   不过，采用这种方法克隆，只能克隆原始对象自身的值，不能克隆它继承的值。如果想要保持继承链，可以采用下面的代码。

   ```
   function clone(origin) {
     let originProto = Object.getPrototypeOf(origin);
     return Object.assign(Object.create(originProto), origin);
   }
   ```

   **（4）合并多个对象**

   将多个对象合并到某个对象。

   ```
   const merge =
     (target, ...sources) => Object.assign(target, ...sources);
   ```

   如果希望合并后返回一个新对象，可以改写上面函数，对一个空对象合并。

   ```
   const merge =
     (...sources) => Object.assign({}, ...sources);
   ```

   **（5）为属性指定默认值**

   ```
   const DEFAULTS = {
     logLevel: 0,
     outputFormat: 'html'
   };
   
   function processContent(options) {
     options = Object.assign({}, DEFAULTS, options);
     console.log(options);
     // ...
   }
   ```

   **注意，由于存在浅拷贝的问题，`DEFAULTS`对象和`options`对象的所有属性的值，最好都是简单类型，不要指向另一个对象。否则，`DEFAULTS`对象的该属性很可能不起作用。**

   ```
   const DEFAULTS = {
     url: {
       host: 'example.com',
       port: 7070
     },
   };
   
   processContent({ url: {port: 8000} })
   // {
   //   url: {port: 8000}
   // }
   ```

3. Object.getOwnPropertyDescriptors()

   ```
   const obj = {
     foo: 123,
     get bar() { return 'abc' }
   };
   
   Object.getOwnPropertyDescriptors(obj)
   // { foo:
   //    { value: 123,
   //      writable: true,
   //      enumerable: true,
   //      configurable: true },
   //   bar:
   //    { get: [Function: get bar],
   //      set: undefined,
   //      enumerable: true,
   //      configurable: true } }
   ```

   `Object.getOwnPropertyDescriptors()`方法返回一个对象，所有原对象的属性名都是该对象的属性名，对应的属性值就是该属性的描述对象。

   ```
   function getOwnPropertyDescriptors(obj) {
     const result = {};
     for (let key of Reflect.ownKeys(obj)) {
       result[key] = Object.getOwnPropertyDescriptor(obj, key);
     }
     return result;
   }
   ```

   该方法的引入目的，主要是为了解决`Object.assign()`无法正确拷贝`get`属性和`set`属性的问题。

   ```
   const source = {
     set foo(value) {
       console.log(value);
     }
   };
   
   const target1 = {};
   Object.assign(target1, source);
   
   Object.getOwnPropertyDescriptor(target1, 'foo')
   // { value: undefined,
   //   writable: true,
   //   enumerable: true,
   //   configurable: true }
   ```

   `source`对象的`foo`属性的值是一个赋值函数，`Object.assign`方法将这个属性拷贝给`target1`对象，结果该属性的值变成了`undefined` 。

   `Object.getOwnPropertyDescriptors()`方法的另一个用处，是配合`Object.create()`方法，将对象属性克隆到一个新对象。这属于浅拷贝。

   ```
   const clone = Object.create(Object.getPrototypeOf(obj),
     Object.getOwnPropertyDescriptors(obj));
   
   // 或者
   
   const shallowClone = (obj) => Object.create(
     Object.getPrototypeOf(obj),
     Object.getOwnPropertyDescriptors(obj)
   );
   ```

   ES6 规定`__proto__`只有浏览器要部署，其他环境不用部署。如果去除`__proto__`，上面代码就要改成下面这样。

   ```
   const obj = Object.create(prot);
   obj.foo = 123;
   
   // 或者
   
   const obj = Object.assign(
     Object.create(prot),
     {
       foo: 123,
     }
   );
   ```

   有了`Object.getOwnPropertyDescriptors()`，我们就有了另一种写法。

   ```
   const obj = Object.create(
     prot,
     Object.getOwnPropertyDescriptors({
       foo: 123,
     })
   );
   ```

   `Object.getOwnPropertyDescriptors()`也可以用来实现 Mixin（混入）模式。

   ```
   let mix = (object) => ({
     with: (...mixins) => mixins.reduce(
       (c, mixin) => Object.create(
         c, Object.getOwnPropertyDescriptors(mixin)
       ), object)
   });
   
   // multiple mixins example
   let a = {a: 'a'};
   let b = {b: 'b'};
   let c = {c: 'c'};
   let d = mix(c).with(a, b);
   
   d.c // "c"
   d.b // "b"
   d.a // "a"
   ```

4. __proto__属性，Object.values(),Object.entries()

   ### __proto__属性

   `__proto__`属性（前后各两个下划线），用来读取或设置当前对象的原型对象（prototype）。目前，所有浏览器（包括 IE11）都部署了这个属性。

   ```
   // es5 的写法
   const obj = {
     method: function() { ... }
   };
   obj.__proto__ = someOtherObj;
   
   // es6 的写法
   var obj = Object.create(someOtherObj);
   obj.method = function() { ... };
   ```

   实现上，`__proto__`调用的是`Object.prototype.__proto__`，具体实现如下。

   ```
   Object.defineProperty(Object.prototype, '__proto__', {
     get() {
       let _thisObj = Object(this);
       return Object.getPrototypeOf(_thisObj);
     },
     set(proto) {
       if (this === undefined || this === null) {
         throw new TypeError();
       }
       if (!isObject(this)) {
         return undefined;
       }
       if (!isObject(proto)) {
         return undefined;
       }
       let status = Reflect.setPrototypeOf(this, proto);
       if (!status) {
         throw new TypeError();
       }
     },
   });
   
   function isObject(value) {
     return Object(value) === value;
   }
   ```

   如果一个对象本身部署了`__proto__`属性，该属性的值就是对象的原型。

   ```
   Object.getPrototypeOf({ __proto__: null })
   // null
   ```

   ### Object.setPrototypeOf()

   `Object.setPrototypeOf`方法的作用与`__proto__`相同，用来设置一个对象的原型对象（prototype），返回参数对象本身。它是 ES6 正式推荐的设置原型对象的方法。

   ```
   // 格式
   Object.setPrototypeOf(object, prototype)
   
   // 用法
   const o = Object.setPrototypeOf({}, null);
   ```

   该方法等同于下面的函数。

   ```
   function setPrototypeOf(obj, proto) {
     obj.__proto__ = proto;
     return obj;
   }
   ```

   下面是一个例子。

   ```
   let proto = {};
   let obj = { x: 10 };
   Object.setPrototypeOf(obj, proto);
   
   proto.y = 20;
   proto.z = 40;
   
   obj.x // 10
   obj.y // 20
   obj.z // 40
   ```

   

5. Object.keys(),Object.values(),Object.entries()

   ### Object.keys()

   ES5 引入了`Object.keys`方法，返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名。

   ```
   var obj = { foo: 'bar', baz: 42 };
   Object.keys(obj)
   ```

   ### Object.values()

   `Object.values`方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值。

   ```
   const obj = { foo: 'bar', baz: 42 };
   Object.values(obj)
   // ["bar", 42]
   ```

   `Object.values`只返回对象自身的可遍历属性。

   ```
   const obj = Object.create({}, {p: {value: 42}});
   Object.values(obj) // []
   ```

   ### Object.entries()

   `Object.entries()`方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。

   ```
   const obj = { foo: 'bar', baz: 42 };
   Object.entries(obj)
   // [ ["foo", "bar"], ["baz", 42] ]
   ```

6. Object.fromEntries()

   `Object.fromEntries()`方法是`Object.entries()`的逆操作，用于将一个键值对数组转为对象。

   ```
   Object.fromEntries([
     ['foo', 'bar'],
     ['baz', 42]
   ])
   // { foo: "bar", baz: 42 }
   ```

主要目的，是将键值对的数据结构还原为对象，因此特别适合将 Map 结构转为对象。

```
// 例一
const entries = new Map([
  ['foo', 'bar'],
  ['baz', 42]
]);

Object.fromEntries(entries)
// { foo: "bar", baz: 42 }

// 例二
const map = new Map().set('foo', true).set('bar', false);
Object.fromEntries(map)
// { foo: true, bar: false }
```

一个用处是配合`URLSearchParams`对象，将查询字符串转为对象。

```
Object.fromEntries(new URLSearchParams('foo=bar&baz=qux'))
// { foo: "bar", baz: "qux" }
```