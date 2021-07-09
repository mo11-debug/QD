**JavaScript**：由js引擎逐行解释执行，Node.js用于服务器端编程。

**JavaScript组成**：ECMAScript（JavaScript语法）、DOM（文档对象模型）、BOM（浏览器对象模型）

**JavaScript作用**：

- 表单动态校验（密码强度检测）
- 网页特效
- 服务端开发（Node.js）
- 桌面程序（Electron)、App(Cordova)、控制硬件-物联网(Ruff)、游戏开发(cocos2d-js)

**书写位置：**

1. 行内式

   ```js
   <input type="button" value="点我试试" onclick="alert('Hello World')"/>
   ```

2. 内嵌式

   ```js
   <script>
       alert('Hello World');
   </script>
   ```

3. 外部式

   ```js
   引用外部js文件
   <script src="my.js"></script>
   ```

**注释**

```js
//单行

/*多行*/
```

#### 变量

变量是程序在内存中申请的一块用于存放数据的控件。变量是用于存放数据的容器，可以通过变量名获取数据，甚至修改数据。

1. 声明变量

   ```js
   var num;
   ```

2. 赋值

   ```js
   num=10;
   ```

3. 变量初始化

   ```js
   var num=10;
   ```

4. 变量语法拓展

   ```js
   var num=10;
   num=11;
   //num会更新
   
   var num=10,age=15,name='fan';
   //同时声明多个
   ```

5. 变量命名规范

   规则：

   - 由字母（A-Za-a)、数字（0-9）、下划线、美元符号（$)组成，如：usrAge,num01,_name
   - 严格区分大小写。var app;和var App；是两个变量
   - 不饿能以数字开头。18age是错误的
   - 不能是关键字、保留字。例如：var、for、while
   - 变量必须有意义，MMD BBD nl-age
   - 遵守驼峰命名法。首字母小写，后面单词的单字母需要大写，myFirstName

##### 数据类型：根据存储空间分为不同是数据类型

**JS的变量数据类型是只有程序在运行过程中，根据等号右边的值进行判断的**。动态语言。变量的数据类型是可以变化的。

简单数据类型(Number,String,Boolean,Undefined,Null) 和复杂数据类型(object) 。

1. Number数字型

   ```js
   var num1=07;//十进制
   var num=0xA;//十六进制
   ```

   - 最大值：Number.MAX_VALUE,值为1.7976931348623157e+308 
   - 最小值:Number.MIN_VALUE,值为：5e-32 
   - 特殊值:Infinity无穷大 ； -Infinity无穷小； NaN代表一个非数字
   - isNaN():用来判断一个变量是否为非数字的类型。非数字型为true，数字型为false。

2. String字符串类型

   ```js
   var msg = '我的名字叫';
   var name = 'fan';
   ```

   - 转义符

     | 转义符 | 说明                   |
     | ------ | ---------------------- |
     | \n     | 换行符，n是newline意思 |
     | \\     | 斜杠\                  |
     | \'     | 单引号                 |
     | \"     | 双引号                 |
     | \t     | tab缩进                |
     | \b     | 空格，b是blank的意思   |

   - 字符串长度

     ```js
     var msg = '我是学生';
     console.log(msg.length);//显示4
     ```

   - 字符串拼接

     多个字符串之间可以使用+进行拼接，其拼接方式为字符串+任何类型=拼接之后的新字符串；**拼接前会把与字符串相加的任何类型转成字符串，再拼接成一个新的字符串**

     ```js
     alert('hello'+''+'world');//hello world
     //字符串相加
     
     alert('100'+'100');//100100
     //数值字符串相加
     
     alert('12'+12);//1212
     //数值字符串+数值
     //数值相加，字符相连
     //只要有字符串相加都会变成字符串类型
     
     var age=18;
     alert('我今年'+age+'岁')；
     //我今年18岁
     ```

3. Boolean

   布尔类型有两个值：true和false，其中true表示真，false表示假；

   **布尔型和数字型相加时候，true为1，false为0.**

   ```js
   console.log(true+2);//3
   console.log(false+1);//1
   ```

4. Undefined:一个变量声明后没有赋值会有一个默认值undefined（如果相连或者相加时，注意结果）

   ```js
   var variable;
   console.log(variable);//undefined
   console.log("你好"+variable);//你好undefined
   console.log(11+variable);//NaN
   console.log(true+variable);//NaN
   ```

5. Null:一个变量声明并赋值为null，里面存的值为空

   ```js
   var var2=null;
   console.log(var2);//null
   console.log("你好"+var2);//你好null
   console.log(11+var2);//11
   console.log(true+var2);//1
   ```

##### 获取变量类型及转换

- 检测变量的数据类型：typeof

  ```js
  var num=10;
  console.log(typeof num)//结果为number
  ```

- 字面量：源代码中一个固定值的表示法，就是字面量如何去表达这个值。通过数据的格式特征可以判断数据的类型

  - [ ] 数字字面量：8，9，10
  - [ ] 字符串字面量：'前端开发'
  - [ ] 布尔字面量：true,false

- 数据类型转换

  - [ ] 转换为字符串

  | 方式           | 说明                         | 案例                              |
  | -------------- | ---------------------------- | --------------------------------- |
  | toString()     | 转成字符串                   | var num=1;alert(num,toString())   |
  | String()       | 强制转换                     | var num=1;alert(String(num))      |
  | 加号拼接字符串 | 和字符串拼接的结果都是字符串 | var num=1;alert(num+'我是字符串') |

  - [ ] 转换为数字型

  | 方式                   | 说明                         | 案例               |
  | ---------------------- | ---------------------------- | ------------------ |
  | parseInt(String)函数   | 将string类型转为整数型       | parseInt('11')     |
  | parseFloat(String)函数 | 将string类型转为浮点型       | parseFloat('11.2') |
  | Number()强制转换函数   | 将string类型转为数值型       | Number('12')       |
  | js隐式转换（-*/）      | 利用算数运算隐式转换为数值型 | '12'-0             |

  - [ ] 转换为布尔型

    代表空、否定的值会被转换为false,如“，0,NaN,undefined其余值都会被转换为true；Boolean('true')

