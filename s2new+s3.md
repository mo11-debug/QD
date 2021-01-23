#### 第二章

###### 变量类型和计算

1. JS中使用typeof能得到的哪些类型 ：undefined string number boolean object function

   typeof只能区分值类型 ，引用类型区分不了细的

2. 何时使用===何时使用==（**这里还不太懂**）

   问：100==‘100’？强制转换？

   ```
   if(obj.a == null){
   	//这里相当于obj.a === null || obj.a === undefined
   } 其余地方都用===
   ```

3. JS中有哪些内置函数：数据封装类对象

   - Object
   -  Array 
   - Boolean 
   - Number 
   - Function 
   - String 
   - Date 
   - Regexp 
   - Error

4. JS变量按照存储方式区分为哪些类型，并描述其特点

   值类型：直接赋值  引用类型：相互干预

5. 如何理解JSON：一个js对象

   常用：

   ```
   	JSON.stringify({a:10,b:20})
   	JSON.parse('{"a":10,"b":20}')
   ```

   强制转换：0,null,NaN,undefined,false在if()会变成false

###### 原型和原型链

1. 如何准确判断一个变量是数组类型：instanceof Array

2. 写一个原型链继承的例子

   ```
   function Animal(){
   	this.eat = function(){
   		console.log('animal eat')
   	}
   }
   
   function Dog(){
   	this.bark = function(){
   		console.log('dog bark')
   	}
   }
   
   Dog.prototype = new Animal()
   var hashiqi = new Dog()
   ```

3. 描述new一个对象的过程:创建一个新对象，this指向新对象，执行代码对this赋值，返回this

4. zepto（或其他框架）源码中如何使用原型链：多阅读源码

- 构造函数 :function Foo(){...}是var Foo = new Function()

- 使用instanceof判断一个函数是否是一个变量的构造函数

- 判断一个函数是否是数组：instanceof Array

- 原型规则：所有的引用类型都具有对象特性（null除外）

  ​		   所有的引用类型，都有__proto__属性

  ​		   所有的函数，都有prototype类型

  ​		   所有的引用类型proto属性指向它的构造函数的prototype类型

  ​		   当某个对象属性找不到，会去它的proto去找

- 原型链：Foo->Foo.prototype,Object->Object.prototype,f的proto属性是Foo.prototype，Foo.prototype的proto属性是Object.prototype，Object.prototype的proto属性是null

- instanceof：用于判断引用类型属于哪个构造函数的方法

##### 主要问题

- 什么叫作原型和原型链

  原型：每个函数的prototype属性,指针指向对象，用来给实例提供共享属性

  原型链：原型对象实例a等于另一个原型对象b,b中有指向a的指针，再让b实例等于原型对象c,构成实例和原型的链条

- 变量类型有哪些，分别是哪些？基本数据类型和引用数据类型的区别？

  undefined、null、string、number 、boolean、object 、function

  基本数据类型没有指针指向，不会相互影响，引用数据类型互相互干预

- 类型转换分几种。

  2种，转数值或转字符串。

- 隐式类型转换的规律。

  当使用==判等，转成同类型再比较，0,null,NaN,undefined,false在转布尔值会变成false，其他都变成true

#### 第三章

###### 作用域和闭包

```
console.log(a)//undefined
var a = 100

fn('zhangsan')//'zhangsan' 20
function fn(name){
    age = 20
    console.log(name, age)
    var age
}
```

- 执行上下文(注意函数和函数声明的区别）：

  ​	全局对象

  ​	函数：变量定义、函数声明、this、arguments

  - [ ] 变量对象
  - [ ] 作用域链
  - [ ] this

- **this要在执行时才能确认值，定义时无法确认**

  - [ ] 作为构造函数执行
  - [ ] 作为对象属性执行
  - [ ] 作为普通函数执行
  - [ ] call apply bind

- 当前作用域没有定义的变量，即“自由变量”

1. 说一下对变量提升的理解

   变量定义

   函数声明和函数表达式的区别

2. 说明this几种不同的使用场景

   - [ ] 构造函数
   - [ ] 对象属性
   - [ ] 普通函数
   - [ ] call apply bind

3. 创建10个<a>标签，点击的时候弹出来对应的序号

   ```
   //错误写法
   var i, a
   for(i = 0; i < 10; i++){
       a = document.createElement('a')
       a.innerHTML = i + '<br>'
       a.addEventListener('click', function(e){
           e.preventDefault()
           alert(i)
       })
       document.body.appendChild(a) 
   }//全局变量会有覆盖问题
   
   //正确写法
   var i
   for(i = 0; i < 10; i++){
   	(function(i){
           var a = document.createElement('a')
           a.innerHTML = i + '<br>'
       	a.addEventListener('click', function(e){
           	e.preventDefault()
           	alert(i)
       })
       document.body.appendChild(a)
   	})(i) //传进函数
   }
   ```

   

4. 如何理解作用域

   - 没有块级作用域
   - 只有函数和全局作用域
   - 自由变量，要去父作用域获取值
   - 作用域链，自由变量的查找
   - 闭包的两个场景

5. 实际开发中闭包的应用

   - 封装变量，收敛权限
   - 函数作为返回值
   - 函数作为参数传递

##### 主要问题

- 什么叫作用域

  负责查找声明变量，确定执行代码对变量的访问权限

- 什么叫闭包

  能够读取其他函数内部变量的函数

- 拓展：

  执行上下文，作用域，作用域链，三者搞清楚

```
function foo(i){
    var a = 'hello'
    var b = function privateB(){
        
    }
    function c(){
        
    }
}
foo(22)
```

执行上下文指执行环境，比如入参i不同的时候，输出a结果也不同，this的指向也不同。作用域链，链接另一个执行环境的指针。

问：函数提升的意思可以理解为改变执行顺序提到前面？

​	什么时候都会在作用域时产生闭包吗？假如要读取的变量在迭代循环里面呢？