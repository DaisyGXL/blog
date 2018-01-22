title: javascript常见问题汇总（一）
date: 2018-01-16
categories: javascript
tags:
- javascript

---

本文用于记录平时浏览资料或者工作过程中遇到的js关键知识点。

<!-- more -->

## 类型和类型转换
在JavaScript中有7个内置类型：null，undefined，boolean，number，string，object，和symbol(ES6)。
除了object外，其他的都叫做基本类型，可以通过typeof查看对应数据的类型。
```
typeof 0              // "number"
typeof true           // "boolean"
typeof 'hi'           // "string"
typeof Math           // "object"
typeof null           // "object"
typeof Symbol('Hi')   // "symbol"
```

### Null vs. undefined
大多数计算机语言，有且仅有一个表示"无"的值，比如，C语言的NULL，Java语言的null，Python语言的None，Ruby语言的nil。
有点奇怪的是，JavaScript语言居然有两个表示"无"的值：undefined和null。
**相似性**
在JavaScript中，将一个变量赋值为undefined或null，老实说，几乎没区别。
```javascript
var a = undefined;
var a = null;
```
上面代码中，a变量分别被赋值为undefined和null，这两种写法几乎等价。
undefined和null在if语句中，都会被自动转为false，相等运算符甚至直接报告两者相等。
**差别**
**null表示"没有对象"，即该处不应该有值。** 
- 作为函数的参数，表示该函数的参数不是对象。
- 作为对象原型链的终点。
**undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。**
- 变量被声明了，但没有赋值时，就等于undefined。
- 调用函数时，应该提供的参数没有提供，该参数等于undefined。
- 对象没有赋值的属性，该属性的值为undefined。
- 函数没有返回值时，默认返回undefined。

### 隐式转换
在if语句的条件判断中，""，0， null，undefined, NaN, false 会自动转换为false。其它的如空数组、对象、函数定义都会自动转换为真。
```
Boolean(null)         // false
Boolean('hello')      // true 
Boolean('0')          // true 
Boolean(' ')          // true 
Boolean([])           // true 
Boolean(function(){}) // true
```

### String & Number之间的转换
首先你要非常小心的是 + 操作符。因为它同时用于数字相加和字符串拼接。
*,/,-只用于数字运算，当这些操作符和字符串一起使用，那么字符串会被强制转换为数字。
```
1 + "2" = "12"
"" + 1 + 0 = "10"
"" - 1 + 0 = -1
"-9\n" + 5 = "-9\n5"
"-9\n" - 5 = -14
"2" * "3" = 6
4 + 5 + "px" = "9px"
"$" + 4 + 5 = "$45"
"4" - 2 = 2
"4px" - 2 = NaN
null + 1 = 1
```

### == vs. ===
一个广泛被接受的认知就是：==判断值是否相等，===同时判断值是否相等和类型是否相同。但是，这里有些误解。
实际上，==在验证相等性的时候，会对类型不同的值做一个类型转换。===对要判断的值不做类型转换。
```
2 == '2'            // True
2 === '2'           // False
undefined == null   // True
undefined === null  // False
'0' == false        // true
false == ""         // true
false == []         // true
false == {}         // false
"" == 0             // true
"" == []            // true
"" == {}            // false
0 == []             // true
0 == {}             // false
0 == null           // false
```

### 值 vs. 引用
对于基本类型的值，赋值是通过值拷贝的形式；比如：null，undefined，boolean，number，string和ES6的symbol。
对于复杂类型的值，通过引用拷贝的形式赋值。比如：对象、对象包括数组和函数。如果想对复杂类型的值进行拷贝，需要自己去对子元素进行拷贝。
```javascript
var a = 2;        // 'a' hold a copy of the value 2.
var b = a;        // 'b' is always a copy of the value in 'a'
b++;
console.log(a);   // 2
console.log(b);   // 3
var c = [1,2,3];
var d = c;        // 'd' is a reference to the shared value
d.push( 4 );      // Mutates the referenced value (object)
console.log(c);   // [1,2,3,4]
console.log(d);   // [1,2,3,4]
/* Compound values are equal by reference */
var e = [1,2,3,4];
console.log(c === d);  // true
console.log(c === e);  // false

const copy = c.slice()    // 'copy' 即使copy和c相同，但是copy指向新的值
console.log(c);           // [1,2,3,4]
console.log(copy);        // [1,2,3,4]
console.log(c === copy);  // false
```

## 作用域(Scope)
作用域值程序的执行环境，它包含了在当前位置可访问的变量和函数。
**全局作用域**是最外层的作用域，在函数外面定义的变量属于全局作用域，可以被任何其他子作用域访问。在浏览器中，window对象就是全局作用域。
**局部作用域**是在函数内部的作用域。在局部作用域定义的变量只能在该作用域以及其子作用域被访问。

