### ES6.9数组的扩展

1. 扩展运算符

   扩展运算符（spread）是三个点（`...`）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。 主要用于函数调用。

   ```js
   console.log(...[1, 2, 3])
   // 1 2 3
   
   console.log(1, ...[2, 3, 4], 5)
   // 1 2 3 4 5
   
   [...document.querySelectorAll('div')]
   // [<div>, <div>, <div>]
   
   //函数调用
   function push(array, ...items) {
     array.push(...items);
   }
   
   function add(x, y) {
     return x + y;
   }
   
   const numbers = [4, 38];
   add(...numbers) // 42
   ```

   扩展运算符与正常的函数参数可以结合使用 

   ```
   function f(v, w, x, y, z) { }
   const args = [0, 1];
   f(-1, ...args, 2, ...[3]);
   ```

   注意，只有函数调用时，扩展运算符才可以放在圆括号中，否则会报错。 

   ```js
   (...[1, 2])
   // Uncaught SyntaxError: Unexpected number
   
   console.log((...[1, 2]))
   // Uncaught SyntaxError: Unexpected number
   
   console.log(...[1, 2])
   // 1 2
   ```

   ##### 替代函数的apply方法

   ```js
   // ES5 的写法
   function f(x, y, z) {
     // ...
   }
   var args = [0, 1, 2];
   f.apply(null, args);
   
   // ES6的写法
   function f(x, y, z) {
     // ...
   }
   let args = [0, 1, 2];
   f(...args);
   
   // ES5 的写法
   Math.max.apply(null, [14, 3, 77])
   
   // ES6 的写法
   Math.max(...[14, 3, 77])
   
   // 等同于
   Math.max(14, 3, 77);
   ```

   扩展运算符的应用

   - 复制数组

     ```js
     //只复制了指针
     const a1 = [1, 2];
     const a2 = a1;
     
     a2[0] = 2;
     a1 // [2, 2]
     
     //复制整个数组
     const a1 = [1, 2];
     // 写法一
     const a2 = [...a1];
     // 写法二
     const [...a2] = a1;
     ```

   - 合并数组

     ```js
     const arr1 = ['a', 'b'];
     const arr2 = ['c'];
     const arr3 = ['d', 'e'];
     
     // ES5 的合并数组
     arr1.concat(arr2, arr3);
     // [ 'a', 'b', 'c', 'd', 'e' ]
     
     // ES6 的合并数组
     [...arr1, ...arr2, ...arr3]
     // [ 'a', 'b', 'c', 'd', 'e' ]
     ```

   - 与解构赋值结合

     ```js
     // ES5
     a = list[0], rest = list.slice(1)
     // ES6
     [a, ...rest] = list
     
     //如果将扩展运算符用于数组赋值，只能放在参数的最后一位
     const [...butLast, last] = [1, 2, 3, 4, 5];
     // 报错
     const [first, ...middle, last] = [1, 2, 3, 4, 5];
     // 报错
     ```

   - 字符串

     ```js
     [...'hello']
     // [ "h", "e", "l", "l", "o" ]
     ```

   - 实现； Iterator接口的对象

     ```js
     //返回NodeList对象
     let nodeList = document.querySelectorAll('div');
     let array = [...nodeList];
     
     //返回真正数组
     Number.prototype[Symbol.iterator] = function*() {
       let i = 0;
       let num = this.valueOf();
       while (i < num) {
         yield i++;
       }
     }
     console.log([...5]) // [0, 1, 2, 3, 4]
     ```

   - Map和Set结构，Generator函数

     ```js
     //Map结构
     let map = new Map([
       [1, 'one'],
       [2, 'two'],
       [3, 'three'],
     ]);
     
     let arr = [...map.keys()]; // [1, 2, 3]
     
     //Generator 函数
     const go = function*(){
       yield 1;
       yield 2;
       yield 3;
     };
     
     [...go()] // [1, 2, 3]
     ```

