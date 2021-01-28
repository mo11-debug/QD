### ES6.8函数的扩展

1. 函数参数的默认值

   ###### 基本使用

   ```js
   //为函数的参数设置默认值，直接写在参数定义后面
   function log(x, y = 'World') {
     console.log(x, y);
   }
   log('Hello') // Hello World
   log('Hello', 'China') // Hello China
   log('Hello', '') // Hello
   
   //参数变量默认声明，不能用let，const再次声明
   function foo(x = 5) {
     let x = 1; // error
     const x = 2; // error
   }
   
   //使用参数默认值时函数不能有同名参数
   // 不报错
   function foo(x, x, y) {
     // ...
   }
   
   // 报错
   function foo(x, x, y = 1) {
     // ...
   }
   
   //参数默认值惰性求值，每次都重新计算默认表达式的值
   let x = 99;
   function foo(p = x + 1) {
     console.log(p);
   }
   
   foo() // 100
   
   x = 100;
   foo() // 101
   ```

   ###### 与解构赋值默认值结合使用

   ```js
   function foo({x, y = 5}) {
     console.log(x, y);
   }
   
   foo({}) // undefined 5
   foo({x: 1}) // 1 5
   foo({x: 1, y: 2}) // 1 2
   foo() // TypeError: Cannot read property 'x' of undefined
   //不提供参数会默认为空对象
   
   //函数参数设定默认值
   // 写法一，函数参数的默认值是空对象，但是设置了对象解构赋值的默认值
   function m1({x = 0, y = 0} = {}) {
     return [x, y];
   }
   
   // 写法二，函数参数的默认值是一个有具体属性的对象，但是没有设置对象解构赋值的默认值
   function m2({x, y} = { x: 0, y: 0 }) {
     return [x, y];
   }
   
   // 函数没有参数的情况
   m1() // [0, 0]
   m2() // [0, 0]
   
   // x 和 y 都有值的情况
   m1({x: 3, y: 8}) // [3, 8]
   m2({x: 3, y: 8}) // [3, 8]
   
   // x 有值，y 无值的情况
   m1({x: 3}) // [3, 0]
   m2({x: 3}) // [3, undefined]
   
   // x 和 y 都无值的情况
   m1({}) // [0, 0];
   m2({}) // [undefined, undefined]
   
   m1({z: 3}) // [0, 0]
   m2({z: 3}) // [undefined, undefined]
   ```

   ###### 参数默认值的位置

   ```js
   // 例一
   function f(x = 1, y) {
     return [x, y];
   }
   
   f() // [1, undefined]
   f(2) // [2, undefined]
   f(, 1) // 报错
   f(undefined, 1) // [1, 1]
   
   // 例二
   function f(x, y = 5, z) {
     return [x, y, z];
   }
   
   f() // [undefined, 5, undefined]
   f(1) // [1, 5, undefined]
   f(1, ,2) // 报错
   f(1, undefined, 2) // [1, 5, 2]
   ```

   ###### 函数的length属性

   ```js
   //失真
   (function (a) {}).length // 1
   (function (a = 5) {}).length // 0
   (function (a, b, c = 5) {}).length // 2
   //length属性的返回值，等于函数的参数个数减去指定了默认值的参数个数。
   ```

   ###### 作用域

   ```js
   //单独作用域
   var x = 1;
   function f(x, y = x) {
     console.log(y);
   }
   f(2) // 2
   
   //函数foo的参数形成一个单独作用域
   var x = 1;
   function foo(x, y = function() { x = 2; }) {
     x = 3;
     y();
     console.log(x);
   }
   foo() // 2
   x // 1
   ```

   ###### 应用

   ```js
   function throwIfMissing() {
     throw new Error('Missing parameter');
   }
   
   function foo(mustBeProvided = throwIfMissing()) {
     return mustBeProvided;
   }
   
   foo()
   // Error: Missing parameter
   
   //将参数默认值设为undefined，表明这个参数是可以省略的。
   function foo(optional = undefined) { ··· }
   ```

