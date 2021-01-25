### ES6.2   let和const命令

#### 1.let命令

##### let和var区别：

二者均可以声明变量，但是let只在它所在的代码块有效，var在全局范围都有效。var命令存在“变量提升”，let命令不存在，会按照顺序执行程序。

```js
{
    let a = 10;
    var b = 1;
}
a//报错
b//1
```

##### for循环计数器：适合let命令

```js
var a = [];
for(var i = 0; i < 10; i++){
    a[i] = function(){
        console.log(i);
    };
}
a[6]();//var命令输出10

var a = [];
for(let i = 0; i < 10; i++){
    a[i] = function(){
        console.log(i);
    };
}
a[6]();//let命令输出6
```

因为var中的i指向全局的i,所有数组中的a都指向的是同一个i。而let中的i每一次循环的i都是一个新的变量，都是重新声明的。

```js
for(let i = 0; i < 3; i++){
    let i = 'abc';
    console.log(i);
}
//输出abc,abc,abc
```

表明函数内部的变量i和循环变量i不在同一个作用域，都有各自的作用域。

##### 变量提升

```js
console.log(foo);//输出undefined
var foo = 2;

console.log(bar);//声明之前变量bar不存在
let bar = 2;//会报错
```

##### 暂时性死区(TDZ)

只要块状作用域存在let，声明变量就会被绑定，不再受外界影响。

```js
var tmp = 123;

if(true){
    tmp = 'abc';
    let tmp;//报错，该块状绑定的局部变量与var定义的全局变量冲突,导致变量不可用。
}

if(true){
    tmp = 'abc';//TDZ开始
    console.log(tmp);//ReferenceError
    
    let tmp;//TDZ结束
    console.log(tmp);//undefined
    
    tmp = 123;
    console.log(tmp);//输出123
}

typeof x;//ReferenceError
let x;//用到该变量就报错

typeof undeclared_variable//"undefined"，不声明反而不报错

function bar(x = y, y = 2){
    return [x,y];
}
bar();//报错

function bar(y = 2, x = y){
    return [x,y];
}
bar();//[2, 2]
```

另一种报错：未声明就去取值，导致报错“未定义”

```js
var x = x;//不报错

let x = x;//报错，x没有定义
```

暂时性死区本质：只要一进入作用域变量已经存在但是不可获取，只有声明变量时，才可以获取和使用该变量。

##### 不允许重复声明(let不允许在相同作用域内重复声明同一变量)

```js
//报错
function func(){
    let a = 10;
    var a = 1;
}

function func(arg){
    let arg;
}
func()//报错

function func(arg){
    {
        let arg;
    }
}
func()//不报错
```

不能在函数内部重新声明参数

#### 2.块级作用域

为什么需要块级作用域？

```js
//1.内层变量可能覆盖外层变量
var tmp = new Date();

function f(){
    console.log(tmp);
    if(false){
        var tmp = 'hello world';
    }
}

f();//undefined
//因为变量提升导致内层的tmp变量覆盖if代码块外层tmp变量

//2.用来计数的循环变量泄露为全局变量
var s = 'hello';

for(var i = 0; i < s.length; i++){
    console.log(s[i]);
}

console.log(i);//5
```

ES6的块级作用域

```js
function f1(){
    let n = 5;
    if(true){
        let n = 10;
    }
    console.log(n);//5
}//let为js新增了块级作用域，让外层代码块不受内层影响
//如果两次都要var定义变量，输出会是10

//es6运行块级作用域的任意嵌套
{{{{
    {let insane = 'hello world'}
    console.log(insane);//报错
}}}}；//第四层无法读取第五层的内部变量

{{{{
    let insane = 'hello world'；
    {let insane = 'hello world'}
}}}}；//内层作用域可以定义外层作用域的同名变量
```

##### 块级作用域与函数声明

ES6：

- 允许在块级作用域内声明函数
- 函数声明类似于var，即会提升到全局作用域函数作用域的头部
- 同时，函数声明还会提升到所在的块级作用域的头部。

```js
function f(){ console.log('I am outside!');}

(function(){
    if(false){
        //重复声明一次函数f
        function f(){ console.log('I am outside!');}
    }
    f();
}());

//块级作用域内部的函数声明语句，建议不要使用
{
    let a = 'secret';
    function f(){
        return a;
    }
}

//块级作用域内部，优先使用函数表达式
{
    let a = 'secret';
    let f = function(){
        return a;
    };
}
```

**注意：ES6的块级作用域必须有大括号**

```js
//报错
if(true) let x = 1;

//不报错
if(true){
    let x = 1;
}

//不报错
'use strict';
if(true){
   function f(){} 
}    

//报错
'use strict';
if(true)
   function f(){}
```

#### 3.const命令

const声明一个只读的常量。一旦声明，常量的值就不能改变。

```js
const PI = 3.1415;
PI//3.1415

PI = 3;//TypeError
```

const一旦声明变量必须立即初始化，不能留到以后再赋值。

```js
if(true){
    const MAX = 5;
}
MAX//Uncaught ReferenceError
```

const作用域和let相同：只在声明所在的块级作用域有效。而且常量也不提升，同样存在暂时性死区，只能在声明的位置后面使用。同样不可以重复使用。

```js
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError
//foo存的是一个地址，这个地址不可以变，但是指向的对象可以变，可以为它增加新属性
```

const本质：变量指向的内存地址所保存的数据不得改动。

```js
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```

Object.freeze :对象冻结

###### ES6声明变量的六种方法

var、function、let、const、import、class

##### 4.顶层对象的属性

顶层对象，在浏览器环境指的是window对象，在 Node 指的是global对象 。一方面规定，为了保持兼容性，var命令和function命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。 

```js
var a = 1;
// 如果在 Node 的 REPL 环境，可以写成 global.a
// 或者采用通用方法，写成 this.a
window.a // 1

let b = 1;
window.b // undefined
```

##### 5.globalThis对象

- 浏览器顶层对象是window,但Node和Web Worker没有window.
- 浏览器和Web Worker里面，self也指向顶层对象，但Node没有self.
- Node里面顶层对象是global，但其他环境都不支持.

为了在各种环境都取到顶层对象，使用**this**变量，但有局限性。

- 全局环境中，this返回顶层对象，ES6模块返回undefined。
- 如果函数不是作为对象运行，this会指向顶层对象。严格模式下，this会返回undefined。
- 不管是严格模式还说普通模式，new Function('return this')()总会返回全局对象。但是如果浏览器使用CSP，那么eval,new Function可能都无法使用。

```js
// 方法一
(typeof window !== 'undefined'
   ? window
   : (typeof process === 'object' &&
      typeof require === 'function' &&
      typeof global === 'object')
     ? global
     : this);

// 方法二
var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};
```

