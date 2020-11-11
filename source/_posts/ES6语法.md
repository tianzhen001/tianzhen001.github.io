---
title: ES6语法
date: 2020-2-11 21:03:54
tags: ES6
categories: 语法
summary:  你知道ES6有哪些新语法吗,本篇文章将介绍一些常用的语法

---
# 基本介绍

> ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的下一代标准，已经在 2015 年 6 月正式发布了。它的目标，是使得 JavaScript 语言可以用来编写复杂的大型应用程序，成为企业级开发语言。


## javascript的诞生

+  1992年底，美国国家超级电脑应用中心（NCSA）开始开发一个独立的浏览器，叫做 Mosaic。这是人类历史上第一个浏览器，从此网页可以在图形界面的窗口浏览。 
+  1994年10月，  Netscape （网景）公司，就是在 Mosaic 的基础上，开发面向普通用户的新一代的浏览器 Netscape Navigator。 
+  1994年12月，Navigator 发布了1.0版，市场份额一举超过90%。 

+ Netscape 公司很快发现，Navigator 浏览器需要一种可以嵌入网页的脚本语言，用来控制浏览器行为。 因为当时网速特别的慢，很多表单的校验不适合放在服务器端进行校验。管理层对于这种脚本语言的设想是：**功能不需要太强大，语法要简单，容易学习和使用**
+  1995年，Netscape 公司雇佣了程序员`Brendan Eich 布兰登·艾奇` 开发这种网页脚本语言。 

+ 1995年5月，Brendan Eich 只用了10天，就设计完成了这种语言的第一版，叫做`LiveScript`。它是一个大杂烩，语法有多个来源。
  + 基本语法：借鉴 C 语言和 Java 语言。
  + 数据结构：借鉴 Java 语言，包括将值分成原始值和对象两大类。
  + 函数的用法：借鉴 Scheme 语言和 Awk 语言，将函数当作第一等公民，并引入闭包。
  + 原型继承模型：借鉴 Self 语言（Smalltalk 的一种变种）。
  + 正则表达式：借鉴 Perl 语言。
  + 字符串和数组处理：借鉴 Python 语言。
+  为了保持简单，这种脚本语言缺少一些关键的功能，比如块级作用域、模块、子类型（subtyping）等等，但是可以利用现有功能找出解决办法。 **后果：对于其他的编程语言，你需要只需要学习该语言提供的各种语法即可。对于javascript，你需要学习解决问题的各种模式，比如原型，原型链实现继承，闭包等等**
+ 1995年 12月，Netscape 公司与 Sun 公司合作，后者允许将这种语言叫做 JavaScript。把LiveScript改名成了Javascript
+ **注意：javascript实质上和java没有什么关系**

![](./images/java和javascript的区别.png)

## ECMAScript与Javascript的关系

1996年11月，JavaScript 的创造者 Netscape 公司，决定将 JavaScript 提交给国际标准化组织ECMA，希望这种语言能够成为国际标准。次年，ECMA 发布 262 号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为 ECMAScript ，这个版本就是1.0版。

该标准从一开始就是针对 JavaScript 语言制定的，但是之所以不叫 JavaScript ，有两个原因。一是商标，Java是 Sun 公司的商标，根据授权协议，只有 Netscape 公司可以合法地使用 JavaScript 这个名字，且 JavaScript 本身也已经被 Netscape 公司注册为商标。二是想体现这门语言的制定者是 ECMA ，不是 Netscape ，这样有利于保证这门语言的开放性和中立性。



ECMAScript，简称ES，是由Ecma国际（欧洲计算机制造商协会,英文名称是European Computer Manufacturers Association）按照标准制定的一种脚本语言规范。

JavaScript是按ECMAScript规范实现的一种脚本语言，JavaScript除了实现了ECMAScript规范，还提供了BOM和DOM的操作。

## ECMAScript版本历史

+ ES1.0, 1997年06月发布
+ ES2.0, 1998年06月发布
+ ES3.0, 1999年12月发布

