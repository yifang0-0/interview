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
**#ifdef**只关注宏是否定义
**#if**除了是否定义还关注逻辑值是否为false

#ifndef
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
