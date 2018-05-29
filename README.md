# JavaScript-Puzzlers

## 1. 

```
['1', '2', '3'].map(parseInt)
```

`map` 接收两个参数，第一个参数是在每一项上运行的函数，第二项是运行的这个函数的作用域对象。

`parseInt`接收两个参数，第一个参数是需要被转换的对象，第二项是转换的进制数。

```js
parseInt('1234blue') // 1234
parseInt('') // NaN
parseInt('0xA') // 10(十六进制数)
parseInt(22.5) // 22
parseInt('070') // 56(八进制数)
parseInt('70') // 70(十进制数)
parseInt('0xf') // 15(十六进制数)
```

> 为了消除在使用 `parseInt()`函数时可能导致的困惑，可以为这个函数提供第二个参数：转换时使用的基数（即多少进制）。如果知道要解析的值是十六进制格式的字符串，那么指定基数 16 作为第二个参数，可以保证得到正确的结果，例如：

```js
parseInt('0xAF', 16)  // 175
```

> 实际上，如果指定了 16 作为第二个参数，字符串可以不带前面的 '0x'

```js
parseInt('AF', 16) // 175
parseInt('AF') // NaN
```

```js
parseInt('10', 2) // 2
parseInt('10', 8) // 8
parseInt('10', 10) // 10
parseInt('10', 16) // 16
```

回到题目：

```js
['1', '2', '3'].map(parseInt)

[
    parseInt('1', 0), // 第二个参数为0，按10进制，-> 1
    parseInt('2', 1), // 没有1进制，-> NaN
    parseInt('3', 2)  // 2进制中没有3，-> NaN
]

[1, NaN, NaN]
```

```js
['4', '3', '1', '2'].map(parseInt)

[
    parseInt('4', 0),
    parseInt('3', 1),
    parseInt('1', 2),
    parseInt('2', 3)
]

[4, NaN, 1, 2]
```

## 2. 

```js
[typeof null, null instanceof Object]
```

- typeof

  ```js
  // Numbers
  typeof 37 === 'number';
  typeof 3.14 === 'number';
  typeof Math.LN2 === 'number';
  typeof Infinity === 'number';
  typeof NaN === 'number'; // 尽管NaN是"Not-A-Number"的缩写
  typeof Number(1) === 'number'; // 但不要使用这种形式!
  
  // Strings
  typeof "" === 'string';
  typeof "bla" === 'string';
  typeof (typeof 1) === 'string'; // typeof总是返回一个字符串
  typeof String("abc") === 'string'; // 但不要使用这种形式!
  
  // Booleans
  typeof true === 'boolean';
  typeof false === 'boolean';
  typeof Boolean(true) === 'boolean'; // 但不要使用这种形式!
  
  // Symbols
  typeof Symbol() === 'symbol';
  typeof Symbol('foo') === 'symbol';
  typeof Symbol.iterator === 'symbol';
  
  // Undefined
  typeof undefined === 'undefined';
  typeof declaredButUndefinedVariable === 'undefined';
  typeof undeclaredVariable === 'undefined'; 
  
  // Objects
  typeof {a:1} === 'object';
  
  // 使用Array.isArray 或者 Object.prototype.toString.call
  // 区分数组,普通对象
  typeof [1, 2, 4] === 'object';
  
  typeof new Date() === 'object';
  
  // 下面的容易令人迷惑，不要使用！
  typeof new Boolean(true) === 'object';
  typeof new Number(1) === 'object';
  typeof new String("abc") === 'object';
  
  // 函数
  typeof function(){} === 'function';
  typeof class C{} === 'function'
  typeof Math.sin === 'function';
  typeof new Function() === 'function';
  ```

  ![](https://diycode.b0.upaiyun.com/photo/2018/4bfed3002b97792f374518a392ad422f.jpg)

- instanceof

  ```js
  object instanceof constructor
  
  // instanceof 运算符用来检测 constructor.prototype 是否存在于参数 object 的原型链上。
  ```

  

