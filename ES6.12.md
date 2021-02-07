### ES6.12 Symbol

1. 概述

   一种新的原始数据类型`Symbol`，表示独一无二的值。 

   注意，`Symbol`函数前不能使用`new`命令，否则会报错。 

   如果 Symbol 的参数是一个对象，就会调用该对象的`toString`方法，将其转为字符串，然后才生成一个 Symbol 值。

   ```
   const obj = {
     toString() {
       return 'abc';
     }
   };
   const sym = Symbol(obj);
   sym // Symbol(abc)
   ```

   注意，`Symbol`函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的`Symbol`函数的返回值是不相等的。

   ```
   // 没有参数的情况
   let s1 = Symbol();
   let s2 = Symbol();
   
   s1 === s2 // false
   
   // 有参数的情况
   let s1 = Symbol('foo');
   let s2 = Symbol('foo');
   
   s1 === s2 // false
   ```

   Symbol 值不能与其他类型的值进行运算，会报错。

   ```
   let sym = Symbol('My symbol');
   
   "your symbol is " + sym
   // TypeError: can't convert symbol to string
   `your symbol is ${sym}`
   // TypeError: can't convert symbol to string
   ```

   但是，Symbol 值可以显式转为字符串。

   ```
   let sym = Symbol('My symbol');
   
   String(sym) // 'Symbol(My symbol)'
   sym.toString() // 'Symbol(My symbol)'
   ```

   另外，Symbol 值也可以转为布尔值，但是不能转为数值。

   ```
   let sym = Symbol();
   Boolean(sym) // true
   !sym  // false
   
   if (sym) {
     // ...
   }
   
   Number(sym) // TypeError
   sym + 2 // TypeError
   ```

2. Symbol.prototype.description

   创建 Symbol 的时候，可以添加一个描述。

   ```
   const sym = Symbol('foo');
   ```

   `sym`的描述就是字符串`foo`。

   但是，读取这个描述需要将 Symbol 显式转为字符串。

   ```
   const sym = Symbol('foo');
   
   String(sym) // "Symbol(foo)"
   sym.toString() // "Symbol(foo)"
   ```

   一个实例属性`description`，直接返回 Symbol 的描述。

   ```
   const sym = Symbol('foo');
   
   sym.description // "foo"
   ```

3. 作为属性名的Symbol

   ```
   let mySymbol = Symbol();
   
   // 第一种写法
   let a = {};
   a[mySymbol] = 'Hello!';
   
   // 第二种写法
   let a = {
     [mySymbol]: 'Hello!'
   };
   
   // 第三种写法
   let a = {};
   Object.defineProperty(a, mySymbol, { value: 'Hello!' });
   
   // 以上写法都得到同样结果
   a[mySymbol] // "Hello!"
   ```

   通过方括号结构和`Object.defineProperty`，将对象的属性名指定为一个 Symbol 值。

   ```
   const mySymbol = Symbol();
   const a = {};
   
   a.mySymbol = 'Hello!';
   a[mySymbol] // undefined
   a['mySymbol'] // "Hello!"
   ```

   注意，Symbol 值作为对象属性名时，不能用点运算符。

   ```
   let s = Symbol();
   
   let obj = {
     [s]: function (arg) { ... }
   };
   
   obj[s](123);
   ```

   在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。 

   ```
   const log = {};
   
   log.levels = {
     DEBUG: Symbol('debug'),
     INFO: Symbol('info'),
     WARN: Symbol('warn')
   };
   console.log(log.levels.DEBUG, 'debug message');
   console.log(log.levels.INFO, 'info message');
   ```

   Symbol 类型还可以用于定义一组常量，保证这组常量的值都是不相等的。

   **Symbol 值作为属性名时，该属性还是公开属性，不是私有属性。** 

4. 实例：消除魔术字符串

   魔术字符串指的是，在代码之中多次出现、与代码形成强耦合的某一个具体的字符串或者数值。 

   常用的消除魔术字符串的方法，就是把它写成一个变量。

   ```
   const shapeType = {
     triangle: 'Triangle'
   };
   
   function getArea(shape, options) {
     let area = 0;
     switch (shape) {
       case shapeType.triangle:
         area = .5 * options.width * options.height;
         break;
     }
     return area;
   }
   
   getArea(shapeType.triangle, { width: 100, height: 100 });
   ```

