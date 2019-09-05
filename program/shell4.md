# 2.4. Shell位置参数

我们先来说一下 Shell 位置参数是怎么回事。

运行 Shell 脚本文件时我们可以给它传递一些参数，这些参数在脚本文件内部可以使用 `$n` 的形式来接收，例如，`$1` 表示第一个参数，`$2` 表示第二个参数，依次类推。

同样，在调用函数时也可以传递参数。Shell 函数参数的传递和其它编程语言不同，没有所谓的形参和实参，在定义函数时也不用指明参数的名字和数目。换句话说，定义 Shell 函数时不能带参数，但是在调用函数时却可以传递参数，这些传递进来的参数，在函数内部就也使用 `$n` 的形式接收，例如，`$1` 表示第一个参数，`$2` 表示第二个参数，依次类推。

这种通过 `$n` 的形式来接收的参数，在 Shell 中称为位置参数。

在讲解变量的命名时，我们提到：变量的名字必须以字母或者下划线开头，不能以数字开头；但是位置参数却偏偏是数字，这和变量的命名规则是相悖的，所以我们将它们视为“特殊变量”。

除了 `$n`，Shell 中还有 `$#`、`$*`、`$@`、`$?`、`$$` 几个特殊参数，我们将在下节讲解。


## 给脚本文件传递位置参数

```ruby

#!/bin/bash

echo "Language: $1"
echo "URL: $2"
```

运行 `test.sh`，并附带参数：

```ruby

ASUS@Peyu MINGW64 /e/Test/shell
$ bash ./test.sh shell http://hhyang.cn/doc7/
Language: shell
URL: http://hhyang.cn/doc7/

ASUS@Peyu MINGW64 /e/Test/shell
$
```

其中 `Shell` 是第一个位置参数，`http://c.biancheng.net/shell/` 是第二个位置参数，两者之间以空格分隔。

## 给函数传递位置参数

请编写下面的代码，并命名为 `test.sh`：

```ruby

#!/bin/bash

#定义函数
function func(){
    echo "Language: $1"
    echo "URL: $2"
}

#调用函数
func shell http://hhyang.cn/doc7/
```
运行 `test.sh`：

```ruby

ASUS@Peyu MINGW64 /e/Test/shell
$ bash ./test.sh
Language: shell
URL: http://hhyang.cn/doc7/

ASUS@Peyu MINGW64 /e/Test/shell
$
```

关于函数定义和调用的具体语法请访问：Shell函数定义和调用、Shell函数参数

#### 注意事项

如果参数个数太多，达到或者超过了 10 个，那么就得用 `${n}` 的形式来接收了，例如 `${10}`、`${23}`。`{ }` 的作用是为了帮助解释器识别参数的边界，这跟使用变量时加 `{ }` 是一样的效果。