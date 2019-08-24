# 1.12. Shell四种运行方式（启动方式）精讲

Shell 是一个应用程序，它的一端连接着 Linux 内核，另一端连接着用户。Shell 是用户和 Linux 系统沟通的桥梁，我们都是通过 Shell 来管理 Linux 系统。

我们可以直接使用 Shell，也可以输入用户名和密码后再使用 Shell；第一种叫做非登录式，第二种叫做登录式。

我们可以在 Shell 中一个个地输入命令并及时查看它们的输出结果，整个过程都在跟 Shell 不停地互动，这叫做交互式。我们也可以运行一个 Shell 脚本文件，让所有命令批量化、一次性地执行，这叫做非交互式。

总起来说，Shell 一共有四种运行方式：
* 交互式的登录 Shell；
* 交互式的非登录 Shell；
* 非交互式的登录 Shell；
* 非交互式的非登录 Shell。

## 判断 Shell 是否是交互式
判断是否为交互式 Shell 有两种简单的方法。

1) 查看变量-的值，如果值中包含了字母`i`，则表示交互式（interactive）。

【实例1】在 ubuntu 终端下输出-的值：
```ruby {.line-numbers}
ubuntu@VM-0-3-ubuntu:~/bin$echo $-
himBH
ubuntu@VM-0-3-ubuntu:~/bin$
```
包含了`i`，为交互式。

【实例2】在 Shell 脚本文件中输出`-`的值：
```ruby {.line-numbers}
ubuntu@VM-0-3-ubuntu:~/bin$bash ./test.sh
hB
```
不包含`i`，为非交互式。注意，必须在新进程中运行 Shell 脚本。

2) 查看变量PS1的值，如果非空，则为交互式，否则为非交互式，因为非交互式会清空该变量。

【实例1】在 ubuntu 终端下输出PS1的值：
```ruby {.line-numbers}
ubuntu@VM-0-3-ubuntu:~/bin$echo $PS1
\u@\h:\w$
ubuntu@VM-0-3-ubuntu:~/bin$
```
非空，为交互式。

【实例2】在 Shell 脚本文件中输出 PS1 的值：
```ruby {.line-numbers}
ubuntu@VM-0-3-ubuntu:~/bin$bash ./test.sh

```
空值，为非交互式。注意，必须在新进程中运行 Shell 脚本。

## 判断 Shell 是否为登录式

判断 Shell 是否为登录式也非常简单，只需执`行shopt login_shell`即可，值为`on`表示为登录式，`off`为非登录式。

shopt 命令用来查看或设置 Shell 中的行为选项，这些选项可以增强 Shell 的易用性。

【实例1】在 ubuntu 终端下查看 login_shell 选项：
```ruby {.line-numbers}
ubuntu@VM-0-3-ubuntu:~/bin$shopt login_shell
login_shell    	on
ubuntu@VM-0-3-ubuntu:~/bin$
```
【实例2】在 Shell 脚本文件中查看 login_shel 选项：
```ruby {.line-numbers}
ubuntu@VM-0-3-ubuntu:~/bin$bash test.sh 
login_shell    	off
ubuntu@VM-0-3-ubuntu:~/bin$
```

## 同时判断交互式、登录式
要同时判断是否为交互式和登录式，可以简单使用如下的命令：
```ruby {.line-numbers}
echo $PS1; shopt login_shell
```
或者
```ruby {.line-numbers}
echo $-; shopt login_shell
```

## 常见的 Shell 启动方式
1) 通过 Linux 控制台（不是桌面环境自带的终端）或者 ssh 登录 Shell 时（这才是正常登录方式），为交互式的登录 Shell。
```ruby {.line-numbers}
ubuntu@VM-0-3-ubuntu:~/bin$echo $PS1;shopt login_shell
\u@\h:\w$
login_shell    	on
ubuntu@VM-0-3-ubuntu:~/bin$
```
2) 执行 bash 命令时默认是非登录的，增加`--login`选项（简写为`-l`）后变成登录式。
```ruby {.line-numbers}
ubuntu@VM-0-3-ubuntu:~/bin$bash test.sh 
login_shell    	off
ubuntu@VM-0-3-ubuntu:~/bin$bash -l test.sh 

                   .----------.
                  /   杨二郎   \
                 |              |
                 |,  .-.  .-.  ,|
                 | )(__/  \__)( |
                 |/     /\     \|
       (@_       (_     ^^     _)
  _     ) \_______\__|IIIIII|__/__________________________
 (_)@8@8{}<________|-\IIIIII/-|___________________________＞
        )_/        \          /
       (@           `--------`

login_shell    	on
ubuntu@VM-0-3-ubuntu:~/bin$
```
3) 使用由`()`包围的组命令或者命令替换进入子 Shell 时，子 Shell 会继承父 Shell 的交互和登录属性。
```ruby {.line-numbers}
ubuntu@VM-0-3-ubuntu:~/bin$ (echo $PS1;shopt login_shell)
\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\u@\h:\w\$
login_shell    	off
ubuntu@VM-0-3-ubuntu:~/bin$ bash -l

                   .----------.
                  /   杨二郎   \
                 |              |
                 |,  .-.  .-.  ,|
                 | )(__/  \__)( |
                 |/     /\     \|
       (@_       (_     ^^     _)
  _     ) \_______\__|IIIIII|__/__________________________
 (_)@8@8{}<________|-\IIIIII/-|___________________________＞
        )_/        \          /
       (@           `--------`

ubuntu@VM-0-3-ubuntu:~/bin$ (echo $PS1;shopt login_shell)
${debian_chroot:+($debian_chroot)}\u@\h:\w\$
login_shell    	on
ubuntu@VM-0-3-ubuntu:~/bin$ 
```
4) ssh 执行远程命令，但不登录时，为非交互非登录式。
```ruby {.line-numbers}
ubuntu@VM-0-3-ubuntu:~/bin$ ssh localhost 'echo $PS1;shopt login_shell'
ubuntu@localhost's password: 

login_shell    	off
ubuntu@VM-0-3-ubuntu:~/bin$ 
```