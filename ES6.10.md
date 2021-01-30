### ES6.10.对象的扩展

1. 属性的简洁表示法

   ```js
   const foo = 'bar';
   const baz = {foo};
   baz // {foo: "bar"}
   // 等同于
   const baz = {foo: foo};
   
   function f(x, y) {
     return {x, y};
   }
   // 等同于
   function f(x, y) {
     return {x: x, y: y};
   }
   
   f(1, 2) // Object {x: 1, y: 2}
   
   let ms = {};
   
   function getItem (key) {
     return key in ms ? ms[key] : null;
   }
   
   function setItem (key, value) {
     ms[key] = value;
   }
   
   function clear () {
     ms = {};
   }
   
   module.exports = { getItem, setItem, clear };
   // 等同于
   module.exports = {
     getItem: getItem,
     setItem: setItem,
     clear: clear
   };
   ```

   ##### 注意，简写的对象方法不能用作构造函数，会报错。 

   ```js
   const obj = {
     f() {
       this.foo = 'bar';
     }
   };
   
   new obj.f() // 报错
   ```

2. 属性名表达式

   定义对象

   ```js
   // 方法一
   obj.foo = true;
   
   // 方法二
   obj['a' + 'bc'] = 123;
   ```

   定义方法名

   ```js
   let obj = {
     ['h' + 'ello']() {
       return 'hi';
     }
   };
   
   obj.hello() // hi
   ```

   ##### 注意，属性名表达式与简洁表示法，不能同时使用，会报错。 

   ```js
   // 报错
   const foo = 'bar';
   const bar = 'abc';
   const baz = { [foo] };
   
   // 正确
   const foo = 'bar';
   const baz = { [foo]: 'abc'};
   ```

   ##### 注意，属性名表达式如果是一个对象，默认情况下会自动将对象转为字符串`[object Object]` 

   ```js
   const keyA = {a: 1};
   const keyB = {b: 2};
   
   const myObject = {
     [keyA]: 'valueA',
     [keyB]: 'valueB'
   };
   
   myObject // Object {[object Object]: "valueB"}
   ```

3. 方法的name属性

   返回函数名

   ```js
   const person = {
     sayName() {
       console.log('hello!');
     },
   };
   
   person.sayName.name   // "sayName"
   ```

   如果对象的方法使用了取值函数（`getter`）和存值函数（`setter`），则`name`属性不是在该方法上面，而是该方法的属性的描述对象的`get`和`set`属性上面，返回值是方法名前加上`get`和`set`。 

   ```js
   const obj = {
     get foo() {},
     set foo(x) {}
   };
   
   obj.foo.name
   // TypeError: Cannot read property 'name' of undefined
   
   const descriptor = Object.getOwnPropertyDescriptor(obj, 'foo');
   
   descriptor.get.name // "get foo"
   descriptor.set.name // "set foo"
   ```

   ##### 两种特殊情况：

   - `bind`方法创造的函数，`name`属性返回`bound`加上原函数的名字

   - `Function`构造函数创造的函数，`name`属性返回`anonymous` 

     ```js
     (new Function()).name // "anonymous"
     
     var doSomething = function() {
       // ...
     };
     doSomething.bind().name // "bound doSomething"
     ```

     如果对象的方法是一个 Symbol 值，那么`name`属性返回的是这个 Symbol 值的描述。 

     ```js
     const key1 = Symbol('description');
     const key2 = Symbol();
     let obj = {
       [key1]() {},
       [key2]() {},
     };
     obj[key1].name // "[description]"
     obj[key2].name // ""
     ```

