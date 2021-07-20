# 华锐C++编码规范

### 设计规范

#### 类型定义规范

+ 仅含数据时使用结构体，其他一律使用类
+ 新项目建议改为采用enum class
+ 声明顺序public protected private

#### 成员设计规范

+ 构造函数做最少的事情，单参数构造函数使用explicit关键字，不需要拷贝构造时继承boost::noncopyable禁止拷贝
+ 基类析构函数定义为public+virtual 或 protected+非virtual
  + public+virtual防止调用析构函数时基类析构了子类没有析构
  + 不想让外部用户直接构造一个类对象A，而是希望用户只能构造A的子类，就可以将A的构造/析构函数定义为protected，子类的定义为public
+ 函数尽量内聚，短小
+ 存取控制：数据成员声明为private并且定义对应的取值赋值函数

```cpp
class A
{
  ...
public:
    a();
    set_a();
private:
    dataType a_;
};
```

+ 尽量少采用运算符重载，多使用仿函数（重载（））
+ for_each()
+ 组合常常比继承更合理
+ 接口以 I 为前缀

> 接口需满足以下条件：
>
> + 只有纯虚函数和静态函数
> + 没有非静态数据成员
> + 没有定义任何构造函数，即使有也不能有参数，并且必须为protected
> + 如果他是一个子类，父类也需满足以上特点
> + 为了保证接口类的所有实现正确被销毁，必须为接口声明虚析构函数（析构函数不能是纯虚函数）

+ 回调函数，移植性设计规范，错误处理（try catch)， 安全性设计规范，略

### 编码规范

#### 头文件

+ 保护性声明防止多重包含

```cpp
#ifndef AMI_AMI_H_
#define AMI_AMI_H_
```

+ 能用前置声明的地方尽量不适用#include
+ 复杂的内联函数放在后缀为-inl.h的头文件中
+ 定义函数时参数顺序为输入参数，输出参数
+ include的文件被实现的头文件和本项目内的头文件用“”包含，库函数和其他库用尖括号<>引入

#### 作用域

+ 不要使用using namespace引入名字空间污染
+ 少用全局函数
+ 禁止使用class类型的静态或全局变量。

#### C++特性规范

+ 正确使用assert，assert只在debug情况下有作用，release时被替换为空操作，建议使用BOOST_ASSERT。
+ 字节对齐

```cpp
#pragma pack(push)	//保存当前对其状态
#pragma pack(1)		//设定为1字节对齐
struct Message
{
    int32_t msg_type;
    int32_t len;
    char a;
}
#pragma pack(pop)	//恢复先前的对齐状态
```

+ 能用const就用， 善用智能指针scoped_ptr最常用，慎用shared_ptr, 尽量不用auto_ptr
+ 前置自增无需临时对象，效率略高于后置
+ 宏用于代码展开
+ 整数用0， 空指针用nullptr
+ sizeof(varname)   优于  sizeof(type)

#### 命名约定

+ 文件名和命名空间全部小写
+ 常量命名以k开头每个单词首字母大写直接相连，枚举值命名与常量命名规则一样
+ 函数命名大小混写，取值函数与变量同名，赋值函数为set_varname()

#### 格式约定

+ @param后面加上[in] [out] [in,out]， /// 为单行注释， ///<为跟在成员变量之后的注释，//与注释间需加空格。
+ “(”后，“ )”前 无空格
+ 回车换行为Carriage Return（CR \r）， Line Feed（LF \n），win 为CRLF ，linux为LF
+ 一行过长时在逗号后，运算符前换行
+ 文件头注释格式

```cpp
/**
 * @file
 * @brief Fun类的声明
 * @author
 */
```

+ 标准函数注释

```cpp
/**
 * @brief 函数作用简介
 *
 * @param[in] 
 *
 * @param[in] 
 *
 * @return
 *
 *
 * @par 示例
 * @code
 
 
   @endcode
 */
```

