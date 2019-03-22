---
title: Java虚拟机 理解结构化程序设计
date: 2018-10-06 16:07:40
categories:
- Java学习
tags:
- Java 虚拟机
---
## 一、 什么是Java虚拟机

### 1.虚拟机
虚拟机是一种抽象化的计算机，通过在实际的计算机上仿真模拟各种计算机功能来实现的。Java虚拟机有自己完善的硬体架构，如处理器、堆栈、寄存器等，还具有相应的指令系统。JVM屏蔽了与具体操作系统平台相关的信息，使得Java程序只需生成在Java虚拟机上运行的目标代码（字节码），就可以在多种平台上不加修改地运行。
<!-- more -->
### 2.Java虚拟机
Java虚拟机（英语：Java Virtual Machine，缩写为JVM），一种能够运行Java bytecode的虚拟机，以堆栈结构机器来进行实做。最早由太阳微系统所研发并实现第一个实现版本，是Java平台的一部分，能够运行以Java语言写作的软件程序。

## 二、Java 代码编译和执行的整个过程

### 1.Java 代码编译是由 Java 源码编译器来完成，流程图如下所示：

![](http://wiki.jikexueyuan.com/project/java-vm/images/javadebug.gif)

![](http://wiki.jikexueyuan.com/project/java-vm/images/jvmdebug.gif)

### 2.Java 源码编译机制

Java 源码编译由以下三个过程组成：

- 分析和输入到符号表
- 注解处理
- 语义分析和生成 class 文件

### 3.类执行机制

JVM 是基于栈的体系结构来执行 class 字节码的。线程创建后，都会产生程序计数器（PC）和栈（Stack），程序计数器存放下一条要执行的指令在方法内的偏移量，栈中存放一个个栈帧，每个栈帧对应着每个方法的每次调用，而栈帧又是有局部变量区和操作数栈两部分组成，局部变量区用于存放方法中的局部变量和参数，操作数栈中用于存放方法执行过程中产生的中间结果。栈的结构如下图所示：

![](http://wiki.jikexueyuan.com/project/java-vm/images/classrun.gif)