##### 关键字和保留字

标识符：指开发人员为变量、属性、函数、参数取得名字。标识符不能是关键字或保留字。

关键字：指JS本身已经使用了的字，不能再用它们充当变量名、方法名

包括：break、case、catch、continue、default、delete、do、else、finally、for、function、if、in、instanceof、new、return、switch、this、throw、try、typeof、var、void、while、with 等。 

保留字：实际上就是预留发”关键字“，意思是限制还不是关键字，但是未来可能会成为关键字，同样不能使用它们充当变量名、方法名。

包括：boolean、byte、char、class、const、debugger、double、enum、export、extends、fimal、float、goto、implements、import、int、interface、long、mative、package、private、protected、public、short、static、super、synchronized、throws、transient、volatile 等。 

##### 运算符与流程控制

运算符：用于实现赋值、比较和执行算数运算等功能的符号。

- 算数运算符

  | 运算符 | 描述         | 案例      |
  | ------ | ------------ | --------- |
  | +      | 加           | 10+20=30  |
  | -      | 减           | 10-20=-10 |
  | *      | 乘           | 10*20=200 |
  | /      | 除           | 10/20=0.5 |
  | %      | 取模（取余） | 9%2=1     |

  浮点数精度问题

  ```js
  var result = 0.1+0.2;//0.30000000000000004
  console.log(0.07 * 100);   // 7.000000000000001
  //最高精度为17位小数，在运行算数运算时其精确度远远不如整数，所以不要之间判断两个浮点数是否相等
  ```

  表达式与返回值

  表法式：由数字、运算符和变量组成的式子

  返回值：每一个表达式经过相应的运算之后，会有一个最终结果，称为表达式的返回值

- 递增和递减运算符：递增和递减运算符必须配合变量使用

  - [ ] 递增运算符

    ```js
    var num=10;
    alert(++num + 10);//21 
    //先自加，后返回值
    
    var num1=10;
    alert(10 + num1++);//20
    //先返回原值，后自加
    ```

    ```js
    var num = 1;
    var num2 = ++num +num++;//num=2
    console.log(num2);//4
    
    var num = 1;
    var num1 = 1;
    var num2 = num++ + num1++;//1+1
    console.log(num2)//2
    
    var num = 1;
    var num2 = num++ + num++;//1+2
    console.log(num2);//3
    ```

- 比较运算符

  | 运算符  | 描述                                  | 案例      | 结果  |
  | ------- | ------------------------------------- | --------- | ----- |
  | <       | 小于                                  | 1<2       | true  |
  | >       | 大于                                  | 1>2       | false |
  | >=      | 大于等于                              | 1>=1      | true  |
  | <=      | 小于等于                              | 2<=1      | false |
  | ==      | 判等号（会转型）                      | 15=='15'  | true  |
  | !=      | 不等号                                | 37!=37    | false |
  | ===!=== | 全等 全不等（要求值和数据类型都一致） | 37==='37' | false |

- 逻辑运算符

  逻辑运算符：用来进行布尔值运算的运算符

  短路运算：当有多个表达式（值）时，左边的表达式值可以确定结果时，就不再继续运算右边的表达式的值

  | 运算符 | 描述                  | 案例          | 特点                     |
  | ------ | --------------------- | ------------- | ------------------------ |
  | &&     | "逻辑与",简称"与" and | true && false | 两边都是 true才返回 true |
  | \|\|   | "逻辑或",简称"或" or  | true          | 有真为真                 |
  | ！     | "逻辑非",简称"非" not | !true         | 取反                     |

- 赋值运算符

  | 运算符    | 描述                 | 案例                 |
  | --------- | -------------------- | -------------------- |
  | =         | 直接赋值             | var userName = 'fan' |
  | +=  -=    | 加减一个数后再赋值   | var age=5; age+=5    |
  | *=  /= %= | 乘、除、取模后再赋值 | var age=5; age*=5    |

- 运算符优先级

  1. ()
  2. ! ++ --
  3. 先*/% 后+-
  4. 关系运算符> >=
  5. == != === !==
  6. 先&&后||
  7. =
  8. ，

##### 流程控制

- 顺序结构
- 分支结构
- 循环结构

分支流程：

```js
//条件成立执行
if(条件表达式){
    
}

//if else语句
if(条件表达式){
    
}else{
    
}

//if else if语句（多分支）
if(条件1){
    语句1：
}else if(条件2){
    语句2：
}else if(条件3){
    语句3:
}else{
    
}
```

三元表达式：

```js
表达式1？表达式2：表达式3;
//1为true则返回2，false返回3
```

switch分支流程控制

```js
switch(表达式){
    case value1:
        break;
    case value2:
        break;
    default:
        
}
```

**循环**

```js
for(初始化变量；条件表达式；操作表达式){
    //循环体
}
```

执行流程：

1. 初始化变量，初始化操作在整个for循环只会执行一次
2. 执行条件表达式，如果为true，则执行循环体语句，否则退出循环，循环结束
3. 执行操作表达式，第一轮结束
4. 第二轮开始，直接去执行条件表达式（不再初始化变量），如果为true，则执行循环体语句，否则退出循环
5. 继续执行......

双重for循环：循环嵌套

