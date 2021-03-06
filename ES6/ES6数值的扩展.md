## ES6数值的扩展

### Number.isFinite()、Number.isNaN()
#### 特点


> ES6 在Number对象上，新提供了Number.isFinite()和Number.isNaN()两个方法,**都返回布尔值**
> **不会再默认调用Number方法转化**


- Number.isFinite()用来检查一个数值是否为有限的（finite）。

    Number.isFinite(15); // true
    Number.isFinite(0.8); // true
    Number.isFinite(NaN); // false
    Number.isFinite(Infinity); // false
    Number.isFinite(-Infinity); // false
    Number.isFinite('foo'); // false
    Number.isFinite('15'); // false
    Number.isFinite(true); // false

- Number.isNaN()用来检查一个值是否为NaN。

**和全局函数 isNaN() 相比，该方法不会强制将参数转换成数字，只有在参数是真正的数字类型，且值为 NaN 的时候才会返回 true。**

    Number.isNaN(NaN);        // true
    Number.isNaN(Number.NaN); // true
    Number.isNaN(0 / 0)       // true
    
    // 下面这几个如果使用全局的 isNaN() 时，会返回 true。
    Number.isNaN("NaN");      // false，字符串 "NaN" 不会被隐式转换成数字 NaN。
    Number.isNaN(undefined);  // false
    Number.isNaN({});         // false
    Number.isNaN("blabla");   // false
    
    // 下面的都返回 false
    Number.isNaN(true);
    Number.isNaN(null);
    Number.isNaN(37);
    Number.isNaN("37");
    Number.isNaN("37.37");
    Number.isNaN("");
    Number.isNaN(" ");

### Number.parseInt()、Number.parseFloat()

- ES6 将全局方法parseInt()和parseFloat()，移植到Number对象上面，行为完全保持不变。

    // ES5的写法
    parseInt('12.34') // 12
    parseFloat('123.45#') // 123.45
    
    // ES6的写法
    Number.parseInt('12.34') // 12
    Number.parseFloat('123.45#') // 123.45

- 这样做的目的，**是逐步减少全局性方法，使得语言逐步模块化。**

    Number.parseInt === parseInt // true
    Number.parseFloat === parseFloat // true

### Number.isInteger()
> Number.isInteger()用来判断一个值是否为整数。
> 需要注意的是，在 JavaScript 内部，整数和浮点数是同样的储存方法，**所以 3 和 3.0 被视为同一个值**。

    Number.isInteger(25) // true
    Number.isInteger(25.0) // true
    Number.isInteger(25.1) // false
    Number.isInteger("15") // false
    Number.isInteger(true) // false

### Math 对象的扩展

> ES6 在 Math 对象上新增了 17 个与数学相关的方法。
> 所有这些方法都是静态方法，只能在 Math 对象上调用。

#### Math.trunc()

> Math.trunc方法用于去除一个数的小数部分，返回整数部分。

    Math.trunc(4.1) // 4
    Math.trunc(4.9) // 4
    Math.trunc(-4.1) // -4
    Math.trunc(-4.9) // -4
    Math.trunc(-0.1234) // -0

- 对于非数值，Math.trunc内部使用Number方法将其先转为数值。
- 对于空值和无法截取整数的值，返回NaN。

#### Math.sign()

> Math.sign方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。

**它会返回五种值。**

- 参数为正数，返回+1；
- 参数为负数，返回-1；
- 参数为 0，返回0；
- 参数为-0，返回-0;
- 其他值，返回NaN。

    Math.sign(-5) // -1
    Math.sign(5) // +1
    Math.sign(0) // +0
    Math.sign(-0) // -0
    Math.sign(NaN) // NaN
    
    Math.sign('')  // 0
    Math.sign(true)  // +1
    Math.sign(false)  // 0
    Math.sign(null)  // 0
    Math.sign('9')  // +1
    Math.sign('foo')  // NaN
    Math.sign()  // NaN
    Math.sign(undefined)  // NaN

#### Math.cbrt()

> Math.cbrt方法用于计算一个数的立方根。

- 对于非数值，Math.cbrt方法内部也是先使用Number方法将其转为数值。

    Math.cbrt(-1) // -1
    Math.cbrt(0)  // 0
    Math.cbrt(1)  // 1
    Math.cbrt(2)  // 1.2599210498948734

#### Math.hypot()

> Math.hypot方法返回所有参数的**平方和的平方根**。

    Math.hypot(3, 4);        // 5
    Math.hypot(3, 4, 5);     // 7.0710678118654755
    Math.hypot();            // 0
    Math.hypot(NaN);         // NaN
    Math.hypot(3, 4, 'foo'); // NaN
    Math.hypot(3, 4, '5');   // 7.0710678118654755
    Math.hypot(-3);          // 3

#### 指数运算符

> ES2016 新增了一个指数运算符（**）。

    2 ** 2 // 4
    2 ** 3 // 8

- 指数运算符可以与等号结合，形成一个新的赋值运算符`（**=）`。

    let a = 1.5;
    a **= 2;
    // 等同于 a = a * a;
    
    let b = 4;
    b **= 3;
    // 等同于 b = b * b * b;