2. Array.from()

   - 类似数组对象

     ```js
     let arrayLike = {
         '0': 'a',
         '1': 'b',
         '2': 'c',
         length: 3
     };
     
     // ES5的写法
     var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']
     
     // ES6的写法
     let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
     ```

   - 可遍历对象

     只要是部署了 Iterator 接口的数据结构，`Array.from`都能将其转为数组。 

     ```js
     Array.from('hello')
     // ['h', 'e', 'l', 'l', 'o']
     
     let namesSet = new Set(['a', 'b'])
     Array.from(namesSet) // ['a', 'b']
     ```

     `Array.from`还可以接受第二个参数，作用类似于数组的`map`方法 

     ```js
     Array.from(arrayLike, x => x * x);
     // 等同于
     Array.from(arrayLike).map(x => x * x);
     Array.from([1, 2, 3], (x) => x * x)
     // [1, 4, 9]
     
     //将字符串转为数组，然后返回字符串的长度
     function countSymbols(string) {
       return Array.from(string).length;
     }
     ```

3. Array.of()

   用于将一组值，转换为数组 。主要目的，是弥补数组构造函数`Array()`的不足 。

   - `Array.of`基本上可以用来替代`Array()`或`new Array()`，并且不存在由于参数不同而导致的重载。 
   - `Array.of`总是返回参数值组成的数组。如果没有参数，就返回一个空数组。 

   ```js
   Array.of(3, 11, 8) // [3,11,8]
   Array.of(3) // [3]
   Array.of(3).length // 1
   
   //模拟实现
   function ArrayOf(){
     return [].slice.call(arguments);
   }
   ```

4. 数组实例的copyWithin()

   会覆盖原有成员，修改原数组

   三个参数：

   - target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
   - start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
   - end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算。

   ```js
   Array.prototype.copyWithin(target, start = 0, end = this.length)
   
   [1, 2, 3, 4, 5].copyWithin(0, 3)
   // [4, 5, 3, 4, 5]
   ```

5. 数组实例的find()和findIndex()

   find:用于找出第一个符合条件的数组成员 ,回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。 

   ```js
   [1, 5, 10, 15].find(function(value, index, arr) {
     return value > 9;
   }) // 10
   ```

   `findIndex`方法的用法与`find`方法非常类似，返回第一个符合条件的数组成员的位置 .

   ```js
   [1, 5, 10, 15].findIndex(function(value, index, arr) {
     return value > 9;
   }) // 2
   ```

6. 数组实例的fill()

   `fill`方法使用给定值，填充一个数组 .

   ```js
   //用于空数组的初始化
   ['a', 'b', 'c'].fill(7)
   // [7, 7, 7]
   new Array(3).fill(7)
   // [7, 7, 7]
   
   //如果填充的类型为对象，那么被赋值的是同一个内存地址的对象
   let arr = new Array(3).fill({name: "Mike"});
   arr[0].name = "Ben";
   arr
   // [{name: "Ben"}, {name: "Ben"}, {name: "Ben"}]
   let arr = new Array(3).fill([]);
   arr[0].push(5);
   arr
   // [[5], [5], [5]]
   ```

7. 数组实例的entries(),keys()和values()

   用于遍历数组 

   ```js
   for (let index of ['a', 'b'].keys()) {
     console.log(index);
   }
   // 0
   // 1
   
   for (let elem of ['a', 'b'].values()) {
     console.log(elem);
   }
   // 'a'
   // 'b'
   
   for (let [index, elem] of ['a', 'b'].entries()) {
     console.log(index, elem);
   }
   // 0 "a"
   // 1 "b"
   ```