2. rest参数

   搭配的变量是**数组**

   ```js
   //利用 rest 参数，可以向该函数传入任意数目的参数。
   function add(...values) {
     let sum = 0;
   
     for (var val of values) {
       sum += val;
     }
   
     return sum;
   }
   
   add(2, 5, 3) // 10
   
   // arguments变量的写法
   function sortNumbers() {
     return Array.prototype.slice.call(arguments).sort();
   }
   
   // rest参数的写法
   const sortNumbers = (...numbers) => numbers.sort();
   
   //rest是真正的数组，所有方法都可以用
   function push(array, ...items) {
     items.forEach(function(item) {
       array.push(item);
       console.log(item);
     });
   }
   
   var a = [];
   push(a, 1, 2, 3)
   
   // 报错
   function f(a, ...b, c) {
     // ...
   }//rest参数之后不能再有其他参数
   ```

3. 严格模式

   ```js
   // 报错
   function doSomething(a, b = a) {
     'use strict';
     // code
   }
   
   // 报错
   const doSomething = function ({a, b}) {
     'use strict';
     // code
   };
   
   // 报错
   const doSomething = (...a) => {
     'use strict';
     // code
   };
   
   const obj = {
     // 报错
     doSomething({a, b}) {
       'use strict';
       // code
     }
   };
   ```

   **只要函数参数使用了默认值、解构赋值、或者扩展运算符，函数内部都不能显式设定为严格模式** 

   ```js
   //规避方法
   'use strict';
   function doSomething(a, b = a) {
     // code
   }//设定全局性的严格模式
   
   const doSomething = (function () {
     'use strict';
     return function(value = 42) {
       return value;
     };
   }());//把函数包在一个无参数的立即执行函数里面
   ```

4. name属性

   返回该函数的函数名 

   ```js
   (new Function).name // "anonymous"
   function foo() {};
   foo.bind({}).name // "bound foo"
   (function(){}).bind({}).name // "bound "
   ```

   `Function`构造函数返回的函数实例，`name`属性的值为`anonymous`。 

   `bind`返回的函数，`name`属性值会加上`bound`前缀。 

5. 箭头函数

   ```js
   var f = v => v;
   // 等同于
   var f = function (v) {
     return v;
   };
   
   //箭头函数可以与变量解构结合使用
   const full = ({ first, last }) => first + ' ' + last;
   // 等同于
   function full(person) {
     return person.first + ' ' + person.last;
   }
   
   //简化回调函数
   // 正常函数写法
   [1,2,3].map(function (x) {
     return x * x;
   });
   // 箭头函数写法
   [1,2,3].map(x => x * x);
   
   //rest 参数与箭头函数结合
   const numbers = (...nums) => nums;
   numbers(1, 2, 3, 4, 5)
   // [1,2,3,4,5]
   
   const headAndTail = (head, ...tail) => [head, tail];
   headAndTail(1, 2, 3, 4, 5)
   // [1,[2,3,4,5]]
   ```

   ##### 使用注意点

   - 函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。

   - 不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。

   - 不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

   - 不可以使用`yield`命令，因此箭头函数不能用作 Generator 函数。

     ```js
     //箭头函数可以让setTimeout里面的this，绑定定义时所在的作用域，而不是指向运行时所在的作用域。
     function foo() {
       setTimeout(() => {
         console.log('id:', this.id);
       }, 100);
     }
     
     var id = 21;
     
     foo.call({ id: 42 });
     // id: 42
     
     //箭头函数可以让this指向固定化
     function Timer() {
       this.s1 = 0;
       this.s2 = 0;
       // 箭头函数
       setInterval(() => this.s1++, 1000);
       // 普通函数
       setInterval(function () {
         this.s2++;
       }, 1000);
     }
     
     var timer = new Timer();
     
     setTimeout(() => console.log('s1: ', timer.s1), 3100);
     setTimeout(() => console.log('s2: ', timer.s2), 3100);
     // s1: 3
     // s2: 0
     ```

     ###### 不适用场合

     ```js
     //定义对象
     const cat = {
       lives: 9,
       jumps: () => {
         this.lives--;
       }
     }
     
     //需要动态this的时候
     var button = document.getElementById('press');
     button.addEventListener('click', () => {
       this.classList.toggle('on');
     });
     ```

     嵌套的箭头函数

     ```js
     let insert = (value) => ({into: (array) => ({after: (afterValue) => {
       array.splice(array.indexOf(afterValue) + 1, 0, value);
       return array;
     }})});
     
     insert(2).into([1, 3]).after(1); //[1, 2, 3]
     ```

