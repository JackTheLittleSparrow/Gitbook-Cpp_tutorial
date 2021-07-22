# Pipeline组件

#### Pipeline 介绍

pipeline,管线，实为流水线，流水线包括几个模块

+ agent 定义全局环境
+ param  为整个流程提供参数
+ stage  pipeline实际上是由多个stage组成，每个stage完成一件事情

#### adk Pipeline

ADK中提供了Pipeline库

#### 使用说明

##### 头文件

```cpp
#include <adk/pipeline.h>
```

##### Pipeline程序写法

1. 定义AppStageWorker类，继承public StageWorker<ADK_INPUT(int64_t), ADK_OUTPUT(2, int64_t)>, 重写OnMessage方法

2. main函数中，定义不同的AppStageWorker对象，定义pipeline对象， 通过Pipeline把不同的stage进行连接。
3.  创建PipelineEntrance对象， （通过Pipeline把不同的stage进行连接和创建PipelineEntrance顺序无要求）
4. 启动pipeline
5. 通过PipelineEntrance对象输入消息
6. 停止Pipeline

##### Pipeline API

`pipeline`库中有三个核心类：

+ Pipeline
+ PipelineEntrance
+ StageWorker

##### PipelineEntrance API

##### StageWorker API
