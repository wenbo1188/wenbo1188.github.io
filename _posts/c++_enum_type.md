---
layout: post
title: C++枚举类型笔记
date: 2017-07-10
categories: C/C++
---

学习FreeNOS的过程中看到了系统中定义类型为enum的情况，做下学习记录。  
源代码如下：  
```C++  
typedef enum Base{  
    Dec,  
    Hex,  
}  
Base;  
```  

enum类型定义：  
enum <枚举类型名> {<枚举表>};  
enum {<枚举表>} <变量名表>;  

源代码中使用的就是第一种格式，结果等价于Dec = 0, Hex = 1.  
True和False相当于多数系统中预定义的枚举.  