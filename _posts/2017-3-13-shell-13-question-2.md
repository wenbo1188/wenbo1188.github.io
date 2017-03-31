---
layout: post
title: "shell十三问学习笔记(2)"
date: 2017-03-13
categories: LearningNote
excerpt: shell十三问学习笔记
---

## " "(双引号)与' '(单引号)的区别 ##
> 還是回到我們的 command line 來吧...  
經過前面兩章的學習，應該很清楚當你在 shell prompt 後面敲打鍵盤、直到按下 Enter 的時候，
你輸入的文字就是 command line 了，然後 shell 才會以行程的方式執行你所交給它的命令。
但是，你又可知道：你在 command line 輸入的每一個文字，對 shell 來說，是有類別之分的呢？

command line的每一个character, 分为如下两种:  
+ literal: 也就是普通纯文字, 对shell来说没特殊功能  
+ meta: 对shell来说, 具有特定功能的特殊保留字  

事实上, 我们在command line中已经碰到两次几乎每次都会碰到的meta:  
+ IFS: 由space或tab或enter三者之一组成(我们常用space).
+ CR: 由enter产生  

IFS是用来拆解command line的每一个次(Word)用的, 因为shell command line是按词来处理的. 而CR则是用来结束command line用的.  
除了IFS与CR, 常用的meta还有:  
=: 设定变量  
$: 作变量或运算替换  
\>: 重导向stdout  
<: 重导向stdin  
|: 命令管道  
&: 重导向file descriptor, 或将命令置于背景执行  
(): 将其内的命令置于nested subshell执行, 或用于运算或命令替换  
{}: 将其内的命令置于non-named function中执行, 或用在变量替换的界定范围  
;: 在前一个命令结束时, 忽略其返回值, 继续执行下一个命令  
&&: 在前一个命令结束时, 若返回值为true, 继续执行下一个命令  
||: 在前一个命令结束时, 若返回值为false, 继续执行下一个命令  
!: 执行history列表中的命令  

加入我们需要在command line中将这些保留字的功能关闭的话, 就需要quoting处理了.在bash中, 常用的quoting有如下三种方法:  
+ hard quote: ' '(单引号), 凡在hard quote中的所有meta均被关闭  
+ soft quote: " "(双引号), 在soft quote中大部分meta都会被关闭, 但某些则保留(如$, 博客原作者表示具体清单不清楚, 不过应该也不是很重要)  
+ escape: \(反斜线), 只有紧接在escape之后的单一meta才被关闭  

下面举一些具体的例子:  
```bash 
$ A=B C		# 空格键未被关掉, 作为IFS处理 
$ C: command not found  
$ echo $A

$ A="B C"	# 空格键已被关掉, 仅作为空格字符处理  
$ echo $A  
B C
```  

事实上, 空格键无论在soft quote还是在hard quote里, 均会被关闭, Enter键亦然:  
```bash
$ A='B
> C
> '
$ echo "$A"
B
C
```  

上例中, 由于<enter>被置于hard quote中, 因此不再作为CR字符来处理.这里的<enter>只是一个断行符号(new-line)而已, 由于command line并没得到CR字符, 因此进入第二个shell prompt(PS2, 以>符号表示), command line并不会结束, 直到第三行, 我们输入的enter不在hard quote里面, 因此并没被关闭, 此时, command line碰到CR字符, 于是结束, 交给shell来处理.  

上例的enter要是被置于soft quote中的话, CR也会同样被关闭:  
```bash
$ A="B
> C
> "
$ echo $A
B C
```  
由于echo $A时的变量没置于soft quote中, 因此当变量替换完成后并作命令行重组时, enter会被解释为IFS, 而不是new-line  
同样的, 用escape亦可关闭CR字符:  
```bash
$ A=B\
> C\
>
$ echo $A
BC
```  

至于soft quote跟hard quote的不同, 主要是对于某些meta的关闭与否, 以$来作说明:  
```bash
$ A=B\ C
$ echo "$A"
B C
$ echo '$A'
$A
```  

```bash
$ A=B\ C
$ echo '"$A"'		# 最外面的是单引号
"$A"
$ echo "'$A'"		# 最外面的是双引号
'B C'
```  

====
> 在 CU 的 shell 版裡，我發現有很多初學者的問題，都與 quoting 理解的有關。
比方說，若我們在 awk 或 sed 的命令參數中調用之前設定的一些變量時，常會問及為何不能的問題。
要解決這些問題，關鍵點就是：
* 區分出 shell meta 與 command meta 