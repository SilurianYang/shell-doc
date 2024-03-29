# 1.9. Shell修改命令提示符

Shell 通过`PS1`和`PS2`这两个环境变量来控制提示符的格式，修改`PS1`和`PS2`的值就能修改命令提示符的格式。
* `PS1` 控制最外层的命令提示符格式。
* `PS2` 控制第二层的命令提示符格式。
在修改 PS1 和 PS2 之前，我们先用 echo 命令输出它们的值，看看默认情况下是什么样子的：

```ruby
ubuntu@VM-0-3-ubuntu:/etc/acpi$ echo $PS1
${debian_chroot:+($debian_chroot)}\u@\h:\w\$
ubuntu@VM-0-3-ubuntu:/etc/acpi$ echo $PS2
>
ubuntu@VM-0-3-ubuntu:/etc/acpi$ 
```
Linux 使用以`\`为前导的特殊字符来表示命令提示符中包含的要素，这使得 PS1 和 PS2 的格式看起来可能有点奇怪。下表展示了可以在 PS1 和 PS2 中使用的特殊字符。

#### Bash shell 命令提示符可以包含的要素：
字符|描述
----| -----
\a	|铃声字符
\d	|格式为“日 月 年”的日期
\e	|ASCII 转义字符
\h	|本地主机名
\H	|完全合格的限定域主机名
\j	|shell 当前管理的作业数
\1	|shell 终端设备名的基本名称
\n	|ASCII 换行字符
\r	|ASCII 回车
\s	|shell 的名称
\t	|格式为“小时:分钟:秒”的24小时制的当前时间
\T	|格式为“小时:分钟:秒”的12小时制的当前时间
\\@	|格式为 am/pm 的12小时制的当前时间
\u	|当前用户的用户名
\v	|bash shell 的版本
\V	|bash shell 的发布级别
\w	|当前工作目录
\W	|当前工作目录的基本名称
\\!	|该命令的 bash shell 历史数
\\#	|该命令的命令数量
\\$	|如果是普通用户，则为美元符号`$`；如果超级用户（root 用户），则为井号`#`。
\nnn	|对应于八进制值 nnn 的字符
\\\	|斜杠
\[	|控制码序列的开头
\]	|控制码序列的结尾

注意，所有的特殊字符均以反斜杠`\`开头，目的是与普通字符区分开来。您可以在命令提示符中使用以上任何特殊字符的组合。

【实例】通过修改 PS1 变量的值来修改命令提示符的格式：
```ruby
ubuntu@VM-0-3-ubuntu:~$ PS1="\t@[\u]\$: "
21:14:09@[ubuntu]$: PS1="${debian_chroot:+($debian_chroot)}\u@\h:\w\$"
ubuntu@VM-0-3-ubuntu:~$
```
第一次修改后可以显示当前的时间和用户名。

遗憾的是，通过这种方式修改的命令提示符只在当前的 Shell 会话期间有效，再次启动 Shell 后将重新使用默认的命令提示符。

如果希望持久性地修改 PS1，让它对任何 Shell 会话都有效，那么就得把 PS1 变量的修改写入到 Shell 启动文件中，我们将在《[编写自己的Shell配置文件](./shell14.md)》一节中展开讨论。