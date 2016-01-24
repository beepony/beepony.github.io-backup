---
Title: SHELL 基本应用
date: 2014-10-03 16:39
status: public
title: 'SHELL 基本应用'
---

### 1.初识 shell
shell 是一种介于系统内核和用户之间的解释器，类似于翻译官的角色。儿脚本是一种特定的语言，可以按照预设定的顺序执行的文件。例如，SHELL、Python、Ruby、Javascript 等。

**编写脚本的思路**      
理清任务过程->整理执行语句->完善文件结构        
例如，请添加一个用户名为 tom 的用户，并且密码设为 123123
理清任务过程：
1. 添加用户
`useradd tom`

2. 设置密码
`passwd 123123`

整理执行语句：

```       
useradd tom         
echo 123123|passwd -stdin tom  			
```
完善文件结构：			

```
# !/bin/bash
# 20141128,by beepy
echo "正在创建用户 tom"
useradd tom
echo "正在设置密码"
echo 123123| passwd -stdin tom
echo "创建成功并且已经设置密码"
```
执行文件			
	
				
*  `chmod +x uad`,`.sh./uad.sh	`				

* `sh uad.sh`			
					
* `source uad.sh`


### 2. shell 组合应用

**管道：命令1 | 命令2**	


查找 test 文本里的 aa		
`cat test | grep aa`				

统计 etc 目录下 conf 后缀的文件综述		
`find /etc -name "*.conf” -type f | wc -l`				

查看 httpd 的进程:				
`ps -aux | grep httpd` 			

**重定向**
	
| 类型        | 操作符           | 用途  |
| ------------- |:-------------:| -----:|
| 重定向输入     | < 				| 从指定文件读取数据 |
| 重定向输出      | <、<<      |   将输出结果覆盖、追加到指定文件 |
| 标准错误输出 | 2>、2>>      |将错误信息覆盖、追加到指定文件 |
| 混合输出	 | &>、&>> |将标准输出和错误信息覆盖、追加到指定文件|

		
查看 linux 内核版本				
`uname -r `					
将结果写入version.txt 中						
`uname -r > version.txt `							

因为没有 version2.txt,所以会报错				
`cat version.txt version2.txt`					
	
如何将错误信息输入到文件呢，使用 `2>` 输出			
`cat version.txt version2.txt 2> error.txt`		


**逻辑分隔符**

* 与: &&
* 或: ||
* 命令分割: ;


逻辑与
`echo “haha” && echo “heihei"`

逻辑或
`echo “haha” || echo “失败"`

通过逻辑与或者逻辑或判断命令是否执行成功，下面语句都可以判读是否创建目录成功       			
`mkdir test/a 2>/dev/null && echo “成功"`		
`mkdir test/a 2>/dev/null || echo  “失败”`

命令分割符，分割多条命令			
`cd boot/grub;ls -ls grub.conf`

###3. shell 中变量

**定义和赋值**		
格式：变量名 = 变量值			

**引用变量**				
格式：$变量名，${变量名}				

```
name = wangbin		
echo $name
```

**双引号：允许引用 \ 转义**

```
echo “$name"
wangbin
```

双引号直接引用，输出 $name 的值

**单引号：禁止引用，禁止转义**

```
echo '$name'
$name
```

单引号不能饮用，所以输出然是变量 $name

**反撇号``或者 $( )：后面跟一条命令**		
```
var = `uname -r`
echo $var
```
上面命令会直接输出 linux 内核版本


**环境变量**
是用来记录或者设置运行参数, env 可以列出所有的环境变量

* 系统赋值: USER,LOGNAME,HOME,SHELL...
* 用户操作: PATH,LANG,CLASSPATH...

echo $USER $HOME $SHELL



系统或者脚本操控，不可人工赋值的

$?:前一条命令的是否执行成功，会返回状态值，0 正常，非 0 异常
$0: 脚本自身的程序名
$1-$9: 第一到第九个参数的位置参数
$*: 命令行所有位置参数的内容
$#:	命令行的位置参数个数


mkdir test
echo $?，成功，返回0
0
mkdir /1/2/3,
echo $?，因为1.2 不存在，所以创建失败返回非0
1

