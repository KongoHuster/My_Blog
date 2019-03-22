---
title: Android studio安装与学习
date: 2018-10-06 16:07:40
categories:
- Android
tags:
- Android
---

## android studio安装与学习

### 一、环境安装踩坑

#### 1.windows下代理配置

- Manual proxy configuration

SOCKS

```
Host Name:127.0.0.1

Poet Number:1080
```

#### 2.mac下代理配置

- Manual proxy configuration

HTTP

```
Host Name:127.0.0.1

Poet Number:1087
```
<!-- more -->
### 二、Android Studio目录结构

#### 1.新建工程项目后AS的Product目录结构如下所示：

- .idea：//AS生成的工程配置文件，类似Eclipse的project.properties。
- app：//AS创建工程中的一个Module。
- gradle：//构建工具系统的jar和wrapper等，jar告诉了AS如何与系统安装的gradle构建联系。
- External Libraries：//不是一个文件夹，只是依赖lib文件，如SDK等。

#### 2.新建工程项目后AS的Module目录结构如下所示：

- build：//构建目录，相当于Eclipse中默认Java工程的bin目录，鼠标放在上面右键Show in Exploer即可打开文件夹，编译生成的apk也在这个目录的outs子目录，不过在AS的工程里是默认不显示out目的，就算有编译结果也不显示，右键打开通过文件夹直接可以看。
- libs：//依赖包，包含jar包和jni等包。
- src：//源码，相当于eclipse的工程。
-  main：//主文件夹 
-
        - java：//Java代码，包含工程和新建是默认产生的Test工程源码。
        - res：//资源文件，类似Eclipse。
            - layout：//App布局及界面元素配置，雷同Eclipse。
            
            - menu：//App菜单配置，雷同Eclipse。 
            - 
            - values：//雷同Eclipse。
                - dimens.xml：//定义css的配置文件。 
                - strings.xml：//定义字符串的配置文件。 
                - styles.xml：//定义style的配置文件。
                ......：//arrays等其他文件。
            ......：//assets等目录

        - AndroidManifest.xml：//App基本信息（Android管理文件） 
        - 
        - ic_launcher-web.png：//App图标 
- build.gradle：//Module的Gradle构建脚本


## 三、Android系统特性与平台架构

### 1.平台架构图：
![](http://www.runoob.com/wp-content/uploads/2015/06/16510882.jpg)

### 2.架构的简单理解：
- Application(应用程序层) 我们一般说的应用层的开发就是在这个层次上进行的，当然包括了系统内置的一组应用程序，使用的是Java语言
- Application Framework(应用程序框架层) 无论系统内置或者我们自己编写的App，都需要使用到这层，比如我们想弄来电黑名单，自动挂断电话，我们就需要用到电话管理(TelephonyManager) 通过该层我们就可以很轻松的实现挂断操作，而不需要关心底层实现

- Libraries(库) + Android Runtime(Android运行时) Android给我们提供了一组C/C++库，为平台的不同组件所使用，比如媒体框架；而Android Runtime则由Android核心库集 + Dalvik虚拟机构成，Dalvik虚拟机是针对移动设备的虚拟机，它的特点:不需要很快的CPU计算速度和大量的内存空间;而每个App都单独地运行在单独的Dalvik虚拟机内每个app对于一条Dalvik进程）而他的简单运行流程如： 
![](http://www.runoob.com/wp-content/uploads/2015/06/12352621.jpg)
- Linux内核 这里就是涉及底层驱动的东西了，一些系统服务，比如安全性，内存管理以及进程管理等