5. 属性名的遍历

   `Object.getOwnPropertySymbols()`方法，可以获取所有 Symbol 属性名。 

   ```
   const obj = {};
   let a = Symbol('a');
   let b = Symbol('b');
   
   obj[a] = 'Hello';
   obj[b] = 'World';
   
   const objectSymbols = Object.getOwnPropertySymbols(obj);
   
   objectSymbols
   // [Symbol(a), Symbol(b)]
   ```

   `Object.getOwnPropertySymbols()`方法与`for...in`循环、`Object.getOwnPropertyNames`方法进行对比。

   ```
   const obj = {};
   const foo = Symbol('foo');
   
   obj[foo] = 'bar';
   
   for (let i in obj) {
     console.log(i); // 无输出
   }
   
   Object.getOwnPropertyNames(obj) // []
   Object.getOwnPropertySymbols(obj) // [Symbol(foo)]
   ```

   `Reflect.ownKeys()`方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。

   ```
   let obj = {
     [Symbol('my_key')]: 1,
     enum: 2,
     nonEnum: 3
   };
   
   Reflect.ownKeys(obj)
   //  ["enum", "nonEnum", Symbol(my_key)]
   ```

6. Symbol.for(), Symbol.keyFor()

   `Symbol.keyFor()`方法返回一个已登记的 Symbol 类型值的`key`。

   ```
   let s1 = Symbol.for("foo");
   Symbol.keyFor(s1) // "foo"
   
   let s2 = Symbol("foo");
   Symbol.keyFor(s2) // undefined
   ```

   上面代码中，变量`s2`属于未登记的 Symbol 值，所以返回`undefined`。

   注意，`Symbol.for()`为 Symbol 值登记的名字，是全局环境的，不管有没有在全局环境运行。

   ```
   function foo() {
     return Symbol.for('bar');
   }
   
   const x = foo();
   const y = Symbol.for('bar');
   console.log(x === y); // true
   ```

   上面代码中，`Symbol.for('bar')`是函数内部运行的，但是生成的 Symbol 值是登记在全局环境的。

7. 实例：模块的Singleton模式

   Singleton 模式指的是调用一个类，任何时候返回的都是同一个实例。 

   保证每次执行这个模块文件，返回的都是同一个实例 。

   ```
   // mod.js
   const FOO_KEY = Symbol.for('foo');
   
   function A() {
     this.foo = 'hello';
   }
   
   if (!global[FOO_KEY]) {
     global[FOO_KEY] = new A();
   }
   
   module.exports = global[FOO_KEY];
   ```

   上面代码中，可以保证`global[FOO_KEY]`不会被无意间覆盖，但还是可以被改写。

   ```
   global[Symbol.for('foo')] = { foo: 'world' };
   
   const a = require('./mod.js');
   ```

   如果键名使用`Symbol`方法生成，那么外部将无法引用这个值，当然也就无法改写。

   ```
   // mod.js
   const FOO_KEY = Symbol('foo');
   
   // 后面代码相同 ……
   ```

