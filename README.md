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

## 3. 

```js
[ [3,2,1].reduce(Math.pow), [].reduce(Math.pow) ]
```

```js
// A. an error
// B. [9, 0]
// C. [9, NaN]
// D. [9, undefined]

// Per spec: reduce on an empty array without an initial value throws TypeError
```

## 4. 

```js
var val = 'smtg';
console.log('Value is ' + (val === 'smtg') ? 'Something' : 'Nothing');
```

```js
// A. Value is Something
// B. Value is Nothing
// C. NaN
// D. other
```

```
it actually prints 'Something' the + operator has higher precedence than the ternary one.
```

```js
var val = 'smtg';
console.log('Value is ' + val === 'smtg' ? 'Something' : 'Nothing');
// 'Nothing'
```

```js
var val = 'smtg';
console.log('Value is ' + (val === 'smtg' ? 'Something' : 'Nothing'));
// 'Value is Something'
```

## 5. 

```js
var name = 'World!';
(function () {
    if (typeof name === 'undefined') {
        var name = 'Jack';
        console.log('Goodbye ' + name);
    } else {
        console.log('Hello ' + name);
    }
})();
```

```js
// A. Goodbye Jack
// B. Hello Jack
// C. Hello undefined
// D. Hello World
```

```
The var declaration is hoisted to the function scope, but the initialization is not.
```

## 6. 

```js
var END = Math.pow(2, 53);
var START = END - 100;
var count = 0;
for (var i = START; i <= END; i++) {
    count++;
}
console.log(count);
```

```js
// A. 0
// B. 100
// C. 101
// D. other
```

```
it goes into an infinite loop, 2^53 is the highest possible number in javascript, and 2^53+1 gives 2^53, so i can never become larger than that.
```

