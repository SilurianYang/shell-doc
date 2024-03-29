# 2.1. Shell变量

变量是任何一种编程语言都必不可少的组成部分，变量用来存放各种数据。脚本语言在定义变量时通常不需要指明类型，直接赋值就可以，Shell 变量也遵循这个规则。

在 Bash shell 中，每一个变量的值都是字符串，无论你给变量赋值时有没有使用引号，值都会以字符串的形式存储。

这意味着，Bash shell 在默认情况下不会区分变量类型，即使你将整数和小数赋值给变量，它们也会被视为字符串，这一点和大部分的编程语言不同。例如在C语言或者 C++ 中，变量分为整数、小数、字符串、布尔等多种类型。

当然，如果有必要，你也可以使用 Shell declare 关键字显式定义变量的类型，但在一般情况下没有这个需求，Shell 开发者在编写代码时自行注意值的类型即可。

## 定义变量
Shell 支持以下三种定义变量的方式：

```ruby
variable=value
variable='value'
variable="value"
```

variable 是变量名，value 是赋给变量的值。如果 value 不包含任何空白符（例如空格、Tab 缩进等），那么可以不使用引号；如果 value 包含了空白符，那么就必须使用引号包围起来。使用单引号和使用双引号也是有区别的，稍后我们会详细说明。

**注意，赋值号=的周围不能有空格，这可能和你熟悉的大部分编程语言都不一样。**

Shell 变量的命名规范和大部分编程语言都一样：
* 变量名由数字、字母、下划线组成；
* 必须以字母或者下划线开头；
* 不能使用 Shell 里的关键字（通过 help 命令可以查看保留关键字）。

变量定义举例：
```ruby
url=http://hhyang.cn/
echo $url

name='hhyang'
echo $name

author="杨二郎"
echo $author
```

## 使用变量
使用一个定义过的变量，只要在变量名前面加美元符号`$`即可，如：

```ruby
name='hhyang'
echo $name
echo ${name}
```

变量名外面的花括号`{ }`是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况：

```ruby
skill="Java"
echo "I am good at ${skill}Script"
```

如果不给 skill 变量加花括号，写成`echo "I am good at $skillScript"`，解释器就会把 $skillScript 当成一个变量（其值为空），代码执行结果就不是我们期望的样子了。

**推荐给所有变量加上花括号`{ }`，这是个良好的编程习惯。**

## 修改变量的值
已定义的变量，可以被重新赋值，如：

```ruby
url="http://hhyang.cn/"
echo ${url}

url="http://hhyang.cn/doc1/request/ajax.html"
echo ${url}
```

第二次对变量赋值时不能在变量名前加`$`，只有在使用变量时才能加`$`。

## 单引号和双引号的区别

前面我们还留下一个疑问，定义变量时，变量的值可以由单引号`' '`包围，也可以由双引号`" "`包围，它们到底有什么区别呢？不妨以下面的代码为例来说明：

```ruby
#!/bin/bash

name='杨二郎'
text1='我的名字叫：${name}'
text2="我的名字叫：${name}"
echo $text1
echo $text2


ASUS@Peyu MINGW64 /e/Test/shell
$ bash check.sh
我的名字叫：${name}
我的名字叫：杨二郎
```

以单引号`' '`包围变量的值时，单引号里面是什么就输出什么，即使内容中有变量和命令（命令需要反引起来）也会把它们原样输出。这种方式比较适合定义显示纯字符串的情况，即不希望解析变量、命令等的场景。

以双引号`" "`包围变量的值时，输出时会先解析里面的变量和命令，而不是把双引号中的变量名和命令原样输出。这种方式比较适合字符串中附带有变量和命令并且想将其解析后再输出的变量定义。

**我的建议：如果变量的内容是数字，那么可以不加引号；如果真的需要原样输出就加单引号；其他没有特别要求的字符串等最好都加上双引号，定义变量时加双引号是最常见的使用场景。**

## 将命令的结果赋值给变量
Shell 也支持将命令的执行结果赋值给变量，常见的有以下两种方式：

```ruby
variable=`command`
variable=$(command)
```

第一种方式把命令用反引号\` `（位于 Esc 键的下方）包围起来，反引号和单引号非常相似，容易产生混淆，所以不推荐使用这种方式；第二种方式把命令用`$()`包围起来，区分更加明显，所以推荐使用这种方式。

例如，我在 demo 目录中创建了一个名为 log.txt 的文本文件，用来记录我的日常工作。下面的代码中，使用 cat 命令将 log.txt 的内容读取出来，并赋值给一个变量，然后使用 echo 命令输出。

```ruby
ASUS@Peyu MINGW64 /e/Test/shell
$ log=$(cat log.txt)

ASUS@Peyu MINGW64 /e/Test/shell
$ echo $log
沧海一遇却难找寻 前路崇山峻岭不再有你同行 纵使微茫如烟 纵有万般思念 流光总将故人搁浅在断简残篇 不成眠

ASUS@Peyu MINGW64 /e/Test/shell
$ log1=`cat log.txt`

ASUS@Peyu MINGW64 /e/Test/shell
$ echo $log1
沧海一遇却难找寻 前路崇山峻岭不再有你同行 纵使微茫如烟 纵有万般思念 流光总将故人搁浅在断简残篇 不成眠

ASUS@Peyu MINGW64 /e/Test/shell
$
```
## 只读变量

使用 **readonly** 命令可以将变量定义为只读变量，只读变量的值不能被改变。

下面的例子尝试更改只读变量，结果报错：

```ruby
#!/bin/bash

myUrl="http://hhyang.cn/"
readonly myUrl
myUrl="http://hhyang.cn/"
```
运行脚本，结果如下：

> bash: myUrl: This variable is read only.

## 删除变量
使用 **unset** 命令可以删除变量。语法：

```ruby
unset variable_name
```
变量被删除后不能再次使用；unset 命令不能删除只读变量。

举个例子：

```ruby
#!/bin/sh

myUrl="http://hhyang.cn/"
unset myUrl
echo $myUrl
```

上面的脚本没有任何输出。