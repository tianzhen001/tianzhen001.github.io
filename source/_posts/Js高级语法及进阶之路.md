---
title: Js高级语法及进阶之路
tags: 
- Js高级
- javascript
categories: js
summary: 提升自己---你必须要知道的js高级语法
abbrlink: 2a35
date: 2019-07-26 21:13:36
---

## 推荐学习资源

- JavaScript 高级程序设计（第三版）
  + 前端的红宝书
  + 建议每一个前端都完整的看一遍
- JavaScript面向对象编程指南（第2版）
- JavaScript面向对象精要
- JavaScript 权威指南
- JavaScript 语言精粹
- 你不知道的 JavaScript



# 面向对象编程

![面向对象编程](./images/mxdxkf.png)



## 什么是对象？

> Everything is object （万物皆对象）

![万物皆对象](./images/20160823024542444.jpg)

对象到底是什么，我们可以从两次层次来理解。

**(1) 对象是具体事物的抽象。**

一本书、一辆汽车、一个人都可以是对象，当实物被抽象成对象，实物之间的关系就变成了对象之间的关系，从而就可以模拟现实情况，针对对象进行编程。

问： 书是对象吗

**(2)对象是无序键值对的集合，其属性可以包含基本值、对象或者函数**

每个对象都是基于一个引用类型创建的，这些类型可以是系统内置的原生类型，也可以是开发人员自定义的类型。



## 什么是面向对象？

面向对象编程 —— `Object Oriented Programming`，简称 OOP ，是一种编程开发思想。

在面向对象程序开发思想中，每一个对象都是功能中心，具有明确分工，可以完成接受信息、处理数据、发出信息等任务。
因此，面向对象编程具有灵活、代码可复用、高度模块化等特点，容易维护和开发，比起由一系列函数或指令组成的传统的过程式编程（procedural programming），更适合多人合作的大型软件项目。



## 面向对象编程的三大特性：

- 封装性
  - 将功能的具体实现，全部封装到对象的内部，外界使用对象时，只需要关注对象提供的方法如何使用，而不需要关心对象的内部具体实现，这就是封装。
- 继承性
  - 在js中，继承的概念很简单，一个对象没有的一些属性和方法，另外一个对象有，拿过来用，就实现了继承。
  - **注意：在其他语言里面，继承是类与类之间的关系，在js中，是对象与对象之间的关系。**
- [多态性]
  - 多态是在强类型的语言中才有的。js是弱类型语言，所以JS不支持多态

# 对象、原型、原型链

## 创建对象的方式

### 内置构造函数创建

我们可以直接通过 `new Object()` 创建：

```javascript
//在js中，对象有动态特性，可以随时的给一个对象增加属性或者删除属性。
var person = new Object()
person.name = 'Jack'
person.age = 18

person.sayName = function () {
  console.log(this.name)
}
```
缺点：麻烦，每个属性都需要添加。

### 对象字面量创建

```javascript
var person = {
  name: 'Jack',
  age: 18,
  sayName: function () {
    console.log(this.name)
  }
}
```
缺点：如果要批量生成多个对象，应该怎么办?代码很冗余

### 简单改进：工厂函数

我们可以写一个函数，解决代码重复问题：

```javascript
function createPerson (name, age) {
  return {
    name: name,
    age: age,
    sayName: function () {
      console.log(this.name)
    }
  }
}
```
然后生成实例对象：

```javascript
var p1 = createPerson('Jack', 18)
var p2 = createPerson('Mike', 18)
```
缺点：但却没有解决对象识别的问题，创建出来的对象都是Object类型的。

### 继续改进：构造函数

构造函数是一个函数，用于实例化对象，需要配合new操作符使用。

```javascript
function Person (name, age) {
  this.name = name
  this.age = age
  this.sayName = function () {
    console.log(this.name)
  }
}

var p1 = new Person('Jack', 18)
p1.sayName() // => Jack

var p2 = new Person('Mike', 23)
p2.sayName() // => Mike
```

