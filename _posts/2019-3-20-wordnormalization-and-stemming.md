---
layout: post
title: NLP学习(3)之词语正规化和词根
date: 2019-03-20
categories: NLP
---

## 正规化 ##  

1. U.S.A -> USA  
2. 非对称性扩展:  
    - 输入:window 搜索:window, windows
    - 输入:windows 搜索:Windows, windows, window
    - 输入:Windows 搜索:Windows(一定是微软的Windows而不是窗户)

### 大小写转化 ###  
一般将所有字母转化为小写。

特例:(固有名词大写)  

- General Motors  
- Fed vs fed  
- SAIL vs sail  

### 词形还原(lemmatization) ###  

- am, are, is -> be
- car, cars, car's, cars' -> car
- 词素组成:  
    + 词根: 表示含义
    + 词缀: 语法功能

### Porter算法 ###  

step 1a:  

原则|例子  
-|-
sses -> ss|caresses -> caress  
ies -> i|ponies -> poni
ss -> ss|caress -> caress
s -> |cats -> cat

step 1b:  

原则|例子  
-|-
(\*v\*)ing -> |walking -> walk;sing ->sing
(\*v\*)ed -> |plastered -> plaster

其中v表示元音

step 2(长词根):  

原则|例子  
-|-
ational -> ate|relational -> relate
izer -> ize|digitizer -> digitize
ator -> ate|operator -> operate

step 3(长词根):  

原则|例子  
-|-
al -> |revival -> reviv
able -> |adjustable -> adjust
ate -> |activate -> activ