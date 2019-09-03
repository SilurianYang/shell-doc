# 2.3. Shell命令替换

Shell 命令替换是指将命令的输出结果赋值给某个变量。比如，在某个目录中输入 ls 命令可查看当前目录中所有的文件，但如何将输出内容存入某个变量中呢？这就需要使用命令替换了，这也是 Shell 编程中使用非常频繁的功能。

Shell 中有两种方式可以完成命令替换，一种是反引号 \`  \`，一种是 `$()`，使用方法如下：

```ruby

variable=`commands`
variable=$(commands)
```

其中，variable 是变量名，commands 是要执行的命令。commands 可以只有一个命令，也可以有多个命令，多个命令之间以分号 `;` 分隔。

例如，date 命令用来获得当前的系统时间，使用命令替换可以将它的结果赋值给一个变量。

```ruby

#!/bin/bash
begin_time=`date`    #开始时间，使用``替换
sleep 10s            #休眠10秒
finish_time=$(date)  #结束时间，使用$()替换
echo "Begin time: $begin_time"
echo "Finish time: $finish_time"
```

```ruby
16067@hhyang MINGW64 /e/codeTest/test_shell
$ . ./b.sh
Begin time: 2019年09月 3日 21:58:28
Finish time: 2019年09月 3日 21:58:38

16067@hhyang MINGW64 /e/codeTest/test_shell
$
```

运行脚本，10 秒后可以看到输出结果：
Begin time: 2019年09月 3日 21:58:28
Finish time: 2019年09月 3日 21:58:38

使用 date 命令的`%s`格式控制符可以得到当前的 UNIX 时间戳，这样就可以直接计算脚本的运行时间了。UNIX 时间戳是指从 1970 年 1 月 1 日 00:00:00 到目前为止的秒数，不了解的读者[请猛击这里](https://baike.baidu.com/item/unix%E6%97%B6%E9%97%B4%E6%88%B3/2078227)。

```ruby
#!/bin/bash

begin_time=`date +%s`    #开始时间，使用``替换

sleep 10s                #休眠10秒

finish_time=$(date +%s)  #结束时间，使用$()替换

run_time=$((finish_time - begin_time))  #时间差

echo "begin time: $begin_time"
echo "finish time: $finish_time"
echo "run time: ${run_time}s"
```

```ruby

16067@hhyang MINGW64 /e/codeTest/test_shell
$ bash b.sh
begin time: 1567519816
finish time: 1567519826
run time: 10s

16067@hhyang MINGW64 /e/codeTest/test_shell
$
```

运行脚本，10 秒后可以看到输出结果：
begin time: 1567519816
finish time: 1567519826
run time: 10s


第 6 行代码中的`(( ))`是 Shell 数学计算命令。和 C++、C#、Java 等编程语言不同，在 Shell 中进行数据计算不那么方便，必须使用专门的数学计算命令，`(( ))` 就是其中之一。更多细节我们将会在《Shell数学计算》一节中详细讲解。

注意，如果被替换的命令的输出内容包括多行（也即有换行符），或者含有多个连续的空白符，那么在输出变量时应该将变量用双引号包围，否则系统会使用默认的空白符来填充，这会导致换行无效，以及连续的空白符被压缩成一个。请看下面的代码：

```ruby
#!/bin/bash
LSL=`ls -l`
echo $LSL  #不使用双引号包围
echo "--------------------------"  #输出分隔符
echo "$LSL"  #使用引号包围
```

运行结果：

```ruby
16067@hhyang MINGW64 /e/codeTest/test_shell
$ bash ./b.sh
total 7 -rwxr-xr-x 1 16067 197609 26 9月 2 22:10 a.sh -rwxr-xr-x 1 16067 197609 147 9月 3 22:35 b.sh -rwxr-xr-x 1 16067 197609 927 8月 26 22:25 log.sh -rw-r--r-- 1 16067 197609 63 8月 25 17:14 log1.txt
--------------------------
total 7
-rwxr-xr-x 1 16067 197609  26 9月   2 22:10 a.sh
-rwxr-xr-x 1 16067 197609 147 9月   3 22:35 b.sh
-rwxr-xr-x 1 16067 197609 927 8月  26 22:25 log.sh
-rw-r--r-- 1 16067 197609  63 8月  25 17:14 log1.txt

16067@hhyang MINGW64 /e/codeTest/test_shell
$
```

所以，为了防止出现格式混乱的情况，我建议在输出变量时加上双引号。


## 再谈反引号和 $()

原则上讲，上面提到的两种变量替换的形式是等价的，可以随意使用；但是，反引号毕竟看起来像单引号，有时候会对查看代码造成困扰，而使用 $() 就相对清晰，能有效避免这种混乱。而且有些情况必须使用 $()：$() 支持嵌套，反引号不行。

下面的例子演示了使用计算 ls 命令列出的第一个文件的行数，这里使用了两层嵌套。

```ruby
16067@hhyang MINGW64 /e/codeTest/test_shell
$ Fir_File_Lines=$(wc -l $(ls | sed -n '1p'))

16067@hhyang MINGW64 /e/codeTest/test_shell
$ echo "$Fir_File_Lines"
3 a.sh

16067@hhyang MINGW64 /e/codeTest/test_shell
$ 
```

要注意的是，$() 仅在 Bash Shell 中有效，而反引号可在多种 Shell 中使用。所以这两种命令替换的方式各有特点，究竟选用哪种方式全看个人需求。