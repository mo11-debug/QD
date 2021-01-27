### ES6.7数值的扩展

1. 二进制和八进制表示法

   ```js
   0b111110111 === 503 // true二进制
   0o767 === 503 // true八进制
   
   // 非严格模式
   (function(){
     console.log(0o11 === 011);
   })() // true
   
   // 严格模式
   (function(){
     'use strict';
     console.log(0o11 === 011);
   })()
   ```

   如果要转为十进制，要用Number

   ```js
   Number('0b111')  // 7
   Number('0o10')  // 8
   ```

2. Number.isFinite(),Number.isNaN()

   **如果参数类型不是数值，Number.isFinite一律返回false**

   ```js
   Number.isFinite(15); // true
   Number.isFinite(0.8); // true
   Number.isFinite(NaN); // false
   Number.isFinite(Infinity); // false
   Number.isFinite(-Infinity); // false
   Number.isFinite('foo'); // false
   Number.isFinite('15'); // false
   Number.isFinite(true); // false
   ```

   Number.isNaN用来检查一个值是否为NaN.不是一律返回false。

   ```js
   Number.isNaN(NaN) // true
   Number.isNaN(15) // false
   Number.isNaN('15') // false
   Number.isNaN(true) // false
   Number.isNaN(9/NaN) // true
   Number.isNaN('true' / 0) // true
   Number.isNaN('true' / 'true') // true
   ```

   与传统全局方法区别：传统方法先调用Number（）将非数值的值转为数值，再进行判断。而两个新方法只对数值有效，非数值一律返回false。

   ```js
   isFinite(25) // true
   isFinite("25") // true
   Number.isFinite(25) // true
   Number.isFinite("25") // false
   
   isNaN(NaN) // true
   isNaN("NaN") // true
   Number.isNaN(NaN) // true
   Number.isNaN("NaN") // false
   Number.isNaN(1) // false
   ```

3. Number.parseInt(),Number.parseFloat()

   ```js
   Number.parseInt('12.34') // 12
   Number.parseFloat('123.45#') // 123.45
   
   Number.parseInt === parseInt // true
   Number.parseFloat === parseFloat // true
   //逐步减少全局性方法，使语言逐步模块化。
   ```

4. Number.isInteger()

   判断一个数值是否为整数。如果参数不是数值，返回false。

   ```js
   Number.isInteger(25) // true
   Number.isInteger(25.1) // false
   Number.isInteger(25.0) // true
   
   Number.isInteger() // false
   Number.isInteger(null) // false
   Number.isInteger('15') // false
   Number.isInteger(true) // false
   
   Number.isInteger(5E-324) // false
   Number.isInteger(5E-325) // true
   //超过精度会误判
   ```

5. Number.EPSILON()

   ```js
   Number.EPSILON === Math.pow(2, -52)
   // true
   Number.EPSILON
   // 2.220446049250313e-16
   Number.EPSILON.toFixed(20)
   // "0.00000000000000022204"
   
   function withinErrorMargin (left, right) {
     return Math.abs(left - right) < Number.EPSILON * Math.pow(2, 2);
   }
   
   0.1 + 0.2 === 0.3 // false
   withinErrorMargin(0.1 + 0.2, 0.3) // true
   
   1.1 + 1.3 === 2.4 // false
   withinErrorMargin(1.1 + 1.3, 2.4) // true
   ```

   Number.EPSILON实质是一个可以接受的最小误差范围。

6. 安全整数和Number.isSafeInteger()

   ```js
   Math.pow(2, 53) // 9007199254740992
   
   9007199254740992  // 9007199254740992
   9007199254740993  // 9007199254740992
   
   Math.pow(2, 53) === Math.pow(2, 53) + 1
   // true
   //超出整数范围不再精确
   
   Number.MAX_SAFE_INTEGER === Math.pow(2, 53) - 1
   // true
   Number.MAX_SAFE_INTEGER === 9007199254740991
   // true//上限
   
   Number.MIN_SAFE_INTEGER === -Number.MAX_SAFE_INTEGER
   // true
   Number.MIN_SAFE_INTEGER === -9007199254740991
   // true//下限
   ```

   Number.isSafeInteger()则是用来判断一个整数是否落在这个范围之内。 

   ```js
   function trusty (left, right, result) {
     if (
       Number.isSafeInteger(left) &&
       Number.isSafeInteger(right) &&
       Number.isSafeInteger(result)
     ) {
       return result;
     }
     throw new RangeError('Operation cannot be trusted!');
   }
   
   trusty(9007199254740993, 990, 9007199254740993 - 990)
   // RangeError: Operation cannot be trusted!
   
   trusty(1, 2, 3)
   // 3
   ```

