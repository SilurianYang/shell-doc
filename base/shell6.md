# 1.6. Shell命令的本质到底是什么？如何自己实现一个命令？

《[Shell是什么](./shell1.md)》一节中讲到，用户通过在 Shell 中输入一些命令来使用 Linux。给命令附带不同的选项后，同一个命令的功能也会有所差异。

Shell 命令分为两种：
* Shell 自带的命令称为内置命令，它在 Shell 内部可以通过函数来实现，当 Shell 启动后，这些命令所对应的代码（函数体代码）也被加载到内存中，所以使用内置命令是非常快速的。
* 更多的命令是外部的应用程序，一个命令就对应一个应用程序。运行外部命令要开启一个新的进程，所以效率上比内置命令差很多。

用户输入一个命令后，Shell 先检测该命令是不是内置命令，如果是就执行，如果不是就检测有没有对应的外部程序：有的话就转而执行外部程序，执行结束后再回到 Shell；没有的话就报错，告诉用户该命令不存在。

## 内置命令
内置命令不宜过多，过多的内置命令会导致 Shell 程序本身体积膨胀，运行 Shell 程序后就会占用更多的内存。Shell 是一个常驻内存的程序，占用过多内存会影响其它的程序。

只有那些最常用的命令才有理由成为内置命令，比如 cd、kill、echo 等；你可以转到《Shell内置命令》来了解所有的内置命令，以及如何判断一个命令是否是内置命令。

## 外部命令
外部命令可能是读者比较疑惑的，一个外部的应用程序究竟是如何变成一个 Shell 命令的呢？

应用程序就是一个文件，只不过这个文件是可以执行的。既然是文件，那么它就有一个名字，并且存放在文件系统中。用户在 Shell 中输入一个外部命令后，只是将可执行文件的名字告诉了 Shell，但是并没有告诉 Shell 去哪里寻找这个文件。

难道 Shell 要遍历整个文件系统，查看每个目录吗？这显然是不能实现的。

为了解决这个问题，Shell 在启动文件中增加了一个叫做 PATH 的环境变量，该变量就保存了 Shell 对外部命令的查找路径，如果在这些路径下找不到同名的文件，Shell 也不会再去其它路径下查找了，它就直接报错。

你不用关心启动文件（我们将在《Shell启动文件》中详解），只需要知道 PATH 变量保存了检索路径即可。

我们使用 echo 命令输出 PATH 变量的值，看看它保存了哪些检索路径：
```ruby
ubuntu@VM-0-3-ubuntu:/$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
ubuntu@VM-0-3-ubuntu:/$ 
```
不同的路径之间以:分隔。你看，Shell 只会在几个固定的路径中查找外部命令。

如果我们自己用C语言或者 C++ 编写一个应用程序，并将它放到这几个目录下面，那么我们的程序也会成为 Shell 命令。当然，你也可以修改 PATH 变量给它增加另外的路径，不过这并不是本文的重点，有兴趣的读者请转到《编写自己的Shell配置文件》。

```ruby{.line-numbers}
#include <stdio.h>
#include <unistd.h>
#include <getopt.h>
#include <stdlib.h>
int main(int argc, char *argv[]){
    int start = 0;
    int end = 0;
    int sum = 0;
    int opt;
    char *optstring = ":s:e:";
    while((opt = getopt(argc, argv, optstring))!= -1){
        switch(opt){
            case 's': start = atoi(optarg); break;
            case 'e': end = atoi(optarg); break;
            case ':': puts("Missing parameter"); exit(1);
        }
    }
   
    if(start<0 || end<=start){
        puts("Parameter error"); exit(2);
    }
   
    for(int i=start; i<=end; i++){
        sum+=i;
    }
    printf("%d\n", sum);
    return 0;
}
```
将这段代码编译成名为 getsum 的应用程序，并放在`~/bin`目录（~ 表示用户主目录）下，然后在 Shell 中输入下面的命令，就可以计算 1+2+3 ...... +99+100 的值。
```ruby
ubuntu@VM-0-3-ubuntu:/$ getsum -s 1 -e 100
5050
ubuntu@VM-0-3-ubuntu:/$ 
```

`-s`选项表示起始数字，`-e`选项表示终止数字。

对于不了解C语言的读者，我也提供了编译好的 getsum 程序，[请猛击这里下载](../assets/files/getsum.zip)。

## 总结

Shell 内置命令的本质是一个自带的函数，执行内置命令就是调用这个自带的函数。因为函数代码在 Shell 启动时已经被加载到内存了，所以内置命令的执行速度很快。

Shell 外部命令的本质是一个应用程序，执行外部命令就是启动一个新的应用程序。因为要创建新的进程并加载应用程序的代码，所以外部命令的执行速度很慢
