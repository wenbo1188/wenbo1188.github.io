---
layout: post
title: "shell十三问学习笔记(1)"
date: 2017-03-13
categories: LearningNote
excerpt: shell十三问学习笔记
---

## echo命令 ##
+ 标准的command line包含三个部件:  
command_name option argument
+ echo是一个非常简单,直接的linux命令:  
将argument送至标准输出(stdout)

  
看一下echo命令的运行结果:  
``` shell 
$ echo

$
```  
你会发现只有一个空白行, 然后又回到shell prompt上了.这是因为echo在预设上, 在显示完argument之后, 还会送出一个换行符.若要取消这个换行符, 可利用echo的-n option:
``` shell  
$ echo -n
$
```  
尝试如下输入:  
``` shell
$ echo first line
first line
$ echo -n first line
first line $
```  
于上两个echo命令中, 你会发现argument的部分显示在你的屏幕, 而换行符则视-n option的有无而别.很明显的, 第二个echo由于换行符被取消了, shell prompt就接在输出结果的同一行了

事实上, echo除了-n option之外, 常用选项还有:  
-e:启用反斜线控制转义字符(参考下表)  
-E:关闭反斜线控制转义字符(预设如此)  
-n:取消行末的换行符(与-e选项下的\c字符同意)  

关于echo命令所支持的转义字符如下表:  
\a：ALERT / BELL (从系统喇叭发出铃声)  
\b：BACKSPACE  
\c：取消行末的换行符  
\E：ESCAPE, 跳脱键  
\f：FORMFEED，换页字符   
\n：NEWLINE，换行字符  
\r：RETURN，回车  
\t：TAB，制表符  
\v：VERTICAL TAB，垂直表格跳位键  
\#：ASCII 八进制编码(如"\042")  
\\：反斜线本身  

例一:  
``` shell
$ echo -e "a\tb\tc\nd\te\tf"
a	b	c
d	e	f
```  

例二:  
``` shell  
$ echo -e "\141\011\142\011\143\012\144\011\145\011\146"  
a	b	c
d	e	f
```  

例三:  
``` shell
$ echo -ne "a\tb\tc\nd\te\bf\a"
a	b	c
d	f $
```  
因为e字母后面是\b, 因此输出结果没有e.在结束时听到一声铃声, 那是\a的作用. 由于有-n选项, 因此shell prompt紧接在第二行之后.

事实上, 在日后的shell操作及shell script设计上, echo命令是最常使用的命令, 比如用ehco来检查变量值:  
``` shell
$ A=B
$ echo $A
B
$ echo $?
0
```  

