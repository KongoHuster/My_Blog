---
title: C语言作业
date: 2018-10-06 16:07:40
categories:
- 讲座培训
tags:
- Dian
---


### 1.采用多文件编译方式运行main
指令输入
```
patch -p0 main.c  v1.0.patch  // 给main.c文件打上补丁
gcc -c add_minus.c  
gcc main.c add_minus.o
```
运行
```
>>>./a.out
```
<!-- more -->
结果
```
Hello DianGroup!
3 + 2 = 5
3 - 2 = 1
```
### 2.静态库：
指令输入
```
gcc -c add_minus.c
ar rcs libstatic.a add_minus.o //将 add_minus.o 打包到静态库
gcc -o main main.c -L. -lstatic

```
运行
```
>>>./main
```
结果
```
Hello DianGroup!
3 + 2 = 5
3 - 2 = 1

```
### 3.采用动态链接的方式运行main：
指令输入
```
patch -p0 -R main.c  v1.0.patch
patch -p0 main.c  v2.0.patch
gcc -c multi_div.c
gcc -shared -fPIC -o libdynamic.so multi_div.o
gcc -o main1 main.c -L. -ldynamic
sudo mv libdynamic.so  /usr/lib/

```
运行
```
>>>./main1
```
结果
```
Hello DianGroup!
3 * 2 = 6
6 / 2 = 3

```
### 4.混合编译：
指令输入
```
patch -p0 -R main.c  v2.0.patch
patch -p0  main.c  v3.0.patch
gcc -o main2 main.c -L. -ldynamic -lstatic

```
运行
```
>>>./main2
```
结果
```
Hello DianGroup!
3 + 2 = 5
3 - 2 = 1
3 * 2 = 6
6 / 2 = 3

```
### 5.意见或建议
```
可以讲下如何阅读复杂的C语言源代码
```