6. 尾调用优化

   指某个函数的最后一步是调用另一个函数 

   ```js
   //不属于尾调用
   // 情况一
   function f(x){
     let y = g(x);
     return y;
   }
   
   // 情况二
   function f(x){
     return g(x) + 1;
   }
   
   // 情况三
   function f(x){
     g(x);
   }
   ```

   尾调用不一定出现在函数尾部，只要是最后一步操作即可。 

   尾调用优化：目前只有 Safari 浏览器支持尾调用优化，Chrome 和 Firefox 都不支持。 

   ###### 尾递归：尾调用自身 

   ```js
   //阶乘函数
   function factorial(n) {
     if (n === 1) return 1;
     return n * factorial(n - 1);
   }
   
   factorial(5) // 120
   
   //尾递归
   function factorial(n, total) {
     if (n === 1) return total;
     return factorial(n - 1, n * total);
   }
   
   factorial(5, 1) // 120
   ```

   ###### 严格模式

   ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。

   这是因为在正常模式下，函数内部有两个变量，可以跟踪函数的调用栈。

   - `func.arguments`：返回调用时函数的参数。
   - `func.caller`：返回调用当前函数的那个函数。

   ###### 尾递归优化的实现

   减少调用栈

   蹦床函数？可以将递归执行转为循环执行 

   ```js
   function trampoline(f) {
     while (f && f instanceof Function) {
       f = f();
     }
     return f;
   }
   ```

   真正的尾递归优化 

   ```js
   function tco(f) {
     var value;
     var active = false;
     var accumulated = [];
   
     return function accumulator() {
       accumulated.push(arguments);
       if (!active) {
         active = true;
         while (accumulated.length) {
           value = f.apply(this, accumulated.shift());
         }
         active = false;
         return value;
       }
     };
   }
   
   var sum = tco(function(x, y) {
     if (y > 0) {
       return sum(x + 1, y - 1)
     }
     else {
       return x
     }
   });
   
   sum(1, 100000)
   // 100001
   ```

   过程不太清楚，再看一遍？

7. 函数参数的尾逗号

   ```js
   function clownsEverywhere(
     param1,
     param2
   ) { /* ... */ }
   
   clownsEverywhere(
     'foo',
     'bar'
   );
   
   //允许参数后面出现逗号
   function clownsEverywhere(
     param1,
     param2,
   ) { /* ... */ }
   
   clownsEverywhere(
     'foo',
     'bar',
   );
   ```

   

8. Function prototype.toString

   ```js
   //以前忽略注释和空格
   function /* foo comment */ foo () {}
   foo.toString()
   // function foo() {}
   
   //现在明确返回一模一样代码
   function /* foo comment */ foo () {}
   foo.toString()
   // "function /* foo comment */ foo () {}"
   ```

9. catch命令的参数忽略

   ```js
   //以前必须有参数err
   try {
     // ...
   } catch (err) {
     // 处理错误
   }
   
   //现在允许忽略参数
   try {
     // ...
   } catch {
     // ...
   }
   ```

   