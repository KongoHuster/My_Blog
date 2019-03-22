---
title: C++入门笔记
date: 2018-10-06 16:07:40
categories:
- C++
tags:
- C++
---

[TOC]
### 1.C++ 中的类型限定符
```
1. const：const 类型的对象在程序执行期间不能被修改改变；
2. volatile：修饰符 volatile 告诉编译器，变量的值可能以程序未明确指定的方式被改变；
3. restrict：由 restrict 修饰的指针是唯一一种访问它所指向的对象的方式。只有 C99 增加了新的类型限定符 restrict。
```
<!-- more -->

### 2.句柄的理解
（1）句柄与指针的区别
```
指针：指针指向系统中物理内存的地址；
句柄：句柄是windows在内存中维护的一个对象内存物理地址列表的整数索引，句柄是一种指向指针的指针。

```
（2）句柄的其他知识
```
- 由于windows是一种以虚拟内存为基础的操作系统，其内存管理器经常会在内存中来回的移动对象，以此来满足各种应用程序对内存的需求。
- 对象的移动意味着对象内存地址的变化，正是因为如此，如果直接使用指针，在内存地址被改变后，系统将不知道到哪里去再调用这个对象。
- windows系统为论文解决这个问题，系统专门为各种应用程序腾出了一定的内存地址（句柄）专门用来记录这些变化的地址
- windows内存管理器在移动某些对象之后，他会将这些对象新的内存地址传给句柄，告诉他移动后对象去了哪里。
```

### 3.virtual关键字
- 虚函数是指一个类中你希望重载的成员函数，当你用一个基类指针或引用指向一个继承类对象的时候，你调用一个虚函数，实际调用的是继承类的版本
- 虚函数的调用取决于指向或者引用的对象的类型，而不是指针或者引用自身的类型。
-  虚函数使得我们可以创建一个统一的基类指针列表，并且调用不同子类的函数而无需知道子类对象究竟是什么。
-  只要基类里面被声明为虚函数，那么在子类中默认都是虚的
-  静态函数不可以声明为虚函数，同时也不能被const 和 volatile关键字修饰。
-  虚函数可以被私有化，但有一些细节需要注意。


### 4.C++零碎知识点
- In general, computer languages deal with two
concepts—data and algorithms.


### 5.namespace的作用
1.实际上就是一个由程序设计者命名的内存区域，程序设计者可以根据需要指定一些有名字的空间域，把一些全局实体分别放在各个命名空间中，从而与其他全局实体分隔开来。

2.在不同的作用域中可以定义相同名字的变量，互不于扰，系统能够区别它们；


### 6.const char *
```
char * const p: 定义一个指向字符的指针常数，即const指针，常指针，这个指针是不可以p++的。 
const char* p : 定义一个指向字符常数的指针，即常量指针，这个指针是可以p++的。 
char const* p : 等同于const char* p
```

### 7.深入浅出VA函数
1. ANSI C标准下，va的宏定义在stdarg.h中，它们有：va_list，va_start()，va_arg()，va_end()
2. 使用案例
```
//格式化到一个文件流，可用于日志文件
FILE *logfile;
int WriteLog(const char * format, ...)
{
va_list arg_ptr;
va_start(arg_ptr, format);
int nWrittenBytes = vfprintf(logfile, format, arg_ptr);
va_end(arg_ptr);
return nWrittenBytes;
}
…
// 调用时，与使用printf()没有区别。
WriteLog("%04d-%02d-%02d %02d:%02d:%02d  %s/%04d logged out.", 
nYear, nMonth, nDay, nHour, nMinute, szUserName, nUserID);
```
```
//求任意个自然数的平方和：
int SqSum(int n1, ...)
{
va_list arg_ptr;
int nSqSum = 0, n = n1;
va_start(arg_ptr, n1);
while (n > 0)
{
    nSqSum += (n * n);
    n = va_arg(arg_ptr, int);
}
va_end(arg_ptr);
return nSqSum;
}
// 调用时
int nSqSum = SqSum(7, 2, 7, 11, -1);
```
3.各大函数的使用方法

- va_list arg_ptr：定义一个指向个数可变的参数列表指针；