而要创建 `Person` 实例，则必须使用 `new` 操作符。
以这种方式调用构造函数会经历以下 4 个步骤：

1. 创建一个新对象
2. 将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）
3. 执行构造函数中的代码
4. 返回新对象

**构造函数需要配合new操作符使用才有意义，构造函数首字母都需要大写**

### 构造函数的缺点

使用构造函数带来的最大的好处就是创建对象更方便了，但是其本身也存在一个浪费内存的问题：

```javascript
function Person (name, age) {
  this.name = name
  this.age = age
  this.type = 'human'
  this.sayHello = function () {
    console.log('hello ' + this.name)
  }
}

var p1 = new Person('lpz', 18)
var p2 = new Person('Jack', 16)
console.log(p1.sayHello === p2.sayHello) // => false
```


解决方案：

```javascript
function sayHello() {
  console.log('hello ' + this.name)
}

function Person (name, age) {
  this.name = name
  this.age = age
  this.type = 'human'
  this.sayHello = sayHello
}

var p1 = new Person('lpz', 18)
var p2 = new Person('Jack', 16)

console.log(p1.sayHello === p2.sayHello) // => true
```
缺点：会暴漏很多的函数，容易造成全局变量污染。



## 原型

### 原型基本概念

Javascript 规定，每一个函数都有一个 `prototype` 属性，指向另一个对象。
这个对象的所有属性和方法，都会被构造函数的实例继承。

这也就意味着，我们可以把所有对象实例需要共享的属性和方法直接定义在 `prototype` 对象上。

```javascript
function Person (name, age) {
  this.name = name
  this.age = age
}

console.log(Person.prototype)

Person.prototype.type = 'human'

Person.prototype.sayName = function () {
  console.log(this.name)
}

var p1 = new Person(...)
var p2 = new Person(...)

console.log(p1.sayName === p2.sayName) // => true
```

这时所有实例的 `type` 属性和 `sayName()` 方法，其实都是同一个内存地址

### 构造函数、实例、原型三者之间的关系

构造函数：构造函数就是一个函数，配合new可以新建对象。

实例：通过构造函数实例化出来的对象我们把它叫做构造函数的实例。一个构造函数可以有很多实例。

原型：每一个构造函数都有一个属性`prototype`，函数的prototype属性值就是原型。通过构造函数创建出来的实例能够直接使用原型上的属性和方法。

<img src="./images/原型三角关系.jpg" alt="">

思考：内置对象中，有很多的方法，这些方法存在哪里？

### `__proto__`

任意一个对象，都会有`__proto__`属性，这个属性指向了构造函数的prototype属性，也就是原型对象。

获取原型对象：

+ 通过`构造函数.prototype`可以获取
+ 通过`实例.__proto__`可以获取（隐式原型）
+ 它们指向了同一个对象`构造函数.prototype === 实例.__proto__`



**注意：`__proto__`是浏览器的一个隐藏（私有）属性，IE浏览器不支持，不要通过它来修改原型里的内容，如果要修改原型中的内容，使用构造函数.prototype去修改**

### constructor属性

默认情况下，原型对象中只包含了一个属性：constructor，constructor属性指向了当前的构造函数。

![](./images/sanjiao.png)

## 原型链

### 原型链概念

任何一个对象，都有原型对象，原型对象本身又是一个对象，所以原型对象也有自己的原型对象，这样一环扣一环就形成了一个链式结构，我们把这个链式结构称为：原型链。

绘制对象的原型链结构：

```javascript
//1. var p = new Person();
//2. var o = new Object();
//3. var arr = new Array();
//4. var date = new Date();
//5. Math
//6. 查看一个div的原型链结构
```

总结：Object.prototype是原型链的尽头，Object.prototype的原型是null。

![](./images/proto.png)



### 属性查找原则

如果是获取操作

1. 会先在自身上查找，如果没有
2. 则根据`__proto__`对应的原型去找，如果没有
3. 一直找到`Object.prototyp`，如果没有，那就找不到了。



