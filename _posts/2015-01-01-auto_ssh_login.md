---
layout: post
title: "SSH 自动登陆脚本"
date: 2015-9-23 17:27
---
使用 expect 可以方便的自动登陆 SSH，Ubuntu 并没有自带 expect，使用之前先安装。
sudo apt-get install expect

脚本如下：
```
#! /usr/bin/expect 
# auto ssh login with expect
set timeout 600
spawn ssh -p 65422 user@host
expect “*passoword:"
send “your_pwd\r"
interact
```
`#! /usr/bin/expect`
expect 安装路径，声明使用 expect 来执行脚本，expect 跟 bash 类似

`set timeout 600`
设置超时时间，单位是秒

`spawn ssh -p 65422 user@host`
spawn 只能在 expect 环境下执行，如果没装 expect 或者 bash 环境下，提示找不到

`expect “*passoword:"`
expect 也是 expect 命令，判断上次输出的结果是否包含 password 字符串，如有则立即返回。否则等待，直至超时

`send “your_pwd\r"`
执行交互，等效输入密码，\r 代表异常的时候可以核查一下

`interact`
保持交互状态，让会话停留在终端上。

如果直接运行 sh script.sh 会提示找不到文件，需要给脚本加上执行权限：`chmod +x script.sh`，执行 `./script.sh`。