- va_start(arg_ptr, argN)：使参数列表指针arg_ptr指向函数参数列表中的第一个可选参数， 说明：argN是位于第一个可选参数之前的固定参数，（或者说，最后一个固定参数；…之前的一个参数），函数参数列表中参数在内存中的顺序与函数声明时的顺序是一致的。如果有一va函数的声明是void va_test(char a, char b, char c, …)，则它的固定参数依次是a,b,c，最后一个固定参数argN为c，因此就是va_start(arg_ptr, c)。
- va_arg(arg_ptr, type)：返回参数列表中指针arg_ptr所指的参数，返回类型为type，并使指针arg_ptr指向参数列表中下一个参数。
- va_copy(dest, src)：dest，src的类型都是va_list，va_copy()用于复制参数列表指针，将dest初始化为src。
- va_end(arg_ptr)：清空参数列表，并置参数指针arg_ptr无效。 说明：指针arg_ptr被置无效后，可以通过调用va_start()、va_copy()恢复arg_ptr。每次调用va_start() / va_copy()后，必须得有相应的va_end()与之匹配。参数指针可以在参数列表中随意地来回移动，但必须在va_start() … va_end()之内。

4.使用的头文件
```
#include <stdio.h>  
#include <stdarg.h>
```
### 8.shared_ptr的使用
1.shared_ptr是一种智能指针（smart pointer），作用有如同指针，但会记录有多少个shared_ptrs共同指向一个对象。这便是所谓的引用计数（reference counting）。
```
  引用计数指的是，所有管理同一个裸指针（rawpointer）的shared_ptr，都共享一个引用计器
，每当一个shared_ptr被赋值（或拷贝构造）给其它shared_ptr时，这个共享的引用计数器就加
1，当一个shared_ptr析构或者被用于管理其它裸指针时，这个引用计数器就减1，如果此时发现
引用计数器为0，那么说明它是管理这个指针的最后一个shared_ptr了，于是我们释放指针指向
的资源。
```

2.一旦最后一个这样的指针被销毁，也就是一旦某个对象的引用计数变为0，这个对象会被自动删除。这在非环形数据结构中防止资源泄露很有帮助。


### 9.实践过程中遇到的问题
1.不能将局域内的指针传回
```
//代码
char* getStringLength(const char* format,...)
    {   
        char p[kLogMaxLength];
        va_list arg;  
        va_start(arg, format); 
        vsnprintf(p,kLogMaxLength,format,arg);
        va_end(arg);

        
        return p;
    } 
```
```
报错： warning: address of stack memory associated with local
      variable 'p' returned [-Wreturn-stack-address]
```
```
//修正
std::string parseParamater(const char* format,...)
    {   
        char p[kLogMaxLength];
        std::string q;
        va_list arg;  
        va_start(arg, format); 
        vsnprintf(p,kLogMaxLength,format,arg);
        va_end(arg);

        q = std::string(p);
        return q;
```

2.typedef与宏定义易错点

- 宏定义后面不要加";"
- 新名称在前面

### 10.内联函数
1.在C语言中，我们使用宏定义函数这种借助编译器的优化技术来减少程序的执行时间，而在C++中内联函数作为编译器优化手段的一种技术，在降低运行时间上非常有用；

2.内联函数是C++的增强特性之一，用来降低程序的运行时间。当内联函数收到编译器的指示时，即可发生内联：编译器将使用函数的定义体来替代函数调用语句，这种替代行为发生在编译阶段而非程序运行阶段。
3.定义函数时，在函数的最前面以关键字“inline”声明函数，即可使函数称为内联声明函数；

4.优点
```
1.它通过避免函数调用所带来的开销来提高你程序的运行速度。
2.当函数调用发生时，它节省了变量弹栈、压栈的开销。
3.它避免了一个函数执行完返回原现场的开销。
4.通过将函数声明为内联，你可以把函数定义放在头文件内。
```
5.缺点
```
1.因为代码的扩展，内联函数增大了可执行程序的体积。
2.C++内联函数的展开是中编译阶段，这就意味着如果你的内联函数发生了改动，那么就需要重新编译代码。
3.当你把内联函数放在头文件中时，它将会使你的头文件信息变多，不过头文件的使用者不用在意这些。
4.有时候内联函数并不受到青睐，比如在嵌入式系统中，嵌入式系统的存储约束可能不允许体积很大的可执行程序。
```
6.什么时候该使用内联函数
```
1.当对程序执行性能有要求时，那么就使用内联函数吧。
2.当你想宏定义一个函数时，那就果断使用内联函数吧。
3.在类内部定义的函数会默认声明为inline函数，这有利于 类实现细节的隐藏。
```