8. 数组实例的includes()

   返回一个布尔值，表示某个数组是否包含给定的值，与字符串的`includes`方法类似。 

   ```js
   [1, 2, 3].includes(2)     // true
   [1, 2, 3].includes(4)     // false
   [1, 2, NaN].includes(NaN) // true
   ```

   检查环境是否支持

   ```js
   const contains = (() =>
     Array.prototype.includes
       ? (arr, value) => arr.includes(value)
       : (arr, value) => arr.some(el => el === value)
   )();
   contains(['foo', 'bar'], 'baz'); // => false
   ```

   - Map 结构的`has`方法，是用来查找键名的，比如`Map.prototype.has(key)`、`WeakMap.prototype.has(key)`、`Reflect.has(target, propertyKey)`。
   - Set 结构的`has`方法，是用来查找值的，比如`Set.prototype.has(value)`、`WeakSet.prototype.has(value)`。

9. 数组实例的flat(),flatMap()

   flat()用于“拉平”嵌套数组返回一维数组。如果有空位会跳过。该方法返回一个新数组，对原数据没有影响 。

   ```js
   [1, 2, [3, [4, 5]]].flat()
   // [1, 2, 3, [4, 5]]
   
   [1, 2, [3, [4, 5]]].flat(2)
   // [1, 2, 3, 4, 5]
   ```

   flatMap()和flat()类似，但是它只能展开一层数组。 

   ```js
   // 相当于 [[[2]], [[4]], [[6]], [[8]]].flat()
   [1, 2, 3, 4].flatMap(x => [[x * 2]])
   // [[2], [4], [6], [8]]
   
   arr.flatMap(function callback(currentValue[, index[, array]]) {
     // ...
   }[, thisArg])
   ```

10. 数组的空位

    ###### ES5:

    - `forEach()`, `filter()`, `reduce()`, `every()` 和`some()`都会跳过空位。
    - `map()`会跳过空位，但会保留这个值
    - `join()`和`toString()`会将空位视为`undefined`，而`undefined`和`null`会被处理成空字符串。

    ```js
    // forEach方法
    [,'a'].forEach((x,i) => console.log(i)); // 1
    
    // filter方法
    ['a',,'b'].filter(x => true) // ['a','b']
    
    // every方法
    [,'a'].every(x => x==='a') // true
    
    // reduce方法
    [1,,2].reduce((x,y) => x+y) // 3
    
    // some方法
    [,'a'].some(x => x !== 'a') // false
    
    // map方法
    [,'a'].map(x => 1) // [,1]
    
    // join方法
    [,'a',undefined,null].join('#') // "#a##"
    
    // toString方法
    [,'a',undefined,null].toString() // ",a,,"
    ```

    ###### ES6:明确将空位转为`undefined` 

    ```js
    Array.from(['a',,'b'])
    // [ "a", undefined, "b" ]
    
    [...['a',,'b']]
    // [ "a", undefined, "b" ]
    
    //copyWithin()
    [,'a','b',,].copyWithin(2,0) // [,"a",,"a"]
    
    //fill()
    new Array(3).fill('a') // ["a","a","a"]
    
    //for...of
    let arr = [, ,];
    for (let i of arr) {
      console.log(1);
    }
    // 1
    // 1
    
    // entries()
    [...[,'a'].entries()] // [[0,undefined], [1,"a"]]
    
    // keys()
    [...[,'a'].keys()] // [0,1]
    
    // values()
    [...[,'a'].values()] // [undefined,"a"]
    
    // find()
    [,'a'].find(x => true) // undefined
    
    // findIndex()
    [,'a'].findIndex(x => true) // 0
    ```

11. Array.prototype.sort()的排序稳定性

    指的是排序关键字相同的项目，排序前后的顺序不变。 

    ```js
    const arr = [
      'peach',
      'straw',
      'apple',
      'spork'
    ];
    
    const stableSorting = (s1, s2) => {
      if (s1[0] < s2[0]) return -1;
      return 1;
    };
    
    arr.sort(stableSorting)
    // ["apple", "peach", "straw", "spork"]
    ```

    排序算法`unstableSorting`是不稳定的 

    ```js
    const unstableSorting = (s1, s2) => {
      if (s1[0] <= s2[0]) return -1;
      return 1;
    };
    
    arr.sort(unstableSorting)
    // ["apple", "peach", "spork", "straw"]
    ```

    

    