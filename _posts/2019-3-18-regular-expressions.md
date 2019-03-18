---
layout: post
title: NLP学习之正则表达式
date: 2019-03-18
categories: NLP
---

## Regular Expressions: Disjunctions ##

1. 符号"[]"  
    "[]"表示或的关系:

    表达式|含义  
    --|--  
    [Ww]|大写W或者小写w其中的一个
    [1234567]|1到7中的一个
    [A-Z]|大写字母

2. 否定"^"  
    "^"表示否定、排除:

    表达式|含义  
    --|--  
    [^A-Z]|非大写
    [^Ss]|既非S又非s
    [^e^]|非e非^
    a^b|a非b

3. 管道符"|"  
    "|"表示或:  

    表达式|含义  
    --|--  
    ground\|woodchuck|ground或woodchuck
    a\|b\|c|[abc]
    [Gg]round\|[Ww]oodchuck

4. "?", "*", "+", "."  

    表达式|含义  
    --|--  
    colou?r|"?"表示0个或1个前面字母
    oo*h!|"\*"表示0个或多个前面字母
    o+h!|"+"表示1个或多个前面字母
    beg.n|"."表示任意字母

5. Anchors: "^", "$"  

    表达式|含义  
    --|--  
    ^[A-Z]|匹配开头的大写字母
    [A-Z]$|匹配结尾的大写字母

6. 符号"{}"  

    表达式|含义  
    --|--  
    [A-Z]{3}|3个大写字母
    [A-Z]{2, 4}|2到4个大写字母