### 11 C++的单例
1.单例模式也称为单件模式、单子模式，可能是使用最广泛的设计模式；

2.其意图是保证一个类仅有一个实例，并提供一个访问它的全局访问点，该实例被所有程序模块共享；

3.有很多地方需要这样的功能模块，如系统的日志输出，GUI应用必须是单鼠标，MODEM的联接需要一条且只需要一条电话线，操作系统只能有一个窗口管理器，一台PC连一个键盘；

4.单例类CSingleton有以下特征：

- 它有一个指向唯一实例的静态指针m_pInstance，并且是私有的；
- 它有一个公有的函数，可以获取这个唯一的实例，并且在需要的时候创建该实例；
- 它的构造函数是私有的，这样就不能从别处创建该类的实例；

### 12 c/c++ 函数类型和函数指针类型
1.在C语言中，函数也是一种类型，可以定义指向函数的指针。我们知道，指针变量的内存单元存放一个地址值，而函数指针存放的就是函数的入口地址.

2.c语言函数指针的定义形式：返回类型 (*函数指针名称)(参数类型,参数类型,参数类型，…);

&emsp;c++函数指针的定义形式：返回类型（类名称::*函数成员名称）（参数类型，参数类型，参数类型，….);   

3.函数作为实参使用时，会自动的转换成函数指针；

4.当把函数名作为一个值使用时，该函数自动的转换成指针.

### 13 char类型与string类型的区别
1. char是字符类型

2. string是字符串类型

虽然一字之差，但其本质是很大的。
```
1. char属于基础类型（C++)，在C#中它属于值类型（Value Type)。char类型的长度是固定的，上一篇讲到，在C++中它可能是1个字节
，或者2个字节（取决于是否为Unicode Char），而在C#中，它永远是2个字节。

2. string是一个模板类型，也就是一个class（C++)。在C#中它属于引用类型（Reference Type)。string的长度是无法明确取得的。
也就是无法通过sizeof来取得，因为它不是一个基础类型，它本身并不固定长度，而取决于内部包含的 字符。

```

### 14 引用（reference）与 指针（pointer）的区别与联系
#### 1.什么是引用？

引用（reference）： 引用只是别名，不是实体类型（也就是说c++编译器不为引用单独分配内存空间），对一个对象的引用，就是直接对这个对象的操作。
```
int a = 3; //定义了一个整形变量a，并且赋初值3 
int & ra = a;//定义了一个引用 ra ,ra与变量占有同一块内存空间 
a = 4; //此时 a 与ra 的值都为 4； 
ra = 5;//此时 a 与 ra的值都是5；
```

#### 2.怎样使用引用？ 

（1）引用必须初始化(引用必须指向所引用的对象)
```
int a = 3;
int& ra = a;
int &b ;//错误，引用必须初始化
const int &b = 10;//正确对字面值常量10的引用
```
（2）引用不能为空
```
int &b ;//错误，引用不能为空必须有所引用的对象
```
(3)引用不能更换目标
```
#include<iostream>
using namespace std;
int main(void){
    int a = 3;
    int b = 4;
    int& ra = a;
    // int& ra = b;//错误，多次初始化

    return 0;
}
```
#### 3.引用与指针的联系与区别
&emsp;&emsp;在c++底层中，引用是通过指针实现的。也就是说，在实现层面上，引用就是指针，但是从c++的程序语言层面上来说，引用不是实体类型（不为引用单独分配内存空间）因此，引用与指针之间的区别主要体现在以下几个方面： 

（1）存在空指针，但是不存在空引用
```
void * a;//空指针，合法
//void& b;//空引用，不合法
```
（2）虽然c++编译器会警告，但是指针可以不初始化，而引用必须初始化，并且，引用的目标一旦确定，后面不能再更改，指针可以更改其指向的目标。
```
#include<iostream>
using namespace std;

int main(void){
    void * a;
    //void& b;

    int x = 1;
    int y = 2;
    int z = 3 ;
    //指针c可以不初始化，可以更改其指向的目标，
    int * c;
    c = &x;
    c = &y;

    //引用必须初始化，不可以更改其指向的目标
    //int& ra ;//报错，ra 必须要指定初值
    int & ra = x;
    ra = y;//这里只是把y的值赋给 ra 也就是x 而并不是使引用的目标由 对x的引用到对y的引用


    return 0;
}
```
（3）存在指针数组 ，不存在引用数组
```
int* a[3] ={&x,&y,&z };//定义了一个有三个整形指针变量的指针数组 a ，合法
//int& a [3] ={x,y ,z};//报错，不允许使用引用数组，因为引用没有内存的分配
```
#### 4.总结
在c++底层中，引用是通过指针实现的，所以，在实现层面上来说，引用就是指针，但是在c++语法上来说，c++编译器并不为引用类型分配内存，所以引用不能为空，必须被初始化，一旦初始化不能更改引用对象。所有对引用的操作都是对原始对象的操作