8. 内置的Symbol值

   ### Symbol.hasInstance

   对象的`Symbol.hasInstance`属性，指向一个内部方法。当其他对象使用`instanceof`运算符，判断是否为该对象的实例时，会调用这个方法。比如，`foo instanceof Foo`在语言内部，实际调用的是`Foo[Symbol.hasInstance](foo)`。

   ```
   class MyClass {
     [Symbol.hasInstance](foo) {
       return foo instanceof Array;
     }
   }
   
   [1, 2, 3] instanceof new MyClass() // true
   ```

   `MyClass`是一个类，`new MyClass()`会返回一个实例。该实例的`Symbol.hasInstance`方法，会在进行`instanceof`运算时自动调用，判断左侧的运算子是否为`Array`的实例。 

   ### Symbol.isConcatSpreadable

   对象的`Symbol.isConcatSpreadable`属性等于一个布尔值，表示该对象用于`Array.prototype.concat()`时，是否可以展开。

   ```
   let arr1 = ['c', 'd'];
   ['a', 'b'].concat(arr1, 'e') // ['a', 'b', 'c', 'd', 'e']
   arr1[Symbol.isConcatSpreadable] // undefined
   
   let arr2 = ['c', 'd'];
   arr2[Symbol.isConcatSpreadable] = false;
   ['a', 'b'].concat(arr2, 'e') // ['a', 'b', ['c','d'], 'e']
   ```

   上面代码说明，数组的默认行为是可以展开，`Symbol.isConcatSpreadable`默认等于`undefined`。该属性等于`true`时，也有展开的效果。

   类似数组的对象正好相反，默认不展开。它的`Symbol.isConcatSpreadable`属性设为`true`，才可以展开。

   ```
   let obj = {length: 2, 0: 'c', 1: 'd'};
   ['a', 'b'].concat(obj, 'e') // ['a', 'b', obj, 'e']
   
   obj[Symbol.isConcatSpreadable] = true;
   ['a', 'b'].concat(obj, 'e') // ['a', 'b', 'c', 'd', 'e']
   ```

   `Symbol.isConcatSpreadable`属性也可以定义在类里面。

   ```
   class A1 extends Array {
     constructor(args) {
       super(args);
       this[Symbol.isConcatSpreadable] = true;
     }
   }
   class A2 extends Array {
     constructor(args) {
       super(args);
     }
     get [Symbol.isConcatSpreadable] () {
       return false;
     }
   }
   let a1 = new A1();
   a1[0] = 3;
   a1[1] = 4;
   let a2 = new A2();
   a2[0] = 5;
   a2[1] = 6;
   [1, 2].concat(a1).concat(a2)
   // [1, 2, 3, 4, [5, 6]]
   ```

   类`A1`是可展开的，类`A2`是不可展开的，所以使用`concat`时有不一样的结果。

   注意，`Symbol.isConcatSpreadable`的位置差异，`A1`是定义在实例上，`A2`是定义在类本身，效果相同。

   ### Symbol.species

   对象的`Symbol.species`属性，指向一个构造函数。创建衍生对象时，会使用该属性。

   ```
   class MyArray extends Array {
   }
   
   const a = new MyArray(1, 2, 3);
   const b = a.map(x => x);
   const c = a.filter(x => x > 1);
   
   b instanceof MyArray // true
   c instanceof MyArray // true
   ```

   ### Symbol.match

   对象的`Symbol.match`属性，指向一个函数。当执行`str.match(myObject)`时，如果该属性存在，会调用它，返回该方法的返回值。

   ```
   String.prototype.match(regexp)
   // 等同于
   regexp[Symbol.match](this)
   
   class MyMatcher {
     [Symbol.match](string) {
       return 'hello world'.indexOf(string);
     }
   }
   
   'e'.match(new MyMatcher()) // 1
   ```

   ### Symbol.replace

   对象的`Symbol.replace`属性，指向一个方法，当该对象被`String.prototype.replace`方法调用时，会返回该方法的返回值。

   ```
   String.prototype.replace(searchValue, replaceValue)
   // 等同于
   searchValue[Symbol.replace](this, replaceValue)
   ```

   ### Symbol.search

   对象的`Symbol.search`属性，指向一个方法，当该对象被`String.prototype.search`方法调用时，会返回该方法的返回值。

   ```
   String.prototype.search(regexp)
   // 等同于
   regexp[Symbol.search](this)
   
   class MySearch {
     constructor(value) {
       this.value = value;
     }
     [Symbol.search](string) {
       return string.indexOf(this.value);
     }
   }
   'foobar'.search(new MySearch('foo')) // 0
   ```

   ### Symbol.split

   对象的`Symbol.split`属性，指向一个方法，当该对象被`String.prototype.split`方法调用时，会返回该方法的返回值。

   ```
   String.prototype.split(separator, limit)
   // 等同于
   separator[Symbol.split](this, limit)
   ```

   ```
   class MySplitter {
     constructor(value) {
       this.value = value;
     }
     [Symbol.split](string) {
       let index = string.indexOf(this.value);
       if (index === -1) {
         return string;
       }
       return [
         string.substr(0, index),
         string.substr(index + this.value.length)
       ];
     }
   }
   
   'foobar'.split(new MySplitter('foo'))
   // ['', 'bar']
   
   'foobar'.split(new MySplitter('bar'))
   // ['foo', '']
   
   'foobar'.split(new MySplitter('baz'))
   // 'foobar'
   ```

   上面方法使用`Symbol.split`方法，重新定义了字符串对象的`split`方法的行为，

   ### Symbol.iterator

   对象的`Symbol.iterator`属性，指向该对象的默认遍历器方法。

   ```
   const myIterable = {};
   myIterable[Symbol.iterator] = function* () {
     yield 1;
     yield 2;
     yield 3;
   };
   
   [...myIterable] // [1, 2, 3]
   ```

   ### Symbol.toPrimitive

   对象的`Symbol.toPrimitive`属性，指向一个方法。该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值。

   `Symbol.toPrimitive`被调用时，会接受一个字符串参数，表示当前运算的模式，一共有三种模式。

   - Number：该场合需要转成数值
   - String：该场合需要转成字符串
   - Default：该场合可以转成数值，也可以转成字符串

   ```
   let obj = {
     [Symbol.toPrimitive](hint) {
       switch (hint) {
         case 'number':
           return 123;
         case 'string':
           return 'str';
         case 'default':
           return 'default';
         default:
           throw new Error();
        }
      }
   };
   
   2 * obj // 246
   3 + obj // '3default'
   obj == 'default' // true
   String(obj) // 'str'
   ```

   ### Symbol.toStringTag

   对象的`Symbol.toStringTag`属性，指向一个方法。在该对象上面调用`Object.prototype.toString`方法时，如果这个属性存在，它的返回值会出现在`toString`方法返回的字符串之中，表示对象的类型。也就是说，这个属性可以用来定制`[object Object]`或`[object Array]`中`object`后面的那个字符串。

   ```
   // 例一
   ({[Symbol.toStringTag]: 'Foo'}.toString())
   // "[object Foo]"
   
   // 例二
   class Collection {
     get [Symbol.toStringTag]() {
       return 'xxx';
     }
   }
   let x = new Collection();
   Object.prototype.toString.call(x) // "[object xxx]"
   ```

   ES6 新增内置对象的`Symbol.toStringTag`属性值如下。

   - `JSON[Symbol.toStringTag]`：'JSON'
   - `Math[Symbol.toStringTag]`：'Math'
   - Module 对象`M[Symbol.toStringTag]`：'Module'
   - `ArrayBuffer.prototype[Symbol.toStringTag]`：'ArrayBuffer'
   - `DataView.prototype[Symbol.toStringTag]`：'DataView'
   - `Map.prototype[Symbol.toStringTag]`：'Map'
   - `Promise.prototype[Symbol.toStringTag]`：'Promise'
   - `Set.prototype[Symbol.toStringTag]`：'Set'
   - `%TypedArray%.prototype[Symbol.toStringTag]`：'Uint8Array'等
   - `WeakMap.prototype[Symbol.toStringTag]`：'WeakMap'
   - `WeakSet.prototype[Symbol.toStringTag]`：'WeakSet'
   - `%MapIteratorPrototype%[Symbol.toStringTag]`：'Map Iterator'
   - `%SetIteratorPrototype%[Symbol.toStringTag]`：'Set Iterator'
   - `%StringIteratorPrototype%[Symbol.toStringTag]`：'String Iterator'
   - `Symbol.prototype[Symbol.toStringTag]`：'Symbol'
   - `Generator.prototype[Symbol.toStringTag]`：'Generator'
   - `GeneratorFunction.prototype[Symbol.toStringTag]`：'GeneratorFunction'

   ### Symbol.unscopables

   对象的`Symbol.unscopables`属性，指向一个对象。该对象指定了使用`with`关键字时，哪些属性会被`with`环境排除。

   ```
   Array.prototype[Symbol.unscopables]
   // {
   //   copyWithin: true,
   //   entries: true,
   //   fill: true,
   //   find: true,
   //   findIndex: true,
   //   includes: true,
   //   keys: true
   // }
   
   Object.keys(Array.prototype[Symbol.unscopables])
   // ['copyWithin', 'entries', 'fill', 'find', 'findIndex', 'includes', 'keys']
   ```

   上面代码说明，数组有 7 个属性，会被`with`命令排除。

   ```
   // 没有 unscopables 时
   class MyClass {
     foo() { return 1; }
   }
   
   var foo = function () { return 2; };
   
   with (MyClass.prototype) {
     foo(); // 1
   }
   
   // 有 unscopables 时
   class MyClass {
     foo() { return 1; }
     get [Symbol.unscopables]() {
       return { foo: true };
     }
   }
   
   var foo = function () { return 2; };
   
   with (MyClass.prototype) {
     foo(); // 2
   }
   ```

   上面代码通过指定`Symbol.unscopables`属性，使得`with`语法块不会在当前作用域寻找`foo`属性，即`foo`将指向外层作用域的变量。