4. 属性的可枚举性和遍历

   ##### 可枚举性

   ```js
   let obj = { foo: 123 };
   Object.getOwnPropertyDescriptor(obj, 'foo')
   //  {
   //    value: 123,
   //    writable: true,
   //    enumerable: true,
   //    configurable: true
   //  }
   ```

   描述对象的`enumerable`属性，称为“可枚举性”，如果该属性为`false`，就表示某些操作会忽略当前属性。 

   ###### 有四个操作会忽略`enumerable`为`false`的属性 ：

   - `for...in`循环：只遍历对象自身的和继承的可枚举的属性。
   - `Object.keys()`：返回对象自身的所有可枚举的属性的键名。
   - `JSON.stringify()`：只串行化对象自身的可枚举的属性。
   - `Object.assign()`： 忽略`enumerable`为`false`的属性，只拷贝对象自身的可枚举的属性

   ##### 属性的遍历

   ###### 5种方法：

   - **for...in**

     `for...in`循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。 

   - **Object.keys(obj)** 

     `Object.keys`返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。 

   - **Object.getOwnPropertyNames(obj)** 

     `Object.getOwnPropertyNames`返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。 

   - **Object.getOwnPropertySymbols(obj)** 

     `Object.getOwnPropertySymbols`返回一个数组，包含对象自身的所有 Symbol 属性的键名 。

   - **Reflect.ownKeys(obj)** 

     `Reflect.ownKeys`返回一个数组，包含对象自身的（不含继承的）所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。 

   遵守同样的属性遍历的次序规则 ：

   - [ ] 首先遍历所有数值键，按照数值升序排列。
   - [ ] 其次遍历所有字符串键，按照加入时间升序排列。
   - [ ] 最后遍历所有 Symbol 键，按照加入时间升序排列。

5. super关键字

   指向当前对象的原型对象 

   ```js
   const proto = {
     foo: 'hello'
   };
   
   const obj = {
     foo: 'world',
     find() {
       return super.foo;
     }
   };
   
   Object.setPrototypeOf(obj, proto);
   obj.find() // "hello"
   ```

   ##### 注意，`super`关键字表示原型对象时，只能用在对象的方法之中，用在其他地方都会报错。 

   ```js
   // 报错
   const obj = {
     foo: super.foo
   }
   
   // 报错
   const obj = {
     foo: () => super.foo
   }
   
   // 报错
   const obj = {
     foo: function () {
       return super.foo
     }
   }
   ```

6. 对象的扩展运算符

   ##### 解构赋值

   ```js
   let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
   x // 1
   y // 2
   z // { a: 3, b: 4 }
   ```

   解构赋值必须是最后一个参数，否则会报错。 

   ```js
   let { ...x, y, z } = someObject; // 句法错误
   let { x, ...y, ...z } = someObject; // 句法错误
   
   et { ...z } = null; // 运行时错误
   let { ...z } = undefined; // 运行时错误
   //无法转为对象
   ```

   注意，解构赋值的拷贝是浅拷贝，即如果一个键的值是复合类型的值（数组、对象、函数）、那么解构赋值拷贝的是这个值的引用，而不是这个值的副本。 

   ```js
   let obj = { a: { b: 1 } };
   let { ...x } = obj;
   obj.a.b = 2;
   x.a.b // 2
   ```

   扩展运算符的解构赋值，不能复制继承自原型对象的属性 

   ```js
   let o1 = { a: 1 };
   let o2 = { b: 2 };
   o2.__proto__ = o1;
   let { ...o3 } = o2;
   o3 // { b: 2 }
   o3.a // undefined
   ```

   解构赋值的一个用处，是扩展某个函数的参数，引入其他操作。 

   ```js
   function baseFunction({ a, b }) {
     // ...
   }
   function wrapperFunction({ x, y, ...restConfig }) {
     // 使用 x 和 y 参数进行操作
     // 其余参数传给原始函数
     return baseFunction(restConfig);
   }
   ```

   ##### 扩展运算符

   对象的扩展运算符（`...`）用于取出参数对象的所有可遍历属性，拷贝到当前对象之中。 

   ```js
   let z = { a: 3, b: 4 };
   let n = { ...z };
   n // { a: 3, b: 4 }
   
   //可用于特殊的对象--数组
   let foo = { ...['a', 'b', 'c'] };
   foo
   // {0: "a", 1: "b", 2: "c"}
   
   //如果扩展运算符后面是一个空对象，则没有任何效果
   {...{}, a: 1}
   // { a: 1 }
   
   //如果扩展运算符后面不是对象，则会自动将其转为对象
   // 等同于 {...Object(1)}
   {...1} // {}
   
   //对象的扩展运算符等同于使用Object.assign()方法
   let aClone = { ...a };
   // 等同于
   let aClone = Object.assign({}, a);
   ```

   拷贝对象原型的属性 

   ```js
   // 写法一
   const clone1 = {
     __proto__: Object.getPrototypeOf(obj),
     ...obj
   };
   
   // 写法二
   const clone2 = Object.assign(
     Object.create(Object.getPrototypeOf(obj)),
     obj
   );
   
   // 写法三
   const clone3 = Object.create(
     Object.getPrototypeOf(obj),
     Object.getOwnPropertyDescriptors(obj)
   )
   ```

   扩展运算符可以用于合并两个对象 

   ```js
   let ab = { ...a, ...b };
   // 等同于
   let ab = Object.assign({}, a, b);
   ```

