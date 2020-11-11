---
title: 最好用的js库之jQuery
date: 2018-12-01 21:23:22
tags: jQuery
categories: js库
summary: jQuery是一个快速，小型且功能丰富的JavaScript库。借助易于使用的API，使HTML文档的遍历和操纵，事件处理，动画和Ajax等事情变得更加简单
---

## jQuery 初体验

【02-让 div 显示与设置内容.html】

```javascript
$(document).ready(function () {
  $("#btn1").click(function () {
    // 隐式迭代：偷偷的遍历，在jQuery中，不需要手动写for循环了，会自动进行遍历。
    $("div").show(200);
  });
  $("#btn2").click(function () {
    $("div").text("我是内容");
  });
});
```

使用 jQuery 的优点

```javascript
//1. 获取元素的方式非常的简单，而且非常的丰富
//2. jQuery的隐式迭代特性，不再需要书写for循环语句。
//3. 使用jQuery完全不用考虑兼容性问题。
//4. jQuery提供了一系列动画相关的函数，使用非常方便。
//5. 代码简单、粗暴。
```

**没有对比，就没有伤害，有了对比，处处戳中要害。**

## 什么是 jQuery?

> jQuery 是一个快速的、轻量的、功能丰富的 js 库。

jQuery 的官网 [http://jquery.com/](http://jquery.com/)

jQuery 就是一个 js 库，使用 jQuery 的话，会比使用 JavaScript 更简单。

js 库：把一些常用到的方法写到一个单独的 js 文件，使用的时候直接去引用这 js 文件就可以了。（animate.js、common.js）

## 入口函数

入口函数的好处：

```javascript
1. 等待文档加载完成，保证能够获取到元素
2. 形成了一个沙箱，防止全局变量污染。
```

两种写法：

```javascript
//第一种写法
$(document).ready(function () {});
//第二种写法
$(function () {});
```

jQuery 入口函数与 js 入口函数的对比

```javascript
1.	JavaScript的入口函数要等到页面中所有资源（包括图片、文件）加载完成才开始执行。
2.	jQuery的入口函数只会等待文档树加载完成就开始执行，并不会等待图片、文件的加载。
```

## jQuery 使用步骤

1. 引包（引入 js 文件）

```html
<script src="jquery-1.11.1.js"></script>
```

2. 写上入口函数

```javascript
$(document).ready(function () {});

// 或者
$(function () {});
```

3. 在入口函数内部实现功能

```javascript
$("#btnShowDiv").click(function () {
  $("div").show(1000);
});
```

## jQuery 对象与 DOM 对象(重点)

基本概念：

```javascript
1. DOM对象：使用JavaScript中的方法获取页面中的元素返回的对象就是dom对象。
2. jQuery对象：jquery对象就是使用jquery的方法获取页面中的元素返回的对象就是jQuery对象。
3. jQuery对象其实就是DOM对象的包装集（包装了DOM对象的集合（伪数组））
```

jQuery 对象与 DOM 对象的区别：

```javascript
1. DOM对象与jQuery对象的方法不能混用。
2. DOM对象可以和jQuery对象相互转化
```

DOM 对象转换成 jQuery 对象：【联想记忆：花钱】

```javascript
var $obj = $(domObj);
// $(document).ready(function(){}); 就是典型的DOM对象转jQuery对象
```

jQuery 对象转换成 DOM 对象：

```javascript
var $li = $("li");

//第一种方法（推荐使用）
$li[0]

//第二种方法
$li.get(0)
```


# 选择器

## 什么是 jQuery 选择器

jQuery 选择器是 jQuery 为我们提供的一组方法，让我们更加方便的获取到页面中的元素。注意：jQuery 选择器返回的是 jQuery 对象。

jQuery 选择器有很多，基本兼容了 CSS1 到 CSS3 所有的选择器，并且 jQuery 还添加了很多更加复杂的选择器。【查看 jQuery 文档】

jQuery 选择器虽然很多，但是选择器之间可以相互替代，就是说获取一个元素，你会有很多种方法获取到。所以我们平时真正能用到的只是少数的最常用的选择器。

## css 选择器

> jQuery 完全兼容 css 选择器

| 名称       | 用法                | 描述                                                           |
| ---------- | ------------------- | :------------------------------------------------------------- |
| ID 选择器  | $(“#id”);          | 获取指定 ID 的元素                                             |
| 类选择器   | $(“.class”);       | 获取同一类 class 的元素                                        |
| 标签选择器 | $(“div”);          | 获取同一类标签的所有元素                                       |
| 并集选择器 | $(“div,p,li”);     | 使用逗号分隔，只要符合条件之一就可。                           |
| 交集选择器 | $(“div.redClass”); | 获取 class 为 redClass 的 div 元素                             |
| 子代选择器 | $(“ul>li”);        | 使用>号，获取儿子层级的元素，注意，并不会获取孙子层级的元素    |
| 后代选择器 | $(“ul li”);        | 使用空格，代表后代选择器，获取 ul 下的所有 li 元素，包括孙子等 |

> 跟 CSS 的选择器一模一样。

## 过滤选择器

> 这类选择器都带冒号:

| 名称         | 用法                                | 描述                                                                |
| ------------ | ----------------------------------- | :------------------------------------------------------------------ |
| :eq（index） | $(“li:eq(2)”).css(“color”, ”red”); | 获取到的 li 元素中，选择索引号为 2 的元素，索引号 index 从 0 开始。 |
| :odd         | $(“li:odd”).css(“color”, ”red”);   | 获取到的 li 元素中，选择索引号为奇数的元素                          |
| :even        | $(“li:even”).css(“color”, ”red”);  | 获取到的 li 元素中，选择索引号为偶数的元素                          |
| :first       | $(“li:first”).css(“color”, ”red”); | 获取到的 li 元素中的第一个                                          |
| :last        | $(“li:last”).css(“color”, ”red”);  | 获取到的 li 元素中的最后一个                                        |


## 筛选选择器(方法)

> 筛选选择器的功能与过滤选择器有点类似，但是用法不一样，`筛选选择器`主要是方法。

| 名称               | 用法                         | 描述                                  |
| ------------------ | ---------------------------- | :------------------------------------ |
| children(selector) | $("ul").children("li")      | 获取当前元素的所有子元素中的 li 元素  |
| find(selector)     | $("ul").find("li");         | 获取当前元素中的后代元素中的 li 元素  |
| siblings(selector) | $("#first").siblings("li"); | 查找兄弟节点，不包括自己本身。        |
| parent()           | $("#first").parent();       | 查找父亲                              |
| eq(index)          | $("li").eq(2);              | 相当于`$("li:eq(2)")`,index 从 0 开始 |
| next()             | $("li").next()              | 找下一个兄弟                          |
| prev()             | $("li").prev()              | 找上一次兄弟                          |


## 其他补充: mouseover 与 mouseenter

> mouseover 和 mouseoverenter 都有鼠标经过的意思，但是在注册鼠标经过事件的时候，推荐使用`mouseenter`

[mouseenter 与 mouseover 的不同](http://www.w3school.com.cn/tiy/t.asp?f=jquery_event_mouseenter_mouseover)

1. mouseover 与 mouseout 是一对事件，当鼠标经过当前元素或者当前元素的子元素的时候，mouseover 事件都会触发【事件冒泡】。
2. mouseenter 与 mouseleave 是一对事件，只有当鼠标经过当前元素时，事件会触发，鼠标经过子元素，mousenter 事件是不会触发的。

## index 方法

> `index()`方法返回的是当前元素在所有兄弟元素里面的索引。

```html
<ul>
  <li><a href="#">我是链接</a></li>
  <li><a href="#">我是链接</a></li>
  <li><a href="#">我是链接</a></li>
  <li><a href="#">我是链接</a></li>
  <li><a href="#">我是链接</a></li>
  <li><a href="#">我是链接</a></li>
  <li><a href="#">我是链接</a></li>
  <li><a href="#">我是链接</a></li>
  <li><a href="#">我是链接</a></li>
</ul>
```

**当碰到这种结构的时候，推荐给 li 注册事件，这样通过 index 方法才能获取到正确的索引值。**

## 区分 jQuery 与 Javascript

JavaScript 是一门编程语言，jQuery 仅仅是用 JavaScript 实现的一个 JavaScript 库，目的是简化我们的开发。

![](images/1.png)





# jQuery操作样式

## css操作

> 功能：设置或者修改样式，操作的是style属性。

> 操作单个样式

```javascript
//name：需要设置的样式名称
//value：对应的样式值
css(name, value);
//使用案例
$("#one").css("background","gray");//将背景色修改为灰色
```

> 设置多个样式

```javascript
//参数是一个对象，对象中包含了需要设置的样式名和样式值
css(obj);
//使用案例
$("#one").css({
    "background":"gray",
    "width":"400px",
    "height":"200px"
});
```

> 获取样式

```javascript
//name:需要获取的样式名称
css(name);
//案例
$("div").css("background-color");
```

注意：获取样式操作只会返回第一个元素对应的样式值。

1. 设置操作的时候，如果是多个元素，那么给所有的元素设置相同的值
2. 获取操作的时候，如果是多个元素，那么只会返回第一个元素的值。


## class操作

> 添加样式类

```javascript
//name：需要添加的样式类名，注意参数不要带点.
addClass(name);
//例子,给所有的div添加one的样式。
$(“div”).addClass(“one”);
```

> 移除样式类

```javascript	
//name:需要移除的样式类名
removeClass(“name”);
//例子，移除div中one的样式类名
$(“div”).removeClass(“one”);
```

> 判断是否有某个样式类

```javascript
//name: 用于判断的样式类名，返回值为true false
hasClass(name)
//例子，判断是否有one的样式类
$(“div”).hasClass(“one”);
```

> 切换样式类

```javascript
//name:需要切换的样式类名，如果有，移除该样式，如果没有，添加该样式。
toggleClass(name);
//例子
$(“div”).toggleClass(“one”);
```


# jQuery操作属性

## attr操作

> 设置单个属性

```javascript
//第一个参数：需要设置的属性名
//第二个参数：对应的属性值
attr(name, value);
//用法举例
$(“img”).attr(“title”,”哎哟，不错哦”);
$(“img”).attr(“alt”,“哎哟，不错哦”);
```

> 设置多个属性

```javascript
//参数是一个对象，包含了需要设置的属性名和属性值
attr(obj)
//用法举例
$("img").attr({
    title:"哎哟，不错哦",
    alt:"哎哟，不错哦",
    style:"opacity:.5"
});
```

> 获取属性

```javascript
//传需要获取的属性名称，返回对应的属性值
attr(name)
//用法举例
var oTitle = $("img").attr("title");
alert(oTitle);
```

> 移除属性

```javascript
//参数：需要移除的属性名，
removeAttr(name);
//用法举例
$("img").removeAttr("title");
```

## prop操作

> 在jQuery1.6之后，对于checked、selected、disabled这类boolean类型的属性来说，不能用attr方法，只能用prop方法。

```javascript
//设置属性
$(“input:checked”).prop(“checked”,true);
//获取属性
$(“input:checked”).prop(“checked”);//返回true或者false
```



# jQuery动画

> jquery提供了三组基本动画，这些动画都是标准的、有规律的效果，jquery还提供了自定义动画的功能。【演示动画例子】

## 三组基本动画

> 显示(show)与隐藏(hide)是一组动画：
> 滑入(slideUp)与滑出(slideDown)与切换(slideToggle)，效果与卷帘门类似
> 淡入(fadeIn)与淡出(fadeOut)与切换(fadeToggle)

```javascript
show([speed], [easing], [callback]);
//speed(可选)：动画的执行时间
	 //1.如果不传，就没有动画效果。如果是slide和fade系列，会默认为normal
	 //2.毫秒值(比如1000),动画在1000毫秒执行完成(推荐)
     //3.固定字符串，slow(200)、normal(400)、fast(600)，如果传其他字符串，则默认为normal。
// easing(可选)： 动画效果，默认是swing，秋千，提供了一个linear 匀速的效果
//callback(可选):执行完动画后执行的回调函数
```



## 自定义动画

> animate: 自定义动画

```javascript
$(selector).animate({params},[speed],[easing],[callback]);
// {params}：要执行动画的CSS属性，带数字（必选）
// speed：执行动画时长（可选）
// easing: 执行效果，默认为swing（缓动）  可以是linear（匀速）
// callback：动画执行完后立即执行的回调函数（可选）
```


## 动画队列与停止动画

> 在同一个元素上执行多个动画，那么对于这个动画来说，后面的动画会被放到动画队列中，等前面的动画执行完成了才会执行。

```javascript
//stop方法：停止动画效果
stop(clearQueue, jumpToEnd);
//第一个参数：是否清除队列
//第二个参数：是否跳转到最终效果
```



# jQuery节点操作

## 创建节点

```javascript
//$(htmlStr)
//htmlStr：html格式的字符串
$("<span>这是一个span元素</span>");
```

## 添加节点

```javascript
// append  appendTo   都是添加到元素内部的最后面
// prepend prependTo  都是添加到元素内部的最前面
// before  		添加到元素前面，做兄弟
// after	    添加到元素后面，做兄弟
```

## 清空节点与删除节点

> empty：清空指定节点的所有元素，自身保留(清理门户)

```javascript
$(“div”).empty();//清空div的所有内容
```

> remove：相比于empty，自身也删除（自杀）

```javascript
$(“div”).remove();
```

## 克隆节点

> 作用：复制匹配的元素

```javascript
// 复制$(selector)所匹配到的元素（深度复制）
// cloneNode(true)
// 返回值为复制的新元素，和原来的元素没有任何关系了。即修改新元素，不会影响到原来的元素。
$(selector).clone();
```





# jQuery 特殊属性操作

## val 方法

> val 方法用于设置和获取表单元素的值，例如 input、textarea 的值

```javascript
//设置值
$("#name").val(“张三”);
//获取值
$("#name").val();
```

## html 方法与 text 方法

> html 方法相当于 innerHTML text 方法相当于 innerText

```javascript
//设置内容
$("div").html("<span>这是一段内容</span>");
//获取内容
$("div").html();

//设置内容
$("div").text("<span>这是一段内容</span>");
//获取内容
$("div").text();
```

区别：html 方法会识别 html 标签，text 方法会将内容直接当成字符串，并不会识别 html 标签。

## width 方法与 height 方法

> 设置或者获取高度

```javascript
//带参数表示设置高度
$(“img”).height(200);
//不带参数获取高度
$(“img”).height();
```

获取网页的可视区宽高

```javascript
//获取可视区宽度
$(window).width();
//获取可视区高度
$(window).height();
```

## scrollTop 与 scrollLeft

> 设置或者获取垂直滚动条的位置

```javascript
//获取页面被卷曲的高度
$(window).scrollTop();
//获取页面被卷曲的宽度
$(window).scrollLeft();
```


# jQuery 事件机制

> JavaScript 中已经学习过了事件，但是 jQuery 对 JavaScript 事件进行了封装，增加并扩展了事件处理机制。jQuery 不仅提供了更加优雅的事件处理语法，而且极大的增强了事件的处理能力。

## jQuery 事件

> 简单事件注册

```javascript
click(handler)			单击事件
mouseenter(handler)		鼠标进入事件
mouseleave(handler)		鼠标离开事件
```

缺点：不能同时注册多个事件 && 不支持动态绑定

## on 注册事件(重点)

> jQuery1.7 之后，jQuery 用 on 来注册事件， 最现代的方式，强烈建议使用。

on 注册简单事件

```javascript
// 表示给$(selector)绑定事件，并且由自己触发，不支持动态绑定。
$(selector).on("click", function () {});
```

on 注册委托事件

```javascript
// 表示给$(selector)绑定代理事件，当必须是它的内部元素span才能触发这个事件，支持动态绑定
$(selector).on( "click",“span”, function() {});
```

on 注册事件的语法：

```javascript
// 第一个参数：events，绑定事件的名称可以是由空格分隔的多个事件（标准事件或者自定义事件）
// 第二个参数：selector, 执行事件的后代元素（可选），如果没有后代元素，那么事件将有自己执行。
// 第三个参数：data，传递给处理函数的数据，事件触发的时候通过event.data来使用（不常使用）
// 第四个参数：handler，事件处理函数
$(selector).on(events[,selector][,data],handler);
```

## 事件解绑

> off 方式（推荐）

```javascript
// 解绑匹配元素的所有事件
$(selector).off();
// 解绑匹配元素的所有click事件
$(selector).off("click");
```

## 触发事件

```javascript
$(selector).click(); //触发 click事件
$(selector).trigger("click");
```

## jQuery 事件对象

jQuery 事件对象其实就是 js 事件对象的一个封装，处理了兼容性。

```javascript
//screenX和screenY	对应屏幕最左上角的值
//clientX和clientY	距离页面左上角的位置（忽视滚动条）
//pageX和pageY	距离页面最顶部的左上角的位置（会计算滚动条的距离）

//event.keyCode	按下的键盘代码
//event.data	存储绑定事件时传递的附加数据

//event.stopPropagation()	阻止事件冒泡行为
//event.preventDefault()	阻止浏览器默认行为
//return false 既能阻止事件冒泡，又能阻止浏览器默认行为。
```





# jQuery补充知识点

## 隐式迭代

### 基本概念

> 隐式迭代：jQuery在设置属性时会自动的遍历，因此我们不需要再遍历

1. jQuery在执行设置性操作时，会给所有的元素都设置上相同的值。
2. jQuery在执行获取性操作时，只会返回第一个元素对应的值。
3. 如果想要给每一个元素都设置不同的值，需要手动进行遍历jQuery对象。

### each方法

> 遍历jQuery对象集合，为每个匹配的元素执行一个函数

语法：

```javascript
// 参数一表示当前元素在所有匹配元素中的索引号
// 参数二表示当前元素, 在function中this也表示当前元素。
$(selector).each(function(index,element){});
```

## 链式编程

> 链式编程的原理：设置性操作会返回一个jQuery对象，因此可以继续调用jQuery的方法。

1. 设置操作的时候，可以使用链式编程。
2. 获取操作的时候，无法使用链式编程。

```javascript
end(); // 上一次返回的jq对象
```


```javascript
prevAll();//获取前面所有的兄弟元素
nextAll();//获取后面所有的兄弟元素
siblings();//获取所有的兄弟元素
prev();//获取前一个兄弟
next();//获取后一个兄弟。
```

# jQuery插件

> 插件：jquery不可能包含所有的功能，我们可以通过插件扩展jquery的功能。

jQuery有着丰富的插件，使用这些插件能给jQuery提供一些额外的功能。

## 使用插件

```javascript
1. 引入jQuery文件
2. 引入插件（如果有用到css的话，需要引入css）
3. 使用插件
```

常用插件的使用

+ jquery.color.js的使用
  + https://github.com/jquery/jquery-color


+ jquery.lazyload.js的使用
  + https://github.com/tuupola/jquery_lazyload