```js
for(外循环的初始；外循环条件；外循环操作表达式){
    for(内循环初始；内循环条件；内循环操作表达式){
        需执行代码;
    }
}
    
//九九乘法表
    var str = "";
    for(var i = 1;i<=9;i++){
    for(var j=1;j<=i;j++){
        str += j + "x" + i "=" j*i + "\t";
    }
    str += "\n";
}
console.log(str);
```

while循环

```js
while(条件表达式){
    //循环体代码
}

//计算1-100的累加和
var i=1;
var sum=0;
whiel(i<=100){
    sum+=i;
    i++;
}
console.log(sum);
```

continue：用于立即跳出本次循环，继续下次循环(本次循环体中continue之后的代码会少执行一次)

break：用于立即跳出整个循环（循环结束）

**代码规范**

1. 标识符命名规范

   - 变量、函数的命名必须要有意义
   - 变量的名称一般用名词
   - 函数的名称一般用动词

2. 操作符规范

   ```js
   for (var i = 1; i <= 5; i++){
       if (i == 3){
           break;
       }
       console.log('我在吃第' + i + '个包')
   }
   ```

3. 单行注释规范

   ```js
   for (var i = 1; i <= 5; i++){
       if (i == 3){
           break; // 单行注释前面注意有个空格
       }
       console.log('我在吃第' + i + '个包')
   }
   ```

4. 其他规范

   ```js
   //关键词 操作符空格
   if (true) { }
   for (var i = 0; i <= 10; i++){}
   ```

##### 数组与函数

1. 数组概念：一组数据的集合，其中的每个数据被称作元素，在数组中可以存放任意类型的元素。数组是一种将一组数据存储在单个变量名下的优雅方式。

2. 创建数组

   - 利用new关键字创建数组

     ```js
     var 数组名 = new Array([n]);//n代表数组的长度
     ```

   - 利用数组字面量创建数组

     ```js
     var 数组名 = [];//创建一个空的数组
     
     var arr = ['1','2','3','4'];
     var arr2 = ['fan',true,17.5];//数组中可以存放任意类型的数据
     ```

3. 访问数组元素

   索引（下标）：用来访问数组元素的序号。索引从0开始

   ```js
   var arr=['小白','小黄','小黑']；
   alert(arr[1]);//小黄
   alert(arr[8]);//undefined
   //如果访问数组时没有和索引值对应的元素，返回值为undefined
   ```

4. 遍历数组

   ```js
   var arr = ["red"，"blue","green"];
   for(var i=0;i<arr.length;i++){
       console.log(arr[i]);
   }
   arr.length=2;
   console.log(arr);//red blue
   ```

5. 数组中新增元素

    ```js
      arr=[]
      for(var i = 0;i<10;i++){
       arr[arr.length]='0';
      }
      console.log(arr);
      //数组末尾插入新元素
    ```

6. 案例

   ```js
   //1.筛选数组
   //法一
   var arr=[2,0,6,1,77,0,52,0,25,7];
   var newArr=[];
   var j;
   for(var i=0;i<arr.length;i++){
       if(arr[i]>=10){
           newArr[j]=arr[i];
           j++;
       }
   }
   console.log(newArr);
   //法二
   for(var i=0;i<arr.length;i++){
       if(arr[i]>=10){
           newArr[newArr.length]=arr[i];
       }
   }
   
   //2.翻转数组
   var arr =['red','green','blue','pink','purple'];
   var newArr=[];
   for(var i=arr.length-1;i>=0;i--){
       newArr[newArr.length]=arr[i]
   }
   console.log(newArr);
   
   //3.数组转换为字符串,用“|”或其他符号分割
   //需要一个新变量用于存放转换为的字符串str
   //遍历取出数据加到str后面然后加上分隔符
   var arr=['red','green','blue','pink','purple'];
   var str='';
   for(var i=0;i<arr.length;i++){
       str+=arr[i]+'|';
   }
   console.log(str);
   
   //冒泡排序
   function sort(arr){
       for(var i=0;i<arr.length-1;i++){
           for(var j=0;j<arr.length-1-i;j++){
               if(arr[j]>arr[j+1]){
                   var temp=arr[j];
                   arr[j]=arr[j+1];
                   arr[j+1]=tmp;
               }
           }
       }
       return arr;
   }
   var arr1=sort([1,4,2,9]);
   console.log(arr1);//1 2 4 9
   ```

##### 函数

1. 概念：封装了一段可被重复调用执行的代码块，通过函数可以实现大量代码的重复使用。函数是一种数据类型。

2. 函数的使用

   - 声明函数

     ```js
     function 函数名(){
         //函数体代码
     }
     //functions 是声明函数的关键字，必须小写
     //函数名命名为动词形式，例：getSum
     
     //匿名函数
     var fn=function(){
         
     };
     //fn变量，不过变量储存的是函数
     //函数表达式创建的函数可以通过变量名();来调用
     //函数表达式也可以定义形参和调用传入实参
     
     //匿名函数自调用
     (function(){
         alert(123);
     })();
     ```

   - 调用函数

     ```js
     函数名();//函数声明后调用才会执行函数体代码
     ```

   - 函数封装：把一个或多个功能通过函数的方式封装起来，对外只提供一个简单的函数接口、

     ```js
     //计算1-100累加
     function getSum(){
         var sumNum=0;
         for(var i=1;i<=100;i++){
             sumNum+=i;
         }
         alert(simNum);
     }
     getSum();
     ```

3. 参数

   - 形参：函数定义时候，传递的参数（实参值传递给形参，不用声明的变量）
   - 实参：函数调用时候，传递的参数

   ```js
   function 函数名(形参1，形参2，形参3......){
       //函数体
   }
   
   //带参调用函数
   函数名(实参1，实参2，实参3...);
   ```

   形参与实参数量不匹配时：

   - 实参等于形参个数，输出正确结果
   - 实参个数多于形参个数，只取到形参个数
   - 实参个数小于形参，多的形参定义为undefined，结果为NaN

   ```js
   function getSum(a,b,c){
       return a+b+c;
   }
   var n=getSum(1,2);//n=NaN
   var n=getSum(1,2,3,4);//n=6
   ```

