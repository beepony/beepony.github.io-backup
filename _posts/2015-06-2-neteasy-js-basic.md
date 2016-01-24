---
layout: post
title: "[前端] Javascript 程序设计－基本语法"
date: 2015-06-2 12:27
---

## 变量标识符
1. 命名要求
* 以 **字母**，**下划线**，**$** 开头
* 由 **字母**，**下划线**和 **$** 和**数字**组成

范例，正确和错误的例子

```
var abc;
var _abc;
var $abc;
var _abc1$;
function $$abc_(){}
```

```
var 1abc;
var *abc;
var abc&;
function #abc(){}
```
## 关键字和保留字

## 大小写敏感

```
var Arr = [1,2,3];
var arr = function(){
	For(var i = 0; i < A.length; i++){
		console.log(A[i]);
	}
}
For 是关键字，应该为小写

```

## 严格模式
引入严格模式的目的
增益
- 消除 js 语法的不合理，不严谨，不安全，减少怪异行为保证代码运行安全
- 提高编译器效率，增加运行速度

使用
全局使用严格模式

```
<script type="text/javascript">
	"user strict";
	(function)(){
		console.log("hello world!");
	})()
</script>
```
函数内使用严格模式

```
<script type="text/javascript">
	(function)(){

		"user strict";
		console.log("hello world!");
	})()
</script>
```
严格模式和标准的区别

* 严格模式下，不能隐式生命和定义变量

```
(function)(){
use "strict" 
abc = 0 //引用错误
})()
```
* 严格模式下，对象不能有重名的属性

```
use "strict"
var obj = {a:1,b:2,a:3}
//语法错误

```
* 不能调用 argumetns.callee 方法，匿名函数的递归

```
var count = 0;
(function)(){
    user "strict"
    if (count > 10){
        return;
    }
    count++;
    arguments.callee(); //TypeError
})();
```
* with 语句
```
var x,y;
(function)(){
    "use strict";
    with(Math) {    //TypeError
        x = cos(3 * PI) + sin (LN10);
        Y = tan(14 * E);
    }
})();
```
严格模式还有其他的限制
## 注释

```
/*
这是一个 js 多行注释
都可以注释多行
*/
// 这是 js 单行注释
```