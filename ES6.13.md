### ES6.13  Set和Map数据结构

1. ##### Set

   `Set`本身是一个构造函数，用来生成 Set 数据结构。 

   ```js
   const s = new Set();
   
   [2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
   
   for (let i of s) {
     console.log(i);
   }
   // 2 3 5 4
   ```

   通过`add()`方法向 Set 结构加入成员，结果表明 Set 结构不会添加重复的值。 

   ```js
   //例一和例二都是Set函数接受数组作为参数，例三是接受类似数组的对象作为参数。
   // 例一
   const set = new Set([1, 2, 3, 4, 4]);
   [...set]
   // [1, 2, 3, 4]
   
   // 例二
   const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
   items.size // 5
   
   // 例三
   const set = new Set(document.querySelectorAll('div'));
   set.size // 56
   
   // 类似于
   const set = new Set();
   document
    .querySelectorAll('div')
    .forEach(div => set.add(div));
   set.size // 56
   ```

   向 Set 加入值的时候，不会发生类型转换 

   ```js
   let set = new Set();
   let a = NaN;
   let b = NaN;
   set.add(a);
   set.add(b);
   set // Set {NaN}
   ```

   ### Set 实例的属性和方法

   Set 结构的实例有以下属性。

   - `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
   - `Set.prototype.size`：返回`Set`实例的成员总数。

   Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。

   - `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身。
   - `Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
   - `Set.prototype.has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
   - `Set.prototype.clear()`：清除所有成员，没有返回值。

   ```js
   s.add(1).add(2).add(2);
   // 注意2被加入了两次
   
   s.size // 2
   
   s.has(1) // true
   s.has(2) // true
   s.has(3) // false
   
   s.delete(2);
   s.has(2) // false
   ```

   对比，看看在判断是否包括一个键上面，`Object`结构和`Set`结构的写法不同。 

   ```js
   // 对象的写法
   const properties = {
     'width': 1,
     'height': 1
   };
   
   if (properties[someName]) {
     // do something
   }
   
   // Set的写法
   const properties = new Set();
   
   properties.add('width');
   properties.add('height');
   
   if (properties.has(someName)) {
     // do something
   }
   ```

   `Array.from`可以将 Set 结构转为数组。 

   ```js
   const items = new Set([1, 2, 3, 4, 5]);
   const array = Array.from(items);
   ```

   ### 遍历操作

   Set 结构的实例有四个遍历方法，可以用于遍历成员。(插入顺序)

   - `Set.prototype.keys()`：返回键名的遍历器
   - `Set.prototype.values()`：返回键值的遍历器
   - `Set.prototype.entries()`：返回键值对的遍历器
   - `Set.prototype.forEach()`：使用回调函数遍历每个成员

   **（1）keys()，values()，entries()** 

   ```js
   let set = new Set(['red', 'green', 'blue']);
   
   for (let item of set.keys()) {
     console.log(item);
   }
   // red
   // green
   // blue
   
   for (let item of set.values()) {
     console.log(item);
   }
   // red
   // green
   // blue
   
   for (let item of set.entries()) {
     console.log(item);
   }
   // ["red", "red"]
   // ["green", "green"]
   // ["blue", "blue"]
   ```

   **（2）forEach()** 

   ```js
   let set = new Set([1, 4, 9]);
   set.forEach((value, key) => console.log(key + ' : ' + value))
   // 1 : 1
   // 4 : 4
   // 9 : 9
   ```

   **（3）遍历的应用** 

   ```js
   //扩展运算符（...）内部使用for...of循环
   let set = new Set(['red', 'green', 'blue']);
   let arr = [...set];
   // ['red', 'green', 'blue']
   
   //扩展运算符和 Set 结构相结合，就可以去除数组的重复成员。
   let arr = [3, 5, 2, 2, 5, 5];
   let unique = [...new Set(arr)];
   // [3, 5, 2]
   
   //数组的map和filter方法也可以间接用于 Set 
   let set = new Set([1, 2, 3]);
   set = new Set([...set].map(x => x * 2));
   // 返回Set结构：{2, 4, 6}
   
   let set = new Set([1, 2, 3, 4, 5]);
   set = new Set([...set].filter(x => (x % 2) == 0));
   // 返回Set结构：{2, 4}
   
   let a = new Set([1, 2, 3]);
   let b = new Set([4, 3, 2]);
   
   // 并集
   let union = new Set([...a, ...b]);
   // Set {1, 2, 3, 4}
   
   // 交集
   let intersect = new Set([...a].filter(x => b.has(x)));
   // set {2, 3}
   
   // （a 相对于 b 的）差集
   let difference = new Set([...a].filter(x => !b.has(x)));
   // Set {1}
   
   //利用原 Set 结构映射出一个新的结构，然后赋值给原来的 Set 结构
   // 方法一
   let set = new Set([1, 2, 3]);
   set = new Set([...set].map(val => val * 2));
   // set的值是2, 4, 6
   
   //Array.from方法
   // 方法二
   let set = new Set([1, 2, 3]);
   set = new Set(Array.from(set, val => val * 2));
   // set的值是2, 4, 6
   ```