测试题1

```js
function Person(name) {
  this.name = name;
}

Person.prototype.name = "ls";
Person.prototype.money = 100;

var p = new Person("zs");

console.log(p.name);
console.log(p.money);
```

测试题2

```js
function Person(name) {
  this.name = name
}

Person.prototype.name = "ls";
Person.prototype.money = 100;

var p = new Person();

console.log(p.name);
console.log(p.money);
```

测试题3

```js
function Person(name) {
  if (name) {
    this.name = name;
  }
}

Person.prototype.name = "ls";
Person.prototype.money = 100;

var p = new Person();

console.log(p.name);
console.log(p.money);
```



### 属性设置原则

只会修改对象自身的属性，如果自身没有这个属性，那么就会添加这个属性，并不会修改原型中的属性。

```js
function Person() {
  
}

var p = new Person();
Person.prototype.money = 200;

console.log(p.money);

p.money = 300;

console.log(p.money);

console.log(Person.prototype.money);
```



# 函数进阶



## 作用域

> 作用域：变量起作用的区域，也就是说：变量定义后，可以在哪个范围内使用该变量。

```javascript
var num = 11;//全局变量
function fn(){
  var num1 = 22;//局部变量
  console.log(num);  // 全局变量在任何地方都能访问到
  console.log(num1);  
}
console.log(num);
```

在js里只有全局作用域和函数作用域。

函数作用域是在函数定义的时候作用域就确定下来了，和函数在哪调用无关。

```javascript
var num = 123;
function f1() {
  console.log(num);
}

function f2(){
  var num = 456;
  f1();
}
f2();//打印啥？
```

### 作用域链

> 作用域链：只要是函数，就会形成一个作用域，如果这个函数被嵌套在其他函数中，那么外部函数也有自己的作用域，这个一直往上到全局环境，就形成了一个作用域链。

`变量的搜索原则`：

1. 从当前作用域开始查找是否声明了该变量，如果存在，那么就直接返回这个变量的值。
2. 如果不存在，就会往上一层作用域查询，如果存在，就返回。
3. 如果不存在，一直查询到全局作用域，如果存在，就返回。如果在全局中也没有找到该变量会报错

### 作用域链练习

```javascript
// 1 
var num = 10;
fn1();
function fn1() {
  console.log(num);  // ?
  var num = 20;
  console.log(num);  // ?
}
console.log(num);    // ?


// 2 -- 改造上面的面试题
var num = 10;
fn1();
function fn1() {
  console.log(num);  // ?
  num = 20;
  console.log(num);  // ?
}
console.log(num);    // ?


// 3
var num = 123
function f1(num) {
    console.log(num) // ?
}

function f2() {
    var num = 456
    f1(num)
}
f2()


// 4
var num1 = 10;
var num2 = 20;
function fn(num1) {
  num1 = 100;
  num2 = 200;
  num3 = 300;
  console.log(num1);
  console.log(num2);
  console.log(num3);
  var num3;
}
fn();
console.log(num1);
console.log(num2);
console.log(num3);
```



## 函数的四种调用模式

> 根据函数内部this的指向不同，可以将函数的调用模式分成4种

1. 函数调用模式
2. 方法调用模式
3. 构造函数调用模式
4. 上下文调用模式（借用方法模式）

```javascript
函数：当一个函数不是一个对象的属性时，我们称之为函数。
方法：当一个函数被保存为对象的一个属性时，我们称之为方法。
```



### 函数调用模式

<font color="red">如果一个函数不是一个对象的属性时，就是被当做一个函数来进行调用的。此时this指向了window</font>

```javascript
function fn(){
  console.log(this);//指向window
}
fn();
```



### 方法调用模式

<font color="red">当一个函数被保存为对象的一个属性时，我们称之为一个方法。当一个方法被调用时，this被绑定到当前对象</font>

