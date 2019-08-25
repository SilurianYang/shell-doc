# 1.13. Shell配置文件（配置脚本）的加载

无论是否是交互式，是否是登录式，Bash Shell 在启动时总要配置其运行环境，例如初始化环境变量、设置命令提示符、指定系统命令路径等。这个过程是通过加载一系列配置文件完成的，这些配置文件其实就是 Shell 脚本文件。

与 Bash Shell 有关的配置文件主要有 /etc/profile、\~/.bash_profile、\~/.bash_login、\~/.profile、\~/.bashrc、/etc/bashrc、/etc/profile.d/*.sh，不同的启动方式会加载不同的配置文件。

>`~`表示用户主目录。`*`是通配符，/etc/profile.d/*.sh 表示 /etc/profile.d/ 目录下所有的脚本文件（以`.sh`结尾的文件）。

## 登录式的 Shell

Bash 官方文档说：如果是登录式的 Shell，首先会读取和执行 /etc/profiles，这是所有用户的全局配置文件，接着会到用户主目录中寻找 \~/.bash_profile、\~/.bash_login 或者 \~/.profile，它们都是用户个人的配置文件。

不同的 Linux 发行版附带的个人配置文件也不同，有的可能只有其中一个，有的可能三者都有，笔者使用的是 ubuntu，该发行版一个都没有。

如果三个文件同时存在的话，到底应该加载哪一个呢？它们的优先级顺序是 \~/.bash_profile > \~/.bash_login > \~/.profile。

如果 \~/.bash_profile 存在，那么一切以该文件为准，并且到此结束，不再加载其它的配置文件。

如果 \~/.bash_profile 不存在，那么尝试加载 \~/.bash_login。\~/.bash_login 存在的话就到此结束，不存在的话就加载 \~/.profile。

注意，/etc/profiles 文件还会嵌套加载 /etc/profile.d/*.sh，请看下面的代码：


``` {.line-numbers}
for i in /etc/profile.d/*.sh ; do
    if [ -r "$i" ]; then
        if [ "${-#*i}" != "$-" ]; then
            . "$i"
        else
            . "$i" >/dev/null
        fi
    fi
done
```

同样，\~/.bash_profile 也使用类似的方式加载 ~/.bashrc：

``` {.line-numbers}

if [ -f ~/.bashrc ]; then
. ~/.bashrc
fi

```

## 非登录的 Shell

如果以非登录的方式启动 Shell，那么就不会读取以上所说的配置文件，而是直接读取 \~/.bashrc。

\~/.bashrc 文件还会嵌套加载 /etc/bashrc，请看下面的代码：

```  {.line-numbers}
if [ -f /etc/bashrc ]; then
. /etc/bashrc
fi
```