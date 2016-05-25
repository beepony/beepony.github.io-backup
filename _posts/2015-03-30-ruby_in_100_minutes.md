---
layout: post
title: "100 分钟爱上 Ruby"
date: 2015-05-16 15:57
---

这个简单的教程，可以帮助初学者了解 Ruby 的基本语法。
> 如果你还没有安装 Ruby，参考下 [the environment setup page for instructions](http://tutorials.jumpstartlab.com/topics/environment/environment.html)

## Ruby 的历史
许多人以为 Ruby 是一门年轻的语言，但是实际上 1994 年就发布了，作者是一个叫 Matz 的开发者。Matz 是个痴迷于编程语言的极客，非常喜欢 Perl。他创立 Ruby 初衷就是希望 Ruby 跟 Perl 一样灵活和强大，同时语法上更加丰富，代码更具有可读性。

Ruby 很快就有了一批追随者。但是在 2000 以前，所有的 Ruby 文档都是日语的。所以，如果你想学习 Ruby，还得先学习日语。大卫.托马斯是个敏捷编程的先驱，迷恋上了 Ruby，决定写 Ruby 的文档。他写了一本颇有影响力的书，叫[ The Pickaxe ](https://pragprog.com/book/ruby/programming-ruby)(镐头)，叫这个名字是因为封面上的那张图。这本书打开了英语世界了解 Ruby 的窗口。

英语世界里知道 Ruby 的人开始多了起来，但是增长十分缓慢。直到系统管理员开始用 Ruby 写维护和「胶水」脚本-以前都用 Perl 写的，Ruby 这才开始流行起来。从 2000-2005 年美国的 
Ruby 社区已经有了几百个会员。

2004-2005 年，芝加哥一个叫 37signal 的公司招了一个年轻的程序员开发一款 Web 应用。他们只关心客户端的设计和功能性，至于实现方面，则给了这个年轻的程序员以极大的自由发挥的空间。彼时，主要的 Web 技术还是 Perl CGI，PHP，JSP，ASP 等。这些语言或多或少都有痛点。所以大卫，现在我们都叫他 DHH，开创了一条新的道路。

他使用 Ruby 完成了这款应用。除了依赖核心库和少量的帮助库，差不多其他都是靠自己完成的。他和 37signal 都为这款应用做开发工作，这款应用现在叫 Basecamp。

Basecamp 完成之后，DHH 就从应用里提取出一种 Web 框架。不同于 Java 和 .NET 从上至下的框架。Rails 完全都从真实的案例中提取出来的。它遵循约定俗成胜于配置的原则，使得通用的问题更容易解决。

Rails 取得了巨大的成功。之后 Rails 驱动了 Ruby 社区的巨大增长。现在你可以在亚马逊上买到十几本关于 Ruby 的书，全世界有几百场有关 Ruby 的会议，数以千计的人成为专业的 Ruby/Rails 开发者。

如果你想学 Rails，得先学学 Ruby，走起...

## 1. 简要说明和解释器
Ruby 是一种解释性的语言，那就意味着他不直接跑在处理器上。它通过虚拟机来运行，这种方式的好处就是一次编写，在各种不同的操作系统和硬件平台上都可以运行。

Ruby 程序不能自己运行，你要先载入虚拟机。在虚拟机里有两种方式执行 Ruby 代码：IRB 和 命令行。

### 通过命令行运行 Ruby
把 Ruby 代码写到一个文件里是比较常用靠谱的方法，文件可以备份，传输，加到源码控制等等。

#### 一个简单的 Ruby 例子
我们创建一个文件，命名为 my_program.rb 如下

```
class Sample
  def hello
    puts "Hello world!"
  end
end

s = Sample.new
s.hello
```
然后我们这样运行程序

```
ruby my_program      
hello world!
```
当你运行 ruby my_program.rb 的时候，你实际上就已经载入虚拟机开始运行 my_program.rb 文件了。

#### 从 IRB 运行程序
Ruby 是第一个普及交互式脚本的语言：读取-计算-输出-循环。把它想成一个计算器，当你输入一个完整的指令，IRB 会执行并且计算出结果，输出给你。

IRB 最好用的就是作为一个实验的草稿箱。许多开发者开发程序的时候都开着 IRB 窗口。用它来回忆方法的使用或者调试代码。

我们也开始用 IRB 来做实验了。打开命令行，输入 `irb`.

## 2. 变量 
编程就是创造抽象的概念。为了创造这种抽象的概念，我们要给事物一个名字，而变量就是为数据命名的。

在有些编程语言中，变量只能保存特定类型的数据。但是 Ruby 不需要，它有非常灵活的类型系统，任何一个变量可以保存各种数据类型。

### 创建变量，并且赋值
在有些编程语言中，你需要先声明变量，然后才能赋值。Ruby 的变量是在你赋值的时候自动创建的，不需要指定。来看个例子

```
2.1.2 :001 > a = 5
 => 5 
2.1.2 :002 > a
 => 5 
2.1.2 :003 > 
```
a = 5 这个语句创建了变量 a，并且将 5 存储到 a 里。

#### 从右边开始看
日常阅读时，我们从左往右读，所以很自然的读代码也这样。但是当用 = 赋值的时候，Ruby 实际上是先执行等号右边的。举个例子：

```
2.1.2 :003 > b = 10 + 5
 => 15 
2.1.2 :004 > b
 => 15 
2.1.2 :005 >
```
10 + 5 先被执行，然后将结果赋值给变量 b。

#### 灵活的变量赋值
Ruby 的变量可以保存任何类型的数据，甚至能够改变保存的数据类型。比如：

```
2.1.2 :005 > c = 20
 => 20 
2.1.2 :006 > c = "hello"
 => "hello" 
2.1.2 :007 > 
```
先给 c 赋值了一个 20，然后再将字符串 "hello" 赋值给 c，c 的值就变成了 "hello"。

#### 变量命名规则
虚拟机对大多数的 Ruby 变量命名都有格式要求，一般

* 小写字母开头，（下划线也可以，但是不常用）
* 没有空格
* 不能包含特殊字符，比如 $, @, & 等。

除此之外，还有一些 Ruby 程序员通用的命名风格：

* 每个单词都小写并且通过下划线连接
* 根据变量的内容命名，而不是内容的类型
* 不能太短

好的变量名应该像 count，students_in_class，或者 fisrt_lesson.

不要像下面那样写：

* studentsInClass - 用驼峰命名法，而不是下划线连接，应该像 students_in_class 
* 1st_lesson - 变量不能以数字开头，应该是这样：fisrt_lesson
* students_array - 这个命名格式是对的，但是名字代表了数据的类型，所以也不规范，因该是 students
* sts - 变量名太短了，用 students 就好了

#### 练习
使用交互命令行 IRB，试试那些名字是好的，那些是非法的，哪些是合法但是不符合 Ruby 风格的。

* time_machine
* student_count_integer
* 3_secitons
* top_ppl

## 3. 字符串
现实生活中，英语 string 代表绳子，绳子可以用来捆东西。程序中的字符串跟现实生活中的绳子没有关系。

字符串用来存储数字和字母的集合。它们既可以是单个字母 "a" 也可以是一个单词 "hi" 或者像一个句子 "hello world!"

### 字符串的书写
一个 Ruby 的字符串以引号开头，可以包含 0 个或者多个字母、数字、连接符，再以引号结尾。最短的字符串是空字符串 "" 。一个字符串包含一段文字甚至几页文本都不足为奇。

### 子串
有时候，我们需要从一个字符串中拿出一部分，这个就叫子串。比如

```
2.1.2 :007 > greeting = "hello Everyone!"
 => "hello Everyone!" 
2.1.2 :008 > greeting[0..4]
 => "hello" 
2.1.2 :009 > greeting[6..14]
 => "Everyone!" 
2.1.2 :010 > greeting[6..-1]
 => "Everyone!" 
2.1.2 :011 > greeting[6..-2]
 => "Everyone" 
2.1.2 :012 > 
```
#### 正书和负数的位置
字符串中的字符通常都有正数数字，从 0 开始。以 "Hi" 为例，第 0 个位置是 "H" "i" 是第一个位置

我们用开始和结束符号来取出子串，比如 greeting[0..4] 代表从第 0 个位置到第 4 个位置的字符，共 5 个字符。

Ruby 解释器处理负数的方法是从字符串的最后开始截取，"Hi" 字符串的 "i" 是第 -1 个位置，"i" 是第 -2 个位置。

那么如果一个字符既有正数位置，又有负数位置，该如何选择呢？如果用正数很容易理解，但是如果你要一个字符串的最后几位（像 "What’s the last character of this string?"）,这时候就用负数位置。

#### 通用的字符串方法
我们在 IRB 里试验一下通用的几个字符串方法

* **length**
length 方法告诉你一个字符串里有多少字符，包括空格：
```
2.1.2 :012 > greeting = "hello world"
 => "hello world" 
2.1.2 :013 > greeting.length
 => 11 
```
试试计算一下你名字的长度

* **split**
你经常需要把字符串切分成几个部分。比如，你在字符串里存了一个句子，现在想分割成单词：

```
2.1.2 :014 > sentence = "This is my sample sentence."
 => "This is my sample sentence." 
2.1.2 :015 > sentence.split
 => ["This", "is", "my", "sample", "sentence."] 
```
.split 方法返回一个数组。他通过空格来分割字符串。

* **.split 使用参数分割**
有时候你想要不只是通过空格分隔，而是通过特定的字符。那么 .split 方法可以加参数来分割，比如
```
2.1.2 :018 > numbers = "one,two,three,four,five"
 => "one,two,three,four,five" 
2.1.2 :019 > numbers.split
 => ["one,two,three,four,five"] 
2.1.2 :020 > numbers.split(",")
 => ["one", "two", "three", "four", "five"] 
```
调用第一个 split 方法时，因为没有空格，返回了一个整个字符的数组。在第二个方法中，我指定了 `,` 
作为分隔符，得到了一个包含五个单独单词的数组。

**.sub 和 .gsub**
这两个方法用来替换字符串。类似于 word 中的查找和替换。`.sub` 是 substitute 的缩写，代表单词替换。`.gsub` 代表全局替换，替换所有符合的字符。

```
2.1.2 :021 > greeting = "hello friend"
 => "hello friend" 
2.1.2 :022 > greeting.gsub("friend","you")
 => "hello you" 
```

### 连接字符串和变量
我们常常需要将变量和字符串连接在一起。比如，以这样一个字符串开始
` "Good morning, Frank！"`

当我们在 IRB 里输入这个字符串的时候，还是返回这个字符串。要是写程序的话，我们希望程序能够跟他们各自的名字交互，而不只是 "Frank".

那我们就需要将字符串和变量连接在一起。有两种方式可以实现

#### 字符串的级联

最简单的方法叫做字符串的级联，用 `+` 将两者连接起来即可。

```
2.1.2 :034'> name = "Frank"
2.1.2 :035'> puts "Good morning, " + name + "!"
```
第一行我们命名了一个变量保存名字。第二行我们打印出了 "Good morinng" 和变量 name 以及 "!" 字符串。

#### 字符串插值
第二种方法是用字符串插值，将数据插入到字符串中。

字符串插值能在双引号包裹的字符串中使用。在字符串中我们使用 `#{}` 表示插值。 我们可以把变量和将被赋值的代码，转换成字符串，然后输出。之前的例子可以用插值重写成这样：

```
2.1.2 :001 > name = "Frank"
 => "Frank" 
2.1.2 :002 > puts "Good morning, #{name}!"
Good morning, Frank!
```
如果你比较一下输出，就可以看到结果是一样的。插值的风格可以输入更少的字符，打开/关闭引号的频次更低，既然如此，那就忘了加号吧。

#### 在插值中执行代码
你可以在插值的括号中放入 Ruby 代码和计算符，就像这样。

```
2.1.2 :003 > modifier = "very"
 => "very" 
2.1.2 :004 > mood = "excited"
 => "excited" 
2.1.2 :005 > puts "I am #{modifier * 3 + mood} for today's class!"
I am veryveryveryexcited for today's class!
 => nil 
```
`modifier * 3 + mood` 先被计算，然后计算结果再插入到字符串中输出。

## 4. Symbols
Symbols（符号）比较难解释，他是介于字符串和数字之间的一种类型。符号很好认，都是以冒号开头，后面跟字母，比如 :flag 或者 :best_friend.

### 如果你是新手
如果你是编程新手，可以把符号想象为字符串的精简，没有很多方法和字符串插值。比较一下一个字符串支持的方法和符号的区别

```
2.1.2 :010 > "hello".methods
 => [:<=>, :==, :===, :eql?, :hash, :casecmp, :+, :*, :%, :[], :[]=, :insert, :length, :size, :bytesize, :empty?, :=~, :match, :succ, :succ!, :next, :next!, :upto, :index, :rindex, :replace, :clear, :chr, :getbyte, :setbyte, :byteslice, :scrub, :scrub!, :freeze, :to_i, :to_f, :to_s, :to_str, :inspect, :dump, :upcase, :downcase, :capitalize, :swapcase, :upcase!, :downcase!, :capitalize!, :swapcase!, :hex, :oct, :split, :lines, :bytes, :chars, :codepoints, :reverse, :reverse!, :concat, :<<, :prepend, :crypt, :intern, :to_sym, :ord, :include?, :start_with?, :end_with?, :scan, :ljust, :rjust, :center, :sub, :gsub, :chop, :chomp, :strip, :lstrip, :rstrip, :sub!, :gsub!, :chop!, :chomp!, :strip!, :lstrip!, :rstrip!, :tr, :tr_s, :delete, :squeeze, :count, :tr!, :tr_s!, :delete!, :squeeze!, :each_line, :each_byte, :each_char, :each_codepoint, :sum, :slice, :slice!, :partition, :rpartition, :encoding, :force_encoding, :b, :valid_encoding?, :ascii_only?, :unpack, :encode, :encode!, :to_r, :to_c, :>, :>=, :<, :<=, :between?, :nil?, :!~, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__] 
2.1.2 :011 > "hello".methods.count
 => 164 
2.1.2 :012 > :hello.methods 
 => [:==, :===, :inspect, :to_s, :id2name, :intern, :to_sym, :to_proc, :succ, :next, :<=>, :casecmp, :=~, :[], :slice, :length, :size, :empty?, :match, :upcase, :downcase, :capitalize, :swapcase, :encoding, :>, :>=, :<, :<=, :between?, :nil?, :!~, :eql?, :hash, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :freeze, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__] 
2.1.2 :013 > :hello.methods.count
 => 78 
```
### 如果你是个有经验的程序员
如果你是个有经验的程序员，可以把符号当成一种 "命名整形"。符号实际引用的值不重要，我们要关心的是在虚拟机里任何指向这个值都会返回同样的值。符号是在一个全局符号表里定义的，他们的值不会改变。

## 5. Numbers
Number 有两种基本的类型：integers (整数)和 floats (浮点型，有小数点的)。

Integers 对于你和计算机来说，都是相对容易处理的。你可以用常用的运算符操作，包括 `+`, `-`, `/`, `*`. Integers 有许多方法可以帮你做数学运算。可以通过 `5.methods` 来查看。

### 重复指令
其他语言里常用的重复模式就是 `for` 循环，通常用户重复一条指令很多次。比如在 JavaScript 中可以这样写：

```
for(var i = 0; i < 5; i++){
    console.log("Hello,world");
}
```

For 循环用的比较多，但是这种代码不具备可读性。因为 Ruby 中的 integers 是对象，有方法。其中一个方法就是 `time` 重复某个指令若干次。用 Ruby 重写上面的循环大概是这样的。

```
5.times do
  puts "Hello,world"
end
```
这里例子里我们再用的 `times` 方法我们叫块。我们会在下一个部分讨论。在 IRB 里运行这个例子看看会发生什么。

## 6. Block (块)
块是 Ruby 里非常强大的概念，经常被用到。可以把它们看成是一系列说明的方法，可以被用到各个地方。

### Blocks 的开始和结束
你刚刚已经看过了 integer 里使用 .times 方法

```
5.times do
  puts "Hello,world"
end
```
块以 `do` 开头，以 `end` 结尾。`do`/`end` 总是合适的。

#### 括号里的块
当块只包含一个简单的指令，我们经常用 `{}` 来替代块的开始和结束

`5.times{puts "Hello,world"}`

### 块传递给方法
块有什么实际的用处呢？他们可以作为参数传递给方法调用。比如，我们刚才调用 `5.times`，Ruby 不知道我们想把什么样的语句执行 5 次。当我们传递块过去的时候，就想说 "我想让你每次都运行这个语句".

很多方法都接受块传递。像刚才在字符串部分看到的 `.gsub` 方法就接受块传递。

```
2.1.2 :027 > "this is a sentence".gsub("e"){ puts "Found an E!"}
Found an E!
Found an E!
Found an E!
 => "this is a sntnc" 
```
注意下那句 `Found an E!` 出现了三次，因为有 3 个 e 在字符串里。

为什么结果是 `"sntnv"`，留给你思考。

### 块参数
我没你经常遇到块里面语句需要引用我们现在使用的值，当我们写块的时候能够通过通过管道符认出块参数

```
irb(main):002:0> 5.times do |i|
irb(main):003:1* puts "hello world"
irb(main):004:1> end
```
块参数的取值取决于我们多调用的方法。这里例子里，我们调用的是 number 的 times 方法。先实验一下上面的语句，观察输出结果，然后再试试下面这个：

```
irb(main):004:0> 5.times do |i|
irb(main):005:1* puts "#{i}:hello,world!"
irb(main):006:1> end

# the output
0:hello,world!
1:hello,world!
2:hello,world!
3:hello,world!
4:hello,world!
=> 5

```
当调用 String 的 .gsub 方法，试试下面的代码（用括号）：
```
irb(main):007:0> "this is a sentence".gsub("e"){|letter| letter.upcase}
```
你就会看到 .gsub 方法使用快语句的结果替换了原始的字符。

## 7. Arrays（数组）
我们写程序的时候，经常会需要处理一组数据的集合。在这一部分，我们就来看看最常用的数据的集合 —— 数组。

### 可视化的模型
数组是一个数字索引的列表。想象一下，在一张白纸上画三个小盒子：

```
 ---  ---  ---
|   ||   ||   |
 ---  ---  ---
```
你可以根据他们从左到右的位置算出每一个

```
 ---  ---  ---
|   ||   ||   |
 ---  ---  ---
  0    1    2
```
然后把字符串放进每个盒子：

```
 -------------  ---------  ----------
| "Breakfast" || "Lunch" || "Dinner" |
 -------------  ---------  ----------
       0            1           2
```
这个数组有三个元素。Ruby 的数组可以增加或者减少。通常都在数组的末尾增加元素。

```
 -------------  ---------  ----------  -----------
| "Breakfast" || "Lunch" || "Dinner" || "Dessert" |
 -------------  ---------  ----------  -----------
       0            1           2           3
```
想一想为什么最后一个数组的位置总是比数组里的元素少一位。
如果你请求数据里位置为 2 的元素，你会得到一个 "Dinner"。请求数组里的最后一个元素，会得到 "Dessert"。

### 代码里的数组
下面就是在 Ruby 代码里运行同样的模型。

```
irb(main):014:0> meals = ["Breakfast", "Lunch", "Dinner"]
=> ["Breakfast", "Lunch", "Dinner"]
irb(main):015:0> meals << "Dessert"
=> ["Breakfast", "Lunch", "Dinner", "Dessert"]
irb(main):016:0> meals[2]
=> "Dinner"
irb(main):017:0> meals.last
=> "Dessert"
```
观察一下上面的情况...

* 数组的元素以逗号分割，用中括号括起来
* 用两个小于号 `<<` 往数组里添加元素
* 通过中 `[]` 加元素的位置来取元素
* 还有一些方法方便区数据，比如 `.last`

### 通用的数组方法
数组还可以做很多酷的事情。举几个例子：

#### `.sort` 方法的解释
`.sort` 方法会返回一个重新排序后的新数组。如果数据里的元素是字符串，那么就会以字母表里的顺序。如果是数字，就会返回升序排列后的数组。下面试一试

```
irb(main):020:0> one = ["this", "is", "an", "array"]
=> ["this", "is", "an", "array"]
irb(main):021:0> one.sort
=> ["an", "array", "is", "this"]
irb(main):022:0> one
=> ["this", "is", "an", "array"]
```
你可以用 `sort` 方法重新对元素排序，你可以用 `each` 方法循环访问每一个元素。你可以通过 `join` 方法将他们连接成字符串。你可以通过 `index` 方法找到某个元素内存中的地址。你也可以通过 `include?` 方法判断该元素是否在数组里。

当我们需要一个有序的列表时，就可以用数组。

#### 试试其他的

* `each`
* `collect`
* `first` and `last`
* `shuffle`

更多详细的说明，可以参考文档：[Arrary](http://www.ruby-doc.org/core-2.1.2/Array.html)

## 8. 哈希
哈希也是一组数据的集合，跟数组不同的是，哈希里的数据没有顺序，每个数据都有一个名字。跟冰箱很类似的。如果我们从冰箱里找东西，我们其实并不关心它在哪一层。顺序不重要。相反，我们是用名字来分类东西。比如苹果的数量是 3 个，橘子的数量是 1 个。胡萝卜是 12 个。在这种情况下，我们就用到了哈希。

### 键值对
哈希是无序的集合，数据是以键值对的形式存储。哈希的语法相对复杂，需要一点时间习惯。


```
irb(main):046:0> produce = {"apples" => 3, "oranges" => 1, "carrotes" => 12}
=> {"apples"=>3, "oranges"=>1, "carrotes"=>12}
irb(main):047:0> puts "There are #{produce["oranges"]} oranges in the fridges."
There are 1 oranges in the fridges.
```
`key` 可以看作是地址，`value` 就是存储在地址的数据。在 `produce` 这个哈希中，我们有 `"apples"` 和 `"oranges"` 这两个键，值就是 `12` 和 `3`。创建哈希时，键和值是通过 `=>` 这个字符连接的，我们把这个字符叫做 rocket（火箭？）。所以哈希是大括号 `{` 开始，零个或者多个字符组成键，`=>` 和值，通过逗号分割，最后以大括号结尾 `"}"` 。

试试下面几个操作

```
irb(main):048:0> produce["grapes"] = 221
=> 221
irb(main):049:0> produce
=> {"apples"=>3, "oranges"=>1, "carrotes"=>12, "grapes"=>221}
irb(main):050:0> produce["oranges"] = 6
=> 6
irb(main):051:0> produce
=> {"apples"=>3, "oranges"=>6, "carrotes"=>12, "grapes"=>221}
irb(main):052:0> produce.keys
=> ["apples", "oranges", "carrotes", "grapes"]
irb(main):053:0> produce.values
=> [3, 6, 12, 221]
```
第一行我们为这个哈希加了一个新的值。因为这个 `"grapes"` 键不在原始的哈希里，所以也给他加了一个值 `221`。键在哈希中必须是唯一的，所以当我们使用相对的语法对 `produce["oranges"]` 操作时， `oranges` 这个键已经存在，但是他的值会被 `6` 代替。 `keys` 和 `value` 方法会分别给出键值对中的另外半部分。

### 简单的哈希语法
实际编码中，我们经常使用符号来代替哈希中的键。当所有的键都变为符号的时候，那么哈希的语法看起来就简单一些了。

```
irb(main):054:0> produce = {apples: 3, oranges: 1, carrots: 12}
=> {:apples=>3, :oranges=>1, :carrots=>12}
irb(main):060:0> puts "There are #{produce[:oranges]} oranges in the fridge."
There are 1 oranges in the fridge.
=> nil
```
仔细观察你就会注意到键是冒号结尾，而不是开头，即便如此，这些也是符号。1.9 以上版本的 Ruby 都支持这种简单的语法。可以在控制台里用 `ruby -v` 看看你现在使用的 Ruby 版本。

## 9. 条件表达式
条件表达式的值是 `ture` 或者 `false`。最常见的条件操作符是 `==`, `>`, `>=`, `<`, `<=`。

有些对象的方法也可以返回 `true` 或者 `false`,他们用的也是条件表达式。比如，每个对象都有一个方法 `.nil?` 当为 `true` 的时候，对象为 `nil`. 数组也有一个方法叫 `.include?` 当返回 `true` 的时候，说明该数组里包含这个元素。在 Ruby 语言的惯例中判断一个方法返回 `true` 还是 `false` 的时候，都会有一个 `?` 结尾。

### 条件分支/语句
我们为什么要用条件声明？很多时候要进行条件控制，特别是 `if`/`elsif`/`else` 结构。我们来写个例子：

```
def water_status(minutes)
  if minutes < 7
    puts "The water is not boiling yet."
  elsif minutes == 7
    puts "It's just barely boiling"
  elsif minutes == 8
    puts "It's boiling!"
  else
    puts "Hot! Hot! Hot!"
  end
end
```
我们看一下这个 `water_status` 这个方法的值分别在 5，7，8，9 时候的取值。

```
irb(main):072:0> water_status(5)
The water is not boiling yet.
=> nil
irb(main):073:0> water_status(7)
It's just barely boiling
=> nil
irb(main):074:0> water_status(8)
It's boiling!
=> nil
irb(main):075:0> water_status(9)
Hot! Hot! Hot!
=> nil
```
#### 理解这个执行的流程
当 `minutes` 是 5 的时候，先判断是不是小于 7, 是的，所以打印出 `The water is not boiling yet.` .

当 `minutes` 是 7 的时候，7 不小于 7，往下执行。等于 7，打印出 `It's just barely boiling`.

当 `minutes` 是 8 的时候，先判断 8 是不是小于 7，不是，再判断 8 是不是小于 7，也不是，继续往下走，8 是不是等于 8，是的，所以打印出 `It's boiling!`.

当 `minutes` 是 9 的时候，同理，执行最后一条语句 `Hot! Hot! Hot!`.

#### if 语句几种可能的情况
if 语句有下面几种...

* 只有一个 if 语句，当语句为 true 时，才会被执行
* 一个或多个 elsif 语句，当语句为 true 时，才会被执行
* 没有或者只有一个 else 声明，当 if 或者 elsif 都为 true 才会被执行。

if/elsif/else 结构里只有其中一项是可以被执行的。如果 if 为真，Ruby 直接忽略后面的 elsif 。一次只执行一个块，就是这样。

### 等于 vs 赋值
在写条件语句时，最容易犯的错误就是搞不清 `=` 和 `==` 的区别。

* `=` 是赋值，意思是把它左边的值放入到右边。是陈述，不是疑问
* `==` 是疑问，意思是左边的这个东东跟右边是否相等，是疑问，不是陈述

你也可以用逻辑操作符跟条件语句连接。最常看到的就是逻辑与和逻辑或，分别可以用 `&&` 和 `||` 表示。

## 10. Nil & Nothingness 
什么是不存在？不存在是不是只在外太空？事实上，当我们认为不存的时候，是不是仅仅只是某种事物的抽象呢？好吧，这个词儿太哲学了...

在 Ruby 中`nil` 代表值不存在的。

你有 3 个鸡蛋，吃了 3 个鸡蛋，你可能认为你就啥也没有了。但是仅就鸡蛋而言，你有 0 个。0 是一个数字，并不是不存在。

如果你有一个字符串 "hello"，然后删除了 "h","e","l","o" 字符，你以为你啥也没了，但是实际上你还有个 ""，空字符串。这也不是不存在。

`nil` 就是 Ruby 里关于不存在的概念。当你请求一个不存在的事物时，它就出现了。我们创建了一个只有 5 个元素的数字，但是你非得请求数组的第六个元素，那 Ruby 就给我们一个 `nil`。这不是说第六个就是空白，他跟数字不一样，就是不存在- `nil`。

你在写 Ruby 时遇到的大部分错误都跟 `nil` 有关系。你以为出了问题，想尝试解决，但你不能解决一个不存在的问题，所以 Ruby 会给出错误。

## 11. Objects, Attributes, and Methods(对象，属性，方法)
### Ruby 是面向对象的语言

Ruby 是面向对象的语言，所有跟我们打交道的在虚拟机里都是对象。每一片数据都是对象。对象拥有的信息，称之为属性。对象可以执行的动作，称之为方法。

举个关于对象的例子，想象一下你作为一个人。你的属性就是身高，体重，眼睛的颜色。你拥有的方法比如走路，跑步，洗碗，做梦。不同的对象有不同的属性和方法。下一个部分我们具体的看看 Ruby 中常见的对象。

### 类和实例
在面向对象语言中定义了类，类是同一类别或者同一类型的描述。它定义了这一类对象都有的属性和方法。

#### 定义一个类
我们来想一下学校的模型。我们创建了一个 `Student` 类，代表所有的学生。学生类的属性有 `first_name`, `last_name`, `primary_phone_number`。还可以定义一个 `introduction` 方法，让学生自我介绍。

```
class Student
  attr_accessor :first_name, :last_name, :primary_phone_number

  def introduction
    puts "Hi, I'm #{first_name}!"
  end
end
```
你可能还不了解 `attr_accessor` 方法，这是用来定义一个类的实例的属性。

#### 创建实例
类本身不能表示一个学生，他只是学生的抽象表达。为了表示一个真实的学生，我们在类的基础上创建了实例。

想象一下你是一个学生。不是一个抽象的概念，你是一个真实存在的人。这个真实的人就是一个学生实例。他是一个抽象概念的实现。所有的属性都是真实的数据。`first_name`,`last_name`,`primary_phone_number`。

### 从文件里运行 Ruby
我们很少在 IRB 里定义类。它只是我们的草稿，随时验证我们的想法。我们来看看如果运行一个 Ruby 文件。

* 退出 IRB 会话
* 在命令行里建个文件夹，作为工作目录
* 用文本编辑器创建一个文件，命名为 student.rb
* 在编辑器里保存好
* 从命令行里运行

`ruby student.rb` 因为文件是空的，所以看不到输出。

### 创建一个学生类
在你的文本编辑器里，像这样的结构写一个类

```
class Student

end
```

在类里面定义方法的时候通常用 `def` 关键字：

```
class Student
  def introduction
    puts "Hi, I'm #{first_name}!"
  end
end
```
我们注意到 `puts` 那一行是一个返回学生第一个名字的方法。我们添加上之前用到的属性。

```
class Student
  attr_accessor :first_name, :last_name, :primary_phone_number

  def introduction
    puts "Hi, I'm #{first_name}!"
  end
end
```
#### 运行这个文件
回到终端里，试着运行这个程序，还是没有任何输出...

为什么？我们定义了一个学生类，然后给这个类赋予了一个方法，还添加了一些属性。-但是我没有实际上没有创建任何的实例或者调用任何方法。

#### 创建实例
我们定义了类之后，还要创建一个实例：`frank = Student.new`

我们调用了学生类的 `new` 方法。并且存储到一个叫做 `frank` 的变量里。一旦我们创建了实例，我们就能设置或者访问他们的属性，调用他们的方法。

#### 在文件里创建实例
在 student.rb 下面，最后一个 end 后面，添加下面的字段。

```
frank = Student.new
frank.first_name = "Frank"
frank.introduction
```

保存之后，返回终端，再次运行 student.rb ，这时候应该输出 `Hi，I'm Frank！`

### 方法的参数
有时候方法会带有一个或者多个参数，告诉他们想做的要怎么做。比如，我想让 `frank.introduction('katrina')` 让他介绍自己给 Katrina .这个参数可以是 numbers,strings,或者任何对象。修改你的方法，让他携带一个参数：

```
class Student
  attr_accessor :first_name, :last_name, :primary_phone_number

  def introduction(target)
    puts "Hi #{target}, I'm #{first_name}!"
  end
end

frank = Student.new
frank.first_name = "Frank"
frank.introduction('Katrina')
```
现在重新运行文件，你就会看到 `Hi Katrina, I'm Frank`.

### 返回值
在 Ruby 里，每次调用方法都会有一个返回值。默认，Ruby 的方法会返回执行最后一个表达式的值。

### 添加 favorite_number
为我们的类添加一个 favorite_number 参数。

```
class Student
  attr_accessor :first_name, :last_name, :primary_phone_number

  def introduction(target)
    puts "Hi #{target}, I'm #{first_name}!"
  end

  def favorite_number
    7
  end
end

frank = Student.new
frank.first_name = "Frank"
puts "Frank's favorite number is #{frank.favorite_number}."
```

从你的终端里运行它，然后就会看到 `Frank's favorite number is 7`.最后这一行调用的就是 `favorite_number` 方法。最后那个方法的最后一行是 7，那么 7 就成了这个方法的返回值，返回给调用这个方法的人。在我们这里，返回了 7 并且作为插值，进入了字符串里。

## 现在你差不多入门了
好了，简单介绍到这儿了，现在你可以深入的探索诱人的 Ruby 了。

> 本文翻译至 [Ruby in 100 Minutes](http://tutorials.jumpstartlab.com/projects/ruby_in_100_minutes.html)