+ ES4.0,  由于关于语言的复杂性出现了分歧。放弃发布
+ ES5.0, 2009年12月发布， 增加了严格模式，增加了少量语法，为ES6铺路
+ ES6.0, 2015年6月发布，增加了大量的新概念和语法特性
  + **第六版的名字， 可以叫做ECMAScript6.0(ES), 也可以叫做ECMAScript 2015（ES2015）**
  + ECMA组织决定以后每年6月份都会发布一版新的语法标准，比如ES7(ECMAScript 2016) 
  + **通过我们说的ES6泛指ES5之后的下一代标准，涵盖了ES6, ES7, ES8....**  

* 参考书籍： https://es6.ruanyifeng.com/ 



# ES5-数组的新方法

## forEach

`forEach()` 方法对数组的每个元素执行一次提供的函数。功能等同于`for`循环.

应用场景：为一些相同的元素，绑定事件处理器！强调的是数组中每一项都要遍历

作用：

- 1.只能是用来遍历 
- 2.一旦开始了遍历就停不下来
- 3.返回值是undefined  对于我们来说没有用

**需求：遍历数组["张飞","关羽","赵云","马超"]**

```javascript
var arr = ["张飞","关羽","赵云","马超"];
//第一个参数：element，数组的每一项元素
//第二个参数：index，数组的下标
//第三个参数：array，正在遍历的数组
arr.forEach(function(element, index, array){
  console.log(element, index, array);
});
```

## map

`map()` 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

**需求：遍历数组，求每一项的平方存在于一个数组中**

**作用：**

- 1.可以循环遍历数组中的每一项数据
- 2.可以对循环遍历到的每一项数据进行操作
- 3.**==重点应用场景: 对数组中的每一项数据进行操作,比如：给每数组中的每一项添加一个相同的样式==**

```javascript
var arr = [1,2,3,4,5];  // 1 4 9 16 25
//第一个参数：element，数组的每一项元素
//第二个参数：index，数组的下标
//第三个参数：array，正在遍历的数组
//返回值：一个新数组，每个元素都是回调函数的结果。
var newArray = arr.map(function(element, index, array){
  return element * element;
});
console.log(newArray);//[1,4,9,16,25]


// 学了箭头函数之后的简写如下:
var res = nums.map(item => item * item )
```

案例：获取所有的名字

~~~js
let list = [
      { name: 'tom', age: 20 },
      { name: 'rose', age: 21 },
      { name: 'jack', age: 22 },
      { name: 'jerry', age: 19 }
    ]
    let res = list.map(item => item.name)
    console.log(res)
~~~



## filter

`filter`用于过滤掉“不合格”的元素 
返回一个新数组，如果在回调函数中返回true，那么就留下来，如果返回false，就扔掉

**需求：遍历数组，将数组中工资超过5000的值找出来[1000, 5000, 20000, 3000, 10000, 800, 1500]**

**作用:**

- 1.可以循环遍历数组中的每一项
- 2.可以对循环遍历到的数据进行判断
- 3.当条件成立时,使用了return true后会将满足条件的那一项存到一个新的数组当中
- 4.**==重点应用场景:根据条件过滤数组中的数据==**

```javascript
var arr = [1000, 5000, 20000, 3000, 10000, 800, 1500];
//第一个参数：element，数组的每一项元素
//第二个参数：index，数组的下标
//第三个参数：array，正在遍历的数组
//返回值：一个新数组，存储了所有返回true的元素
var newArray = arr.filter(function(element, index, array){
  if(element > 5000) {
    return false;
  }else {
    return true;
  }
});
console.log(newArray);//[1000, 5000, 3000, 800, 1500]

// 学了箭头函数之后的简写如下:
var res = nums.filter(item => item > 5000)
```

## some 

`some`用于遍历数组，如果有至少一个满足条件，就返回true，否则返回false。

**需求：遍历数组，查找数组中的某条数据 比如：查找[10, 20, 30, 40, 50, 60] 中的30

作用:

- 1.可以用来循环遍历数组中的每一项
- 2.在回调函数中进行条件判断，如果return true执行之后，会阻止后续代码的遍历执行
- 3.重点应用场景: **==条件成立时不再执行后续的循环==**

