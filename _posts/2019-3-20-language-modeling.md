---
layout: post
title: NLP学习之语言模型
date: 2019-03-20
categories: NLP
---

# 介绍N-grams #  

- 语言模型目标: 计算一个语句或一组单词序列的可能性。  
- P(W) = P(w1, w2, ..., wn)或P(w5 | w1, w2, w3, w4)  
- 计算P(W)或P(wn | w1, w2, ..., wn-1)的模型称为语言模型  

回顾:  
P(A, B, C, D) = P(A) P(B|A) P(C|A, B) P(D|A, B, C)  

想要估计P(w3|w1, w2) = count(w1, w2, w3) / count(w1, w2)是否可行?  
答案是不可行，因为出现w1, w2, w3这样的语句太少了，尤其是语句比较长的情况下，所以无法直接统计词频来估计概率  

# 如何解决计算概率的问题呢 #  

需要引入Markov Assumption:  
P(the|its water is so transparent that) ~= P(the|that)  
或者  
P(the|its water is so transparent that) ~= P(the|transparent that)  
即P(wi|w1, w2, ..., wi-1) ~= P(wi|wi-k, ..., wi-1)  

# 应用此假设的一些例子 #  

## 最简单例子: Unigram Model ##  

Unigram Model假设:  
P(w1, w2, ..., wn) ~= P(w1)P(w2)...P(wn)

## Bigram Model ##  

假设:  
P(wi|w1, w2, ..., wi-1) ~= P(wi|wi-1)  

# 扩展到N-gram models(trigrams, 4-grams, 5-grams) #  

总体上讲N-grams model是一个不完善的模型，原因在于在语言中单词之间是有长远的向前依赖的，即不符合Markov模型，但实际问题中N-grams模型有时对于问题的解决已经足够  
实际操作的问题:  

- 我们在对数空间下进行所有计算，优点在于:  

    1. 避免概率在连乘后下溢
    2. 转化为加法计算更快