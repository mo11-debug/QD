### ES6.3变量的解构赋值

1. 数组的解构赋值

   解构：按照一定模式，从数组和对象中提取值，对变量进行赋值。

   ```js
   let [a, b, c] = [1, 2, 3]//从数组中按照对应位置对变量赋值
   
   //模式匹配
   let [foo, [[bar], baz]] = [1, [[2], 3]];
   foo//1
   bar//2
   baz//3
   
   let[, , third] = ["foo", "bar", "baz"];
   third//"baz"
   
   let[x, , y] = [1, 2, 3];
   x//1
   y//3
   
   let[head, ...tail] = [1, 2, 3, 4];
   head//1
   tail//[2,3,4]
   
   let[x, y, ...z] = ['a'];
   x//"a"
   y//undefined
   z//[]
   
   //解构不成功，变量的值会等于undefined
   let[foo] = [];
   let[bar, foo] = [1];
   ```

   不完全解构：等号左边只匹配部分的等号右边的数组。

   ```js
   let[x, y] = [1, 2, 3];
   x//1
   y//2
   
   let[a, [b], d]= [1, [2, 3], 4];
   a//1
   b//2
   c//4
   
   //报错
   let[foo] = 1;
   let[foo] = false;
   let[foo] = NaN;
   let[foo] = undefined;
   let[foo] = null;
   let[foo] = {};
   ```

   只要某种数据结构具有Iterator接口，都可以采用数组形式的解构赋值。

   ```js
   //Set结构
   let[x, y, z] = new Set(['a', 'b', 'c']);
   x//"a"
   
   //fibs:Generator函数
   function* fibs(){
       let a = 0;
       let b = 1;
       while(true){
           yield a;
           [a, b] = [b, a + b];
       }
   }
   
   let[first, second, third, fourth, fifth, sixth] = fibs();
   sixth//5
   ```

   解构赋值允许指定默认值

   ```js
   let [foo = true] = [];
   foo//true
   
   let[x, y = 'b'] = ['a'];//x='a',y='b'
   let[x, y = 'b'] = ['a', undefined];//x='a',y='b'
   
   //当使用严格相等运算符（===）判断一个位置是否有效，只有当数组成员严格等于undefined，默认值才会生效
   let[x = 1] = [undefined];x//1
   let[x = 1] = [null];x//null
   //当数组成员是null，默认值不会生效
   
   function f(){
       console.log('aaa');
   }
   let [x = f()] = [1];//惰性求值，x能取到值，函数f不会执行
   ```

   默认值可以引用解构赋值的其他变量，但变量必须声明

   ```js
   let[x = 1, y = x] = [];//x=1;y=1
   let[x = 1, y = x] = [2];//x=2;y=2
   let[x = 1, y = x] = [1, 2];//x=1;y=2
   let[x = y, y = 1] = [];//y没有定义，报错
   ```

2. 对象的解构赋值

   **对象解构和数组不同：数组元素按次序排列，变量取值由位置决定，而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。**

   ```js
   let { bar, foo } = { foo: 'aaa', bar: 'bbb' };
   foo // "aaa"
   bar // "bbb"
   
   let { baz } = { foo: 'aaa', bar: 'bbb' };
   baz // undefined
   //如果解构失败，变量的值等于undefined。
   
   //嵌套
   //loc属性
   let obj = {
     p: [
       'Hello',
       { y: 'World' }
     ]
   };
   let { p: [x, { y }] } = obj;
   x // "Hello"
   y // "World"
   
   //start属性
   let obj = {
     p: [
       'Hello',
       { y: 'World' }
     ]
   };
   let { p, p: [x, { y }] } = obj;
   x // "Hello"
   y // "World"
   p // ["Hello", {y: "World"}]
   
   //line属性
   const node = {
     loc: {
       start: {
         line: 1,
         column: 5
       }
     }
   };
   let { loc, loc: { start }, loc: { start: { line }} } = node;
   line // 1
   loc  // Object {start: Object}
   start // Object {line: 1, column: 5}
   //只有line是变量，loc和start是模式
   
   //嵌套赋值
   let obj = {};
   let arr = [];
   ({ foo: obj.prop, bar: arr[0] } = { foo: 123, bar: true });
   obj // {prop:123}
   arr // [true]
   
   // 子对象的父属性不存在，报错
   let {foo: {bar}} = {baz: 'baz'};
   
   //继承
   const obj1 = {};
   const obj2 = { foo: 'bar' };
   Object.setPrototypeOf(obj1, obj2);
   const { foo } = obj1;
   foo // "bar"
   ```

   默认值：生效条件，对象属性值严格等于undefined。

   ```js
   var {x = 3} = {x: undefined};
   x // 3
   
   var {x = 3} = {x: null};
   x // null
   ```

   ##### 注意

   - 如果要将一个已声明的变量用于解构赋值，需要注意括号的使用

     ```js
     // 错误的写法
     let x;
     {x} = {x: 1};
     // SyntaxError: syntax error
     
     // 正确的写法
     let x;
     ({x} = {x: 1});
     ```

   - 解构赋值允许等号左边的模式之中，不放置任何变量名。

     ```js
     ({} = [true, false]);
     ({} = 'abc');
     ({} = []);
     ```

   - 以对数组进行对象属性的解构 

     ```js
     let arr = [1, 2, 3];
     let {0 : first, [arr.length - 1] : last} = arr;
     first // 1
     last // 3
     //属性名表达式
     ```

