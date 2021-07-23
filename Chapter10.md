# Boost.Asio

#### 前言

`asio`库基于操作系统提供的异步机制，采用前摄器(Proactor)实现了可移植的异步（或同步）IO操作， 而且并不要求使用多线程的锁定，有效的避免了多线程编程带来的诸多副作用（条件竞争，死锁等）

`asio`使用不需要编译，他依赖其他的Boost组件，最基本的是boost.system和boost.datetime库，用来提供系统错误和时间支持。

#### 头文件

```cpp
#define BOOST_ASIO_DISABLE_STD_CHRONO	//使用boost.chrono库的时间功能
#include <boost/asio.hpp>
using namespace boost::asio;
```

#### 概述

`asio`库基于Proactor模式，封装了操作系统的select，poll，epoll，kqueue，IOCP等机制，实现了异步IO模型。他的核心类是`io_service`，相当于前摄器模式中的Proactor角色，`asio`的任何操作都需要有`io_service`的参与。

在 **同步模式**下，程序发起一个IO操作，向`io_service`提交请求，`io_service`把操作转交给窜哦做系统，同步的等待。当io操作完成时，操作系统通知`io_service`，然后`io_service`再把结果发回给程序，完成整个同步流程。类似join()。

在 **异步模式**下，程序发起一个IO操作，还需定义一个用于回调完成的处理函数（complete handler）。

`io_service`同样把IO操作转交给操作系统执行，但他不同步等待，而是立即返回。调用`io_service`的run()成员函数可以等待异步操作完成，当异步操作完成时`io_service`从操作系统获取执行结果，调用handler处理后续逻辑（回调）。

`asio`不直接使用操作系统提供的线程，而是定义一个自己的线程概念：strand 它保证在多线程的环境中代码可以正确的执行，而无需使用互斥量。`io_service::strand::wrap()`可以包装一个函数在strand中执行。

IO操作会经常使用到缓冲区，asio库专门用了两个类`mutable_buffer`和`const_buffer`来封装这个概念，它们可以别安全的应用在异步的读写操作中，使用自由函数`buffer()`能够包装常用的C++容器类型，如数组，array/vector/string等，用read(),write()函数来读取缓冲区。

`asio`库使用system库的error_code和system_error来表示程序运行的错误。基本上所有的函数都有两种重载方式，一种是有一个error_code的输出参数，调用后必须检查这个参数是否发生了错误；另一种形式没有error_code参数，如果发生了错误会抛出system_error异常，调用代码必须使用try-catch块来捕获错误。



……后面先不写了，动手试试才行。