```javascript
 // 查找数组有没有30 如果有，就不要再向下遍历了
    let nums = [10, 20, 30, 40, 50, 60]

    nums.some(function (item, index, arr) {
      // console.log(item,index,arr);
      if (item == 30) {
        return true
      }
      console.log(item);
    })

```

## every

`every`用于遍历数组，只有当所有的元素返回true，才返回true，否则返回false。

**需求：遍历数组，判断==整体==是否都满足条件**

作用：

- 1.可以对数组中的每一项进行遍历，但是只打印第一项
- 2.对数组中的每一项进行判断，都满足条件则返回true,如果有一项不满足条件则返回false
- 3.==应用场景：重点是强调整体的一个处理结果，比如，某班考试成绩是否都合格==

```javascript
// every 比如说: 考试完成之后,判断一下成绩当中是否有不及格的 如果都满足条件则返回true,有一个不满足条件则返回false
    // var score = [100,99,96,93,65,74,41,25,62,18];
    var score = [100,99,62,25,88];

   var flag =  score.every(function(item,index,arr){
      // console.log(item,index,arr);
      return item > 60
    })
    console.log(flag);

// 学了箭头函数之后的简写如下:
var res = nums.every(item => item > 60)
```



## reduce

>  `**reduce()**` 方法对数组中的每个元素执行一个由您提供的**reducer**函数，将其结果汇总为单个返回值 

语法：`reduce(callback, initValue)`

callback: 每个元素都会执行一次的回调函数

initValue: 初始值



callback的4个参数

+  prev： 上一次的值，第一次为初始值
+ item:  当前值
+ index: 下标
+ arr: 数组



案例：计算数组所有值的和

~~~js
let nums = [10, 20, 30, 40, 50, 60]
var res = nums.reduce(function (prev, item) {
      // console.log(prev, item);
      return prev + item
 }, 0)

console.log(res);

// 学了箭头函数之后的简写如下:
var res = nums.reduce((prev,item)=>prev + item)
~~~



# ES6

## 变量

> ES6中提供了两个声明变量的关键字：const和let 

### let的使用

ES6 新增了`let`命令，用来声明变量。它的用法类似于`var`。

- let声明的变量只有在当前作用域有效

```js
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```

- 不存在变量提升

```js
// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```

- 不允许重复声明

```js
let a = 10;
let a = 1;//报错 Identifier 'a' has already been declared
```

### const的使用

`const`声明一个只读的常量。常量：值不可以改变的量 

- const声明的量不可以改变

```js
const PI = 3.1415;
PI = 3; //报错
```

- const声明的变量必须赋值

```js
const num;
```

- 如果const声明了一个对象，仅仅保证地址不变

```js
const obj = {name:'zs'};
obj.age = 18;//正确
obj = {};//报错
```

- 其他用法和let一样

```js
1. 只能在当前代码块中使用
2. 不会提升
3. 不能重复
```

### let与const的使用场景

```js
1. 如果声明的变量不需要改变，那么使用const
2. 如果声明的变量需要改变，那么用let
3. 学了const和let之后，尽量别用var
```

## 解构赋值

### 数组解构

以前，为变量赋值，只能直接指定值。

```javascript
let a = 1;
let b = 2;
let c = 3;
```

ES6 允许写成下面这样。

```javascript
let [a, b, c] = [1, 2, 3];
```

解构默认值

```js
let [a = 0, b, c] = [1, 2, 3];
```

### 对象解构

解构不仅可以用于数组，还可以用于对象。

```javascript
let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
foo // "aaa"
bar // "bbb"
```

如果变量名与属性名不一致，必须写成下面这样。

```javascript
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'
```

函数的参数也可以使用解构赋值。

```javascript
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```

## 字符串

### 模版字符串

传统的 JavaScript 语言，输出模板通常是这样写的（下面使用了 jQuery 的方法）。

```javascript
$('#result').append(
  'There are <b>' + basket.count + '</b> ' +
  'items in your basket, ' +
  '<em>' + basket.onSale +
  '</em> are on sale!'
);
```

上面这种写法相当繁琐不方便，ES6 引入了模板字符串解决这个问题。

```javascript
$('#result').append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);
```

字符串模版的优点

+ 允许换行
+ 可以使用插值  `${}`

### 字符串方法

