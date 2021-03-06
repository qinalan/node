## ES6对象的扩展

### 属性的简洁表示法

#### ES6 允许直接写入变量和函数
- 作为对象的属性和方法。这样的书写更加简洁。

    const foo = 'bar';
    const baz = {foo};
    baz // {foo: "bar"}
    
    // 等同于
    const baz = {foo: foo};

- 方法也可以简写。

    const o = {
      method() {
        return "Hello!";
      }
    };
    
    // 等同于
    
    const o = {
      method: function() {
        return "Hello!";
      }
    };

#### 属性名表达式

- JavaScript 定义对象的属性，有两种方法。

    // 方法一
    obj.foo = true;
    
    // 方法二
    obj['a' + 'bc'] = 123;

- **但是**，如果使用字面量方式定义对象（使用大括号），在 `ES5` 中只能使用方法一（标识符）定义属性。

    var obj = {
      foo: true,
      abc: 123
    };

- ES6 允许字面量定义对象时，用方法二（表达式）作为对象的属性名，**即把表达式放在方括号内。**

    let propKey = 'foo';
    
    let obj = {
      [propKey]: true,
      ['a' + 'bc']: 123
    };

- 表达式还可以**用于定义方法名**。

    let obj = {
      ['h' + 'ello']() {
        return 'hi';
      }
    };
    
    obj.hello() // hi

- **`注意`，属性名表达式与简洁表示法，不能同时使用，会报错。**

    // 报错
    const foo = 'bar';
    const bar = 'abc';
    const baz = { [foo] };
    
    // 正确
    const foo = 'bar';
    const baz = { [foo]: 'abc'};

### Object.is( )
#### ES5 比较两个值是否相等的不足
> ES5 比较两个值是否相等，只有两个运算符：相等运算符 (`= =`) 和严格相等运算符（`===`）。
> **它们都有缺点**，前者会自动转换数据类型，后者的NaN不等于自身，以及+0等于-0。
> **JavaScript 缺乏一种运算**，在所有环境中，**只要两个值是一样的，它们就应该相等**
#### ES6的解决办法
> **ES6 提出同值相等算法**，用来解决这个问题。
> Object.is就是部署这个算法的新方法。
> 它用来比较两个值是否严格相等，与**严格比较运算符（===）的行为基本一致**

    Object.is('foo', 'foo')
    // true
    Object.is({}, {})
    // false
    

- 不同之处只有两个：**一是+0不等于-0，二是NaN等于自身**

    +0 === -0 //true
    NaN === NaN // false
    
    Object.is(+0, -0) // false
    Object.is(NaN, NaN) // true

### Object.assign()
#### 深拷贝与浅拷贝
![Alt text](./1530358855361.png)
[深拷贝与浅拷贝](https://www.cnblogs.com/Chen-XiaoJun/p/6217373.html)

- **Object.assign**方法用于对象的合并，**将源对象（source）的所有可枚举属性**，`复制到目标对象（target）`。
- **Object.assign()** 进行的是浅拷贝，拷贝的是对象的`属性的引用`，而不是对象本身。
    
    const target = { a: 1 };
    
    const source1 = { b: 2 };
    const source2 = { c: 3 };
    
    Object.assign(target, source1, source2);
    target // {a:1, b:2, c:3}

- 如果只有一个参数，Object.assign会`直接返回该参数`。

     const obj = {a: 1};
    Object.assign(obj) === obj // true

- 由于**undefined和null**无法转成对象，所以如果它们作为参数，就会`报错`。

    Object.assign(undefined) // 报错
    Object.assign(null) // 报错

- 注意：Object.assign可以用来处理数组，`但是会把数组视为对象`。

    Object.assign([1, 2, 3], [4, 5])
    // [4, 5, 3]
    //把数组视为属性名为 0、1、2 的对象，因此源数组的 0 号属性4覆盖了目标数组的 0 号属性1。

### Object.keys()

- ES5 引入了Object.keys方法，返回一个数组，成员是**参数对象自身**的（不含继承的）所有可遍历（**enumerable**）属性的键名。

    var obj = { foo: 'bar', baz: 42 };
    Object.keys(obj)
    // ["foo", "baz"]

- ES2017 引入了跟Object.keys配套的**Object.values**和`Object.entries`，作为遍历一个**对象的补充手段**，供`for...of`循环使用。

    let {keys, values, entries} = Object;
    let obj = { a: 1, b: 2, c: 3 };
    
    for (let key of keys(obj)) {
      console.log(key); // 'a', 'b', 'c'
    }
    
    for (let value of values(obj)) {
      console.log(value); // 1, 2, 3
    }
    
    for (let [key, value] of entries(obj)) {
      console.log([key, value]); // ['a', 1], ['b', 2], ['c', 3]
    }

### Object.values()

**Object.values**方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的`键值`

    const obj = { foo: 'bar', baz: 42 };
    Object.values(obj)
    // ["bar", 42]

返回数组的**成员顺序**，

    const obj = { 100: 'a', 2: 'b', 7: 'c' };
    Object.values(obj)
    // ["b", "c", "a"]
    

- 上面代码中，属性名为数值的属性，是按照数值大小，从小到大遍历的，**因此返回的顺序是b、c、a**

### Object.entries

- Object.entries方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的**键值对数组**。

    const obj = { foo: 'bar', baz: 42 };
    Object.entries(obj)
    // [ ["foo", "bar"], ["baz", 42] ]

- 除了返回值不一样，**该方法的行为与Object.values基本一致**

### 对象的扩展运算符

- 解构赋值

    let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
    x // 1
    y // 2
    z // { a: 3, b: 4 }

- 由于解构赋值要求等号右边是一个对象，所以如果等号右边**是undefined或null，就会报错，因为它们`无法转为对象`**

    let { x, y, ...z } = null; // 运行时错误
    let { x, y, ...z } = undefined; // 运行时错误

- 解构赋值必须是最后一个参数，否则会报错。

    let { ...x, y, z } = obj; // 句法错误
    let { x, ...y, ...z } = obj; // 句法错误

- 注意，解构赋值的**拷贝是浅拷贝**，即如果一个键的值是复合类型的值（数组、对象、函数）、那么解构赋值拷贝的是这个值的引用，而不是这个值的副本。

    let obj = { a: { b: 1 } };
    let { ...x } = obj;
    obj.a.b = 2;
    x.a.b // 2

#### 扩展运算符

- 扩展运算符（**...**）用于取出参数对象的**所有可遍历属性**，拷贝到当前对**象之中**

    let z = { a: 3, b: 4 };
    let n = { ...z };
    n // { a: 3, b: 4 }
    

- 这等同于使用**Object.assign方法**

    let aClone = { ...a };
    // 等同于
    let aClone = Object.assign({}, a);


