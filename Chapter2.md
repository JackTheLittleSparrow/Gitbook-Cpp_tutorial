# Boost.Build

#### 环境配置

+ 下载源码boost_v_tar.gz，解压
+ 进入文件夹， 执行./bootstrap检查安装环境，缺啥补啥
+ 将生成的b2拷贝到/usr/bin下
+ 配置BOOST_ROOT全局变量



#### 基本概念

1. 概况：Boost.Build 是一个高级构建系统
2. Jam变量可以是字符串，赋值可以直接一对多建立数组

```Jam
X = a b 1 "2 3"
```

3. 取值方式 $(X[2]) = b , 索引从1开始

4. ```Jam
   # 一个通用的Jamroot
   
   import os ;
   import path ;
   
   path-constant CWD : . ;
   
   local BOOST_ROOT = [ os.environ BOOST_ROOT ] ;
   ECHO "Boost lib path is: " $(BOOST_ROOT) ;
   
   use-project /boost : $(BOOST_ROOT) ;
   
   alias dependencies
   	: /boost/system//boost_system
   	  /boost/date_time//boost_date_time
   	;
   	
   for cpp_path in [ path.glob-tree $(CWD) : *.cpp : io_engine ]
   {
   	local cpp_file = [ path.basename $(cpp_path) ] ;
   	local case_name = $(cpp_file:B) ;
   	
   	ECHO "cpp_file: " $(cpp_file) ;
   	ECHO "case_name: " $(case_name) ;
   	
   	exe $(case_name)
   		: $(cpp_file)
   		  dependencies
   		: <dependency>/boost//headers
   		  <threading>multi
   		  <cxxflags>-std=c++11
   		  <cxxflags>-ggdb3
   		  <toolset>gcc
   		;
   }
   ```

5. 内嵌规则：

   + exe	Creates an executable file
   + lib      Creates an library file
   + intall  Installs built targets and other files. 
   + alias   Creates an alias for other targets
   + unit-test    Creates an executable that will be automatically run
   + obj    Creates an object file 
   + preprocessed    Creates an preprocessed source file
   + glob    The glob rule takes a list shell pattern and returns the list of files in the project's source directory that match the pattern.
   + glob-tree    The glob-tree is similar to the glob except that it operates recursively from the directory of the containing              Jamfile 
   + project    
   + use-project    Assigns a symbolic project ID to a project at a given path
   + explicit    The explicit rule takes a single parameter------------a list of target name
   + always    ????????
   + constant    Sets project-wide constant. Takes two parameters: variable name and a value and makes the specified variable name accessible in this Jamfile and any child Jamfiles.
   + constant-path    Same as the constant except that the value is treated as path relative to Jamfile location. For example, if b2 is invoked in the current directory , and Jamfile in helper subdirectory has:

   

   path-constant DATA : data/a.txt ;

   ​	

   then the variable DATA will be set to helper/data/a.txt, and if b2 is invoked from the helper directory, then the variable DATA will be set to data/a.txt

   

   + build-project    Cause some other project to be built.
   + test-suite This rule is deprecated and equivalent to alias

6. project 比较特殊，通过名字标识参数，其他规则使用顺序标识参数

7. action 动作一般用于包含多条shell的系统命令





#### 项目层级及依赖调用关系

+ 项目从其父项继承所有属性（不含alias）
+ 当在跟Jamroot的Project中有提及的参数，同样会传递到子项目中
+ 当在根目录中没有指定构建子目录，构建项目时并不会自动构建子项目，可以通过build-project来进行构建



#### 构建规则

+ Jamroot的主要作用就是将类似的配置放在Jamroot中方便子项目共同使用
+ b2寻找Jamroot和Jamfile的规则

b2不会在整个文件系统寻找Jamroot、Jamfile，如果当前工作目录没有找到会报错，什么都不做

b2在当前工作路径找到Jamfile， 就会向父目录寻找Jamroot， 直到找到位置，如果找不到Jamroot文件，则会报错；

当前工作目录有Jamroot就不需要别的Jamfile