- **includes()**：返回布尔值，表示是否找到了参数字符串。
- **startsWith()**：返回布尔值，表示参数字符串是否在原字符串的头部。
- **endsWith()**：返回布尔值，表示参数字符串是否在原字符串的尾部。

## 数组

### find

**find是ES6新增的语法**

`find()` 方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)。 

```js
// 获取第一个大于10的数
var array1 = [5, 12, 8, 130, 44];

var found = array1.find(function(element) {
  return element > 10;
});
console.log(found);
```

### findexIndex

**findIndex是ES6新增的语法**

`findIndex()`方法返回数组中满足提供的测试函数的第一个元素的**索引**。否则返回-1。 

```js
// 获取第一个大于10的下标
var array1 = [5, 12, 8, 130, 44];

function findFirstLargeNumber(element) {
  return element > 13;
}

console.log(array1.findIndex(findFirstLargeNumber));
```

## includes



## 函数-箭头函数

ES6标准新增了一种新的函数：Arrow Function（箭头函数）。

为什么叫Arrow Function？因为它的定义用的就是一个箭头：

### 基本使用

```js
var fn = function(x, y) {
    console.log(x + y);
}

相当于
//语法： (参数列表) => {函数体}
var fn = (x, y) => {
    console.log(x + y);
}
```

### 参数详解

- 如果没有参数列表，使用()表示参数列表

```js
var sum = () => {
    console.log('哈哈')
};
// 等同于：
var sum = function() {    
    console.log('哈哈')
};
```

- 如果只有一个参数，可以省略()

```js
// 等同于：
var sum = function(n1) {    
    console.log('哈哈')
};

var sum = n1 => {
    console.log('哈哈')
};

```

- 如果有多个参数，需要使用()把参数列表括起来

```js
var sum = function(n1, n2) {    
    console.log('哈哈')
};

var sum = (n1, n2) => {
    console.log('哈哈')
};
```

### 返回值详解

- 如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来

```js
var sum = function(n1) {    
    console.log('哈哈')
};

var sum = n1 => {
    console.log('哈哈')
};
```

- 如果函数体只有一行一句，那么可以省略{}和return

```js
var fn = function(n1, n2) {
    return n1 + n2;
}

var fn = (n1, n2) => n1 + n2;
```

### 案例

1. 有一个数组`[1,3,5,7,9,2,4,6,8,10]`,请对数组进行排序
2. 有一个数组`['a','ccc','bb','dddd']`,请按照字符串长度对数组进行排序
3. 有一个数组，`[57,88,99,100,33,77]`,请保留60分以上的成绩，返回一个新的数组

### 箭头函数的注意点

1. 箭头函数内部没有this，因此箭头函数内部的this指向了外部的this
2. 箭头函数不能作为构造函数，因为箭头函数没有this

【定义一个对象，定时器打招呼】

**苦口婆心一下：箭头函数刚开始用，肯定会有点不习惯，但是任何东西都有一个习惯的过程，慢慢接受就好了，多用，多练**

### 默认参数

ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。

```javascript
function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello
```

### rest参数

```javascript
function add(...values) {

}

add(2, 5, 3) // 10
```

## 对象

### 简写

属性简写

方法简写

### 展开运算符 ...

扩展运算符（spread）是三个点（`...`）, 可用于 数组 / 对象 /Set 等结构；

下面开始在数组中应用扩展运算符

**（1）简单的应用**

```js
var arr = [1,2,3];

console.log(...arr); // 输出 1,2,3
```



**（2）复制数组**

```js
var arr = [1,2,3];

// 1.使用concat合并数组会返回一个新的数组
// 由于传的是空数组，所以返回的数组元素都是arr的，
// 所以相当于复制出来一份arr数组, 并赋值给arr2
var arr2 = arr.concat([]);

// 2.ES6 的方法
// 把arr数组展开到一个新的空数组中
var arr3 = [...arr];
```



**（3）合并数组**

```js
var arr = [1,2,3];
var arr2 = [4,5,6];

// 1.es5的方法，使用数组原生的concat
arr.concat(arr2) // 输出 [1, 2, 3, 4, 5, 6]

// 2.ES6 的合并数组
[...arr, ...arr2]
```