[javascript 里最大的安全的整数为什么是2的53次方减一？](https://www.zhihu.com/question/29010688)

## 7. 

```js
var ary = [0,1,2];
ary[10] = 10;
ary.filter(function(x) { return x === undefined;});
```

```js
// A. [undefined × 7]
// B. [0, 1, 2, 10]
// C. []
// D. [undefined]
```

```
Array.prototype.filter is not invoked for the missing elements.
```

```
forEach、map、filter、reduce、reduceRight、some、every 中的 callbackfn 只被实际存在的数组元素调用；它不会被缺少的数组元素调用。
```

## 8. 

```js
var two   = 0.2
var one   = 0.1
var eight = 0.8
var six   = 0.6
[two - one == one, eight - six == two]
```

```js
// A. [true, true]
// B. [false, false]
// C. [true, false]
// D. other
```

```
JavaScript does not have precision math, even though sometimes it works correctly.
```

## 9.

```js
function showCase(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase(new String('A'));
```

```js
// A. Case A
// B. Case B
// C. Do not know!
// D. undefined
```

```
switch uses === internally and new String(x) !== x
```

## 10. 

```js
function showCase2(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase2(String('A'));
```

```js
// A. Case A
// B. Case B
// C. Do not know!
// D. undefined
```

```
String(x) does not create an object but does return a string, i.e. typeof String(1) === "string"
```

## 11. 

```js
function isOdd(num) {
    return num % 2 == 1;
}
function isEven(num) {
    return num % 2 == 0;
}
function isSane(num) {
    return isEven(num) || isOdd(num);
}
var values = [7, 4, '13', -9, Infinity];
values.map(isSane);
```

```js
// A. [true, true, true, true, true]
// B. [true, true, true, true, false]
// C. [true, true, true, false, false]
// D. [true, true, false, false, false]
```

```
Infinity % 2 gives NaN, -9 % 2 gives -1 (modulo operator keeps sign so it's result is only reliable compared to 0)
```

## 12. 

```js
parseInt(3, 8)
parseInt(3, 2)
parseInt(3, 0)
```

```js
// A. 3, 3, 3
// B. 3, 3, NaN
// C. 3, NaN, NaN
// D. other
```

```
3 doesn't exist in base 2, so obviously that's a NaN, but what about 0? parseInt will consider a bogus radix and assume you meant 10, so it returns 3.
```

## 13. 

```js
Array.isArray( Array.prototype )
```

```js
// A. true
// B. false
// C. error
// D. other
```

```
Array.prototype is an Array. Go figure.
```

## 14. 

```js
var a = [0];
if ([0]) {
  console.log(a == true);
} else {
  console.log("wut");
}
```

```js
// A. true
// B. false
// C. "wut"
// D. other
```

```
[0] as a boolean is considered true. Alas, when using it in the comparisons it gets converted in a different way and all goes to hell.
```

## 15. 

```js
[] == []
```

```js
// A. true
// B. false
// C. error
// D. other
```

```
== is the spawn of satan.
== 两边都是对象的时候，比较二者是不是指向同一个对象，如果是返回 true,如果不是返回 false
```

## 16.

```js
'5' + 3
'5' - 3
```

```js
// A. "53", 2
// B. 8, 2
// C. error
// D. other
```

```
Strings know about + and will use it, but they are ignorant of - so in that case the strings get converted to numbers.
```

## 17.

```js
1 + - + + + - + 1
```

```js
// A. 2
// B. 1
// C. error
// D. other
```

## 18.

```js
var ary = Array(3);
ary[0]=2
ary.map(function(elem) { return '1'; });
```

```js
// A. [2, 1, 1]
// B. ["1", "1", "1"]
// C. [2, "1", "1"]
// D. other
```

```
The result is ["1", undefined × 2], as map is only invoked for elements of the Array which have been initialized.
```

## 19.

```js
function sidEffecting(ary) {
  ary[0] = ary[2];
}
function bar(a,b,c) {
  c = 10
  sidEffecting(arguments);
  return a + b + c;
}
bar(1,1,1)
```

```js
// A. 3
// B. 12
// C. error
// D. other
```

```
The result is 21, in javascript variables are tied to the arguments object so changing the variables changes arguments and changing arguments changes the local variables even when they are not in the same scope.
```

## 20.

```js
var a = 111111111111111110000,
    b = 1111;
a + b;
```

```js
// A. 111111111111111111111
// B. 111111111111111110000
// C. NaN
// D. Infinity
```

```
Lack of precision for numbers in JavaScript affects both small and big numbers.
```

## 21. 

```js
var x = [].reverse;
x();
```

```js
// A. []
// B. undefined
// C. error
// D. window
```

```
[].reverse will return this and when invoked without an explicit receiver object it will default to the default this AKA window
```

```
Array.prototype.reverse.call(window)
```

## 22.

```js
Number.MIN_VALUE > 0
```

```js
// A. false
// B. true
// C. error
// D. other
```

```
Number.MIN_VALUE is the smallest value bigger than zero, -Number.MAX_VALUE gets you a reference to something like the most negative number.  5e-324
```

\* Number.MAX_VALUE：最大的正数
\* Number.MIN_VALUE：最小的正数
\* Number.NaN：特殊值，用来表示这不是一个数
\* Number.NEGATIVE_INFINITY：负无穷大
\* Number.POSITIVE_INFINITY：正无穷大

如果要表示最小的负数和最大的负数，可以使用-Number.MAX_VALUE和-Number.MIN_VALUE

## 23.

```js
[1 < 2 < 3, 3 < 2 < 1]
```

```js
// A. [true, true]
// B. [true, false]
// C. error
// D. other
```

```
This is parsed as (1 > 2) > 3 and (3 > 2) > 1. Than it's implicit conversions at work: true gets intified and is 1, while false gets intified and becomes 0.
```

## 24.

```js
// the most classic wtf
2 == [[[2]]]
```

```js
// A. true
// B. false
// C. undefined
// D. other
```

```
both objects get converted to strings and in both cases the resulting string is "2"
```

## 25.

```js
3.toString()
3..toString()
3...toString()
```

```js
// A. "3", error, error
// B. "3", "3.0", error
// C. error, "3", error
// D. other
```

```
3.x is a valid syntax to define "3" with a mantissa of x, toString is not a valid number, but the empty string is.
```

## 26. 

```js
(function(){
  var x = y = 1;
})();
console.log(y);
console.log(x);
```

```js
// A. 1, 1
// B. error, error
// C. 1, error
// D. other
```

```
y is an automatic global, not a function local one.
```

## 27.

```js
var a = /123/,
    b = /123/;
a == b
a === b
```

```
// A. true, true
// B. true, false
// C. false, false
// D. other
```

```
Per spec Two regular expression literals in a program evaluate to regular expression objects that never compare as === to each other even if the two literals' contents are identical.
```

## 28

```js
var a = [1, 2, 3],
    b = [1, 2, 3],
    c = [1, 2, 4]
a ==  b
a === b
a >   c
a <   c
```

```
// A. false false false true
// B. false false false false
// C. true true false true
// D. other
```

```
Arrays are compared lexicographically with > and <, but not with == and ===
```

## 29

```js
var a = {}, b = Object.prototype;
[a.prototype === b, Object.getPrototypeOf(a) === b]
```

```js
// A. false true
// B. true true
// C. false false
// D. other
```

```
Functions have a prototype property but other objects don't so a.prototype is undefined. 
Every Object instead has an internal property accessible via Object.getPrototypeOf
```

## 30

```js
function f() {}
var a = f.prototype, b = Object.getPrototypeOf(f);
a === b
```

```
// A. true
// B. false
// C. null
// D. other
```

```
f.prototype is the object that will become the parent of any objects created with new f while Object.getPrototypeOf returns the parent in the inheritance hierarchy.
```

## 31

```js
function foo() { }
var oldName = foo.name;
foo.name = "bar";
[oldName, foo.name]
```

```
// A. error
// B. ["", ""]
// C. ["foo", "foo"]
// D. ["foo", "bar"]
```

```
name is a read only property. Why it doesn't throw an error when assigned, I do not know.
```

### 32

```js
"1 2 3".replace(/\d/g, parseInt)
```

```
// A. "1 2 3"
// B. "0 1 2"
// C. "NaN 2 3"
// D. "1 NaN 3"
```

```
String.prototype.replace invokes the callback function with multiple arguments where the first is the match, then there is one argument for each capturing group, then there is the offset of the matched substring and finally the original string itself. so parseInt will be invoked with arguments [1, 0], [2, 2], [3, 4].
```

```
如果replaceValue是一个函数的话那么，这个函数的arguments会有n+3个参数（n为正则匹配到的次数），参数为：
1. 匹配到的字符串
2. 如果正则使用了分组匹配就为多个否则无此参数。
3. 匹配字符串的对应索引位置
4. 原始字符串
```

### 33

```js
function f() {}
var parent = Object.getPrototypeOf(f);
f.name // ?
parent.name // ?
typeof eval(f.name) // ?
typeof eval(parent.name) //  ?
```

```
// A. "f", "Empty", "function", "function"
// B. "f", undefined, "function", error
// C. "f", "Empty", "function", error
// D. other
```

```
The function prototype object is defined somewhere, has a name, can be invoked, but it's not in the current scope.
```

### 34 

```js
var lowerCaseOnly =  /^[a-z]+$/;
[lowerCaseOnly.test(null), lowerCaseOnly.test()]
```

```
// A. [true, false]
// B. error
// C. [true, true]
// D. [false, true]
```

```
the argument is converted to a string with the abstract ToString operation, so it is "null" and "undefined".
```

### 35

```js
[,,,].join(", ")
```

```
// A. ", , , "
// B. "undefined, undefined, undefined, undefined"
// C. ", , "
// D. ""
```

```
JavaScript allows a trailing comma when defining arrays, so that turns out to be an array of three undefined.
```

### 36

```js
var a = {class: "Animal", name: 'Fido'};
a.class
```

```
// A. "Animal"
// B. Object
// C. an error
// D. other
```

```
The answer is: it depends on the browser. class is a reserved word, but it is accepted as a property name by Chrome, Firefox and Opera. It will fail in IE. On the other hand, everybody will accept most other reserved words (int, private, throws etc) as variable names too, while class is verboten.
```

### 37

```js
var a = new Date("epoch")
```

```
// A. Thu Jan 01 1970 01:00:00 GMT+0100 (CET)
// B. current time
// C. error
// D. other
```

```
You get "Invalid Date", which is an actual Date object (a instanceof Date is true). But invalid. This is because the time is internally kept as a Number, which in this case it's NaN.
```