4. 返回值:函数调用整体代表的数据，函数执行完成后可以通过return语句将指定数据返回

   ```js
   function 函数名(){
       return 需要返回的值;
   }//函数遇到return会停止执行，并返回指定的值
   //如果函数没有return返回的值是undefined
   
   函数名();//此时调用函数就可以得到函数体return的值
   ```

   break,continue,return区别：

   - break：结束当前循环体
   - continue：跳出本次循环，继续执行下次循环
   - return：不仅可以退出函数体内循环，还能够返回return语句中的值，同时还可以结束当前的函数体的代码

   ```js
   //return只能结束函数体的代码
   function breakDown(){
       for(var i=0;i<10;i++){
           if(i==5){
               return 1;
           }
           console.log(i);
       }
   }
   breakDown();
   //如果有return，则返回的是return后面的值，如果没有，则返回undefined
   ```

5. arguments的使用

   当不确定有多少个参数传递的时候，可以用arguments来获取。JS中，arguments实际上是当前函数的一个内置对象，所有函数都内置了一个arguments对象，arguments对象中存储了传递的所有实参。arguments展示形式是一个伪数组，因此可以进行遍历。

   特点：

   - 具有length属性
   - 按索引方式存储数据
   - 不具有数组的push，pop等方法

   ```js
   function fn(){
       console.log(arguments);//[1,2,3...]
       console.log(arguments[1]);//2
       console.log(arguments.length);//3
   }
   fn(1,2,3);
   
   //用伪数组实现求最大值
   function getMax(){
       var max=arguments[0];
       for(var i=1;i<arguments.length;i++){
           if(arguments[i]>arguments[0]){
               max=arguments[i];
           }
       }
       return max;
   }
   var result=getMax(1,3,77,5,85)
   console.log(result);
   ```

##### 作用域:

一段程序代码中所用到的名字并不总是有效和可靠的，而限定这个名字的可用性代码范围就是这个名字的作用域。

- 作用域的使用提高了程序逻辑的局部性，增强了程序的可靠性，减少了名字冲突。
- ES6之前作用域有两种全局作用域和局部作用域（函数作用域）

**全局作用域**：

作用于所有代码执行的环境（整个script标签内部）或者一个独立的js文件。

**局部作用域**：

作用于函数内部的代码环境，就是局部作用域。因为跟函数有关系，所以也被称为函数作用域。

**JS没有块级作用域**：块作用域由{}包括。

```js
if(true){
    var num=123;
    console.log(num);//123
}
console.log(num);//123
```

##### 变量的作用域

根据作用域的不同，变量分为全局变量、局部变量

**全局变量**：在全局作用域下声明的变量（在函数外部定义的变量）

- 全局变量在代码的任何位置都可以使用
- 在全局作用域下var声明的变量是全局变量
- 特殊情况下，在函数内不使用var声明的变量也是全局变量

**局部变量**：在局部作用域下声明的变量（在函数内部定义的变量）

- 局部变量只能在函数内部使用
- 在函数内部var声明的变量是局部变量
- 函数的形参实际上就是局部变量

**全局变量和局部变量的区别**：

全局变量在任何一个地方都可以使用，只有在浏览器关闭时才会销毁，因此比较占内存；局部变量旨在函数内部使用，当其所在的代码块被执行时，才会被初始化；当代码块运行结束后，就会被销毁，因此更节省内存空间。

##### 作用域链:

只要是代码都在一个作用域中，写在函数内部的局部作用域，未卸载仍和行数内部即在全局作用域中；如果函数中还有函数，那么在这个作用域中就又可以诞生一个作用域；根据**内部函数可以访问外部函数变量**机制，用链式查找决定哪些数据能被内部函数访问，就称作作用域链。

```js
function f1(){
    var num=123;
    function f2(){
        console.log(num);
    }
    f2();
}
var num=456;
f1();
```

采用**就近原则**来查找变量最终的值

```js
var a=1;
function fn1(){
    var a=2;
    var b='22';
    fn2();
    function fn2(){
        var a=3;
        fn3();
        function fn3(){
            var a=4;
            console.log(a);//4
            console.log(b);//'22'
        }
    }
}
fn1();
```

**预解析**

在当前作用域下，JS代码执行之前，浏览器会默认把带有var和function声明的变量在内存中进行提前声明或定义

**代码执行**

从上往下执行JS语句

预解析会把变量和函数的声明在代码执行之前完成，预解析也叫做变量、函数提升。

**变量预解析（变量提升）**：

变量的声明会被提升到当前作用域的最上面，变量的赋值不提升。

```js
console.log(num);
var num=10;

相当于
var num;
console.log(num);//undefined
num=10;
注意：变量提升只提升声明，不提升赋值。
```

**函数预解析（函数提升）**：函数的声明会被提升到当前作用域的最上面，但是不会调用函数。

```js
fn();
function fn(){
    console.log('打印');
}
```

**函数表达式声明函数问题**

```js
函数表达式创建函数，会执行变量提升，此时接收函数的变量名无法正确的调用
fn();
var fn=function(){
    console.log('想不到');
}//报错：fn is not a function
```

案例：