2. ##### WeakSet

   ### 含义

   WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。

   首先，WeakSet 的成员只能是对象，而不能是其他类型的值。

   ```js
   const ws = new WeakSet();
   ws.add(1)
   // TypeError: Invalid value used in weak set
   ws.add(Symbol())
   // TypeError: invalid value used in weak set
   ```

   ### 语法

   WeakSet 是一个构造函数，可以使用`new`命令，创建 WeakSet 数据结构。

   ```js
   const ws = new WeakSet();
   
   const a = [[1, 2], [3, 4]];
   const ws = new WeakSet(a);
   // WeakSet {[1, 2], [3, 4]}
   
   const b = [3, 4];
   const ws = new WeakSet(b);
   // Uncaught TypeError: Invalid value used in weak set(…)
   ```

   WeakSet 结构有以下三个方法。

   - **WeakSet.prototype.add(value)**：向 WeakSet 实例添加一个新成员。
   - **WeakSet.prototype.delete(value)**：清除 WeakSet 实例的指定成员。
   - **WeakSet.prototype.has(value)**：返回一个布尔值，表示某个值是否在 WeakSet 实例之中。

   ```js
   const ws = new WeakSet();
   const obj = {};
   const foo = {};
   
   ws.add(window);
   ws.add(obj);
   
   ws.has(window); // true
   ws.has(foo);    // false
   
   ws.delete(window);
   ws.has(window);    // false
   ```

