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
这种方法目前无法和 __\-\-project__ 选项同时工作并且仅当提供一个输入文件夹时才有效。忽略多个目录需要多次使用-i选项，以下的命令会同时忽略src/b 和src/c目录:  
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
默认情况下仅显示error信息，通过 __\-\-enable__ 命令可以启用更多检查。  
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
请注意 __\-\-enable=unusedFunction__ 应仅用于整个程序检查过的情况。因此 __\-\-enable=all__ 也应仅用于整个程序检查过的情况，因为未被使用函数检查会对未被调用的函数给出警告，如果函数调用未被检测到则会造成干扰。

### 将结果保存到文件 ###
很多时候你希望将结果保存到文件里，你可以使用shell命令将错误信息重定向到指定文件。
```
cppcheck file1.c 2> err.txt
```

### 多线程检查 ###
参数-j用来确定你想要的线程数，比如说，你想用4个线程来检查文件：
```
cppcheck -j 4 path
```
请注意这会取消掉未调用函数检查。

### 平台 ###
你必须使用匹配你的目标的平台设置。  
默认情况下，如果你的代码是在本地编译和执行的话，Cppcheck会使用本地的平台设置。  
Cppcheck有着内置的针对unix和windows平台的设置。你可以方便地通过 __\-\-platform__ 命令行参数表示来使用这些。  
你也可以在xml文件中创建你自己的平台设置，例子如下：  
```
<?xml version="1"?>
<platform>
 <char_bit>8</char_bit>
 <default-sign>signed</default-sign>
 <sizeof>
 <short>2</short>
 <int>4</int>
 <long>4</long>
 <long-long>8</long-long>
 <float>4</float>
 <double>8</double>
 <long-double>12</long-double>
 <pointer>4</pointer>
 <size_t>4</size_t>
 <wchar_t>2</wchar_t>
 </sizeof>
</platform>
```

## 第三章 工程 ##
当你使用CMake或Visual Studio时，可以使用 __\-\-project__ 来分析你的工程。  
这会给你一个快速且简单的结果，你并不需要做太多的配置，但很难讲这是否能给你最好的结果。因此建议同时尝试不使用 __\-\-project__ 来分析你的源代码，看看那种方式更好。  

### CMake ###
Cppcheck能够理解编译数据库，你可以通过CMake来产生这些数据库。  
例如：  
```
$ cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
```
将会在当前目录内产生compile_commands.json文件  
现在运行Cppcheck:  
```
$ cppcheck --project=compile_commands.json
```

### Visual Studio ###
你可以在单独的工程文件(\*.vcxproj)上或者整个解决方法(\*.sln)上运行Cppcheck
```
# 在整个解决方法上运行cppcheck
$ cppcheck --project=foobar.sln

# 在单独的工程上运行cppcheck
$ cppcheck --project=foobar.vcxproj
```
请注意，有一个Visual Studio插件允许你在Visual Studio内部调用cppcheck。  

## 第四章 预处理设置 ##
如果你使用 __\-\-project__ 选项，Cppcheck会使用工程文件中的预处理设置。否则你需要配置include以及define的路径。  

### Defines ###
以下是一个拥有两种配置的文件(有A定义和无A定义)：  
```
#ifdef A
    x = y;
#else
    x = z;
#endif
```
默认情况下，Cppcheck会检查所有预处理配置(除了那些其中包含#error的)，因此以上的代码会分析在两种情况下的结果。  
你可以使用 -D 来改变这一点。当你使用 -D 时，cppcheck默认只会检查指定的配置而不检查其他的。 编译器就是这样工作的。但是你可以使用 __\-\-force__ 或者 __\-\-max\-configs__ 来忽略配置的数量。  
```
# 检查所有的配置
cppcheck file.c

# 仅检查配置A
cppcheck -DA file.c

# 当宏A被定义时，检查所有配置
cppcheck -DA --force file.c
```
-U是可能另外一个有用的标识，它取消一个符号的定义，例如：  
```
cppcheck -UX file.c
```
这意味着X被取消了定义。Cppcheck将不会检查X被定义情况下的代码。  

### include 路径 ###
想要添加一个include路径，使用-I，后面接路径。  
Cppcheck的预处理器处理include的方式与其他预处理器基本相同。然而，当其他预处理器没找到一个头文件时会停止工作，而cppcheck仅会打印信息并且继续对代码进行分析。  
这样做的目的在于，cppcheck应该具有在没有看见全部代码的情况下工作的能力。事实上，我们并不推荐把所有的include路径写完全。虽然用cppcheck检查一个类的声明和成员函数的实现是有用的，但是将标准库的头文件传递给cppcheck是强烈不建议的，因为它可能造成更糟糕的结果或者是更长的检查时间。在这些情况下，.cfg文件(见下文)是更好的提供信息的方式。  

## 第五章 XML输出 ##
cppcheck可以将输出保存成XML格式。有一种旧版本的XML格式(version 1)和一种新版本的(version 2)。如果可能的话，请尽量使用新版本。  
保留老版本仅仅是为了保证向上兼容性。这一点不会修改，但它可能在将来被移除。使用 __\-\-xml__ 来启用这种格式。  
新版本修复了一些旧版本格式的问题。新格式将会随着未来版本的cppcheck同步更新，加入一些新的属性和元素。检查文件并输出错误到新的XML格式的命令如下：  
```
cppcheck --xml-version=2 file1.cpp
```
这是一个version 2报告样例：  
```
<?xml version="1.0" encoding="UTF-8"?>
<results version="2">
 <cppcheck version="1.66">
 <errors>
 <error id="someError" severity="error" msg="short error text"
 verbose="long error text" inconclusive="true" cwe="312">
 <location file0="file.c" file="file.h" line="1"/>
 </error>
 </errors>
</results>
```

### <error\> 元素 ###
每个错误都跟随一个<error>元素报告出来。属性：  
```
id              错误的id，有效的符号
severity        error, warning, style, performance 或者 information
msg             短格式的错误信息
verbose         长格式的错误信息
inconclusive    仅当信息是不确定的时候这个属性才被使用
cwe             信息的CWE ID，仅当信息的CWE ID已知时使用
```

### <location\> 元素