7. 链判断运算符

   链判断运算符有三种用法。

   - `obj?.prop` // 对象属性
   - `obj?.[expr]` // 同上
   - `func?.(...args)` // 函数或对象方法的调用

   ```js
   a?.b
   // 等同于
   a == null ? undefined : a.b
   
   a?.[x]
   // 等同于
   a == null ? undefined : a[x]
   
   a?.b()
   // 等同于
   a == null ? undefined : a.b()
   
   a?.()
   // 等同于
   a == null ? undefined : a()
   ```

   ##### 使用这个运算符，有几个注意点 :

   - 短路机制 

     `?.`运算符相当于一种短路机制，只要不满足条件，就不再往下执行。

     ```
     a?.[++x]
     // 等同于
     a == null ? undefined : a[++x]
     ```

   - delete 运算符 

     ```
     delete a?.b
     // 等同于
     a == null ? undefined : delete a.b
     ```

     如果`a`是`undefined`或`null`，会直接返回`undefined`，而不会进行`delete`运算。 

   - 括号的影响 

     如果属性链有圆括号，链判断运算符对圆括号外部没有影响，只对圆括号内部有影响。

     ```
     (a?.b).c
     // 等价于
     (a == null ? undefined : a.b).c
     ```

     `?.`对圆括号外部没有影响，不管`a`对象是否存在，圆括号后面的`.c`总是会执行。 一般来说，使用`?.`运算符的场合，不应该使用圆括号。 

   - 报错场合 

     ```
     // 构造函数
     new a?.()
     new a?.b()
     
     // 链判断运算符的右侧有模板字符串
     a?.`{b}`
     a?.b`{c}`
     
     // 链判断运算符的左侧是 super
     super?.()
     super?.foo
     
     // 链运算符用于赋值运算符左侧
     a?.b = c
     ```

   - 右侧不得为十进制数值 

     为了保证兼容以前的代码，允许`foo?.3:0`被解析成`foo ? .3 : 0`， 也就是说，那个小数点会归属于后面的十进制数字，形成一个小数 

8. Null判断运算符

   引入了一个新的 Null 判断运算符`??`。它的行为类似`||`，但是只有运算符左侧的值为`null`或`undefined`时，才会返回右侧的值。 

   ```js
   const headerText = response.settings.headerText ?? 'Hello, world!';
   const animationDuration = response.settings.animationDuration ?? 300;
   const showSplashScreen = response.settings.showSplashScreen ?? true
   ```

   这个运算符的一个目的，就是跟链判断运算符`?.`配合使用，为`null`或`undefined`的值设置默认值。 

   ##### 如果多个逻辑运算符一起使用，必须用括号表明优先级，否则会报错。 

   ```js
   // 报错
   lhs && middle ?? rhs
   lhs ?? middle && rhs
   lhs || middle ?? rhs
   lhs ?? middle || rhs
   ```

   

