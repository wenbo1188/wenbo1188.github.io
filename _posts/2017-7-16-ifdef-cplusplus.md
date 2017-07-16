---
layout: post
title: #ifdef __cplusplus
date: 2017-07-16
categories: C/C++
---

### #ifdef __cplusplus 使用的目的 ###
由于C++和C编译器有一定的不同，因此在C++与C语言混合调用的情况下，需要通过一种声明的方式来告知编译器使用哪种方式来进行编译。  

C++支持重载而C不支持，因此函数在编译过程中在符号库中的名称两者不同。例如，假设某个函数的原型为：voidfoo( int x, int y );该函数被C编译器编译后在符号库中的名字为\_foo，而C++编译器则会产生像\_foo\_int\_int之类的名字（不同的编译器可能生成的名字不同，但是都采用了相同的机制，生成的新名字称为“mangled name”）。\_foo\_int\_int这样的名字包含了函数名、函数参数数量及类型信息，C++就是靠这种机制来实现函数重载的。例如，在C++中，函数void foo( int x, int y )与void foo( int x, float y )编译生成的符号是不相同的，后者为\_foo\_int\_float。

### #ifdef __cplusplus 常用格式 ###
```C++
#ifdef __cplusplus
extern "C"{
endif
//some code
#ifdef __cplusplus
}
endif
```
首先，__cplusplus是C++里自定义的宏，上述代码的含义是：如果这是一段cpp代码，那么加入extern "C" {}来处理其中的代码。  

加入extern "C" {}与否有什么区别呢？  
看两个编译的实例就比较好理解了：  
对于同一段代码：  
```C++
int f(void){
	return 1;
}
```
在加入extern "C" {}的时候产生的汇编代码为：  
```
file "test.cxx" 
.text 
.align 2 
.globl _f 
.def _f; .scl 2; .type 32; .endef 
_f: 
pushl %ebp 
movl %esp， %ebp 
movl $1， %eax 
popl %ebp 
ret
```
在不加时产生的汇编代码为：  
```
.file "test.cxx" 
.text 
.align 2 
.globl __Z1fv 
.def __Z1fv; .scl 2; .type 32; .endef 
__Z1fv: 
pushl %ebp 
movl %esp， %ebp 
movl $1， %eax 
popl %ebp 
ret
```
产生的函数名，一个叫_f, 一个叫_Z1fv。 因此为了C++能够尽可能地对C兼容，并且能够使用C写好地库，必要时候需要加上extern "C" {}来加以声明。