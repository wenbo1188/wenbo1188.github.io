---
layout: post
title: NLP学习之语句划分
date: 2019-03-20
categories: NLP
---

## 语句划分 ##  

- !, ? 等标点作为语句结束划分相对无歧义  
- 有歧义 -> 解决歧义: 建立一个二分器(decision tree)  

    1. 找到一个"."
    2. 判断是否为句尾

Decision Tree可以通过:  

- logistic regression
- SVM
- Neural network
- etc