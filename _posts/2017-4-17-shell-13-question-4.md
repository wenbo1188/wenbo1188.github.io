---
layout: post
title: shell十三问学习笔记(8)
date: 2017-4-17
categories: LearningNote
excerpt: shell十三问学习笔记
---

## \$(())与\$()还有\${}差在哪? ##  

在bash shell中, $()与反引号都是用来进行命令替换用的, 即重组命令行：  
* 完成引号里的命令行, 然后将其结果替换出来, 再重组命令行。  
例如：  
```bash  
$ echo the last sunday is $(date -d "last sunday" + %Y-%m-%d)  
```

比较$()与反引号：  
* $()比较直观
* 反引号可移植行较强，因为其可以适应全部的unix shell使用  

${}作用就是变量替换:  
* 一般情况下,\$var与\$\{var\}并没有什么区别， 但是用${}会比较精确地界定变量名称的范围  
例如：  
```bash
$ A=B
$ echo \$AB
```
原本是打算将$A的结果替换出来， 然后紧接着一个B， 但结果确实将名称为AB的值替换出来了。正确使用方式为:  
```bash
$ echo \${A}B
$ BB
```
