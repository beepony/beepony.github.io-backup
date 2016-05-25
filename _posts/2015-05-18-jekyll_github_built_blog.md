---
layout: post
title: '使用 Jekyll 和 Github Pages 搭建免费博客'
date: 2015-05-18 16:17
---

又折腾了一个 Blog 作为备份（万一 Farbox 不在了呢）：[binge](http://binge.im)，简述下搭建过程。 

### 1. 本地部署
本地部署 Jekyll 的步骤，跟新建一个 Rails 项目类似，首先安装 Jekyll
`sudo gem install jekyll`
然后创建一个 Jekyll 项目
`jekyll new test`
进入项目，启动服务
`cd test`
`jekyll s`
浏览器输入 127.0.0.1:4000 即可预览，至此本地部署成功。

### 2. 远程发布到 Github
登入 Github 账号，新建一个 `test.github.io` (把这里的 **test** 替换成你再 Github 的账号)
将本地项目纳入 Git 版本管理，并且 Push 到 Github，在本地依次执行一下命令

```
cd test 
git init # 初始化
git add -A # 将所有文件加入到 Git 控制
git commit -m "the first commit" # 提交第一个 commit
git remote add origin https://github.com/test/test.github.io.git # 添加远程库
git push -u origin master # 推送到远程库的 master 分支
```
10 分钟之后，访问 http://test.github.com，博客初步搭建完成。

### 3. 绑定自定义域名
除了用 Github 提供的默认域名之后，还可以绑定自定义域名，分两步走
1. dig 一下 test.github.io 找到 A 记录的 IP，将 test.com 解析到改 IP 上
2. 在 test 目录下执行 `echo "test.com" > CNAME`        
     
将改动推送到远程分支，依次执行以下命令：

```
git add .
git commit -m "add the CNAME file to band the custom domain"
git push
```
### 4. 写文章

Jekyll 的每篇文章都是一个 markdown 文件，存储在 _posts 目录下。每篇文章的名字遵循固定的格式，系统会根据文件名生成固定的文章链接。格式为：`YYYY-MM-DD-文章名.md`，`YYYY` 是年份，`MM` 是月份 `DD` 是日期。例如：`2015-05-18-test.md`
除了文件名格式固定之外，在写每篇文章之前先要设置头信息。头信息包含在两行三虚线之间。在头信息可以设置预定义的全局变量的值，Jekyll 会根据变量的值来生成文章页面。下图是本篇文章的头信息：

```
---
layout: post
title:  "test" #文章标题
date:   2015-05-18 13:59:25 #时间
categories: test # 分类，将文章分成不同的属性。例如 test.com/test/2015-05-18-test.md
tags: Jekyll Github  # 标签，以空格隔开
---
```
其他的头信息设置，可以参考: [frontmatter](http://jekyllrb.com/docs/frontmatter/)          
Markdown 语法参考：[献给新手的 Markdown 指南](http://www.jianshu.com/p/q81RER)

### 5. 加入评论
评论系统采用的是国外著名的：[Disqus](https://disqus.com/home/)，常规流程，注册、登陆、获取代码、添加到页面。
在 Disqus 的 Settings-Add Disqus To Site，点击 Start Using Engage，进入 Site profile，填写相关信息，Category 选择 Auto，点击 Finish registration。将代码复制到 _layouts/post.html 内的 `</article>` 和 `</div>` 标签之间即可

至此，一个免费的博客搭建完毕，当然这只是开始，坚持写下去才是初衷，Happy Blogging :)