# TCPEngine组件

#### 概述

封装底层TCP编程接口，大大降低了TCP服务端和客户端的难度

#### 特点

1. 资源自动化管理
2. 支持海量连接
3. 支持用户自定义消息解析模板，辅助消息分解和拼接
4. 心跳检查和心跳保持
5. 连接接口和发送消息接口异步调用且线程安全。

#### 使用说明

##### 引入头文件`#include <adk/io_engine.h>`

##### 初始化阶段

每个使用TCP引擎的程序至少需要一个`adk::io_engine::TcpEngine`对象。

通过该类的`Create`静态方法获得对象指针。

应用初始化`Create`的时候，通过入参`adk::io_engine::Property`容器对象的各个key-value来决定该Tcp引擎对象是 **服务端**还是 **客户端**。如果指定了`kAcceptHandler`属性说明是服务端，如果指定了`kConnectHandler`属性说明了该Tcp引擎是客户端。

这两个属性键值对应的值是一个对象指针（句柄Handler），该句柄对应的类应该要继承`AcceptHandler ConnectHandler`来重写`OnAccept OnConnect`回调函数。

# 剩下的部分将在内网编写，无法同步到github了

# 参考文档位于内网路径\\192.168.102.21\share_dir\业务及技术培训\2021年度C++开发基础系列培训