7. Math对象的扩展(都是静态方法)

   - Math.trunc()

     ```js
     //除去小数部分返回整数部分
     Math.trunc(4.1) // 4
     Math.trunc(4.9) // 4
     Math.trunc(-4.1) // -4
     
     //非数值会使用内部Number方法转为数值
     Math.trunc('123.456') // 123
     Math.trunc(true) //1
     Math.trunc(false) // 0
     Math.trunc(null) // 0
     
     //空值或者无法截取整数的值返回NaN
     Math.trunc(NaN);      // NaN
     Math.trunc('foo');    // NaN
     Math.trunc();         // NaN
     Math.trunc(undefined) // NaN
     
     //模拟环境
     Math.trunc = Math.trunc || function(x) {
       return x < 0 ? Math.ceil(x) : Math.floor(x);
     };
     ```

   - Math.sign()

     - 参数为正数，返回+1；
     - 参数为负数，返回-1；
     - 参数为 0，返回0；
     - 参数为-0，返回-0;
     - 其他值，返回NaN。

     ```js
     Math.sign(-5) // -1
     Math.sign(5) // +1
     Math.sign(0) // +0
     Math.sign(-0) // -0
     Math.sign(NaN) // NaN
     
     //非数值会自动转为数值，转不了就返回NaN
     Math.sign('')  // 0
     Math.sign(true)  // +1
     Math.sign(false)  // 0
     Math.sign(null)  // 0
     Math.sign('9')  // +1
     Math.sign('foo')  // NaN
     Math.sign()  // NaN
     Math.sign(undefined)  // NaN
     
     //模拟环境
     Math.sign = Math.sign || function(x) {
       x = +x; // convert to a number
       if (x === 0 || isNaN(x)) {
         return x;
       }
       return x > 0 ? 1 : -1;
     };
     ```

   - Math.cbrt()

     计算立方根

     ```js
     Math.cbrt(-1) // -1
     Math.cbrt(0)  // 0
     Math.cbrt(1)  // 1
     Math.cbrt(2)  // 1.2599210498948732
     
     //内部转为数值
     Math.cbrt('8') // 2
     Math.cbrt('hello') // NaN
     
     //模拟环境
     Math.cbrt = Math.cbrt || function(x) {
       var y = Math.pow(Math.abs(x), 1/3);
       return x < 0 ? -y : y;
     };
     ```

   - Math.clz32()

     将参数转为32位无符号数，然后返回32位有多少个前导0

     ```js
     Math.clz32(0) // 32
     Math.clz32(1) // 31
     Math.clz32(1000) // 22
     Math.clz32(0b01000000000000000000000000000000) // 1
     Math.clz32(0b00100000000000000000000000000000) // 2
     
     //左移运算符
     Math.clz32(0) // 32
     Math.clz32(1) // 31
     Math.clz32(1 << 1) // 30
     Math.clz32(1 << 2) // 29
     Math.clz32(1 << 29) // 2
     
     //只考虑整数
     Math.clz32(3.9) // 30
     
     //其他值均先转为数值再计算
     Math.clz32({}) // 32
     Math.clz32(true) // 31
     ```

   - Math.imul()

     返回两个数以32位带符号整数形式相乘结果

     ```js
     Math.imul(2, 4)   // 8
     Math.imul(-1, 8)  // -8
     Math.imul(-2, -2) // 4
     
     (0x7fffffff * 0x7fffffff)|0 // 0
     Math.imul(0x7fffffff, 0x7fffffff) // 1
     ```

   - Math.fround()

     转变精度

     ```js
     // 未丢失有效精度
     Math.fround(1.125) // 1.125
     Math.fround(7.25)  // 7.25
     
     // 丢失精度
     Math.fround(0.3)   // 0.30000001192092896
     Math.fround(0.7)   // 0.699999988079071
     Math.fround(1.0000000123) // 1
     
     //返回单精度浮点数
     Math.fround(NaN)      // NaN
     Math.fround(Infinity) // Infinity
     
     Math.fround('5')      // 5
     Math.fround(true)     // 1
     Math.fround(null)     // 0
     Math.fround([])       // 0
     Math.fround({})       // NaN
     
     //模拟环境
     Math.fround = Math.fround || function (x) {
       return new Float32Array([x])[0];
     };
     ```

   - Math.hypot()

     返回参数平方根

     ```js
     Math.hypot(3, 4);        // 5
     Math.hypot(3, 4, 5);     // 7.0710678118654755
     Math.hypot();            // 0
     Math.hypot(NaN);         // NaN
     Math.hypot(3, 4, 'foo'); // NaN
     Math.hypot(3, 4, '5');   // 7.0710678118654755
     Math.hypot(-3);          // 3
     ```

     只要有一个参数不能转为数值，都会返回NaN。

   ##### 对数方法

   - Math.expm1()

     返回 e的x次方 - 1 

     ```js
     Math.expm1(-1) // -0.6321205588285577
     Math.expm1(0)  // 0
     Math.expm1(1)  // 1.718281828459045
     
     //模拟环境
     Math.expm1 = Math.expm1 || function(x) {
       return Math.exp(x) - 1;
     };
     ```

   - Math.log1p()

     返回1+x的自然对数

     ```js
     Math.log1p(1)  // 0.6931471805599453
     Math.log1p(0)  // 0
     Math.log1p(-1) // -Infinity
     Math.log1p(-2) // NaN
     
     //模拟环境
     Math.log1p = Math.log1p || function(x) {
       return Math.log(1 + x);
     };
     ```

   - Math.log10()

     返回以10为底的x的对数

     ```js
     Math.log10(2)      // 0.3010299956639812
     Math.log10(1)      // 0
     Math.log10(0)      // -Infinity
     Math.log10(-2)     // NaN
     Math.log10(100000) // 5
     
     //模拟环境
     Math.log10 = Math.log10 || function(x) {
       return Math.log(x) / Math.LN10;
     };
     ```

   - Math.log2()

     返回以2为底的x的对数

     ```js
     Math.log2(3)       // 1.584962500721156
     Math.log2(2)       // 1
     Math.log2(1)       // 0
     Math.log2(0)       // -Infinity
     Math.log2(-2)      // NaN
     Math.log2(1024)    // 10
     Math.log2(1 << 29) // 29
     
     //模拟环境
     Math.log2 = Math.log2 || function(x) {
       return Math.log(x) / Math.LN2;
     };
     ```

   ##### 双曲函数方法

   - `Math.sinh(x)` 返回`x`的双曲正弦（hyperbolic sine）
   - `Math.cosh(x)` 返回`x`的双曲余弦（hyperbolic cosine）
   - `Math.tanh(x)` 返回`x`的双曲正切（hyperbolic tangent）
   - `Math.asinh(x)` 返回`x`的反双曲正弦（inverse hyperbolic sine）
   - `Math.acosh(x)` 返回`x`的反双曲余弦（inverse hyperbolic cosine）
   - `Math.atanh(x)` 返回`x`的反双曲正切（inverse hyperbolic tangent）