## 提升
在编译过程中，将var和function的定义移动到他们作用域最前面的行为叫做提升。
**整个函数定义会被提升**
```javascript
console.log(toSquare(3));  // 9

function toSquare(n){
  return n*n;
}
```
**变量只会被部分提升(let和const不会被提升)。而且只有变量的声明会被提升，赋值不会动**
```javascript
{  /* Original code */
  console.log(i);  // undefined
  var i = 10
  console.log(i);  // 10
}

{  /* Compilation phase */
  var i;
  console.log(i);  // undefined
  i = 10
  console.log(i);  // 10
}
// ES6 let & const
{
  console.log(i);  // ReferenceError: i is not defined
  const i = 10
  console.log(i);  // 10
}
{
  console.log(i);  // ReferenceError: i is not defined
  let i = 10
  console.log(i);  // 10
}
```

### 函数表达式和函数声明
- 函数表达式
一个函数表达式是在函数执行到函数表达式定义的位置才开始创建，并被使用。它不会被提升。
```javascript
var sum = function(a, b) {
  return a + b;
}
```
- 函数声明
函数声明的函数可以在文件中任意位置调用，因为它会被提升。
```javascript
function sum(a, b) {
  return a + b;
}
```
- 立即执行函数
立即执行函数，正如它的名字，就是创建函数的同时立即执行。它没有绑定任何事件，也无需等待任何异步操作,包围它的一对括号将其转换为一个表达式，紧跟其后的一对括号调用了这个函数。显而易见，它不会被提升。
立即执行函数可以用来实现模块化。
```javascript
(function() {
     // 代码
     // ...
})();
```

### 变量let,var,const
- 在ES6之前，只能使用var来声明变量。在一个函数体中声明的变量和函数，周围的作用域内无法访问。在块作用域if和for中声明的变量，可以在if和for的外部被访问。如果没有使用var,let或则const关键字声明的变量将会绑定到全局作用域上。
- ES6的let和const都是新引入的关键字。它们不会被提升，而且是块作用域。也就是说被大括号包围起来的区域声明的变量外部将不可访问。
- 使用const声明的变量，其值不可更改。准确地说它不可以被重新赋值，但是可以更改。
```javascript
const tryMe = 'initial assignment';
tryMe = 'this has been reassigned';  // TypeError: Assignment to constant variable.
// You cannot reassign but you can change it…
const array = ['Ted', 'is', 'awesome!'];
array[0] = 'Barney';
array[3] = 'Suit up!';
console.log(array);     // [“Barney”, “is”, “awesome!”, “Suit up!”]
const airplane = {};
airplane.wings = 2;
airplane.passengers = 200;
console.log(airplane);   // {passengers: 200, wings: 2}
```

## 闭包
在前端面试中，闭包基本上是一个必问基础知识，然而一直以来我对闭包的理解都处于表面。
那么什么是闭包呢？
对于闭包(closure)，当外部函数返回之后，内部函数依然可以访问外部函数的变量。
```javascript
function f1() {
    var N = 0; // N是f1中的局部变量

    // 内部函数f2中使用了外部函数f1中的变量N
    function f2() {
        N += 1;
        console.log(N);
    }

    return f2;
}

var result = f1();

result(); // 输出1
result(); // 输出2
result(); // 输出3
```
代码中，外部函数f1只执行了一次，变量N设为0，并将内部函数f2赋值给了变量result。由于外部函数f1已经执行完毕，其内部变量N应该在内存中被清除，然而事实并不是这样：我们每次调用result的时候，发现变量N一直在内存中，并且在累加。为什么呢？这就是闭包的神奇之处了！

### 使用闭包定义私有变量
```javascript
function Product() {

	var name;

    this.setName = function(value) {
        name = value;
    };

    this.getName = function() {
        return name;
    };
}

var p = new Product();
p.setName("hello");

console.log(p.name); // 输出undefined
console.log(p.getName()); // 输出hello
```
对象p的的name属性为私有属性，使用p.name不能直接访问。

## 继承
JavaScript仅支持通过prototype属性进行继承属性和方法。每个JavaScript构造函数都有一个prototype属性，用于设置所有实例对象需要共享的属性和方法。
```javascript
function Rectangle(x, y)
{
    this._length = x;
    this._breadth = y;
}

Rectangle.prototype.getDimensions = function()
{
    return {
        length: this._length,
        breadth: this._breadth
    };
};

var x = new Rectangle(3, 4);
var y = new Rectangle(4, 3);

console.log(x.getDimensions()); // { length: 3, breadth: 4 }
console.log(y.getDimensions()); // { length: 4, breadth: 3 }
```

## 柯里化
我们可以一次性传入多个参数调用它；也可以只传入一部分参数来调用它，让它返回一个函数去处理剩下的参数。
```javascript
var add = function(x) {
    return function(y) {
        return x + y;
    };
};

console.log(add(1)(1)); // 输出2

var add1 = add(1);
console.log(add1(1)); // 输出2

var add10 = add(10);
console.log(add10(1)); // 输出11
```

