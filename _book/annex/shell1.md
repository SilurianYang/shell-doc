# Shell 环境变量一览表

Bash Shell 还使用了许多环境变量。虽然环境变量不是命令，但它们通常会影响 Shell 命令的执行，所以了解这些 Shell 环境变量很重要。下表列出了 Bash Shell 中可用的默认环境变量。

## Bash Shell 环境变量

变量 |  说明
--- |----
*	| 含有所有命令行参数（以单个文本值的形式）
@	|含有所有命令行参数（以多个文本值的形式）
\#	|命令行参数数目
?	|最近使用的前台进程的退出状态码
-	|当前命令行选项标记
$	|当前shell的进程 ID (PID)
!	|最近执行的后台进程的 PID
0	|命令行中使用的命令名称
_	|shell 的绝对路径名
BASH	|用来调用 shell 的完整文件名
BASHOPTS	|允许冒号分隔列表形式的 Shell 选项
BASHPID	|当前 bash shell 的进程 ID
BASH_ALIASED	|含有当前所用别名的数组
BASH_ARGC	|当前子函数中的参数数量
BASH_ARGV	|含有所有指定命令行参数的数组
BASH_CMDS	|含有命令的内部散列表的数组
BASH_COMMAND	|当前正在被执行的命令名
BASH_ENV	|如果设置了的话，每个 bash 脚本都会尝试在运行前执行由该变量定义的起始文件
BASH_EXECUTION_STRING	|在 -c 命令行选项中用到的命令
BASH_LINENO	|含有脚本中每个命令的行号的数组
BASH_REMATCH	|含有与指定的正则表达式匹配的文本元素的数组
BASH_SOURCE	|含有 shell 中已声明函数所在源文件名的数组
BASH_SUBSHELL	|当前 shell 生成的子 shell 数目
BASH_VERS INFO	|含有当前 bash shell 实例的主版本号和次版本号的数组
BASH_VERS ION	|当前 bash shell 实例的版本号
BASH_XTRACEFD	|当设置一个有效的文件描述符整数时，跟踪输出生成，并与诊断和错误信息分离开文件描述符必须设置 -x 启动
COLUMNS	|含有当前 bash shell 实例使用的终端的宽度
COMP_CWORD	|含有变量 COMP_WORDS 的索引直，COMP_WORDS 包含当前光标所在的位置
COMP_KEY	|调用补全功能的按键
COMP_LINE	|当前命令行
COMP_POINT	|当前光标位置相对干当前命令起始位置的索引
COMP_TYPE	|补全类型所对应的整数值
COMP_WORDBREAKS	|在进行单词补全时闬作单词分隔符的一组字符
COMP_WORDS	|含有当前命令行上所有单词的数组
COMPREPLY	|含有由 shell 函数生成的可能补全码的数组
COPROC	|含有若干匿名协程 I/O 的文件描述符的数组
DIRSTACK	|含有目录栈当前内容的数组
EMACS	|如果设置了该环境变量，则 shell 认为其使用的是 emacs shell 缓冲区，同时禁止行编辑功能
ENV	|当 shell 以 POSIX 模式调用时，每个 bash 脚本在运行之前都会执行由该环境变量所定义的起始文件
EUID	|当前用户的有效用户 ID（数字形式）
FCEDIT	|fc 命令使用的默认编辑器
FIGNORE	|以冒号分隔的后缀名列表，在文件名补全时会被忽略
FUNCNAME	|当前执行的 shell 函数的名称
FUNCNEST	|嵌套函数的最髙层级
GLOBIGNORE	|以冒号分隔的模式列表，定义了文件名展开时要忽略的文件名集合
GROUPS	|含有当前用户属组的数组
histchars	|控制历史记录展开的字符（最多可有3个）
HISTCMD	|当前命令在历史记录中的编号
HISTCONTROL	|控制哪些命令留在历史记录列表中
HISTFILE	|保存 shell 历史记录列表的文件名（默认是 .bash_history）
HISTFILESIZE	|保存在历史文件中的最大行数
HISTIGNORE	|以冒号分隔的模式列表，用来决定哪些命令不存进历史文件
HISTSIZE	|最多在历史文件中保存多少条命令
HISTIMEFORMAT	|设置后，决定历史文件条目的时间戳的格式字符串
HOSTFILE	|含有 shell 在补全主机名时读取的文件的名称
HOSTNAME	|当前主机的名称
HOSTTYPE	|当前运行 bash shell 的机器
IGNOREEOF	|shell 在退出前必须收到连续的 EOF 字符的数量。如果这个值不存在，默认是 1
INPUTRC	|readline 初始化文件名（默认是 .inputrc）
LANG	|shell 的语言环境分类
LC_ALL	|定义一个语言环境分类，它会覆盖 LANG 变量
LC_COLLATE	|设置对字符串值排序时用的对照表顺序
LC_CTYPE	|决定在进行文件名扩展和模式匹配时，如何解释其中的字符
LC_MESSAGES	|决定解释前置美元符（$）的双引号字符串的语言环境设置
LC_NUMERIC	|决定格式化数字时的所使用的语言环境设置
LINENO	|脚本中当前执行代码的行号
LINES	|定义了终端上可见的行数
MACHTYPE	|用“cpu-公司-系统”格式定义的系统类型
MAILCHECK	|Shell 多久查看一次新邮件（以秒为单位，默认值是 60）
MAPFILE	|含有 mapfile 命令所读入文本的数组，当没有给出变量名的时候，使用该环境变量
OLDPWD	|shell 之前的工作目录
OPTERR	|设置为 1 时，bash shell 会显示 getopts 命令产生的错误
OSTYPE	|定义了 shell 运行的操作系统
PIPESTATUS	|含有前台进程退出状态码的数组
POSIXLY_CORRECT	|如果设置了该环境变量，bash 会以 POSIX 模式启动
PPID	|bash shell 父进程的 PID
PROMPT_COMMAND	|如果设置该环境变量，在显示命令行主提示符之前会执行这条命令
PS1	|主命令行提示符字符串
PS2	|次命令行提示符字符串
PS3	|select 命令的提示符
PS4	|如果使用了 bash 的 -x 选项，在命令行显示之前显示的提示符
PWD	|当前工作目录
RANDOM	|返回一个 0~32 767 的随机数，对其赋值可作为随机数生成器的种子
READLINE_LINE	|保存了 readline 行缓冲区中的内容
READLINE_POINT	|当前 readline 行缓冲区的插入点位置
REPLY	|read 命令的默认变量
SECONDS	|自 shell 启动到现在的秒数，对其赋值将会重置计时器
SHELL	|shell 的全路径名
SHELLOPTS	|已启用 bash shell 选项列表，由冒号分隔
SHLVL	|表明 shell 层级，每次启动一个新的 bash shell 时计数加 1
TIMEFORMAT	|指定了 shell 显示的时间值的格式
TMOUT	|select 和 read 命令在没输入的情况下等待多久（以秒为单位)。默认值为零，表示无限长
TMPDIR	|如果设置成目录名，shell 会将其作为临时文件目录
UID	|当前用户的真实用户 ID (数字形式）

可以用 set 内建命令来显示这些环境变量。对于不同的 Linux 发行版，开机时设置的默认 shell 环境变量经常会不一样。