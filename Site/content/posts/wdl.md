---
title: 'wdl'
author: "dingqi"
date: '2021-08-03'
slug: ''
tags: shell
categories: linux
---

### 1. 什么是WDL？

WDL，全称Workflow Description Language，是由Broad Institute开发的一门流程语言，可以用来定义复杂的分析任务，并可以将各个任务串起来成流程，并且支持并行。

### 2. WDL的主要架构？

WDL脚本核心结构主要有5个基础的成分（components）组成，分别是：workflow、task、call、command、out。还要一些可选的成分：runtime参数，用于定以docker镜像，cpu，内存，投递队列，执行次数；meta参数，用于定义作者，邮件等；parameter_meta参数，用于描述输入与输出的相关信息。

### 3. WDL脚本举例

```
workflow myWorkflow {
        call myTask
}

task  myTask {
        command {
        echo  "hello world"
        }
        output {
              String out = read_string( stdout( ) )
        }
}
```

### 4.执行WDL需要什么？

环境配置

- Unix-based的操作系统；
- [Java 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)的运行环境，可以通过命令java -version查看版本
- A sense of adventure!

下载Cromwel

WDL脚本需要[Cromwell](https://github.com/broadinstitute/cromwell/releases/tag/66)(Cromwell is a Workflow Management System geared towards scientific workflows)来执行

### 5. 如何运行WDL?

将上述代码保存为myWorkflow.wdl，运行如下命令：

```
java -jar cromwell-XY.jar run myWorkflow.wdl
```

Cromwell 运行完毕后会打印出：

```
{
    "myWorkflow.myTask.out": "hello world"
}
```

### 6. WDL如何自定义输入？

下面这个例子定义变量name，用户可以自定义name的值，得到不同的输出结果

```
workflow myWorkflow {
        String  name
        call myTask  {
        input:
        name =  name
        }
}

task  myTask {
        String name
        command {
        echo  "hello world ${name}"
        }
        output {
              String out = read_string( stdout( ) )
        }
}
```

首先需要编写json格式的输入文件，内容模板如下（可由工具生成，见下个模块），"String"为自定义：

```
{
  "myWorkflow.name": "String"
}
```

通过 -i 参数指定输入的json文件，命令如下：

```
java -jar  cromwell-XY.jar run  myWorkflow.wdl -i myWorkflow_inputs.json
java -jar Cromwell-XY.jar run myWorkflow.wdl --inputs myWorkflow_inputs.json
```

如定义name为 "Beijing"，输出如下：

```
 {
    "myWorkflow.myTask.out": "hello world Beijing"
  }
```

### 7. WDL相关的辅助软件

[womtool](https://github.com/broadinstitute/cromwell/releases/tag/66)，全称Workflow Object Model，该工具主要有三个方面的作用：

验证WDL的语法有无错误，无错误会打印Success!

```
java -jar  womtool-XY.jar  validate   yourfile.wdl
```

输出WDL相应的json文件模板

```
java -jar  womtool-XY.jar   inputs  yourfile.wdl  >  yourfile.json
```

输出WDL对应的流程图

```
java -jar womtool-XY.jar graph yourfile.wdl | dot -Tsvg > pipeline.svg
```

### 8. WDL有哪些变量类型？

Primitive Types:

- String：A series of alphanumeric characters; typically used to store (short) text, filenames or, in genomics, information such as DNA sequences
- Int：An integer number; for example 16 (can be negative too)
- Float：A decimal number; for example 3.1459 (can be negative too)
- File：An object that represents a file, which is a bit different from just the filename itself
- Boolean： A logical element that represents binary values; for example true or false

File 与 String 的区别是什么？

Compound Types:

- Array：数组， [A,B,C,D]
- Map：字典， {"color": "blue", "size": "large"}
- Object：不常用
- Pair：不常用