---
title: Ajax技术原理剖析
summary: >-
  Ajax（Asynchronous Javascript And
  XML）是指一种创建交互式、快速动态网页应用的网页开发技术，无需重新加载整个网页的情况下，能够更新部分网页的技术。
categories: 网络请求
tags:
  - ajax
abbrlink: 968f
date: 2019-01-20 09:06:42
---

#### 什么是Ajax？

是浏览器提供的一套方法，是一门发送请求的技术，可以在不整体刷新网页的情况下向服务器发送请求，用来提升用户体验

#### Ajax 的应用场景

1. 页面上拉加载更多数据

2. 列表数据无刷新分页

3. 表单项离开焦点数据验证

4. 搜索框提示文字下拉列表

   以上是基础应用场景

#### Ajax 的运行环境

Ajax技术需要在运行在**网站环境**中才能生效，所以需要在**开启的服务中运行**。如node服务 开启后 通过**localhost:端口号/路由或者文件名**访问  ！！！切勿直接双击打开文件执行，也不能通过IDE右键在浏览器打开，这两种方式都是打开的静态资源，不能访问服务，相应的请求也无法实现。

Ajax 的实现步骤

1. 开启node服务 express 第三方模块

2. 开启静态资源访问  express 模块中开放的方法express。static("开放的绝对路径") ---实质上是express模块导入了server-

   中间件：拦截所有请求，交由express.static()处理请求

   app.use(express.static(path.join(__dirname,"public")))

   **这样我们就可以通过localhost:3000/文件名 的方式访问HTML页面了**

3. app.listen(3000) 监听端口



以下代码需要写在

1. 创建Ajax对象  （代替浏览器发送请求的代理人，因为浏览器发送请求，要么需要刷新，要么需要转跳，这不是我们期望的）

   ```js
   let xhr = new XMLHttpRequest();
   ```

2. 告诉Ajax请求地址以及请求方式（告诉Ajax实例对象要想哪发送请求，用什么方式发送请求）

   ```js
   xhr.open('get','http://www.example.com');
   ```

3. 发送请求（get方式的请求，send中不用传参，post需要传请求的参数）

   ```js
   xhr.send();
   ```

4. 获取服务器端给客户端的相应数据（响应需要一定时间，才能拿到响应的数据，当Ajax对象接收完成响应数据的时候，会触发xhr.onload()事件，所以将响应数据写在onload事件中）

   ```js
   xhr.onload = functon() {
   	console.log(xhr.responseText);
       console.log(xhr.status)
   }
   ```

5. JSON对象的获取

   ```js
   let j = {"name":"zs"};
   console.log(j);
   // JSON字符串生成
   let strJ = JSON.stringify(j);
   console.log(strJ)
   // JSON字符串转JSON对象
   let objJ = JSON.parse(strJ);
   console.log(objJ)
   ```

6. 

Ajax运行原理

浏览器端 ---  服务器端  （开发人员不可控）

浏览器端   ---  Ajax ---  服务器端 （开发人员可控）

函数封装的建议

封装方法的时候，传入的参数可以是一个对象或者是一个数组，这样方便在赋值的时候区分各个参数的意义

```
function ajax(options) {
	let xhr = new XMLHttpRequest();
	xhr.open(options.type,options.url);
	xhr.send();
	xhr.onload = () => {
		options.success(xhr.responseText)
	}
}
ajax({
	type: "get",
	url: "http://localhost:3000/first",
	success(data) {
		console.log(data)
	},
	data: {
		name: "COder Rat",
		age: 25
	}
})
```

请求参数要考虑的问题

1. 请求参数位置的问题

   将请求参数传递到Ajax函数内部，在函数内部根据请求方式的不同，参数的添加位置不同

   get 放在请求地址后面

   post 放在send方法中

2. 请求参数格式的问题

   application/x-www-form-urlencoded

   ​	参数名称 

> .后面不能加变量 a.b ---> a[b] 

#### 跨域请求的解决方案

1. 通过script的src属性 进行请求，绕过浏览器的同源策略
2. 设置服务器允许跨域访问
3. 通过自身服务器，访问目标服务器

