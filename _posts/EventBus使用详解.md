---
title: EventBus使用详解
date: 2018-10-06 16:07:40
categories:
- Android
tags:
- Android
---

## 一、概述

### 1.四种订阅函数
1、onEvent
```
onEvent:如果使用onEvent作为订阅函数，那么该事件在哪个线程发布出来的，onEvent
就会在这个线程中运行，也就是说发布事件和接收事件线程在同一个线程。使用这个方
法时，在onEvent方法中不能执行耗时操作，如果执行耗时操作容易导致事件分发延迟
```
2、onEventMainThread
```
onEventMainThread:如果使用onEventMainThread作为订阅函数，那么不论事件是在哪
个线程中发布出来的，onEventMainThread都会在UI线程中执行，接收事件就会在UI线
程中运行，这个在Android中是非常有用的，因为在Android中只能在UI线程中跟新UI，
所以在onEvnetMainThread方法中是不能执行耗时操作的。
```
<!-- more -->
3、onEventBackgroundThread
```
onEventBackground:如果使用onEventBackgrond作为订阅函数，那么如果事件是在UI线
程中发布出来的，那么onEventBackground就会在子线程中运行，如果事件本来就是子 
线程中发布出来的，那么onEventBackground函数直接在该子线程中执行。
```
4、onEventAsync
```
onEventAsync：使用这个函数作为订阅函数，那么无论事件在哪个线程发布，都会创建
新的子线程在执行onEventAsync.
```

