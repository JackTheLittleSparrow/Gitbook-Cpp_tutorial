# Shell

#### Shell 概述

+ Linux上面打开的终端就是Shell进程，或者Bash进程（Born Again Shell）
+ Shell 支持脚本语言，按句执行
+ shell 脚本中需要指定解释器路径

```shell
#！bin/bash
```

#### Shell 编码规范

+ 需要注释，参考命令帮助文档
+ 参数规范，进行严格的参数检查，适当回显
+ shell中默认变量都是全局的，全局变量要交g_前缀，局部变量使用local修饰
+ 别用魔数，需要使用时可以定义变量，既方便修改也方便明确变量含义
+ 缩进分为sotf tab和hard tab，soft就是转化成空格，hard就是\t
+ 字符编码要统一，还有换行
+ win平台下使用UTF-8编写shell时要注意这个UTF-8是否有BOM。因为Linux中无BOM，所以要选不带BOM的UTF-8
+ 语句太长直接反斜杠  \  分行
+ 使用 “$”取变量的值时，双引号能用就用，防止歧义
+ 正确获取路径的方法

```shell
# 正确获取路径的方法
# $0 当前shell程序的文件名
# dirname $0 获取当前shell程序的路径
g_script_dir=$(cd $(dirname $0) && pwd)
echo "$g_script_dir"
g_script_dir=$(dirname $(readlink -f $0))
echo "$g_script_dir"
g_script_dir=`realpath $(dirname $0)`
echo "$g_script_dir"
```



#### Shell 语法

+ 特殊符号： ! 执行历史命令！！上一条命令&后台运行``反引号可以执行 命令‘ ’单引号无法解析变量

+ 管道，重定向，read， 数学运算， printf， 颜色代码略过，只记不用卵用没有

+ shell变量定义等号两侧无空格，unset删除变量，export定义全局变量

+ 特殊变量：$0 , $n, #, *整体,@,?上个命令退出状态,$$当前Shell进程ID

+ 字符串查找expr index  "$str" x(其他操作替换index，length， substr)，定义，拼接略

+ 数组，使用（）定义，赋值时多个值使用空格（不使用逗号）隔开

+ 关联数组，（类似map），自定义索引（name），值为（value）

+ 查看数组declate -A

+ 访问数组中的元素

  ```shell
  echo ${ass_arr[index2]}	#访问第二个元素
  echo ${ass_arr[@]}		#遍历数组元素
  echo ${#ass_arr[@]}		#获取数组长度
  echo ${!ass_arr[@]} 	#获取数组元素的索引
  ```

#### Shell 流程控制

+ 数学比较 -eq -gt -lt -ge -le -ne ……
+ 字符串比较 ==  !=  -n  -z 检查字符串长度，可以用双中括号[[]],或者字符串取值都用“ ”包含
+ 文件检查比较，参数太多，用再去查
+ 逻辑 &&  ||   ！
+ if for while case
+ Shell函数，多种形式

#### 正则表达式

+ 正则表达式定位符

> ^  ^a锚定开头，以a开头
>
> $ a$锚定结尾，以$结尾

+ 正则表达式匹配符

> .  匹配除回车外任意字符
>
> ()字符串分组
>
> []定义字符类，匹配括号中的一个字符
>
> [^]表示否定括号中出现字符类的字符，去反
>
> \ 转义字符
>
> | 管道

+ 正则表达式限定符

> \*   字符重复0到无数次
>
> ？ 字符出现一次或0次
>
> \+   加号前面字符至少出现一次
>
> {n,m}  某字符出现至少n次，最多m次
>
> {m}  出现m次

#### 常用命令

+ nohup (no hang up)

```shell
nohup run.sh >> train.log 2>&1 &
```

+ 注意

  + 0 stdin
  + 1 stdout
  + 2 stderr
  + 2>&1 是一个整体，表示将出错信息重定向到标准输出,&1时为了区分文件名叫1的文件和标准输出
  + \>> 表示附加(append)，最后的&表示后台执行，nohup即使推出终端也不会影响程序运行
  + 与后台运行相关的几个命令 
    + Ctrl+z  暂停并放到后台
    + bg ,fg  %jobnumber 后台，前台执行
    + jobs 查看后台运行的程序

+ grep 查找命令

  + -r -v -e

+ xargs (eXtended ARGuments)

  + xargs用于给命令传递参数，参数可以是文件内容，标准输入输出内容，也可以捕获前一个命令的输出
  + xargs捕获的文件会将其中的换行和空白替换为空格
  + -n  多行输出，定义列数量 -d  自定义分界符（特殊的 -0 是将\0 作为定界符)
  + cat   arg.txt | xargs -I {} ./sk.sh -p {} -l

  ```shell
  # arg.txt
  aaa
  bbb
  ccc
  
  # sk.sh
  echo $*
  
  #output
  -p aaa -l
  -p bbb -l
  -p ccc -l
  ```

  + -i(大写，为与L区分暂时使用小写) -i {}  ./sk.sh {} 解释：前一个{}相当于一个占位符，会在xargs展开时被替换掉，{（字符串，包含空串）}

+ [sed](sed.md)

+ awk是一种处理文本文件的语言，是一个强大的文本分析工具，名字是三位创始人Family Name的首字母
  + 按行处理文件，或者按指定分割符处理，默认分隔符是空格，换行，和tab
  + -v 定义变量（局部） -F 定义分隔符
  + 例： awk  '{printf $1, $4}'  test.txt      打印test.txt中每一行的第一项和第四项
