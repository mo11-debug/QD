### ES6.bli1

变量声明：

- var  函数作用域，重新赋值定义，存在变量提升
- let 块级作用域
- const 块级作用域 声明后不能修改

同一作用域下let和const不能声明同名变量，var可以

let vs const关键字：

- let关键字不会提升为全局变量，始终是块级作用域，不能在同一块级作用域内声明同名变量
- const声明一个只读常量，一旦声明值不能改变，块级作用域，不存在变量类型提升

let和const理解：

块级作用域

```js
for (var i = 0; i < 5; i++) {
    console.log(i)
}
console.log(i) // 5
for (let j = 0; j < 5; j++) {
    console.log(j)
}
console.log(j) // error: j is not defined
```

暂时性死区：

```js
var i = 5;
(function hh() {
  console.log(i) // undefined
  var i = 10
})()

let j = 55;
(function hhh() {
  console.log(j) // ReferenceError: j is not defined
  let j = 77
})()
```

TDZ:

- var声明之前

  ```js
  console.log(color);
  var color = 'yellow';
  //undefined
  ```

- let声明之前

  ```js
  console.log(color);
  let color = 'yellow';
  //uncaught reference:color is not defined
  ```

- const声明之前

  ```js
  console.log(color);
  const color = 'yellow';
  //uncaught reference:color is not defined
  ```

箭头函数：

将原函数的“function”关键字和函数名都删掉，并使用“=>”连接参数列表和函数体。 

- 箭头函数this为父作用域的this，不是调用时的this 
- 箭头函数不能作为构造函数，不能使用new 
- 箭头函数没有arguments，caller，callee 
- 箭头函数通过call和apply调用，不会改变this指向，只会传入参数 
- 箭头函数没有原型属性 
- 箭头函数不能作为Generator函数，不能使用yield关键字 
- 箭头函数返回对象时，要加一个小括号 

箭头函数this指向：

非严格模式默认指向全局对象：

```js
function f1(){
  return this;
}

f1() === window; // true
```

严格模式，this为undefined

```js
function f2(){
  "use strict"; // 这里是严格模式
  return this;
}

f2() === undefined; // true
```

未完待补。。。