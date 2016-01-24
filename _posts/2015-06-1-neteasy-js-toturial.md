---
layout: post
title: "[前端] JavaScript 程序设计-介绍"
date: 2015-06-1 16:14
---

##前端开发三要素
* html，描述网页内容
* css，描述网页样式
* Javacript，控制网页和浏览器的行为，对动作做响应，跟服务端互动

##什么是 js
* 解释型变成语言（脚本）
* 运行环境，借助于浏览器引擎来运行，引擎可以独立于浏览器运行

## js 历史
* 1995 年10 天诞生
* 为了推广跟 sun 合作，改名为 Javascript，实际跟 Java 没关系
* 1998 年发布 DOM 规范
* 2005 年 Ajax 发布，进入 Web 2.0 时代
* 2011 年 ECMAScript 5.1 发布

## 浏览器中的 js
1. ECMAScript js 语言的核心标准
2. DOM（文档对象模型） W3C 标准，可以提供操作文档对象的 API 
3. BOM（浏览器对象模型） 操作浏览器对象的 API

## js 引入
* 内嵌代码，用 <script></script> 包裹，一般放在 <body> 里
```
<!DOCTYPE html>
<head>
	<meta charset="UTF-8">
	<title>js</title>
</head>
<body>
	<script type="text/javascript">
		document.write("hello world");
	</script>
</body>
</html>
```
* 外联文件
```
<!DOCTYPE html>
<head>
	<meta charset="UTF-8">
	<title>js</title>
</head>
<body>
	<script type="text/javascript" src = "demo.js"></script>
</body>
</html>
```