### 15 向量初始化数组
#### 1.不带参数的构造函数初始化
```
//初始化一个size为0的vector
vector<int> abc;
```
#### 2.带参数的构造函数初始化
```
//初始化size,但每个元素值为默认值
vector<int> abc(10);    //初始化了10个默认值为0的元素
//初始化size,并且设置初始值
vector<int> cde(10，1);    //初始化了10个值为1的元素
```
#### 3.通过数组地址初始化
```
int a[5] = {1,2,3,4,5};
//通过数组a的地址初始化，注意地址是从0到5（左闭右开区间）
vector<int> b(a, a+5);
```
#### 4.通过同类型的vector初始化
```
vector<int> a(5,1);
//通过a初始化
vector<int> b(a);
```
#### 5.通过insert初始化
```
//insert初始化方式将同类型的迭代器对应的始末区间（左闭右开区间）内的值插入到vector中
vector<int> a(6,6);
vecot<int> b;
//将a[0]~a[2]插入到b中，b.size()由0变为3
b.insert(b.begin(), a.begin(), a.begin() + 3);
```


### 16 多态
#### 1.多态概述
- 多态性可以简单地概括为“一个接口，多种方法”，程序在运行时才决定调用的函数，它是面向对象编程领域的核心概念。
- 多态与非多态的实质区别就是函数地址是早绑定还是晚绑定。
```
- 如果函数的调用，在编译器编译期间就可以确定函数的调用地址，并生产代码，是静态的，就是说地址是早绑定的。

- 如果函数调用的地址不能在编译器期间确定，需要在运行时才确定，这就属于晚绑定。
```
- 多态的作用:多态的目的则是为了接口重用
- 封装可以使得代码模块化，继承可以扩展已存在的代码，他们的目的都是为了代码重用。

