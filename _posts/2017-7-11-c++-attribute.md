---
layout: post
title: C++ __attribute__
date: 2017-07-11
categories: C/C++
---  

### \_\_ATTRIBUTE\_\_ ###
GNU C 的一大特色就是\_\_attribute\_\_ 机制。\_\_attribute\_\_ 可以设置函数属性（Function Attribute ）、变量属性（Variable Attribute ）和类型属性（Type Attribute )  
\_\_attribute\_\_ 书写特征是：\_\_attribute\_\_前后都有两个下划线，并切后面会紧跟一对原括弧，括弧里面是相应的\_\_attribute\_\_参数.  
\_\_attribute\_\_语法格式为：\_\_attribute\_\_ ((attribute-list))
其位置约束为：放于声明的尾部";"之前.  

在使用\_\_attribute\_\_ 参数时，你也可以在参数的前后都加上两个下划线，例如，使用\_\_aligned\_\_而不是aligned,这样，你就可以在相应的头文件里使用它而不用关心头文件里是否有重名的宏定义.  

#### aligned ####
该属性设定一个指定大小的对齐格式（以字节 为单位），例如：  
struct S {  
short b[3];  
} \_\_attribute\_\_ ((aligned (8)));  
typedef int int32_t \_\_attribute\_\_ ((aligned (8)));  

该声明将强制编译器确保（尽它所能）变量类型为struct S 或者int32_t 的变量在分配空间时采用8 字节对齐方式。  

如上所述，你可以手动指定对齐的格式，同样，你也可以使用默认的对齐方式。如果aligned 后面不紧跟一个指定的数字值，那么编译器将依据你的目标机器情况使用最大最有益的对齐方式。例如：  
struct S {  
short b[3];  
} \_\_attribute\_\_ ((aligned));  

这里，如果sizeof(short)的大小为2(byte)，那么，S的大小就为6 。取一个2 的次方值，使得该值大于等于6,则该值为8,所以编译器将设置S类型的对齐方式为8 字节  

aligned 属性使被设置的对象占用更多的空间，相反的，使用packed 可以减小对象占用的空间  

#### packed ####
使用该属性对struct或者union类型进行定义，设定其类型的每一个变量的内存约束。当用在enum 类型 定义时，暗示了应该使用最小完整的类型（it indicates that the smallest integral type should be used）  
下面的例子中，packed_struct 类型的变量数组中的值将会紧紧的靠在一起，但内部的成员变量s 不会被“pack” ，如果希望内部的成员变量也被packed 的话，unpacked-struct 也需要使用packed 进行相应的约束  

struct unpacked_struct{  
char c;  
int i;  
};  

struct packed_struct{  
char c;  
int  i;  
struct unpacked_struct s;  
}\_\_attribute\_\_ ((\_\_packed\_\_));  

示例：  
```C++
struct p

{

int a;

char b;

short c;

}__attribute__((aligned(4))) pp;

struct m

{

char a;

int b;

short c;

}__attribute__((aligned(4))) mm;

struct o

{

int a;

char b;

short c;

}oo;

struct x

{

int a;

char b;

struct p px;

short c;

}__attribute__((aligned(8))) xx;

int main()

{

printf("sizeof(int)=%d,sizeof(short)=%d.sizeof(char)=%d\n",sizeof(int),sizeof(short),sizeof(char));

printf("pp=%d,mm=%d \n", sizeof(pp),sizeof(mm));

printf("oo=%d,xx=%d \n", sizeof(oo),sizeof(xx));

return 0;

}
```
输出结果：  
sizeof(int)=4,sizeof(short)=2.sizeof(char)=1  
pp=8,mm=12  
oo=8,xx=24  

### 更多内容详见 ###
[cnbyl123的博客](http://www.cnblogs.com/astwish/p/3460618.html)