8. 指数运算符

   (**)次幂

   ```js
   2 ** 3 // 8
   
   //运算符特点：右结合
   // 相当于 2 ** (3 ** 2)
   2 ** 3 ** 2
   // 512
   
   let a = 1.5;
   a **= 2;
   // 等同于 a = a * a;
   
   let b = 4;
   b **= 3;
   // 等同于 b = b * b * b;
   ```

9. BigInt数据类型

   用来表示整数，没有位数限制，任何整数都可以精确表示。

   ```js
   const a = 2172141653n;
   const b = 15346349309n;
   
   // BigInt 可以保持精度
   a * b // 33334444555566667777n
   
   // 普通整数无法保持精度
   Number(a) * Number(b) // 33334444555566670000
   
   //加后缀n区别number
   1234 // 普通整数
   1234n // BigInt
   
   // BigInt 的运算
   1n + 2n // 3n
   0b1101n // 二进制
   0o777n // 八进制
   0xFFn // 十六进制
   
   //不相等
   42n === 42 // false
   
   //不能使用+，因为和asm.js冲突，会报错
   -42n // 正确
   +42n // 报错
   ```

   ##### BigInt对象

   必须有参数，且不能为小数，否则会报错

   ```js
   new BigInt() // TypeError
   BigInt(undefined) //TypeError
   BigInt(null) // TypeError
   BigInt('123n') // SyntaxError
   BigInt('abc') // SyntaxError
   BigInt(1.5) // RangeError
   BigInt('1.5') // SyntaxError
   ```

   实例方法：

   - `BigInt.prototype.toString()`(继承Object对象)
   - `BigInt.prototype.valueOf()`(继承Object对象)
   - `BigInt.prototype.toLocaleString()`(继承Number对象)

   - `BigInt.prototype.toLocaleString()``BigInt.asUintN(width, BigInt)`： 给定的 BigInt 转为 0 到 2width - 1 之间对应的值。(静态)
   - `BigInt.asIntN(width, BigInt)`：给定的 BigInt 转为 -2width - 1 到 2width - 1 - 1 之间对应的值。(静态)
   - `BigInt.parseInt(string[, radix])`：近似于`Number.parseInt()`，将一个字符串转换成指定进制的 BigInt。(静态)

   转换规则

   - Boolean()

   - Number()

   - String()

     ```js
     Boolean(0n) // false
     Boolean(1n) // true
     Number(1n)  // 1
     String(1n)  // "1"
     
     //!也可以将BigInt转为布尔值
     !0n // true
     !1n // false
     ```

   数学运算：

   除了不带符号的右移位运算符，一元的求正运算符，其他都可以用。

   其他运算：

   ```js
   if (0n) {
     console.log('if');
   } else {
     console.log('else');
   }
   // else
   
   //>和==允许BigInt 与其他类型的值混合计算，不会损失精度
   0n < 1 // true
   0n < true // true
   0n == 0 // true
   0n == false // true
   0n === 0 // false
   ```

   