#### 2.多太分类
(1)多态有静态多态，也有动态多态，静态多态，比如函数重载，能够在编译器确定应该调用哪个函数；动态多态，比如继承加虚函数的方式
![](http://jbcdn2.b0.upaiyun.com/2016/11/ae91bded2fb09a4ae9b2d9d051bf7528.png)

(2) 静态多态
```
long long Add(int left, int right)  
{  
    return left + right;  
}  
  
double Add(float left, float right)  
{  
    return left + right;  
}  
  
int main()  
{  
    cout<<Add(10, 20)<<endl; //语句一  
    cout<<Add(12.34f, 43.12f)<<endl; //语句二  
  
    return 0;  
}
```
(3)动态多态
```c++
class Person  
{  
public:  
    void GoToWashRoom()  
    {  
        cout<<"Person-->?"<<endl;  
    }  
};  
  
class Man : public Person  
{  
public:  
    void GoToWashRoom()  
    {  
        cout<<"Man-->Please Left"<<endl;  
    }  
};  
  
class Woman : public Person  
{  
public:  
    void GoToWashRoom()  
    {  
        cout<<"Woman-->Please Right"<<endl;  
    }  
};  
  
int main()  
{  
    Person per, *pp;  
    Man man, *pm;  
    Woman woman, *pw;  
  
    pp = &per;  
    pm = &man;  
    pw = &woman;  
      
    //第一组       //这些都是毫无疑问的  
    per.GoToWashRoom(); //调用基类Person类的函数  
    pp->GoToWashRoom();  //调用基类Person类的函数  
    man.GoToWashRoom(); //调用派生类Man类的函数   
    pm->GoToWashRoom();  //调用派生类Man类的函数   
    woman.GoToWashRoom();   //调用派生类Woman类的函数   
    pw->GoToWashRoom();  //调用派生类Woman类的函数   
      
    //第二组  
    pp = &man;  
    pp->GoToWashRoom();  //调用基类Person类的函数  
    pp = &woman;  
    pp->GoToWashRoom();  //调用基类Person类的函数  
      
    return 0;  
}
```

#### 3.多态小结
- 基类中定义了虚函数，在派生类中该函数始终保持虚函数的特性
- 只有类的成员函数才能定义为虚函数，静态成员函数不能定义为虚函数
- 如果在类外定义虚函数，只能在声明函数时加virtual关键字，定义时不用加
- 构造函数不能定义为虚函数，虽然可以将operator=定义为虚函数，但最好不要这么做，使用时容 易混淆
- 最好将基类的析构函数声明为虚函数。(析构函数比较特殊，因为派生类的析构函数跟基类的析构 函数名称不一样，但是构成覆盖，这里编译器做了特殊处理)

### 十七 map用法详解
#### 1.map简介
map是一类关联式容器。它的特点是增加和删除节点对迭代器的影响很小，除了那个操作节点，对其他的节点都没有什么影响。对于迭代器来说，可以修改实值，而不能修改key。

#### 2.map的功能
- 自动建立Key － value的对应。key 和 value可以是任意你需要的类型。
- 根据key值快速查找记录，查找的复杂度基本是Log(N)，如果有1000个记录，最多查找10次，1,000,000个记录，最多查找20次。

- 快速插入Key -Value 记录。

#### 3.数据的插入
```
#include <map>  
  
#include <string>  
  
#include <iostream>  
  
using namespace std;  
  
int main()  
  
{  
  
    map<int, string> mapStudent;  
  
    mapStudent.insert(map<int, string>::value_type (1, "student_one"));  
  
    mapStudent.insert(map<int, string>::value_type (2, "student_two"));  
  
    mapStudent.insert(map<int, string>::value_type (3, "student_three"));  
  
    map<int, string>::iterator iter;  
  
    for(iter = mapStudent.begin(); iter != mapStudent.end(); iter++)  
  
       cout<<iter->first<<' '<<iter->second<<endl;  
  
}

```

#### 4.数据的查找
- 第一种：用count函数来判定关键字是否出现，其缺点是无法定位数据出现位置,由于map的特性，一对一的映射关系，就决定了count函数的返回值只有两个，要么是0，要么是1，出现的情况，当然是返回1了

 
- 第二种：用find函数来定位数据出现位置，它返回的一个迭代器，当数据出现时，它返回数据所在位置的迭代器，如果map中没有要查找的数据，它返回的迭代器等于end函数返回的迭代器。

```
#include <map>  
  
#include <string>  
  
#include <iostream>  
  
using namespace std;  
  
int main()  
  
{  
  
    map<int, string> mapStudent;  
  
    mapStudent.insert(pair<int, string>(1, "student_one"));  
  
    mapStudent.insert(pair<int, string>(2, "student_two"));  
  
    mapStudent.insert(pair<int, string>(3, "student_three"));  
  
    map<int, string>::iterator iter;  
  
    iter = mapStudent.find(1);  
  
    if(iter != mapStudent.end())  
  
       cout<<"Find, the value is "<<iter->second<<endl;  
  
    else  
  
       cout<<"Do not Find"<<endl;  
      
    return 0;  
}  

```

#### 5.数据的删除
- iterator erase（iterator it);//通过一个条目对象删除

- iterator erase（iterator first，iterator - last）//删除一个范围

- size_type erase(const Key&key);//通过关键字删除

- clear()就相当于enumMap.erase(enumMap.begin(),enumMap.end());
- 
```
#include <map>  
  
#include <string>  
  
#include <iostream>  
  
using namespace std;  
  
int main()  
  
{  
  
       map<int, string> mapStudent;  
  
       mapStudent.insert(pair<int, string>(1, "student_one"));  
  
       mapStudent.insert(pair<int, string>(2, "student_two"));  
  
       mapStudent.insert(pair<int, string>(3, "student_three"));  
  
        //如果你要演示输出效果，请选择以下的一种，你看到的效果会比较好  
  
       //如果要删除1,用迭代器删除  
  
       map<int, string>::iterator iter;  
  
       iter = mapStudent.find(1);  
  
       mapStudent.erase(iter);  
  
       //如果要删除1，用关键字删除  
  
       int n = mapStudent.erase(1);//如果删除了会返回1，否则返回0  
  
       //用迭代器，成片的删除  
  
       //一下代码把整个map清空  
  
       mapStudent.erase( mapStudent.begin(), mapStudent.end() );  
  
       //成片删除要注意的是，也是STL的特性，删除区间是一个前闭后开的集合  
  
       //自个加上遍历代码，打印输出吧  
  
}  


```
 
