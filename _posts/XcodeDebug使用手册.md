---
title: Xcode Debug使用手册
date: 2018-10-06 16:07:40
categories:
- 深信服
tags:
- Xcode调试
---
## Xcode Debug使用手册

### 一、LLDB基础知识
#### 1.LLDB控制台
(1).Xcode中内嵌了LLDB控制台，在Xcode中代码的下方，我们可以看到LLDB控制台。
![](http://upload-images.jianshu.io/upload_images/1122433-97b0619a169dcb50.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

(2).LLDB控制台平时会输出一些log信息。如果我们想输入命令调试，必须让程序进入暂停状态。让程序进入暂停状态的方式主要有2种：
<!-- more -->
- 断点或者watchpoint: 在代码中设置一个断点（watchpoint），当程序运行到断点位置的时候，会进入stop状态
- 直接暂停，控制台上方有一个暂停按钮，上图红框已标出，点击即可暂停程序

#### 2.LLDB语法
- command: breakpoint 表示断点命令
- action: set 表示设置断点
- option: -n 表示根据方法name设置断点
- arguement: mian 表示方法名为mian

#### 3.LLDB命令