```js
var num=10;
fun();
function fun(){
    console.log(num);
    var num=20;
}
相当于执行以下操作，结果打印undefined
var num;
function fun(){
    var num;
    console.log(num);
    num=20;
}
num=10;
fun();

案例2
var a = 18;
f1();
function f1(){
    var b=9;
    console.log(a);
    console.log(b);
    var a ='123';
}
相当于执行以下操作，结果为undefined 9
var a;
function f1(){
    var b;
    var a;
    b=9;
    console.log(a);
    console.log(b);
    a='123';
    
}
a=18;
f1();

案例3
f1();
console.log(c);
console.log(b);
console.log(a);

function f1(){
    var a = b = c = 9;
    console.log(a);
    console.log(b);
    console.log(c);
}
相当于,结果为9 9 9 9 9 "报错：a is not defined"
function f1(){
    var a;
    a=b=c=9;//b和c没有直接赋值，没有var声明，当全局变量
    //集体声明var a=9,b=9,c=9;
    console.log(a);
    console.log(b);
    console.log(c);
}
f1();
console.log(c);
console.log(b);
console.log(a);
```

##### 对象

对象是一组无序的相关属性和方法的集合，所有的事物都是对象，例如字符串、数值、数组、函数等。

- 对象是由属性和方法组成的

  - [ ] 属性：事物的特征，在对象中用属性来表示（常用名词）
  - [ ] 方法：事物的行为，在对象中常用方法来表示（常用动词）

- 为什么需要对象

  - [ ] 保存一个值时，可以使用变量，保存多个值（一组值）时，可以使用数组，如果保存一个完整的信息呢？
  - [ ] 为了更好地存储一组数据，对象应运而生；对象中为每项数据设置了属性名词，可以访问数据更语义化，数据结构清晰，表意明显，方便开发者使用。

  ```js
  var obj={
      "name":"fan",
      "sex":"male",
      "age":18;
      "height":180
  }
  ```

##### 创建对象的三种方式

1. 利用字面量创建对象：键值对形式

   ```js
   var star = {
       name : 'pink',
       age : 18,
       sex : '男',
       sayHi : function(){
           alert('大家好');
       }
   };
   ```

   对象的使用：

   - 对象属性：对象中存储具体数据的"键值对"中的键称为对象的属性，即对象中存储具体数据的项
   - 对象的方法：对象中存储函数的"键值对"中的"键"称为对象的方法，即对象中存储函数的项
   - 访问对象的属性：对象里面的属性调用：对象.属性名；对象里面属性的另一种调用方式：对象['属性名']，注意方括号里面的属性必须加上引号。
   - 调用对象的方法：对象.方法名();
   - 变量、属性、函数、方法：
     - [ ] 变量：单独声明赋值，单独存在
     - [ ] 属性：对象里面的变量称为属性，不需要声明，用来描述该对象的特征
     - [ ] 方法：方法是对象的一部分，函数不是对象的一部分，函数是单独封装操作的容器。对象里面的函数称为方法，方法不需要声明，使用"对象.方法名()"的方式就可以调用，方法用来描述该对象的行为和功能
     - [ ] 函数：单独存在的，通过"函数名()"的方式就可以调用

   ```js
   consle.log(star.name)//pink
   console.log(star['name'])
   star.sayHi();
   ```

2. 利用new Object创建对象：

   - 创建空对象

     ```js
     var andy = new Object();//此时andy变量已经保存了创建出来的空对象
     ```

   - 给空对象添加属性和方法

     ```js
     andy.name='pink';
     andy.age=19;//修改属性
     andy.phoneNum=110//添加属性
     andy.sex='男';
     andy.sayHi=function(){
         alert('大家好');
     }
     obj.sayHi();//调用对象的方法
     obj['sayHi']();
     new Object()//需要关键字new，使用格式：对象.属性=值
     ```

3. 利用构造函数创建对象：

   构造函数是一种特殊的函数，主要用来初始化对象，即为对象成员变量赋值初始值，它总与new运算符一起使用，我们可以把对象中一些公共的属性和方法抽出来，然后封装到这个函数里面。

   - 构造函数的封装格式：

     ```js
     function 构造函数名(形参1，形参2...){
         this.属性名1=参数1;
         this.属性名2=参数2;
         this.方法名=函数体;
     }
     ```

   - 构造函数的调用格式

     ```js
     var obj=new 构造函数名(实参1，实参2...)
     ```

     **注意事项**：

     - [ ] 构造函数约定首字母大写
     - [ ] 函数内的属性和方法前面需要添加this，表示当前对象的属性和方法
     - [ ] 构造函数中不需要return返回结果
     - [ ] 但我们创建对象的时候，必须用new来调用构造函数
     - [ ] 创建对象，如new Stars();特指某一个，利用new关键字创建对象的过程称为对象实例化

#### news关键字的作用（面试题）

1. 在构造函数代码开始执行之前，创建一个空对象

2. 修改this的指向，把this指向创建出来的空对象

3. 执行构造函数内的代码，给这个新对象添加属性和方法

4. 在函数完成之后，返回这个创建出来的新对象（所以构造函数里面不需要return)

   ```js
   function createPerson(name,age,job){
       var persion=new Object();
       persion.name=name;
       persion.age=age;
       persion.job=job;
       persion.sayHi=function(){
           console.log('Hello everyBody');
       }
       return persion;
   }
   var p1=createrPerson('张三',22,'actor');
   ```

##### 遍历对象

```js
for(var k in obj){
    console.log(k); // 这里的 k 是属性名
    console.log(obj[k]);// 这里的 obj[k] 是属性值
}
```

##### Math对象：

不是构造函数，它具有数学常数和函数的属性和方法，跟数学相关。

| 属性、方法名          | 功能                                         |
| --------------------- | -------------------------------------------- |
| Math.PI               | 圆周率                                       |
| Math.floor()          | 向下取整                                     |
| Math.ceil()           | 向上取整                                     |
| Math.round()          | 四舍五入版 就近取整   注意 -3.5   结果是  -3 |
| Math.abs()            | 绝对值                                       |
| Math.max()/Math.min() | 求最大和最小值                               |
| Math.random()         | 获取范围在[0,1)内的随机值                    |

