# GDB

#### GDB调试常用命令

+ 前置条件 编译时使用 -g

+ 基础操作： gdb -p pid  / gdb program core /  r,n,s,c,bt,,list,pwd,set directory， b
+ 其他命令：
  + finish忽略函数未执行部分直接返回
  + call单步执行，进入函数
  + bt N/-N  显示开头/结尾的N个栈帧
  + f(rame) N  显示第N层栈帧
  + 反复执行 c ,s , n   N
+ 断点 
  + b a.cpp:21
  + info b
  + b if condition
  + b a.cpp thread 线程号
  + delete disable enable
  + command 断点号，断点触发时，执行命令，一般用于打印变量
+ 检测点 watch ， watch+*(pointer)
+ 查看变量
  + set print element N 定打印的长度，对长字符串特别好用
  + set print element 0  输出完整的字符串
  + set print pretty  
  + info watchpoints
  + print 变量  /x 16进制显示变量
  + print str@str_len  
  + info locals/ args 打印局部变量/ 打印当前函数的参数名及其值
  + display / undisplay 自动打印变量，取消自动打印
  + show print elements
+ 内存查看 x /nfu <addr>
  + n:显示内存单元的个数
  + f: x,d,u,o,t,a,i,c,f,s
  + u: b,h,w,g 一个地址单元的长度单双四八
+ 多线程调试
  + info threads
  + thread thread_id
  + thread apply all command
  + 线程锁
    + show scheduler-locking
    + set scheduler-locking on/off
  + non-stop 模式，除了断点有关的线程会停下来，其他的线程会执行。(GDB7.0后支持)
    + set target-async 1
    + set pagination off
    + set non-stop on、
+ 信号量signal，处理函数handle
  + handle 信号名 处理函数
    + 处理函数：nostop/stop , print/noprint, pass/nopass
    + 例： handle SIGPIPE stop print
+ core-dump 记录程序运行时的内存，寄存器，堆栈指针，函数调用堆栈等信息，通过工具分析可以定位程序退出时的问题。
  + 前置条件 ulimit -c查看是否开启core文件存储0为未开启， ulimit -c unlimited开启
  + 可以设置core文件存储位置，现用现查
  + man 7 signal 查看一些core产生对应的信号

#### GDB调试实例

+ 实操内容：略
+ 注：需要原教程可以内网滴滴我
