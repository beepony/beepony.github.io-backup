---
layout: post
title: "[前端]Javascript 程序设计—类型系统—基本类型"
date: 2015-06-4 10:25
---

主要介绍6种基本类型（Undefine、Null、Boolean、Number、String、Object）、原生类型及引用类型概念等.

## 标准类型
标准类型分为原始类型和引用对象，原始类型(primitive)包括

* Undefined
* Null
* Boolean
* String
* Number
* Object

## 引用类型

* Object

## 原始类型和引用类型在栈内存中的区别

```
var a;
var b = null;
var c = true;
var d = 123;
var e = "hello";
var f = {a:1};
// a,b,c,d,e 都是原始类型，f 是引用类型
```
原始类型在栈内存中的情况
| a |undefined(Undefined 类型)|
|---|---:|
|b  |   null(Null 类型)|
|c  |   true(Boolean 类型)|
|d  |   123(Number 类型)|
|e  |   "hello"(String 类型)|
|f  |   Object 类型|
原始类型都是保存在栈内存中，而引用类型在栈内存内存中只是一个地址，该地址指向堆内存，如图所示

![yinyongleixing]( http://pic-bin.b0.upaiyun.com/wangyi/yinyongleixing.png)
  
由上图可以推断出，原始类型的复制都是在栈内存中复制一份，而引用类型只是复制了指向堆内存的地址，如下图。

```
var obj1 = {a:1};
var obj2 = obj1;
```
![qubie](http://pic-bin.b0.upaiyun.com/wangyi/yuanshi-yinyong-qubie.png)

下面详细的说明原始类型的用法
### Undefined
#### 1. 类型说明

该类型的值就是：undefined

#### 2. 出现场景

* 已经声明但是没有赋值的变量

```
var a;
console.log(a);
console.log(b)
// 只定义，未赋值
```
* 获取对象不存在的属性
```
var obj = {a:1,b:2};
console.log(obj.c);
// obj 不存在 c 属性
```
* 无返回值的函数的执行结果

```
function f(){
    alert("test");
}
var ret = f();
console.log(ret);
```
* 函数的参数没有传入
```
functiom f(i,j){
    console.log(i);
    console.log(j);
};
f(1);
```
#### 3. 类型转换
undefined ＝> Boolean，在判断该属性是否存在时使用

```
var obj = {a:1,b:function(){},...};
if(obj.c){
    console.log(obj.c);
}
// 结果为 false
```
undefined ＝> Number
```
var count;
count + 1;
//结果为 NaN
```
undefined ＝> String
```
(function(a){
    console.log("a:" + a);
})()
```
### Null
#### 1. 类型说明
值为 null
#### 2. 出现场景
对象不存在时为 null
`document.getElementById('notExistElement');`
#### 3. 类型转换
null => Boolean，结果为 false
null => Number， 结果为 0
null => String， 结果为 "null"

### Boolean
#### 1. 类型说明
值为 true,false
#### 2. 出现场景
* 条件语句导致系统执行的隐式类型转换
`if(document.getElementById('notExistElement')){...}else{...}`
当元素不存在的时候，会返回 null,条件语句会隐式的转换为 false，执行 else 语句。
* 字面量或者定义变量
```
true
var a = true;
```
#### 3. 类型转换
| 转换|Number|String|
|--|--:|:--:|
| true | 1 |"null"|
|false|0|"false"|

### String
#### 1. 类型说明
由单引号或者双引号括起来的字符序列
#### 2. 出现场景

```
'hello world!'
var s = 'name = "myform"';
```
#### 3. 类型转换
| 转换|Number|Boolean|
|--|--:|:--:|
| "" | 0 |false|
|"123"|123|true|
|"1a"|true|NaN|

### Number
#### 1. 类型说明
整形直接量，八进制(0)直接量，十六进制(0x)直接量，浮点型直接量
#### 2. 出现场景
```
10
1.4
1.2e5
var count = 0x10;
```
#### 3. 类型转换
| 转换|String|Boolean|
|--|--:|:--:|
|0 | "0" |false|
|1  |"1"|true|
|Infinity   |"Infinity"|true|
|NaN|"NaN"|false|

### Object 
#### 1. 类型说明
Object 类型是一组属性的集合
#### 2. 出现场景
`var obj = {name:'jerry', age: 0};`
#### 3. 类型转换
| 转换|String|Boolean|Number|
|--|--:|:--:|:--:|
|{} | "[object Object]" |true|NaN|