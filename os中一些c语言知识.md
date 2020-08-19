# os中一些c语言知识

## 宏相关
### #if与#ifdef
``` c
  #define XXX 0

  // 第一段条件编译
  #ifdef XXX
    逻辑1
  #else
    逻辑2
  #endif

  // 第二段条件编译
  #if XXX
    逻辑1
  #else
    逻辑2
  #endif
```
结果:

第一段:逻辑一编译
第二段:逻辑二编译

原因:
**\#ifdef**只关注宏是否定义
**#if**除了是否定义还关注逻辑值是否为false

\#ifndef
与#ifdef用法相反
### 换行符
- 为避免过长字符串中间出现空格,长字符串的宏定义可以采用几个子字符串用反斜杠\连接的方式
```c
#define str2 "123"\
                        "456" 
```
- 过长非字符串宏定义的时候直接在断开处添加\再换行即可
```c
#define DEBUG_PRINT printf(\
                    "File %s line %d:"\
                    "x=%d,y=%d,z=%d",\
                    __FILE__,__LINE,x,y,z)
``` 
[更多宏定义规则参考](https://blog.csdn.net/jmh1996/article/details/72832737)
## 下划线的命名规则
表示所使用的变量在系统内

## typedef
### 函数指针的概念
```c
typedef void (*PFUNA)(int a); //在语句开头加上typedef关键字，PFUNA就是我们定义的新类型
```
## 数组
### 声明时下标为0
表示数组指针,没有内容
### 声明时直接定义
### 赋值与类型问题 extern
- 数组不能作左值,作右值的时候看作首地址指针
- 在extern的时候需要指定为数组类型
``` c
//example.h
int a[100];
```
``` c
//example.c
#include "example.h"
a={0,0};
int main(){
  //.....
}
```
``` c
//example2.h
extern int a[];
//错误写法 extern int *a;
```
首先需要明确:
数组的寻址模式就是**首地址+sizeof(数组元素类型)\*index**

开始分析两种情况
-  extern int a[]  
假设``a[100]``是从0x0001开始的100*sizeof(int)字节个地址 那么``extern int a[]``编译器所做的就是把0x01这个地址取名为a,**即使得0x0001与a链接**使得该文件也能通过``a[index]``访问``0x0001+index*sizeof(int)``
-  extern int *a  
而``extern int *a``编译器所做的就是创建一个新的整型指针,假设其地址为``0x1001``,此时把0x0001这个原来文件的a数组的地址值赋值给当前文件的a指针.因此a[index]寻址模式就是``0x1001+index*sizeof(int)``从而找不到原数组.
## 内联函数 inline
[参考](https://www.runoob.com/w3cnote/cpp-inline-usage.html)
### 举例
```c
#include <stdio.h>
//函数定义为inline即:内联函数
inline char* dbtest(int a) {
    return (i % 2 > 0) ? "奇" : "偶";
} 
 
int main()
{
   int i = 0;
   for (i=1; i < 100; i++) {
       printf("i:%d    奇偶性:%s /n", i, dbtest(i));    
   }
}
```
在内部的工作就是在每个 for 循环的内部任何调用 dbtest(i) 的地方都换成了 (i%2>0)?"奇":"偶"，这样就避免了频繁调用函数对栈内存重复开辟所带来的消耗。
### 使用注意点
#### 非递归
inline 只适合函数体内代码简单的涵数使用，不能包含复杂的结构控制语句例如 while、switch，并且不能内联函数本身不能是直接递归函数（即，自己内部还调用自己的函数）。
#### 用于声明时无效
在定义的时候加入才有效
```c
//关键字 inline 必须与函数定义体放在一起才能使函数成为内联，仅将 inline 放在函数声明前面不起任何作用。

//如下风格的函数 Foo 不能成为内联函数：
inline void Foo(int x, int y); // inline 仅与函数声明放在一起
void Foo(int x, int y){}


//而如下风格的函数 Foo 则成为内联函数：
void Foo(int x, int y);
inline void Foo(int x, int y) {} // inline 与函数定义体放在一起
```
#### 避免不必要使用
- 函数代码太长会使得内存消耗较高
- 如果函数体内有循环,执行函数体内代码会长于函数调用的开销
- 编译器可能拒绝内联关键字

## 转义符
### "\x"

## 枚举类型enum
[枚举类型的赋值](https://www.jianshu.com/p/83379e53a572) 