```javascript
var obj = {
  sayHi:function(){
    console.log(this);//在方法调用模式中，this指向调用当前方法的对象。
  }
}
obj.sayHi();
```



### 构造函数调用模式

<font color="red">如果函数是通过new关键字进行调用的，此时this被绑定到创建出来的新对象上。</font>

```javascript
function Person(){
  console.log(this);
}
Person();//this指向什么？
var p = new Person();//this指向什么？
```

**总结：分析this的问题，主要就是区分函数的调用模式，看函数是怎么被调用的。**

+ 猜猜看：

```javascript
// 分析思路：1. 看this是哪个函数的  2. 看这个函数是怎么调用的，处于什么调用模式

// 1
var age = 38;
var obj = {
    age: 18,
    getAge: function () {
        console.log(this.age);
    }
}

var f = obj.getAge;
f();//???


// 2
var age = 38;
var obj = {
  age:18,
  getAge:function () {
    console.log(this.age);//???
    function foo(){
      console.log(this.age);//????
    }
    foo();
  }
}
obj.getAge();
obj["getAge"]();


// 3
var length = 10

function fn() {
    console.log(this.length)
}
var obj = {
    length: 5,
    method: function (fn) {
        fn() 
        arguments[0]();
    }
}
obj.method(fn, 10, 5);
```

几种特殊的this指向

+ 定时器中的this指向了window，因为定时器的function最终是由window来调用的。
+ 事件中的this指向的是当前的元素，在事件触发的时候，浏览器让当前元素调用了function



### 上下文调用模式

> 上下文调用模式也叫方法借用模式，分为apply与call
>
> 使用方法： 函数.call() 或者 函数.apply()


#### call方法

call方法可以调用一个函数，并且可以指定这个函数的`this`指向

```javascript
//所有的函数都可以使用call进行调用
//参数1：指定函数的this，如果不传，则this指向window
//其余参数：和函数的参数列表一模一样。
//说白了，call方法也可以和()一样，进行函数调用，call方法的第一个参数可以指定函数内部的this指向。
fn.call(thisArg, arg1, arg2, arg2);
```

+ 借用对象的方法

#### 伪数组与数组

> 伪数组也叫类数组

1. 伪数组其实就是一个对象，但是跟数组一样，伪数组也会有`length`属性，也有`0,1,2,3`等属性。
2. 伪数组并没有数组的方法，不能使用`push/pop`等方法
3. 伪数组可以跟数组一样进行遍历，通过下标操作。
4. 常见的伪数组：`arguments`、`document.getElementsByTagName的返回值`、`jQuery对象`

```javascript
var arrayLike = {
  0:"张三",
  1:"李四",
  2:"王五",
  length:3
}
//伪数组可以和数组一样进行遍历
```

- 伪数组借用数组的方法

```javascript
Array.prototype.push.call(arrLike, "赵六");
```



#### apply方法

`apply()`方法的作用和 `call()`方法类似，只有一个区别，就是`apply()`方法接受的是**一个包含多个参数的数组**。而`call()`方法接受的是**若干个参数的列表**

call和apply的使用场景：

+ 如果参数比较少，使用call会更加简洁
+ 如果参数存放在数组中，此时需要使用apply



#### bind方法   

**bind()**方法创建一个新的函数, 可以绑定新的函数的`this`指向

```javascript
// 返回值：新的函数
// 参数：新函数的this指向，当绑定了新函数的this指向后，无论使用何种调用模式，this都不会改变。
var newFn = fn.bind(window);
```



## 递归函数

> 递归函数：函数内部直接或者间接的调用自己

递归的要求：

1. 自己调用自己（直接或者间接）
2. 要有结束条件（出口）

递归函数主要是`化归思想` ,将一个复杂的问题简单化，主要用于解决数学中的一些问题居多。

+ 把要解决的问题，归结为已经解决的问题上。
+ 一定要考虑什么时候结束让函数结束，也就是停止递归（一定要有已知条件）



# 闭包

## 闭包的基本概念