#### cookie session

是一个容器，保存到客户端或服务器端一些客户信息等等

在http协议中规定，客户端与服务器端之间是无状态性的

### 弄清接口文档 & 后端接口之间的联系

---

以上为原生ajax的使用 目前JQuery提供的$.ajax()是用的比较多的，功能更加强大，使用更加便捷。



---

### ajax的原理：

 通过XMLHttpRequest()对象 向服务器发出 异步 请求；从服务器获取数据 通过js Dom操作更新页面
 异步：向服务器发送请求 不阻碍用户的操作  实现无刷新数据更新

### Ajax的优点：

 1、最大的一点是能在不刷新整个页面的情况下维持与服务器通信 这使得web应用程序迅捷的响应用户交互，
 在页面内与服务器通信给用户的体验非常好。
 2、使用异步方式与服务器通信，不需要打断用户的操作，具有更加迅速的响应能力。
 3、可以把以前一些服务器负担的工作转嫁到客户端，利用客户端闲置的能力来处理，减轻服务器和带宽的负担，
 节约空间和宽带租用成本。并且减轻服务器的负担，ajax的原则是“按需取数据”，可以最大程度的减少冗余请求，
 和响应对服务器造成的负担。
 4、基于标准化的并被广泛支持的技术，不需要下载插件或者小程序 但需要客户允许JavaScript在浏览器上执行。
 5.界面与应用分离 Ajax使得界面与应用分离，也就是数据与呈现分离 有利于分工合作。

### Ajax的缺点：

 1、ajax不支持浏览器back按钮。在动态更新页面的情况下，用户无法回到前一页的页面状态，因为浏览器仅能记忆历史纪录中的静态页面
 2、安全问题 AJAX暴露了与服务器交互的细节，就如同对企业数据建立了一个直接通道。
 这使得开发者在不经意间会暴露比以前更多的数据和服务器逻辑。。
 3、对搜索引擎的支持比较弱。
 4、破坏了程序的异常机制。
 5、不容易调试。
 6.不能很好地支持移动设备 例如手机APP;
 7.客户端肥大，太多客户段代码造成开发上的成本

### Ajax的转换

 通过Ajax的函数封装，获取到这个json文件 通过json.parse()或 jaon.stringify 的方法。
 将这个json文件解析成数组或字符串，然后循环遍历获取
 例如：
 //obj-->string json字符串 JSON.stringify();
 //string json字符串-->obj JSON.parse();

### Ajax请求数据的过程  步骤

 1.创建Ajax对象
 创建一个能够发数据的对象；--->XMLHttpRequest  //数据请求协议
 创建一个请求对象  new XMLHttpRequest();  兼容所有浏览器
 new XMLHttpRequest();
 new ActiveXObject("mircosoft.XMLHTTP");//只兼容IE6浏览器使用
 2.打开请求
 发送数据的方式 向服务器传输数据  open("get/post","提交的地址","是否异步");
 3.发送请求
 (1)、get 在网址栏   域名 /?text=123&pass=12135
 区别：get 方式  提交数据时  数据 url/?名称=数据&名称=数据
 通过send();提交数据
 (2)、post方法是提交时 设置请球头  声明数据类型
 setRequestHeader('content-type','application/x-www-form-urlencoded')
 通过send(data);提交数据
 4.等待服务器，返回内容
 XMLHttpRequest 对象属性描述
 onreadystatechange状态改变的事件触发器，每个状态改变时都会触发这个事件处理器，通常会调用一个JavaScript函数
 readyState
 请求的状态。有5个可取值：
 0 = 未初始化，
 1 = 正在加载，
 2 = 已加载，
 3 = 交互中，
 4 = 完成
 responseText    服务器的响应，返回数据的文本。请求的数据文本  ----->string
 status  服务器的HTTP状态码（如：404 = "文件末找到" 、200 ="成功" ，等等）
 statusText  服务器返回的状态文本信息 ，HTTP状态码的相应文本（OK或Not Found（未找到）等等）
 status:   服务器的状态码
 200  服务器请求成功
 404  服务器请求失败
 302  307  服务器请求异常