获取指定范围的随机整数：

```js
function getRandom(min,max){
    return Math.floor(Math.random()*(max-min+1))+min;
}
```

##### 日期对象

Date是一个构造哈桑农户，所以使用时需要实例化后才能使用其中具体方法和属性。Date实例用来处理日期和时间

- 使用Date实例化日期对象

  - [ ] 获取当前时间必须实例化

  - [ ] 获取指定时间的日期对象

    ```js
    var now=new Date();
    var future = new Date('2020/10/1')
    //如果创建实例时并未传入参数，则得到的日期对象是当前时间对应的日期对象
    ```

- 使用Date实例的方法和属性

- getMonth()方法获取到的月份+1=当月

  | 方法名        | 说明                       | 代码               |
  | ------------- | -------------------------- | ------------------ |
  | getFullYear() | 获取当年                   | dObj.getFullYear() |
  | getMonth()    | 获取当月（0-11）           | dObj.getMonth()    |
  | getDate()     | 获取当天日期               | dObj.getDate()     |
  | getDay()      | 获取星期几（周日0到周六6） | dObj.getDay()      |
  | getHours()    | 获取当前小时               | dObj.getHours()    |
  | getMinutes()  | 获取当前分钟               | dObj.getMinutes()  |
  | getSeconds()  | 获取当前秒钟               | dObj.getSeconds()  |

  ```js
  var date1=new Date(2019,10,1);
  
  //日期格式化
  var date=new Date();
  console.log(date.getFullYear());//2020
  console.log(date.getMonth() + 1); //月份 返回的月份小1个月，记得月份加1呦
  console.log(date.getDate()); //返回的是 几号
  console.log(date.getDay); //周一返回的是1 周六返回的是6 周日返回的是0
  //我们写一个 2020年 9月 6日 星期日
  var year = date.getFullYear();
  var month = date.getMonth() + 1;
  var dates = date.getDate();
  var day = date.getDay();
  if (day == 0) {
     day = "星期日";
  }
  console.log("今天是" + year + "年" + month + "月" + dates + "日" + day);
  ```

  ```js
  //格式化日期 时分秒
  var date=new Date();
  console.log(date.getHours());
  console.log(date.getMinutes());
  console.log(date.getSeconds());
  
  //封装一个函数返回当前的时分秒格式08：08：08
  functuon getTimer(){
      var time = new Date();
      var h = time.getHours();
      var h = h<10?"0"+h:h;
      
      var m = time.getMintues();
      var m=m<10?"0"+m:m;
      
      var s=time.getSeconds();
      var s=s<10?"0"+s:s;
      return h+":"+h+":"+s;
  }
  console.log(getTimer());
  ```

**获取Date日期总的毫秒数（时间戳）**：基于1970年1月1日（世界标准世界）起的毫秒数

```js
//实例化Date对象
var now=new Date();
//1.用于获取对象的原始值
console.log(now.valueof())
console.log(now.getTime())
//2.简单写可以这么做（最常用的）
var now = + new Date();
//3.HTML5中提供的方法，有兼容性问题
var now=Date.now();
```

倒计时案例：

```js
1. 输入的时间减去现在的时间就是剩余的时间，即倒计时。
2.用时间戳来做，用户输入时间总的毫秒数减去现在时间的总的毫秒数，
   得到的就是剩余时间的毫秒数
3.把剩余时间总的毫秒数转换为天、时、分、秒  (时间戳转换时分秒)
转换公式如下：
d = parseInt(总秒数/60/60/24) // 计算天数
h = parseInt(总秒数/60/60%24) // 计算小时
m = parseInt(总秒数/60%60);   // 计算分钟  
s = parseInt(总秒数%60);      // 计算当前秒数 

//封装函数实现
function countDown(time)(){
    var nowTime = +new Date();//当前时间总的毫秒数
    var inputTime = + new Date(time);//用户输入时间总的毫秒数
    var times = (inputTime - nowTime)/1000;// times是剩余时间总的秒数
    var d = parseInt(times/60/60/24);//天
    d = d < 10 ? "0" + d : d;
    var h = parseInt(times/60/60%24);//时
    h = h < 10 ? "0" + h : h;
    var m = parseInt((times/60)%60);//分
    m = m < 10 ? "0" + m : m;
    var s = parseInt(times%60)%;//当前的秒
    s = s < 10 ? "0" + s : s;
    return d + "天" + h + "时" + m + "分" + s + "秒";
}
console.log(conutDown("2020-10-1 18:00:00"));
var date = new Date();
console.log(date);
```

##### 数组对象

**创建数组的两种方式**

1. 字面量方式：var arr = [1,"test",true];
2. 实例化数组对象:new Array():var arr = new Array();
   - 注意：上面代码中arr创建出的是一个空数组，如果需要使用构造函数Array创建非空数组，可以在创建数组时传入参数
   - 如果只传入一个参数（数字），则参数规定了数组的长度。
   - 如果传入了多个参数，则参数称为数组的元素

**检测是否为数组**：

1. instanceof运算符

   ```js
   var arr = [1,23];
   var obj = {};
   console.log(arr instanceof Array);//true
   console.log(obj instanceof Array);//false
   ```

2. Array.isArray()

   ```js
   var arr = [1,23];
   var obj={};
   console.log(Array.isArray(arr));//true
   console.log(Array.isArray(obj));//false
   ```

3. 注意typeof用法:typeof用于判断变量的类型

   ```js
   var arr=[1,23];
   console.log(typeof arr)//object
   ```

**添加删除数组元素的方法**：

