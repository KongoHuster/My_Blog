---
title: Objective-C 入门教程
date: 2018-10-06 16:07:40
categories:
- 深信服
tags:
- Objective-C
---

### 一、Objective-C：C的超集
&emsp;&emsp;Objective-Objective-C是C语言的严格超集－－任何C语言程序不经修改就可以直接通过Objective-C编译器，在Objective-C中使用C语言代码也是完全合法的。Objective-C被描述为盖在C语言上的薄薄一层，因为Objective-C的原意就是在C语言主体上加入面向对象的特性。
<!-- more -->
### 二、Objective-C代码的文件扩展名
- .h头文件:头文件包含类，类型，函数和常数的声明。
- .m源代码文件:这是典型的源代码文件扩展名，可以包含 Objective-C 和 C 代码。
- .mm源代码文件:带有这种扩展名的源代码文件，除了可以包含Objective-C和C代码以外还可以包含C+ +代码。仅在你的Objective-C代码中确实需要使用C++类或者特性的时候才用这种扩展名。

### 三、类
#### 1.类的基础知识
- Objective-C 的类规格说明包含了两个部分：定义（interface）与实现（implementation）。定义（interface）部分包含了类声明和实例变量的定义，以及类相关的方法。
- 类的定义文件遵循C语言之惯例以.h为后缀，实现文件以.m为后缀。

#### 2.类声明图
![](http://www.runoob.com/wp-content/uploads/2015/08/2011021920525849.jpg)

#### 3.Interface
&emsp;&emsp;定义部分，清楚定义了类的名称、数据成员和方法。 以关键字@interface作为开始，@end作为结束。

```
@interface MyObject : NSObject {
    int memberVar1; // 实体变量
    id  memberVar2;
}

+(return_type) class_method; // 类方法

-(return_type) instance_method1; // 实例方法
-(return_type) instance_method2: (int) p1;
-(return_type) instance_method3: (int) p1 andPar: (int) p2;
@end
```
#### 4.Implementation
&emsp;&emsp;实现区块则包含了公开方法的实现，以及定义私有（private）变量及方法。 以关键字@implementation作为区块起头，@end结尾。

```
@implementation MyObject {
  int memberVar3; //私有實體變數
}

+(return_type) class_method {
    .... //method implementation
}
-(return_type) instance_method1 {
     ....
}
-(return_type) instance_method2: (int) p1 {
    ....
}
-(return_type) instance_method3: (int) p1 andPar: (int) p2 {
    ....
}
@end
```
&emsp;&emsp;值得一提的是不只Interface区块可定义实体变量，Implementation区块也可以定义实体变量，两者的差别在于访问权限的不同，Interface区块内的实体变量默认权限为protected，宣告于implementation区块的实体变量则默认为private。