编写一个脚本，用上以上变量：

```
#!/bin/bash
echo “本程序名，$0"
echo “所使用的位置参数，$#"
echo “第一个位置参数是，$1"
echo “所有位置参数的内容是，$*"
```


###4. 数值运算和处理

**整数运算**	
两种格式：			
`expr 命令，计算表达式	`			
`expr 数值1 操作符 数值2`				
可以直接输出数值

$[]表达式，算式替换		
$[数值1 操作符 数值2]			
变量需要 echo 输出

`操作符：+ - * / %`

expr 322 /* 322，因为 * 在 linux 中是通配符，所以在作为乘号使用时要转移



**几个数值处理技巧**

变量递增递减			
let 变量++			
let 变量--

```			
x = 10,y = 30
let x++;echo $x
let y—;echo $y
let x+=2;echo $x，以 2 为单位递增
```

生成随机数可使用 RANDOM，大小范围在：0~32767		
`echo $RANDOM`

随机数去余数，可以求 0~99 随机数			
`echo $[RANDOM%100]`

seq 生成数字序列		
seq 首数 末数						
seq 首数 长度 末数					


`seq 4`		
`seq 3 6`		
`seq 3 3 19`		


**小数处理**
shell 无法直接处理小数，需要 bc 这个命令，例如计算小数	
`echo 433.44-43.5 | bc`			
计算小数并取最后四位		
`echo “scale = 4;10/3” | bc`		

###5. 字符串处理

**字符串的截取**

1. 路径分割：dirname命令、basename命令
2. expr:expr substr $var1 起始位置 截取长度，编号从 1 开始
3. ${}表达式：${$var:起始位置:截取长度}，编号为 0 开始

dirname 路径，basename 文件名			

```
var = “/etc/beep/test.txt"
dirname $var = “/etc/beep/"
basename $var = “test.txt"
```

expr 截取

```
var = “hahaheiheihehehehehehe"
expr var 4 5
```

${} 截取

```
var = “hahaheiheihehehehehehe"
echo ${var: 4:6}
echo ${var ::5} ,不写就从 0 开始截取
```

**字符串替换**

${} 表达式替换			
`${var/old/new}，只替换第一个 old 成 new`			
`${var//old/new}，替换这个变量里所有的 old,用两个 //`

**随机字符串**

随机字符串生成的过程：

1. /dev/urandom(生成随机字符，包含各种字符)
2. /usr/bin/md5sum（处理成标准编码字符）
3. /bin/cut（切割成需要的字符）

举例：
先将随机设备字符转为 ASII 字符,赋值给 var，然后使用 cut 切割
echo $var | cut -b 起始位置-结束位置，起始位置和结束位置都可省略.

```
head -1 /dev/urandom | md5sum
var = head -1 /dev/urandom | md5sum
head -1 /dec/urandom | md5sum | cut -b -8
```

###6、条件测试

**测试本质**			
测试是一条操作命令，$? 返回值来判断是否成立

**格式**			
两种格式：test 条件表达式
或者 [ 条件表达式 ],方括号前后要有空格

**练习方法**					
直接跟 && echo YES 判断结果			
[ 条件表达式 ] && echo YES


**文件状态监测**			
-e : 目标是否存在（exist)			
-d：判断是否是目录 directory			
-f :  判断是否是文件 file				

`[-f “/boot/fstable”] && echo yes`		
`[-f “/etc”] && echo yes	`			

**权限检测**			
-r 是否具有读权限（read）			
-w 是否具有写权限（write）			
-x 是否具有执行权限（excute）				

```
ls -l /etc/shadow #保存账号信息和密码信息，只有 root 用户又读取权限的
[-x  “/etc/shadow”] && echo  YES #判断是否有可执行权限
[-r  “/etc/shadow”] && echo  YES #判断是否有可读权限
[-w  “/etc/shadow”] && echo  YES #提示 yes，这个写权限对所有者例外
```

**整数值比较**

-eq：等于（equal）		
-nq：不等于（not equal）	
-gt：大于（greater than）	
-lt：小于（lesser than）			
-ge：大于或等于（greater or equal）			
-le：小于或等于(lesser or equal)		

```
who| wc -l #统计当前登录用户数,$() 代表表达式
[$(who | wc -l) -eq 2] && echo YES #判断当前用户数是否等于 2
[$(who | wc -l) -gt 2] && echo YES #判断当前用户数是否大于2
```
**字符串匹配**
= 两个字符相等		
!= 俩个字符不等			

```
[$USER = “root] && echo YES
[$USER != “root”] && echo YES
```

###7. if 判断结构


**单分支 if 语句结构**

```
if 条件测试
     then 命令序列
fi
```

举例：检查备份目录 /opt/repo，若不存在则创建。

```
BACKUP_DIR = “/opt/repo"

if [  ! -d $BACKUP_DIR ]
     then mkdir -p $BACKUP_DIR
fi

双分支 if 语句结构
if 条件测试
          then 命令序列
          else 另一个命令序列
fi
```

举例：判断目标主机是否存活，显示测试结果

```
ping -c 3 -i 0 2 -W $1 &> /dev/null
if [$? -ew 0]
then
     echo “host $1 is up"
else
     echo “host $1 is down"
fi

```

**多分支 if 语句结构**

if  条件测试1
     then 命令序列1
elif 条件测试2
     then 命令序列
...
else
     命令序列n
fi

举例：判断分数，区分优秀/合格/不合格

```
read -p “请输入分数（0-100）：” grade
if [ $grade -gt 85] && [ $grade -lt 100]
     then echo “$grade,优秀"
elif [ $grade -gt 60] && [ $grade -lt 75]
     then echo “$grade,合格“
else
     echo “$grade,不合格”
fi
```

###8. for 循环

语法格式

```
for 变量名 in 取值列表
do
     命令序列
done
```

检查 IP 地址是否在主机地址列表里

```
for IP地址 in 主机地址列表
do
     检查状态
done
```

用法示范举例：

```
for i in “1st” “2nd” “3rd"
do
     echo $i
done
```

逐词输出 hosts 里面的信息

```
for i in $ (cat /etc/hosts.conf)
do
     echo $i
done
```
批量加用户账号,用户列表为 users.txt,每行一个将初始口令设为 123456，首次登陆必须更改.

```
for i in $(cat /root/users.txt)
do
     useradd $i
     echo “123456” | passwd —stdin $i
     chage -d 0 $i
done
```

检测一个 IP 范围的主机状态,192.168.4.1 - 192.168.4.10
是否能 ping 通.

```
IP_PRE = “192.168.4."

for IP in $(seq 1 5)
do
    ping -c 3 -i 0 2 -W 3 $[IP_PRE]& IP &> /dev/null
     if [ $? -eq 0];then
          echo “$[IP_PRE]$IP is up"
     else
           echo “$[IP_PRE]$IP is down"
     fi
done
```

###9. case 分支

**语法格式**

```
case 变量值 in
模式1）
     命令序列
     ；；
模式2）
     命令序列2
     ；；
…...
*）
     默认命令序列
esac
```

一个服务器控制的简单示例

```
case 控制参数 in
start)
          启动服务
          ;;
stop)
          停止服务
          ;;
*)
          显示脚本的用法

esac
```

**案例示范**

识别用户输入的类型

```
read -p “请输入一个字符，并按 enter 确认：” key
case “$key” in
     [a-z]|[A-Z])
          echo “您输入的是字母"
     ;;
     [0-9])
          echo “您输入的是数字"
     ;;
     *)
          echo “您输入的时其他控制字符”
esca
```

编写服务脚本 sleepd

	* 能都响应 start stop 控制参数
	* 将服务交给 chkconfig 进行管理

```
#！/bin/bash
chkconfig: - 90 10
#description :Daemon script sleepd server
case “$1” in
start)
     sleep 3600 & ,后台运行
     ;;
stop)
     pkill -x “sleep"
     ;;
*)
     echo “用法：$0 [start | stop]

esca
```

chkconfig 处理
     在脚本开头设置chkconfig 参数
     添加为系统服务

在 /etc/init.d/sleepd 目录下执行
chkconfig -add sleepd

###10. awk 文本处理工具

**作用**
awk ，三个作者名字的第一个字母		
基于模式匹配的检查输入	
将期望的记过 print 到屏幕			

**格式**				
awk ‘模式{操作}’ 文件1 文件2

`awk ‘NR==1{print} /etc/hosts`    #把 host 第一行输出来，NR 是 awk 内置变量，表示第几行			
也可以使用 `head -1 /etc/hosts` 达到同样的效果			


常见的几个内置变量：		
NR   当前处理行的序数 (行号)，number row 		
FS   字符分割，缺省为空格或者 tab 		
$n   当前行的第 n 个字段		
$0   当前行所有文本内容		

**示例**	

```	
test-awk.txt
1 This is a first line
2 Hello world
3 64.233.185.99   google.com
4 hunter:503:504:504::/home/sites:/bin/bash
```

输入第一到第三行			
`awk ‘NR==1,NR==3’{print}’ test-awk.txt`

输入第一和第三行			
`awk ‘NR==1||NR==3’{print}’ test-awk.txt`

输出所有奇数行			
`awk ‘NR%2==1{print}’ test-awk.txt`

输出所有偶数行				
`awk 'NR%2==0{print}' test-awk.txt`

正则匹配，使用俩个斜杠匹配  /字符/		

列出所有包含 a 的行				
`awk '/a/{print}' test-awk.txt`					
	
列出以 bash 结尾的行				
`awk ‘/bash$/{print}' test-awk.txt`				

awk 默认的分隔符是空格或者 tab，这样可以输出具体字段，比如输出第一到第三行的第一到第三个字段				

`awk ‘NR==1,NR==3{print $1,$3} test-awk.txt`			

除了默认分隔符，还可以使用 -F 参数指定分割符，比如,输出以点做分割，第三个字段是 185 的那一行的第二个字段，$2 代表第二个字段，$0 代表整行输出				
`awk -F. '$3=="185”{print $2}' test-awk.txt`			

###11. sed 文本处理

stream editor，叫流编译器,不仅仅是输出，还可以替换和删除，功能更强大,基于模式匹配过滤修改和删除文本.

**格式**
sed ‘编辑指令’ 文件1 文件2 …		
sed   -n    ‘编辑指令’ 文件1 文件2 ……，-n 不直接修改行，只输出		
sed   -i    ‘编辑指令’ 文件1 文件2 ……，-i 输出修改过的行			

编辑指令的格式
格式[地址1[,地址2]]操作类型		
多个指令以分号隔开		

最常用的操作类型				
p 输出/打印文本行	
n 取下一行文本（跳过当前行）			
d 删除文本		
s 替换文本		
a 追加文本		

**示例**

```
输出hosts第三行到第五行的内容
sed -n '3,5p' /etc/hosts

输出hosts第三行和第五行的内容
sed -n '3p;5p' /etc/hosts

sed 隔行输出文本，输出所有的奇数行
sed -n 'p;n' test-awk.txt

sed 隔行输出文本，输出所有的偶数行
sed -n 'n;p' test-awk.txt
```
注：n p 之间用分号隔开


**sed 使用正则表达式**		
`sed -n '/a/,$p' test-sed.txt`		
输出从第一个包含 a 的行到最后一个包含 a 的行，$ 代表最后一行

`sed -n /\<This\>/p' test.txt`			
输出包含单词 this 的行，必须前后都是空格，中间反斜杠是转义作用

**删除符合条件的行**

```
删除第 2-3 行文本
sed '2,3d' test.txt

删除包含 aa 的行和最后一行,同样 $ 代表最后一行
sed '/aa/d;$d' test.txt 

删除不符合的行，!d 
sed '/aa/!d' test.txt
```

**替换符合条件的文本**

```
sed '3,4s/aa/bb/g' test.txt
其中 s 代表替换，sustitude, g 代表全部 globe 全部替换

替换的特殊效果
sed '1,2s/^/#/g' test.txt
其中 ^ 代表一行的开头，将一行的开头用 # 替换，代表注释行

sed 's/ter//g' test.txt
删除字符 ter，用空格替换
```
以上 sed 没有加参数，所以输出只是效果，并没有改变文本自身，如果想直接操作文本，可以使用 -i 参数。