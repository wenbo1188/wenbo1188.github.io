---
layout: post
title: cppcheck中文手册
date: 2017-07-23
categories: C/C++
---

## 第一章 介绍 ##
Cppcheck是一个用于C/C++代码分析的工具。与C/C++编译器和许多其他的分析工具不同的是，它并不检测语法错误，而是更多的关注于编译器多数不能检测出来的bug。它的目标是做到不误报。  

支持的代码和平台:  
+ 你可以检查包括各种编译器扩展的非标准化的代码，内联汇编代码，等等。
+ Cppcheck 可以由任意的C++编译器编译，只要编译器支持最新的C++标准。
+ Cppcheck 可以在任意具有良好CPU和内存的平台上运行。  

请记住，Cppcheck也具有一定的局限性，虽然它几乎不会误报，但是也有很多bug是它检测不出的。  
通过仔细测试你的程序，你会发现更多的bug，而不是仅仅依赖Cppcheck。尽管如此，Cppcheck仍然会在你测试程序的过程中检测出一些你遗漏下来的bug。  

## 第二章 入门 ##
### 小试牛刀 ###
这里有一段示例程序:  
```C++
int main()
{
    char a[10];
    a[10] = 0;
    return 0;
}
```
如果你将其保存到file1.c然后执行:  
cppcheck file1.c  
cppcheck将会返回:  
```
Checking file1.c...
[file1.c:4]: (error) Array 'a[10]' index 10 out of bounds
```
### 检查文件夹中的所有文件 ###
通常一个程序有许多源文件，此时你想同时检查所有的文件。Cppcheck能够通过以下的命令检查目录中的所有文件:  
cppcheck \[path\]  
如果\[path\]是一个目录，Cppcheck会迭代式地检查目录中的所有源文件。  
```
Checking path/file1.cpp...
1/2 files checked 50% done
Checking path/file2.cpp...
2/2 files checked 100% done
```
### 手动检查或者使用工程文件 ###
使用Cppcheck你可以通过设置检查的文件或路径手动检查文件，或者你也可以使用一个工程文件(cmake/visual studio)  

使用工程文件的方法更快捷，因为它不需要你做很多的配置。  

而手动检查则会让你更好地控制分析过程。  

我们不知道哪种方式的结果更好，建议大家两种都进行尝试，很有可能你会得到不同的结果，这样通过同时使用两种方式你能够找到更多的bug。  

详情见后面的章节。  
### 将文件或文件夹从检查中排除 ###
有两种方法能够将文件或文件夹从检查中排除，第一种方法是仅仅提供你想检查的路径和文件:  
```
cppcheck src/a src/b
```
所有在src/a和src/b路径下的文件都将进行检查。  

第二种方法是使用-i选项，来设置忽略的文件或路径。使用以下的命令，则src/c路径下的所有文件都不会被检查:  
```
cppcheck -isrc/c src
```
这种方法目前无法和--project选项同时工作并且仅当提供一个输入文件夹时才有效。忽略多个目录需要多次使用-i选项，以下的命令会同时忽略src/b 和src/c目录:  
```
cppcheck -isrc/b -isrc/c
```
### 严重程度 ###
message信息的严重程度分为以下几种:  
```
error           发现bug
warning         防御性编程来防止bug的建议
style           代码清理以及风格相关的问题(未被使用的函数，冗余的代码、常数等)
performance     使得代码运行速度更快的建议，这些建议仅仅基于常识而并不保证接修改后能够有巨大的提升
portability     可移植性警告。64位可移植性。代码可能在不同的编译器上结果不同等。
information     配置问题。要求仅在配置过程中启用。
```
### 启用messages ###
默认情况下仅显示error信息，通过--enable命令可以启用更多检查。  
```
# 启用warning
cppcheck --enable=warning file.c

# 启用performance
cppcheck --enable=performance file.c

# 启用information
cppcheck --enable=information file.c

# 由于历史原因，--enable=style 会同时启用warning, performance
# portability以及style. 使用老式的xml格式时这些都被称为"style"
cppcheck --enable=style file.c

# 启用warning和performance
cppcheck --enable=warning,performance file.c

# 启用未被使用函数检查，不能通过--enable=style来启用，因为它在检查库时效果不好
cppcheck --enable=unusedFunction file.c

# 启用所有的message
cppcheck --enable=all
```
请注意 --enable=unusedFunction 应仅用于整个程序检查过的情况。因此，--enable=all也应仅用于整个程序检查过的情况，因为未被使用函数检查会对未被调用的函数给出警告，如果函数调用未被检测到则会造成干扰。