#### 6.map的基本操作函数

     begin()         返回指向map头部的迭代器

     clear(）        删除所有元素

     count()         返回指定元素出现的次数

     empty()         如果map为空则返回true

     end()           返回指向map末尾的迭代器

     equal_range()   返回特殊条目的迭代器对

     erase()         删除一个元素

     find()          查找一个元素

     get_allocator() 返回map的配置器

     insert()        插入元素

     key_comp()      返回比较元素key的函数

     lower_bound()   返回键值>=给定元素的第一个位置

     max_size()      返回可以容纳的最大元素个数

     rbegin()        返回一个指向map尾部的逆向迭代器

     rend()          返回一个指向map头部的逆向迭代器

     size()          返回map中元素的个数

     swap()           交换两个map

     upper_bound()    返回键值>给定元素的第一个位置

     value_comp()     返回比较元素value的函数
     

### 18 vector容器

#### 1.vector是什么
向量（Vector）是一个封装了动态大小数组的顺序容器（Sequence Container）。跟任意其它类型容器一样，它能够存放各种类型的对象。可以简单的认为，向量是一个能够存放任意类型的动态数组。

#### 2. vector函数
```
1.push_back 在数组的最后添加一个数据

2.pop_back 去掉数组的最后一个数据

3.at 得到编号位置的数据

4.begin 得到数组头的指针

5.end 得到数组的最后一个单元+1的指针

6．front 得到数组头的引用

7.back 得到数组的最后一个单元的引用

8.max_size 得到vector最大可以是多大

9.capacity 当前vector分配的大小

10.size 当前使用数据的大小

11.resize 改变当前使用数据的大小，如果它比当前使用的大，者填充默认值

12.reserve 改变当前vecotr所分配空间的大小

13.erase 删除指针指向的数据项

14.clear 清空当前的vector

15.rbegin 将vector反转后的开始指针返回(其实就是原来的end-1)

16.rend 将vector反转构的结束指针返回(其实就是原来的begin-1)

17.empty 判断vector是否为空

18.swap 与另一个vector交换数据
```

### 19 纯虚函数
#### 1.定义
纯虚函数是在基类中声明的虚函数，它在基类中没有定义，但要求任何派生类都要定义自己的实现方法。在基类中实现纯虚函数的方法是在函数原型后加“=0”
```
virtual void funtion1()=0
```
#### 2.引入原因
- 1、为了方便使用多态特性，我们常常需要在基类中定义虚拟函数。
- 2、在很多情况下，基类本身生成对象是不合情理的。例如，动物作为一个基类可以派生出老虎、孔雀等子类，但动物本身生成对象明显不合常理

#### 3.总结
- 虚函数必须实现，如果不实现，编译器将报错，错误提示为：
error LNK****: unresolved external symbol "public: virtual void __thiscall ClassName::virtualFunctionName(void)"
- 实现了纯虚函数的子类，该纯虚函数在子类中就编程了虚函数，子类的子类即孙子类可以覆盖该虚函数，由多态方式调用的时候动态绑定
- 在有动态分配堆上内存的时候，析构函数必须是虚函数，但没有必要是纯虚的。
- 析构函数应当是虚函数，将调用相应对象类型的析构函数，因此，如果指针指向的是子类对象，将调用子类的析构函数，然后自动调用基类的析构函数

### 20 抽象类的介绍
#### 1.定义
称带有纯虚函数的类为抽象类。
#### 2.抽象类的作用：
抽象类的主要作用是将有关的操作作为结果接口组织在一个继承层次结构中，由它来为派生类提供一个公共的根，派生类将具体实现在其基类中作为接口的操作。所以派生类实际上刻画了一组子类的操作接口的通用语义，这些语义也传给子类，子类可以具体实现这些语义，也可以再将这些语义传给自己的子类。
#### 3.使用抽象类时注意：
- 抽象类只能作为基类来使用，其纯虚函数的实现由派生类给出。如果派生类中没有重新定义纯虚函数，而只是继承基类的纯虚函数，则这个派生类仍然还是一个抽象类。如果派生类中给出了基类纯虚函数的实现，则该派生类就不再是抽象类了，它是一个可以建立对象的具体的类。
- 抽象类是不能定义对象的。