### post get  服务器获取和提交方式的区别

 get方式：get请求数据时  是将数据暴露在网址中 /?user=value&pass=value&时间毫秒数
 1-提交数据的大小受限制 比较小
 2-数据提交相对不安全  提交的数据在网址栏中通过字符串拼接发送
 3-有缓存问题  需要使用不断改变字符 后缀 来解决  添加："&"+new Date().getTime();
 4-有些浏览器不能提交中文数据  encoddeURI();解决中文      ie不能  encodeURI("你好")
 post方式：post请求数据 是通过请求的域名
 1-理论上大小不受限制 可设置
 2-相对于get方式比较安全
 3-没有缓存问题
 4-可以提交中文数据
 5-提交数据时需要 请求头 声明请求数据类型
 6-提交的数据要通过send()传入参数提交数据
 7-content-type :提交的数据格式 application/x-www-form-urlencoded

###  Jsonp的原理和用法

 1-jsonp是一种解决解决方案
 2-Jsonp就是跨域请求数据，通过动态创建script标签，
 然后通过标签的src属性 获取 js文件中的程序 该程序就是一个回调函数 参数是请求
 其他站点的资源文件；所以要预先定义好回调函数，不是ajax技术Jsonp的原理和用法
 1-jsonp是一种解决解决方案
 2-Jsonp就是跨域请求数据，通过动态创建script标签，
 然后通过标签的src属性 获取 js文件中的程序 该程序就是一个回调函数 参数是请求
 其他站点的资源文件；所以要预先定义好回调函数，不是ajax技术

### AJAX封装与调用

#### AJAX封装

```jsx
function ajax(obj){
                //1.创建ajax对象
                var xhr
                if(window.XMLHttpRequest){           支持此对象
                    xhr=new XMLHttpRequest()                    //IE7以上浏览器
                }else{
                    xhr=new ActiveXObject("Microsoft.XMLHTTP")  //只有IE6支持此对象
                }
                //2.打开请求
                //第一个参数表示请求方式,值为get/post,是字符串
                //第二个参数表示请求的地址
                //第三个参数是布尔值,默认是true表示异步,false表示同步
                xhr.open(obj.type,obj.url,obj.async)
                //3.判断请求方式get/post，发送数据(post方式必须发送请求头)
                if(obj.type.toLowerCase()=="get"){
                    xhr.send()
                }else if(obj.type.toLowerCase()=="post"){
                    xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded")      //设置请求头
                    xhr.send(toUrl(obj.data))//name=1&name2=2
                }
                
                //4.操作返回的数据
                xhr.onreadystatechange=function(){
                    if(xhr.readyState==4 && xhr.status==200){
                        //1.readyState属性:ajax工作状态
                        //2.每当readyState的值发生改变时,就会触发         onreadystatechange事件
                        //存有XMLHttpRequest的状态.从0-4发生变化
                        //0:请求未初始化
                        //1:服务器连接已建立
                        //2:请求已接收
                        //3:请求处理中
                        //4:请求已完成,且响应已就绪

                        //http状态码
                        //200代表请求成功
                        //403禁止访问
                        //404文件未找到
                        //500服务器错误
                        //对responseText进行json转化
                        var data=JSON.parse(xhr.responseText)
                        //把转化好的数据当做参数返回给obj.success函数
                        obj.success(data)
                    }
                }
                
                //对obj.data进行转化，把对象转化成url形式
                function toUrl(obj){
                    var arr=[]
                    for(var i in obj){
                        arr.push(i+"="+obj[i])
                    }
                    return arr.join("&")
                }
            }
```

#### ajax调用

```php
ajax({
    type:"post或者get",
    url:"地址",
    //加post的话用到data
    //get的话直接用&拼接  
    data:{
            key:"c0bf48603646fc9a7c831342cb34a9b,
        },
    async:true,
    success:function(res){      
        console.log(res);
    }
})
//要点:
//直接返回调用的对象
//请求方式type用get/post
//get方式直接拼接key必选项
//post方式直接传入data对象再设置key必选项
 ```