3. 字符串的解构赋值

   字符串被转换成了类似数组的对象，有length属性，可以解构赋值。

   ```js
   const[a, b, c, d, e] = 'hello'
   a//"h"
   b//"e"
   c//"e"
   d//"l"
   e//"o"
   
   let{legth: len} = 'hello';
   len//5
   ```

4. 数值和布尔值的解构赋值

   解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。但是undefined和null无法转为对象，所以解构会报错.

   ```js
   let{toString: s} = 123;
   s === Number.prototype.toString//true
   
   let{toString: s} = true;
   s === Boolean.prototype.toString//true
   
   let{prop: x} = undefined;//TypeError
   let{prop: y} = null;//TypeError
   ```

5. 函数参数的结构赋值

   ```js
   function add([x, y]){
       return x + y;
   }
   add([1, 2]);//3
   
   [[1, 2], [3, 4]].map(([a, b]) => a + b);//[3,7]
   
   function move({x = 0, y = 0} = {}){
       return [x, y];
   }
   move({x: 3, y: 8});//[3,8]
   move({x: 3});//[3,0]
   move({});//[0,0]
   move();//[0,0]
   //如果解构失败，x和y等于默认值
   
   [1, undefined, 3].map((x = 'yes') => x);//[1,'yes',3]
   //undefined会触发函数参数默认值
   ```

6. 圆括号问题

   - 不能使用圆括号情况

     ```js
     //(1)变量声明
     //全部报错
     let[(a)] = [1];
     
     let{x: (c)} = {};
     let({x: c}) = {};
     let{(x: c)} = {};
     let{(x): c} = {};
     
     let{ o: ({ p: p}) } = { o: { p: 2} }; 
     
     //(2)参数函数
     //报错
     function f([(z)]) { return z; }
     function f([z,(x)]){ return x; }
     
     //(3)赋值语句
     //全部报错
     ({p: a}) = { p: 42 };
     ([a]) = [5];
     ```

   - 可以使用圆括号情况

     ```js
     [(b)] = [3];//正确
     ({ p: (d)} = {});//正确
     [(parseInt.prop)] = [3];//正确
     ```

7. 用途

   - 交换变量的值

     ```js
     let x = 1;
     let y = 2;
     
     [x, y] = [y, x];
     ```

   - 从函数返回多个值

     ```js
     //返回一个数组
     function example(){
         return [1, 2, 3];
     }
     let[a, b, c] = example();
     
     //返回一个对象
     function example(){
         return{
             foo:1,
             bar:2
         };
     }
     let{ foo, bar } = example();
     ```

   - 函数参数的定义

     ```js
     //参数是一组有次序的值
     function f([x, y, z]){ ... }
     f([1, 2, 3]);
                           
     //参数是一组无次序的值
     function f([x, y, z]){ ... }
     f({z: 3, y: 2, x: 1});
     ```

   - 提取JSON数据

     ```js
     let jsonData = {
         id: 42,
         status: "OK",
         data: [867, 5309]
     };
     let{ id, status, data: number } = jsonData;
     console.log(id, status, number);
     //42,"OK",[867, 5309]
     ```

   - 函数参数的默认值

     ```js
     jQuery.ajax = function(url, {
         async = true,
         beforeSend = function(){},
         cache = true,
         complete = function(){},
         crossDomain = false,
         global = true,
         // ... more config
     } = {}){
         // ... do stuff
     };
     ```

   - 遍历Map结构

     ```js
     const map = new Map();
     map.set('first', 'hello');
     map.set('second', 'world');
     for(let[key, value] of map){
         console.log(key + "is" + value);
     }//first is hello,second is world
     
     //获取键名
     for(let[key] of map){
         //...
     }
     //获取键值
     for(let[,value] of map){
         //...
     }
     ```

   - 输入模块的指定方法

     ```js
     const{SourceMapConsumer, SourceNode} = require("Source-map");
     ```

     

