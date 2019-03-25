---
layout: post
title: NLP学习(7)之LSTM与RNN
date: 2019-03-25
categories: NLP
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# LSTM与RNN #  

## RNN ##  

### 引入RNN的原因 ###  

传统的神经网络存在遗忘的问题，而现实中要解决的问题有很多是前后关联的，因此引入RNN来解决。  

## LSTM ##  

LSTM属于一类特殊的RNN。  

### 长期依赖的问题 ###  

当需要的依赖信息距离较近时RNN能够很好的利用这些信息，  
比如：the cloud is in the *sky*.  
但当距离较远时，RNN就不能将信息联系起来了，  
比如：I grew up in France, ..., I speak fluent *French*.  
幸运的是，LSTM不会有这个问题。  

### LSTM(Long Short Term Memory Networks) ###  

标准的RNN具有链式结构，但是先循环传递的可能只是一个tanh层。  
> tanh:双曲正切函数  
> sinh(x) = (e^x - e^-x) / 2  
> cosh(x) = (e^x + e^-x) / 2  
> tanh(x) = sinh(x) / cosh(x)

LSTM在具有链式结构的同时，循环传递模块具有一个更复杂的结构。  

#### LSTM的核心思想 ####  

核心在于cell状态，LSTM有能力通过门(gate)的形式和结构向cell状态中增添或删除一些信息。们是一种具有让信息选择性通过的方式，由sigmoid神经网络层和一个点乘的操作组成。  
sigmoid: S(t) = 1 / (1 + e^-t)  
sigmoid层输出0~1之间的值，表示信息通过率，0表示无信息通过，1表示完全通过。  
一个LSTM有3个这种门来保护和控制cell状态。  

#### 一步一步看LSTM ####  

1. 首先，LSTM需要决定哪些信息要被丢弃，这个过程通过一个sigmoid层(也叫遗忘层)来实现。遗忘层为每个Ct-1输入ht-1和xt并输出一个0~1之间的数值，1表示完全保留，0表示完全遗忘。让我们回到语言模型中基于前面单词预测后面单词的例子，在这样的问题中，cell状态可能需要将当前主语的性别保存下来，以保证后面的代词使用的正确性，而当我们遇到新主语时，要将旧主语的信息遗忘。$$ft = test$$  
2. 第二步我们要判断需要加入cell状态的新信息，新信息分为两部分：  

    - 一个sigmoid层，决定哪些值需要更新  
    - 一个tanh层，创造新的可加入cell状态的备选值~Ct