# Protobuf组件

protobuf全称Google Protocol Buffers， 可以通过定义protobuf的数据结构。用protobuf编译器生成特定语言的源代码。

#### 使用方法

+ 编写.proto文件
+ 编译.proto文件，生成对应语言的操作文件

```cpp
protoc --cpp_out=./test_pb.proto
```

生成`test_pb.h test_pb.cc`文件

```cpp
# 完整命令
protoc -I=$SRC_DIR --cpp_out=$DST_DIR $SRC_DIR/xxx.proto
```

#### protobuf的另一个重要功能是序列化和反序列化

有很多接口，详见文档

reference:

https://developers.google.cn/protocol-buffers/docs/cpptutorial

https://www.jianshu.com/p/a24c88c0526a

https://zhuanlan.zhihu.com/p/141415216ppp