3. ##### Map

   ### 含义和基本用法

   JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构）。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。

   ```js
   //添加成员
   const m = new Map();
   const o = {p: 'Hello World'};
   
   m.set(o, 'content')
   m.get(o) // "content"
   
   m.has(o) // true
   m.delete(o) // true
   m.has(o) // false
   ```

    构造函数执行算法

   ```js
   const items = [
     ['name', '张三'],
     ['title', 'Author']
   ];
   
   const map = new Map();
   
   items.forEach(
     ([key, value]) => map.set(key, value)
   );
   ```

   如果对同一个键多次赋值，后面的值将覆盖前面的值。 

   ```js
   const map = new Map();
   
   map
   .set(1, 'aaa')
   .set(1, 'bbb');
   
   map.get(1) // "bbb"
   ```

   **注意，只有对同一个对象的引用，Map 结构才将其视为同一个键。** 

   ```js
   const map = new Map();
   
   map.set(['a'], 555);
   map.get(['a']) // undefined
   ```

   ### 实例的属性和操作方法

   **（1）size 属性**

   `size`属性返回 Map 结构的成员总数。

   ```js
   const map = new Map();
   map.set('foo', true);
   map.set('bar', false);
   
   map.size // 2
   ```

   **（2）Map.prototype.set(key, value)**

   `set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。

   ```js
   const m = new Map();
   
   m.set('edition', 6)        // 键是字符串
   m.set(262, 'standard')     // 键是数值
   m.set(undefined, 'nah')    // 键是 undefined
   
   //链式写法
   let map = new Map()
     .set(1, 'a')
     .set(2, 'b')
     .set(3, 'c');
   ```

   **（3）Map.prototype.get(key)**

   `get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。

   ```js
   const m = new Map();
   
   const hello = function() {console.log('hello');};
   m.set(hello, 'Hello ES6!') // 键是函数
   
   m.get(hello)  // Hello ES6!
   ```

   **（4）Map.prototype.has(key)**

   `has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。

   ```js
   const m = new Map();
   
   m.set('edition', 6);
   m.set(262, 'standard');
   m.set(undefined, 'nah');
   
   m.has('edition')     // true
   m.has('years')       // false
   m.has(262)           // true
   m.has(undefined)     // true
   ```

   **（5）Map.prototype.delete(key)**

   `delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。

   ```js
   const m = new Map();
   m.set(undefined, 'nah');
   m.has(undefined)     // true
   
   m.delete(undefined)
   m.has(undefined)       // false
   ```

   **（6）Map.prototype.clear()**

   `clear`方法清除所有成员，没有返回值。

   ```js
   let map = new Map();
   map.set('foo', true);
   map.set('bar', false);
   
   map.size // 2
   map.clear()
   map.size // 0
   ```

   ### 遍历方法

   Map 结构原生提供三个遍历器生成函数和一个遍历方法。

   - `Map.prototype.keys()`：返回键名的遍历器。
   - `Map.prototype.values()`：返回键值的遍历器。
   - `Map.prototype.entries()`：返回所有成员的遍历器。
   - `Map.prototype.forEach()`：遍历 Map 的所有成员。

   **需要特别注意的是，Map 的遍历顺序就是插入顺序。**

   ```js
   const map = new Map([
     ['F', 'no'],
     ['T',  'yes'],
   ]);
   
   for (let key of map.keys()) {
     console.log(key);
   }
   // "F"
   // "T"
   
   for (let value of map.values()) {
     console.log(value);
   }
   // "no"
   // "yes"
   
   for (let item of map.entries()) {
     console.log(item[0], item[1]);
   }
   // "F" "no"
   // "T" "yes"
   
   // 或者
   for (let [key, value] of map.entries()) {
     console.log(key, value);
   }
   // "F" "no"
   // "T" "yes"
   
   // 等同于使用map.entries()
   for (let [key, value] of map) {
     console.log(key, value);
   }
   // "F" "no"
   // "T" "yes"
   ```

   ### 与其他数据结构的互相转换

   **（1）Map 转为数组**

   ```js
   const myMap = new Map()
     .set(true, 7)
     .set({foo: 3}, ['abc']);
   [...myMap]
   // [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
   ```

   **（2）数组 转为 Map**

   将数组传入 Map 构造函数，就可以转为 Map。

   ```js
   new Map([
     [true, 7],
     [{foo: 3}, ['abc']]
   ])
   // Map {
   //   true => 7,
   //   Object {foo: 3} => ['abc']
   // }
   ```

   **（3）Map 转为对象**

   如果所有 Map 的键都是字符串，它可以无损地转为对象。

   ```js
   function strMapToObj(strMap) {
     let obj = Object.create(null);
     for (let [k,v] of strMap) {
       obj[k] = v;
     }
     return obj;
   }
   
   const myMap = new Map()
     .set('yes', true)
     .set('no', false);
   strMapToObj(myMap)
   // { yes: true, no: false }
   ```

   **（4）对象转为 Map**

   对象转为 Map 可以通过`Object.entries()`。

   ```js
   let obj = {"a":1, "b":2};
   let map = new Map(Object.entries(obj));
   ```

   **（5）Map 转为 JSON**

   Map 转为 JSON 要区分两种情况。一种情况是，Map 的键名都是字符串，这时可以选择转为对象 JSON。

   ```js
   function strMapToJson(strMap) {
     return JSON.stringify(strMapToObj(strMap));
   }
   
   let myMap = new Map().set('yes', true).set('no', false);
   strMapToJson(myMap)
   // '{"yes":true,"no":false}'
   ```

   另一种情况是，Map 的键名有非字符串，这时可以选择转为数组 JSON。 

   ```js
   function mapToArrayJson(map) {
     return JSON.stringify([...map]);
   }
   
   let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
   mapToArrayJson(myMap)
   // '[[true,7],[{"foo":3},["abc"]]]'
   ```

   **（6）JSON 转为 Map**

   JSON 转为 Map，正常情况下，所有键名都是字符串。

   ```js
   function jsonToStrMap(jsonStr) {
     return objToStrMap(JSON.parse(jsonStr));
   }
   
   jsonToStrMap('{"yes": true, "no": false}')
   // Map {'yes' => true, 'no' => false}
   ```

   有一种特殊情况，整个 JSON 就是一个数组，且每个数组成员本身，又是一个有两个成员的数组。 

   ```js
   function jsonToMap(jsonStr) {
     return new Map(JSON.parse(jsonStr));
   }
   
   jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
   // Map {true => 7, Object {foo: 3} => ['abc']}
   ```

