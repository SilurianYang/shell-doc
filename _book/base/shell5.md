# 1.5. Shell命令的基本格式

进入 Shell 以后，我们就可以输入命令来使用 Linux 的各种功能了，但是在真正使用 Shell 命令之前，我们有必要先学习一下 Shell 命令的基本格式。

进入 Shell 之后第一眼看到的内容类似下面这种形式：

```ruby
ubuntu@VM-0-3-ubuntu:~$ 
```

这叫做命令提示符，看见它就意味着可以输入命令了。命令提示符不是命令的一部分，它只是起到一个提示作用，我们将在《Shell命令提示符》一节中详细分析，本节只分析 Shell 命令的基本格式。

Shell 命令的基本格式如下：

```ruby
command [选项] [参数]
```

[]表示可选的，也就是可有可无。有些命令不写选项和参数也能执行，有些命令在必要的时候可以附带选项和参数。

ls 是常用的一个命令，它属于目录操作命令，用来列出当前目录下的文件和文件夹。ls 可以附带选项，也可以不带，不带选项的写法为：

```ruby
ubuntu@VM-0-3-ubuntu:~$ ls
bin
ubuntu@VM-0-3-ubuntu:~$ cd /
ubuntu@VM-0-3-ubuntu:/$ ls
bin  boot  data  dev  etc  home  initrd.img  lib  lib64  lost+found  media  mnt  opt  patch  proc  root  run  sbin  srv  sys  tmp  usr	var  vmlinuz  www
ubuntu@VM-0-3-ubuntu:/$ 
```

## 使用选项
ls 命令之后不加选项和参数也能执行，不过只能执行最基本的功能，即显示当前目录下的文件名。那么加入一个选项，会出现什么结果？
```ruby
ubuntu@VM-0-3-ubuntu:/$ ls -l
total 96
drwxr-xr-x   2 root root  4096 Mar 27 14:38 bin
drwxr-xr-x   3 root root  4096 Sep  7  2018 boot
drwxr-xr-x   2 root root  4096 Oct 28  2016 data
drwxr-xr-x  18 root root  3800 Mar 27 14:06 dev
drwxr-xr-x  98 root root  4096 Aug  8 16:13 etc
drwxr-xr-x   3 root root  4096 Oct 26  2016 home
lrwxrwxrwx   1 root root    33 Jul 24  2018 initrd.img -> boot/initrd.img-4.4.0-130-generic
drwxr-xr-x  22 root root  4096 Mar 27 14:59 lib
drwxr-xr-x   2 root root  4096 Mar 27 14:48 lib64
drwx------   2 root root 16384 Oct 26  2016 lost+found
drwxr-xr-x   3 root root  4096 Oct 26  2016 media
drwxr-xr-x   2 root root  4096 Jul 20  2016 mnt
drwxr-xr-x   2 root root  4096 Jul 20  2016 opt
drwxr-xr-x   2 root root  4096 Mar 27 14:43 patch
dr-xr-xr-x 134 root root     0 Mar 27 14:06 proc
drwx------   5 root root  4096 Mar 27 14:59 root
drwxr-xr-x  26 root root  1100 Aug 24 18:24 run
drwxr-xr-x   2 root root 12288 Mar 27 14:07 sbin
drwxr-xr-x   2 root root  4096 Jul 20  2016 srv
dr-xr-xr-x  13 root root     0 Aug 23 12:00 sys
drwxrwxrwx   9 root root  4096 Aug 24 18:26 tmp
drwxr-xr-x  10 root root  4096 Oct 26  2016 usr
drwxr-xr-x  12 root root  4096 Mar 27 14:36 var
lrwxrwxrwx   1 root root    30 Jul 24  2018 vmlinuz -> boot/vmlinuz-4.4.0-130-generic
drwxr-xr-x   7 root root  4096 Apr  3 09:46 www
ubuntu@VM-0-3-ubuntu:/$ 
```
如果加一个-l选项，则可以看到显示的内容明显增多了。-l是长格式（long list）的意思，也就是显示文件的详细信息。

可以看到，选项的作用是调整命令功能。如果没有选项，那么命令只能执行最基本的功能；而一旦有选项，则能执行更多功能，或者显示更加丰富的数据。

#### 短格式选项和长格式选项
Linux 的选项又分为短格式选项和长格式选项。
* 短格式选项是长格式选项的简写，用一个减号-和一个字母表示，例如ls -l。
* 长格式选项是完整的英文单词，用两个减号--和一个单词表示，例如ls --all。

一般情况下，短格式选项是长格式选项的缩写，也就是一个短格式选项会有对应的长格式选项。当然也有例外，比如 ls 命令的短格式选项-l就没有对应的长格式选项，所以具体的命令选项还需要通过帮助手册来查询。

## 使用参数
参数是命令的操作对象，一般情况下，文件、目录、用户和进程等都可以作为参数被命令操作。例如：
```ruby
ubuntu@VM-0-3-ubuntu:/$ ls -l ./home
total 4
drwxr-xr-x 7 ubuntu ubuntu 4096 Aug 24 18:24 ubuntu
ubuntu@VM-0-3-ubuntu:/$ 
```
但是为什么一开始 ls 命令可以省略参数？那是因为有默认参数。命令一般都需要加入参数，用于指定命令操作的对象是谁。如果可以省略参数，则一般都有默认参数。例如 ls：
```ruby
ubuntu@VM-0-3-ubuntu:/$ ls
bin  boot  data  dev  etc  home  initrd.img  lib  lib64  lost+found  media  mnt  opt  patch  proc  root  run  sbin  srv  sys  tmp  usr	var  vmlinuz  www
ubuntu@VM-0-3-ubuntu:/$ 
```
这个 ls 命令后面如果没有指定参数的话，默认参数是当前所在位置，所以会显示当前目录下的文件名。
#### 选项和参数一起使用
Shell 命令可以同时附带选项和参数，例如：
```ruby
ubuntu@VM-0-3-ubuntu:/$ echo "杨二郎"
杨二郎
ubuntu@VM-0-3-ubuntu:/$ echo -n "杨二郎"
杨二郎ubuntu@VM-0-3-ubuntu:/$ 
```
-n是 echo 命令的选项，"杨二郎"是 echo 命令的参数，它们被同时用于 echo 命令。

echo 命令用来输出一个字符串，默认输出完成后会换行；给它增加-n选项，就不会换行了。

## 选项附带的参数
有些命令的选项后面也可以附带参数，这些参数用来补全选项，或者调整选项的功能细节。

例如，read 命令用来读取用户输入的数据，并把读取到的数据赋值给一个变量，它通常的用法为：
```ruby
read name
```
name 为变量名。

如果我们只是想读取固定长度的字符串，那么可以给 read 命令增加-n选项。比如读取一个字符作为性别的标志，那么可以这样写：
```ruby
read -n 1 sex
```
`1`是`-n`选项的参数，`sex`是 read 命令的参数。

`-n`选项表示读取固定长度的字符串，那么它后面必然要跟一个数字用来指明长度，否则选项是不完整的。

## 总结
Shell 命令的选项用于调整命令功能，而命令的参数是这个命令的操作对象。有些选项后面也需要附带参数，以补全命令的功能。