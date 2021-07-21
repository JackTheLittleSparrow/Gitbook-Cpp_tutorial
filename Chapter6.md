# C++11新特性

注1：这篇笔记远远不够，关于c++11还需要深入了解才行

注2：如果有人打开了这篇笔记，作者强烈建议不要读，因为理解不透彻所以笔记非常的不清晰

#### 新增简单特性

+ auto类型推导，涉及cv属性需查文档
+ decltype关键字 ，decltype(expr)在编译时推导表达式类型
+ final和override
+ default和delete，=default， =delete
+ for循环，for(auto n : vec){}
+ nullptr， 引入空指针类型，区别与NULL（0）
+ enum class，引入强枚举类型，避免自动推导
+ tuple元组，泛化的std::pair

#### 模板细节改进

+ 支持连续右尖括号>>
+ 模板别名 using str_map_t = std::map<string, T>;
+ 支持函数的默认模板参数

```cpp
template<typename T = int>
void func(){}
```

#### 列表初始化

+ 支持初始化列表Initializer_list

#### std::function和bind

+ 四种可调用对象

  > 是一个函数指针
  >
  > 仿函数
  >
  > 类成员函数
  >
  > 可别转换为函数指针的类对象

+ bind可以将函数与其参数进行自由绑定

#### lambda表达式

```cpp
[capture](param)opt->ret{body;};
```

+ capture是捕获列表
+ opt是函数选项
+ 捕获规则

> []不捕获
>
> [&]捕获所有外部变量，传入引用
>
> [=]传值
>
> [=, &foo]捕获外部变量，并按引用捕获foo变量
>
> [bar]按值捕获bar
>
> [this]捕获当前类中的this指针

#### 右值引用，move语义，forward和完美转发

+ C++中右值包括将忘值和纯右值
+ 纯右值：非引用返回的临时变量，运算表达式产生的临时变量，原始字面量和lambda表达式都是纯右值
+ 将亡值：将要移动的对象，T&&函数返回值，std::move 返回值
+ 区分表达式左右值方法，&，可以取地址才为左值
+ 右值引用 && 因为右值不具名，只能这样找到它
+ 常量左值引用是一个万能引用类型，可以接受右值
+ 右值引用可以避免深拷贝
+ move将一个左值转换为右值
+ forward可以保持右值特性，即使是作为函数入参时

#### emplace_back减少内存拷贝和移动

+ emplace系列的函数通过直接构造对象的方式避免了内存的拷贝和移动

#### 新增unordered container无序容器

#### 日期时间库chrono

#### 智能指针

#### C++11支持正则表达式