| 方法名            | 说明                                                  | 返回值               |
| ----------------- | ----------------------------------------------------- | -------------------- |
| push(参数1...)    | 末尾添加一个或多个元素，注意修改原数组                | 返回新的长度         |
| pop()             | 删除数组最后一个元素，把数组长度减1无参数、修改原数组 | 返回它删除的元素的值 |
| unshift(参数1...) | 向数组的开头添加一个或更多元素，注意修改原数组        | 返回新的长度         |
| shift()           | 删除数组的第一个元素，数组长度减1无参数、修改原数组   | 返回第一个元素的值   |

```js
var arr=[1,2,3];
console.log(arr.push(4,5));//5 向数组末尾添加元素
arr.pop();//删除数组最后一个值并返回
console.log(arr);//[1,2,3,4]

//向数组的开头添加元素并返回数组长度
console.log(arr.unshift(5,6));//6 数组变为【5，6，1，2，3，4】
console.log(arr.shift());//5 删除数组开头的元素并返回该值
```

**数组排序**

- 数组中有对数组本身排序的方法：

  reverse():颠倒数组中元素的顺序，无参数；该方法会改变原来的数组，返回新数组

  sort():对数组的元素进行排序；该方法会改变原来的数组，返回新数组

- 注意：sort方法需要传入参数（函数）来设置升序、降序排序

  - [ ] 如果传入"function(a,b){return a-b;}",则为升序
  - [ ] 如果传入"function(a,b){return b-a;}",则为降序

  ```js
  var arr1 = [13,4,77,1,7];
  arr1.sort(function(a,b){
      return a-b;
  })
  console.log(arr1)
  ```

**数组索引方法**

| 方法名        | 说明                           | 返回值                           |
| ------------- | ------------------------------ | -------------------------------- |
| indexOf()     | 数组中查找给定元素的第一个索引 | 如果存在返回索引号，不存在返回-1 |
| lastIndexOf() | 在数组中的最后一个索引         | 如果存在返回索引号，不存在返回-1 |

```js
var arr=[1,2,3,4,5,4,1,2];
//查找元素2的索引
console.log(arr.indexOf(2));//1
//查找元素1在数组中的最后一个索引
console.log(arr.lastIndexOf(1));//6
```

**数组转换为字符串**

- 数组中有把数组转化为字符串的方法

