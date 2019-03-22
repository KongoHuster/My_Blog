---
title: 第十一章 AWT编程
date: 2018-10-06 16:07:40
categories:
- Java学习
tags:
- Java
---

## 第十二章 AWT编程


### 一、Swing组件

- Swing为绝大部分的界面组件都提供了实现。这些界面组件都是直接绘制空白区域上。
- Swing自己实现了这些界面组件，因此Swing无需使用各操作系统上界面组件的交集。
- 由于Swing不再需要调用操作系统上相应的界面组件，Swing的UI界面更加统一。
<!-- more -->
### 二、容器

- 容器(Container)实际上是Component的子类，因此容器类对象本身也是一个组件，具有组件的所有性质，另外还具有容纳其它组件和容器的功能
- 容器类对象可使用方法add()添加组件
- 两种主要容器类型：
```
Window：可独立存在的顶级窗口

Panel：可作为容器容纳其它组件，但不能独立存在，必须被添加到其它容器中(如Window 或 Applet)
```
- 组件定制
```
组件的大小和位置由布局管理器（LayoutManager）决定。

使用布局管理器则可以定制组件的大小和位置，但必须在容器中使用组件的setLocation(), setSize(), setBounds()方法确定大小位置
```

- Frame类
```
代表一个窗口

是Window类的子类

初始化时为不可见，可用setVisible(true)使其显示出来

使用BorderLayout作为其缺省布局管理器
```
- FlowLayout
```
GUI Component从左到右按顺序配置在Container中，若到达右边界，则会折回到下一行中

FlowLayout是Panel和Applet的默认管理器

FlowLayout()/ FlowLayout(int align)/ FlowLayout(int align,int hgap,int vgap)
```
- GridLayout
```
GridLayout将Component配置在纵横格线分割的格子中，从左到右，从上到下；

构造器：GridLayout()/ GridLayout(int rows,int cols)/ GridLayout(int rows,int cols,int hgap,int vgap) 
```

- CardLayout
```
将加入到Container中的Component看成一叠卡片，只有最上面的那个Componet才可见

构造器：CardLayout()/ CardLayout(int hgap,int vgap)
```

