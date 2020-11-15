---
title: PHP基础
tags:
  - php
  - 后端
categories: 后端
summary: 在PHP语言的使用中，可以分别使用面向过程和面向对象， 而且可以将PHP面向过程和面向对象两者一起混用，这是其它很多编程语言做不到的.
abbrlink: a6c6
date: 2019-07-26 21:13:36
---
# PHP基础

> 文件以.php后缀结尾，所有程序包含在`<?php 这里是代码 ?>`
> 避免使用中文目录和中文文件名 
>
> php页面无法直接打开需要运行在服务器环境当中


## php初体验

```php
<?php
  	//echo 相当于document.write
    echo "hello world";
?>
```

输入中文乱码问题：如果使用echo输出中文，会乱码。

***在php的语法中，末尾必须加分号，不然就报错了（最后一行可以不加分号）***

```php
<?php
    //content-Type:text/html;返回内容是一个HTML文档，这样设置后，就能识别HTML标签了。
    //charset=utf-8 设置编码集
    header("content-Type:text/html;charset=utf-8");
    echo "hello world";
    echo "<br/>";
    echo "大家好，我是胡八一";
?>
```

## 变量

>
> 变量其实就是存储数据的容器
>
> php是一门弱类型语法，变量的类型可以随意改变。

**变量的命名规则**

```php
//1. 不需要关键字进行声明，变量在第一次赋值的时候被创建。
//2. 必须以$符号开始
//3. $后面的命名规则与js的变量命名规则一致。
$name = "胡八一";
echo $name;
```

## 数据类型

### 简单数据类型

**字符串** 

```php
$str = "胡八一";
echo $str;
```

**整数** 

```php
$num = 100;
echo $num;
```

**浮点型** 

```php
$float = 11.11;
echo $float;
```

**布尔类型** 

```php
$flag = true;
//当布尔类型值为true时，输出1
echo $flag;
$flag = false;
//当布尔类型为false时，输出空字符串
echo $flag;
```

**php的拼串** 

```php
//1. 在php中，+号只有算数的功能，并不能拼串
//2. 在php中，拼串使用.
$name = "胡八一";
echo "大家好，我是" . $name . "，今年18岁";
```

**php中的单引号与双引号**

```php
//1. 字符串的定义可以使用单引号，也可以使用双引号
$name = "胡八一";
$desc = '很帅';
//2. 在单引号中，完全当做字符看待
//3. 在双引号中，能够识别变量。如果有变量格式的字符串，可以直接解析

$name = "胡八一";//胡八一
echo $name;
$desc = '很帅';
echo $desc;//很帅

$str = '$name 很帅';//$name 很帅
echo $str;

$str = "$name 很帅";//胡八一 很帅
echo $str;
```

### 数组

> 在php中，数组分为两种，索引数组和关联数组

**索引数组（类似与JS中的数组）**

```php
$arr = array("张飞","赵云","马超");
echo $arr;//echo只能打印基本数据类型
echo $arr[0];//张飞
```

**关联数组（类似与JS中的对象）** 

```php
//属性名必须用引号引起来
$arr = array("name"=>"zhangsan", "age"=>18);
echo $arr["name"];
```

**输出语句** 

```php
//1. echo 输出简单数据类型
//2. print_r() 输出数据结构，一般用于输出复杂类型。
print_r($arr);//print_r是一个函数，不要忘记小括号
```

## 语句

### 判断语句

基本上来说，所有语言的if..else语法都是一样

```php
$age = 17;
if ($age >= 18) {
  echo "终于可以看电影了,嘿嘿嘿";
} else {
  echo "哎，还是回家学习把";
}
```

### 循环语句

**遍历索引数组** 

```php
$arr = array("张三", "李四", "王五", "赵六", "田七", "王八");
//获取数组的长度： count($arr)
for($i = 0; $i < count($arr); $i++) {
  echo $arr[$i];
  echo "<br>";
}
```

**遍历关联数组** 

```php
//遍历关联数组
$arr = array(
  "name"=>"zs",
  "age"=>18,
  "sex"=>20
);
foreach($arr2 as $key => $value) {
    echo $key . "=" . $value . "\n";
}
```

## 表单处理

> 表单（form）：表单用于收集用户输入信息，并将数据提交给服务器。是一种常见的与服务端数据交互的一种方式

```javascript
//1. action：指定表单的提交地址
//2. method:指定表单的提交方式，get/post，默认get
//3. input的数据想要提交到后台，必须指定name属性，后台通过name属性获取值
//4. 想要提交表单，不能使用input:button 必须使用input:submit
```

**php获取表单数据** 

```
 //$_GET是系统提供的一个变量，是一个数组，里面存放了表单通过get方式提交的数据。
 //$_POST是系统提供的一个变量，是一个数组，里面存放了表单通过post方式提交的数据。
```

**get与post的区别** 

```javascript
//1. get方式
	//1.1 数据会拼接在url地址的后面?username=hcc&password=123456
	//1.2 地址栏有长度限制，因此get方式提交数据大小不会超过4k
//2. post方式
	//2.1 数据不会在url中显示，相比get方式，post更安全
	//2.2 提交的数据没有大小限制

//根据HTTP规范，GET用于信息获取，POST表示可能修改变服务器上的资源的请求
```