`闭包（closure）`是JavaScript语言的一个难点，也是JavaScript的一个特色，很多高级的应用都要依靠闭包来实现。



### 闭包的概念

> 在[计算机科学](https://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)中，**闭包**（英语：Closure），又称**词法闭包**（Lexical Closure）或**函数闭包**（function closures），是引用了自由变量的函数 

在JavaScript中，在函数中可以（嵌套）定义另一个函数时，如果内部的函数引用了外部的函数的变量，则产生闭包。

产生闭包的条件

```js
当内部函数访问了外部函数的变量的时候，就会形成闭包。
```



## 闭包的作用

> 保护私有变量不被修改

### 计数器

需求：统计一个函数的调用次数

```javascript
var count = 0;
function fn(){
  count++;
  console.log("我被调用了，调用次数是"+count);
}
fn();
fn();
fn();
```

缺点：count是全局变量，不安全。

使用闭包解决这个问题！！！！

```javascript
function outer(){
  var count = 0; // 私有变量, 将count保护起来了
  function add(){
    count++;
    console.log("当前count"+count);
  }
  return add;
}

var result = outer();
result();
```



## 闭包经典面试题

1. 点击按钮, 打印当前按钮的下标

2. ```js
   function fn() {
       var a = 0;
   
       return function() {
           a++;
           console.log(a);
       }
   
   }
   
   var f1 = fn();
   
   f1();
   f1();
   f1();
   
   var f2 = fn();
   f2();
   f2();
   f2();
   f2();
   ```

3. ```js
   function f1(){
       var name="abc";
       
       return function(){
           console.log(name);
       }
       
   }
   var fn = f1();
   
   var name="张三";
   
   fn();
   ```



# 继承

> 现实生活中的继承，子承父业, 如儿子继承了父辈的财产，公司等
>
> js中的继承，一个对象可以使用另一个对象中的方法或属性

继承的目的:  方便代码的复用

```js
var lw = {
    skill: "翻墙",
	age: 28
}

function Person(){}
var xm = new Person();

// 如何实现让xm可以使用到lw对象上的skill属性???
xm.skill; // ==> 翻墙
```

## JS常见的几种继承模式



### 原型链继承

一个对象可以访问构造函数的原型中的属性和方法，那么如果想要让一个对象增加某些属性和方法，只需要把这些属性和方法放到原型对象中即可。这样就实现了继承, 称之为原型链继承

- 直接给原型增加属性和方法
- 原型替换（注意：constructor）

```js
var Person = function() {};

Person.prototype.n = 1;
var p1 = new Person();

Person.prototype = {
    n: 2,
    m: 3
}
var p2 = new Person();

console.log(p1.n);
console.log(p1.m);

console.log(p2.n);
console.log(p2.m);
```





## 借用构造函数继承

> 从构造函数中继承到构造函数中的成员

```js
function Person(name, age, gender){
    this.name = name;
    this.age = age;
    this.gender = gender;
}

function Chinese(name, age, gender, skin){
    this.name = name;
    this.age = age;
    this.gender = gender;
    // 以上代码重复，在构造函数Person中已经给this添加
    this.skin = skin;
}

// 重写构造函数Chinese
function Chinese(name, age, gender, skin){
    // 借用Person构造函数，继承到name、age、gender属性
    Person.call(this, name, age, gender);
    this.skin = skin;
}
```



## 组合继承

> 借用构造函数 + 原型链继承组合在一起使用

```js
function Person(name, age, gender){
    this.name = name;
    this.age = age;
    this.gender = gender;
}

// Person原型上的sayHi方法
Person.prototype.sayHi = function(){
    console.log("hello, 我是" + this.name);
}

function Chinese(name, age, gender, skin){
    // 借用Person构造函数，继承到name、age、gender属性
    Person.call(this, name, age, gender);
    this.skin = skin;
}

var xm = new Chinese("xm", 20, "male", "黄色");
// xm有name/age/gender属性从Person构造函数中继承到的

// 那么如何让Chinese的实例对象xm去继承到Person原型上的sayHi方法???
xm.sayHi();
```

```js
function Person() {
    this.name = "is me";
    this.color = ["lime", "red"];
}

function Chinese() {

}
Chinese.prototype = new Person();

var c1 = new Chinese();
var c2 = new Chinese();

c1.name = "change";
c1.color.push("black");

console.log(c2.name);
console.log(c2.color);
```



# 正则表达式

> 正则表达式：用于匹配规律规则的表达式，正则表达式最初是科学家对人类神经系统的工作原理的早期研究，现在在编程语言中有广泛的应用，经常用于表单校验，高级搜索等。

## 创建正则表达式

【07-正则表达式的创建.html】

构造函数的方式

```javascript
var regExp = new RegExp(/\d/);
```

正则字面量

```javascript
var regExp = /\d/;
```

正则的使用

```javascript
/\d/.test("aaa1");
```

## 元字符

> 正则表达式由一些普通字符和元字符组成，普通字符包括大小写字母、数字等，而元字符则具有特殊的含义。

### 常见元字符

`|`表示或，优先级最低

`()`优先级最高，表示分组

### 字符类的元字符

`[]`在正则表达式中表示一个字符的位置，[]里面写这个位置可以出现的字符。

```javascript
console.log(/[abc]/);//匹配a,b,c
```

`[^]`在中扩号中的^表示非的意思。

```javascript
//^表示该位置不可以出现的字符
console.log(/[^abc]/);//匹配除了a，b，c以外的其他字符
```

`[a-z]` `[1-9]`表示范围

```javascript
console.log(/[a-z]/.test("d"));//小写字母
console.log(/[A-Z]/.test("d"));//大写字母
console.log(/[0-9]/.test("8"));//数字
console.log(/[a-zA-Z0-9]/);//所有的小写字母和大写字母以及数字
```

### 边界类元字符

> 我们前面学习的正则只要有满足的条件的就会返回true，并不能做到精确的匹配。

【12-正则边界.html】

^表示开头   ***[]里面的^表示取反***

$表示结尾

```javascript
console.log(/^chuan/.test("dachuan"));//必须以chuan开头
console.log(/chuan$/.test("chuang"));//必须以chuan结尾
console.log(/^chuan$/.test("chuan"));//精确匹配chuan

//精确匹配chuan,表示必须是这个
console.log(/^chuan$/.test("chuanchuan"));//fasle
```

### 量词类元字符

> 量词用来控制出现的次数，一般来说量词和边界会一起使用

【13-正则量词.html】

1. `*`表示能够出现0次或者更多次，x>=0;
2. `+`表示能够出现1次或者多次，x>=1
3. `?`表示能够出现0次或者1次，x=0或者x=1
4. `{n}`表示能够出现n次
5. `{n,}`表示能够出现n次或者n次以上
6. `{n,m}`表示能够出现n-m次

思考：如何使用{}来表示*+? 

## 正则的使用

### 正则测试

2. 验证QQ

   - 只能是数字
   - 开头不能是0
   - 长度为5-11位

   ```javascript
   var qqReg = /^[1-9]\d{4,10}$/;
   ```

3. 验证手机

   - 11位数字组成
   - 号段13[0-9] 147 15[0-9] 17[0178] 18[0-9]

   ```javascript
   var mobileReg = /^(13[0-9]|147|15[0-9]|17[0178]|18[0-9])\d{8}$/;
   ```

4. 验证邮箱

   - 前面是字母或者数字
   - 必须有@
   - @后面是字母或者数字
   - 必须有.
   - .后面是字母或者数字

   ```javascript
   var emailReg = /^\w+@\w+(\.\w+)+$/;
   ```

5. 验证姓名

   - 只能是汉字
   - 长度2-6位之间
   - 汉字范围[\u4e00-\u9fa5]

   ```javascript
   var nameReg = /^[\u4e00-\u9fa5]{2,6}$/;
   ```
