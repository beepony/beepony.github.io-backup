---
layout: post
title:  "EXPECT 的几个基本概念"
date:   2015-11-22 13:59:25
---

由于经常需要把本地内容 `scp` 到服务器，同时在服务器上执行一些固定的命令，了解到 `exepect` 脚本可以自动实现，于是做个记录。尽力做到简单重复的工作交给计算机去处理，流程和任务自动化，提升干活的效率。
当你需要跟控制台进行交互的时候，你就会发现 `TCL-Expect` 是多么好用的工具，能够将你从重复繁杂的任务中解救出来。以前只能手动去做的工作，现在通过 `expect` 脚本可以轻松完成。他们跟 shell 脚本很像，但是利用 .tcl 扩展实现，并且通过 #! 调用。

## 设置脚本初始环境
第一步，跟写 `bash` 脚本很像，告诉脚本在什么环境下执行命令。对于 expect 来说，我们一般这么调用：

```
#!/usr/bin/expect

最好在脚本最后加上 `interact`，这样脚本执行完毕，还停留在终端，不至于退出，类似这样：

#!/usr/bin/expect

'''代码部分'''

interact
```

## Puts & Output
`shell` 脚本一般使用 `echo` 输出，而 `expect` 则使用 `puts` 输出，这一点倒是跟 `Ruby` 很像。

`puts "I am performing a output command"`

结果就会输出 `I am performing a output command`,跟 `echo` 效果一样。

一般情况下，脚本会输出你正在执行的命令。测试的时候很有用，但是在生产环境或者日常使用未必需要。如果只需要显示 `puts` 的文本，隐藏其他命令，可以通过：
>log_user 0

或者你可以强制开启输出命令及其响应，可以通过：
>log_user 1

## 变量 Variables
定义变量非常简单，只用 `set {name}{value}`,例如:

```
set a "apple"
set b "banana"
set c "cantalope"
```
通过 `$` 加变量的 `name` 来引用变量，`$a` 即代表 `apple`.

## 参数 Arguments
如果想通过传递参数的方式来设置变量，可以通过这种方式：

```
set user [lindex $argv 0]
set password [lindex $argv 1]
```
然后，当执行脚本的时候带上参数
`./mycript.tcl youruser yourpassword`

## Spawn & Send
你可以使用 `spawn` 开启一个新的进程，例如 `ssh` 或者 `scp`

```
set user "myuser"
set server "myserver.com"
spawn ssh "$user\@server"
```
上面的例子就是开启了一个 `ssh` 进程，同时提交用户名和服务器，本质上就是在控制台输入 `ssh myuser@myserver.com`.

`send` 命令可以让你发送命令给控制台，比如发送一个密码：

```
set password [lindex $argv 0]
set app [lindex $argv 1]

spawn sudo "apt-get install $app"

expect "assword"

send "$password\r"
```

上面的例子允许你传递你的 sudo 密码和需要从 apt 安装的应用的名字，然后等着提示密码并且发送。No assword is not a mispelling，让我们看看 expect 声明。

## Expect
`expect` 才是见证奇迹的时候。我们先从最基本的开始，假设你运行了一个命令，然后等着控制台提示，然后再继续执行...

```
spawn apt-get "update"

expect "$"

# Move on to the next thing...
```

你开启一个 `spawn` 进程 `apt-get`,然后等着 `$` 或者提示。
上面的 `expect` 语句遵循就近匹配的原则。所以上面的例子里的提示可以像这样，比如`root@server  $~`。`expect` 语句找到 `$`，然后知道该继续执行下去了。

下面再来看看一些稍微复杂的例子，不只有一条 `expect`,让我们再看看 `ssh`

```
spawn ssh "$user\@server"

expect {
    "assword" {
        send "$password\r"
    }
    "yes/no" {
        send "yes\r"
    }
}

# NOW we can move on...
```

这里例子里，我们可能会遇到两种结果。第一种在连接到服务器之前会提示输入密码。`assword` 宽松的定义覆盖了两种情况，提交 `$password` 变量。
如果你尝试连接远程服务器，就会提示 `yes/no`,他会提交 `yes` 为你，然后等着密码提示，随后处理。

这也适合不同的提示-你可以做不同风格的提示，就像下面这样。

```
expect {
    "> " { }
    "$ " { }
}
```

This can be built-upon more to really close the error-gap:(不知道该如何翻译更得体，索性附上原句)

```
expect {
    "> " { }
    "$ " { }
    "assword: " { 
        send "$password\n" 
        expect {
            "> " { }
            "$ " { }
        }
    }
    "(yes/no)? " { 
        send "yes\n"
        expect {
          "> " { }
          "$ " { }
        }
    }
    default {
        send_user "Login failed\n"
        exit
    }
}
```

## 条件 Conditionals
`if` 语句非常简单，下面的例子足以说明：

```
if { $a == "soup" } {
    puts "You have soup!"
else {
    puts "No soup for you!"
}
```

## 循环 Looping
有好几种方法可以实现循环，我倾向于使用 `while` 方法

```
set count 10;
while {$count > 0 } {
    puts "$count\n"
    set count [expr $count-1];
}
```

这个语句会从 10 开始向后循环，然后输出每个数字

## 函数
函数可以复用代码，像这样：

```
proc do_something { a b c } {
    puts "You should buy some $a, $b, and $c!\n"
}
```
可以这样被调用

```

set running [do_something "apples" "bananas" "cantalopes"]

```

输出结果是这样：
`You should buy some apples, bananas, and cantalopes!"
`
我的小脚本，供参考:

```
# !/usr/bin/expect -f
# 同步本地目录到服务器，登陆服务器，删除 tkb 目录，修改 site 为 tkb

set timeout -1
set user "username"
set server "server_ip"
spawn /usr/bin/scp -r site $user@$server:/usr/share/nginx/html
expect eof
spawn ssh $user@$server
expect "*"
send "cd /usr/share/nginx/html\r"
send "rm -rf tkb\r"
send "mv site tkb\r"
interact
```

## 结论
上面就是 expect 脚本的一些最基本的概念。以前需要手工处理一系列动作，现在可以通过 expect 转化为执行一系列列动作的脚本。
本文翻译自 [Basic principles of using tcl-expect scripts](https://gist.github.com/Fluidbyte/6294378)
详细的说明，可以参考 [expect](http://www.tcl.tk/man/expect5.31/expect.1.html)