- 注意：join方法如果不传入参数，则按照","拼接元素

  ```js
  var arr=[1,2,3,4];
  var arr2=arr;
  var str=arr.toString();//将数组转换为字符串
  console.log(str);//1,2,3,4
  
  var str2=arr2.join("|");//按照键入字符将数组转换为字符串
  console.log(str2);
  ```

  ```js
  var arr=[1,2,3,4];
  var arr2=[5,6,7,8];
  var arr3=arr.concat(arr2);
  console.log(arr3);//[1,2,3,4,5,6,7,8]
  
  //slice(begin,end)是一种左闭右开区间[1,3)
  //从索引1出开始截取，到索引3之前
  var arr4=arr.slice(1,3);
  console.log(arr4);//[2,3]
  
  var arr5=arr2.splice(0,3);
  console.log(arr5);//[5,6,7]
  console.log(arr2);//[8] splice()会影响原数组
  ```

  ##### 字符串对象

  基本包装类型：就是把简单数据理想包装成为复杂数据类型，这样基本数据类型就有了属性和方法；提供了三个特殊的引用类型：String、Number和Boolean.

  ```js
  var str='andy';
  console.log(str.length);//4
  //按道理基本数据类型是没有属性和方法的，而对象才有属性和方法，但是上面代码却可以执行，这是因为js会把基本数据类型包装为复杂数据类型，其执行过程：
  var temp=new String('andy');
  str=temp;
  temp=null;
  //生成临时变量，把简单类型包装为复杂数据类型；赋值给我们声明的字符变量；销毁临时变量
  ```

  **字符串的不可变**

  - [ ] 指的是里面的值不可变，虽然看上去可以改变内容，但其实是地址变了，内存中新开辟了一个内存空间
  - [ ] 当重新给字符串变量赋值的时候，变量之前保存的字符串不会被修改，依然在内存中重新给字符串赋值，会重新在内存中开辟空间，这个额点就是字符串的不可变
  - [ ] 由于字符串的不可变，在**大量拼接字符串**的时候会有效率问题

  **根据字符返回位置**

  - [ ] 字符串通过基本包装类型可以调用部分方法来操作字符串：

  - indexOf('要查找的字符',开始的位置)：返回指定内容在元字符串中的位置，如果找不到就返回-1，开始的位置是index索引号

  - lastIndexOf():从后往前找，只找第一个匹配的

    ```js
    var str="anndy";
    console.log(str.indexOf("d"));//3
    //指定从索引号为4的地方开始查找字符"d"
    console.log(str.indexOf("d",4));//-1
    console.log(str.lastIndexOf("n"));//2
    ```

  案例：查找字符串”abcoefoxyozzopp“中所有o出现的位置以及次数

  1. 先查找第一个o出现的位置

  2. 然后 只要indexOf返回的结果不说-1就继续往后查找

  3. 因为indexOf只能查找到第一个，所以后面的查找，利用第二个参数，当前索引加1，从而继续查找

     ```js
     var str="oabcoefoxyozzopp";
     var index=str.indexOf("o");
     var num=0;
     while(index !== -1){
         console.log(index);
         num++;
         index=str.indexOf("o",index+1);
     }
     ```

  **根据位置返回字符**

  字符串通过基本包装类型可以调用部分方法来操作字符串，根据位置返回指定位置上的字符：

  | 方法名            | 说明                                       | 使用                         |
  | ----------------- | ------------------------------------------ | ---------------------------- |
  | charAt(index)     | 返回指定位置的字符（index字符串的索引号）  | str.cahrAt(0)                |
  | charCodeAt(index) | 获取指定位置处字符的ASCII码（index索引号） | str.charCodeAt(0)            |
  | str[index]        | 获取指定位置处字符                         | HTML5,IE8+支持和charAt()等效 |

  ```js
  //根据位置返回字符
  //1.charAt(index)
  var str='andy';
  console.log(str.charAt(3));//
  //遍历所有的字符
  for(var i=0;i<str.length;i++){
      console.log(str.charAt(i));
  }//a n d y
  
  //2.charCodeAt(index)
  //返回相应索引号的字符ASCII值 目的：判断用户按下了那个键
  console.log(str.charCodeAt(0));//97
  //3.str[index] H5新增的
  console.log(str[0]);//a
  ```

  案例：判断一个字符串 'abcoefoxyozzopp' 中出现次数最多的字符，并统计其次数 

  1. 核心算法：利用charAt()遍历这个字符串

  2. 把每个字符都存储给对象，如果对象没有这个属性，就为1，如果存在了就+1

  3. 遍历对象，得到最大值和该字符 注意：在遍历的过程中，把字符串中的每个字符作为对象的属性存储在对象中，对应的属性值是该字符出现的次数

     ```js
     var str='abcoefoxyozzopp';
     var o = {};
     for(var i=0;i<str.lengtj;i++){
         var chars=str.charAt(i);//chars是字符串的每一个字符
         if(o[chars]){
             //o[chars]得到的是属性值
             o[chars]++;
         } else{
             o[chars]=1;
         }
     }
     console.log(o);
     //2.遍历对象
     var max=0;
     var ch="";
     for(var k in o){
         //k得到是属性名
         //o[k]得到的是属性值
         if(o[k]>max){
             max=o[k];
             ch=k;
         }
     }
     console.log(max);
     console.log("最多的字符是"+ch);
     ```

  **字符串操作方法**：

  | 方法名                    | 说明                                                         |
  | ------------------------- | ------------------------------------------------------------ |
  | concat(str1,str2,str3...) | concat()方法用于连接两个或多个字符串。拼接字符串，等效于+，+更常用 |
  | substr(start,length)      | ==从start位置开始（索引号），length取的个数== 重点记住这个   |
  | slice(start,end)          | 从start位置开始，截取到end位置，end取不到（他们两都是索引号） |
  | substring(start,end)      | 从start位置开始，截取到end位置，end取不到  基本和slice相同，但是不接受负值 |

  ```js
  //字符串操作方法
  //1.concat('字符串1','字符串2'...)
  var str='andy';
  console.log(str.concat('red'));//andyred
  
  //2.substr('截取的起始位置','截取几个字符');
  var str1='改革春风吹满地';
  //第一个2是索引号的2 从第几个开始 第二个2 是取几个字符
  console.log(str1.substr(2,2));//春风
  ```

  replace()方法：用于在字符串中用一些字符替换另一些字符，其使用格式如下：

  ```js
  字符串.replacce(被替换的字符串，要替换为的字符串);
  ```

  splace()方法：用于切分字符串，它可以将字符串切分为数组。在切分完毕之后，返回的是一个新数组。

  ```js
  字符串.split("分割字符")
  ```

  ```js
  //1.替换字符 replace('被替换的字符','替换为的字符') 它只会替换第一个字符
  var str="andyandy";
  console.log(str.replace("a","b"));//bndyandy
  //有一个字符串 'abcoefoxyozzopp'  要求把里面所有的 o 替换为 *
  var str1="abcoefoxyozzopp";
  while(str1.indexOf("o")!== -1){
      str1=str1.replace("o","*");
  }
  console.log(str1);//abc*ef*xy*zz*pp
  
  //2.字符转换为数组 split('分隔符')
  //join把数组转换为字符串
  var str2="red,pink,blue";
  console.log(str2.split(","));
  var str3="red&pink&blue";
  console.log(str3.split("&"));
  ```

  **简单数据类型和复杂数据类型**

  简单类型（基本数据类型、值类型）：在存储时变量中存储的是值本身，包括string，number，undefined，null

  复杂数据类型（引用类型）：在存储时变量中存储的仅仅是地址（引用），通过new关键字创建的对象（系统对象、自定义对象），如Object、Array、Date等

  **堆栈**

  - [ ] 堆栈空间分配区别：
  - 栈（操作系统）：由操作系统自动分配释放存放函数的参数值、局部变量的值等。其操作方式类似于数据结构中的栈；
  - 简单数据类型存放到栈里面
  - 堆（操作系统）：存储复杂类型（对象），一般由程序员分配释放，若程序员不释放，由垃圾回收机制回收。

  - [ ] 简单数据类型的存储方式
  - 值类型变量的数据直接存放在变量（栈空间）中

  - [ ] 复杂数据类型的存储方式
  - 引用类型变量（栈空间）里存放的是地址，真正的对象实例存放在堆空间中

  **简单类型传参**

  函数的形参也可以看做是一个变量，当我们把一个值类型变量作为参数传给函数的形参时，其实是把变量在栈空间里的值复制了一份给形参，那么在方法内部对形参做任何修改，都不会影响到的外部变量。

  ```js
  function fn(a){
      a++;
      console.log(a);
  }
  var x = 10;
  fn(x);
  console.log(x);
  ```

  **复杂数据类型传参**

  函数的形参耶可以看做是一个变量，当我们把引用类型变量传给形参时，其实是把变量在栈空间里保存的栈地址复制给了形参，形参和实参其实保存的是同一个堆地址，所以操作的是同一个对象。

  ```js
  function Person(name){
      this.name=name;
  }
  function f1(x){ //x=p
      console.log(x.name);
      x.name="张学友";
      console.log(x.name);
  }
  var p = new Person("刘德华");
  console.log(p.name);//刘德华
  f1(p);//刘德华 张学友
  console.log(p.name);//张学友
  ```

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  