4. ##### WeakMap

   ### 含义

   `WeakMap`结构与`Map`结构类似，也是用于生成键值对的集合。

   ```js
   // WeakMap 可以使用 set 方法添加成员
   const wm1 = new WeakMap();
   const key = {foo: 1};
   wm1.set(key, 2);
   wm1.get(key) // 2
   
   // WeakMap 也可以接受一个数组，
   // 作为构造函数的参数
   const k1 = [1, 2, 3];
   const k2 = [4, 5, 6];
   const wm2 = new WeakMap([[k1, 'foo'], [k2, 'bar']]);
   wm2.get(k2) // "bar"
   ```

   `WeakMap`与`Map`的区别 :

   - [ ] `WeakMap`只接受对象作为键名（`null`除外），不接受其他类型的值作为键名。 

     ```js
     const map = new WeakMap();
     map.set(1, 2)
     // TypeError: 1 is not an object!
     map.set(Symbol(), 2)
     // TypeError: Invalid value used as weak map key
     map.set(null, 2)
     // TypeError: Invalid value used as weak map key
     ```

   - [ ] `WeakMap`的键名所指向的对象，不计入垃圾回收机制。 

     ```js
     const e1 = document.getElementById('foo');
     const e2 = document.getElementById('bar');
     const arr = [
       [e1, 'foo 元素'],
       [e2, 'bar 元素'],
     ];
     ```

   WeakMap 弱引用的只是键名，而不是键值。键值依然是正常引用。 

   ```js
   const wm = new WeakMap();
   let key = {};
   let obj = {foo: 1};
   
   wm.set(key, obj);
   obj = null;
   wm.get(key)
   // Object {foo: 1}
   ```

   ### WeakMap 的语法

   ```js
   const wm = new WeakMap();
   
   // size、forEach、clear 方法都不存在
   wm.size // undefined
   wm.forEach // undefined
   wm.clear // undefined
   ```

   ### WeakMap 的示例

   首先，打开 Node 命令行。 

   ```js
   $ node --expose-gc//--expose-gc参数表示允许手动执行垃圾回收机制。
   ```

   然后，执行下面的代码。 

   ```js
   // 手动执行一次垃圾回收，保证获取的内存使用状态准确
   > global.gc();
   undefined
   
   // 查看内存占用的初始状态，heapUsed 为 4M 左右
   > process.memoryUsage();
   { rss: 21106688,
     heapTotal: 7376896,
     heapUsed: 4153936,
     external: 9059 }
   
   > let wm = new WeakMap();
   undefined
   
   // 新建一个变量 key，指向一个 5*1024*1024 的数组
   > let key = new Array(5 * 1024 * 1024);
   undefined
   
   // 设置 WeakMap 实例的键名，也指向 key 数组
   // 这时，key 数组实际被引用了两次，
   // 变量 key 引用一次，WeakMap 的键名引用了第二次
   // 但是，WeakMap 是弱引用，对于引擎来说，引用计数还是1
   > wm.set(key, 1);
   WeakMap {}
   
   > global.gc();
   undefined
   
   // 这时内存占用 heapUsed 增加到 45M 了
   > process.memoryUsage();
   { rss: 67538944,
     heapTotal: 7376896,
     heapUsed: 45782816,
     external: 8945 }
   
   // 清除变量 key 对数组的引用，
   // 但没有手动清除 WeakMap 实例的键名对数组的引用
   > key = null;
   null
   
   // 再次执行垃圾回收
   > global.gc();
   undefined
   
   // 内存占用 heapUsed 变回 4M 左右，
   // 可以看到 WeakMap 的键名引用没有阻止 gc 对内存的回收
   > process.memoryUsage();
   { rss: 20639744,
     heapTotal: 8425472,
     heapUsed: 3979792,
     external: 8956 }
   ```

   ### WeakMap 的用途

    DOM 节点作为键名 

   ```js
   let myWeakmap = new WeakMap();
   
   myWeakmap.set(
     document.getElementById('logo'),
     {timesClicked: 0})
   ;
   
   document.getElementById('logo').addEventListener('click', function() {
     let logoData = myWeakmap.get(document.getElementById('logo'));
     logoData.timesClicked++;
   }, false);
   ```

   部署私有属性 

   ```js
   const _counter = new WeakMap();
   const _action = new WeakMap();
   
   class Countdown {
     constructor(counter, action) {
       _counter.set(this, counter);
       _action.set(this, action);
     }
     dec() {
       let counter = _counter.get(this);
       if (counter < 1) return;
       counter--;
       _counter.set(this, counter);
       if (counter === 0) {
         _action.get(this)();
       }
     }
   }
   
   const c = new Countdown(2, () => console.log('DONE'));
   
   c.dec()
   c.dec()
   // DONE
   ```

   `Countdown`类的两个内部属性`_counter`和`_action`，是实例的弱引用，所以如果删除实例，它们也就随之消失，不会造成内存泄漏。 