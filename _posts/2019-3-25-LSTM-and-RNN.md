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
sigmoid:  
$$S(t) = \frac{1}{1 + e^{-t}}$$
sigmoid层输出0~1之间的值，表示信息通过率，0表示无信息通过，1表示完全通过。  
一个LSTM有3个这种门来保护和控制cell状态。  

#### 一步一步看LSTM ####  

1. 首先，LSTM需要决定哪些信息要被丢弃，这个过程通过一个sigmoid层(也叫遗忘层)来实现。遗忘层为每个$$C_{t-1}$$输入$$h_{t-1}$$和$$x_t$$并输出一个0~1之间的数值，1表示完全保留，0表示完全遗忘。让我们回到语言模型中基于前面单词预测后面单词的例子，在这样的问题中，cell状态可能需要将当前主语的性别保存下来，以保证后面的代词使用的正确性，而当我们遇到新主语时，要将旧主语的信息遗忘。  
$$f_t = \sigma(W_f[h_{t-1}, x_t] + b_f)$$  
2. 第二步我们要判断需要加入cell状态的新信息，新信息分为两部分：  

    - 一个sigmoid层，决定哪些值需要更新  
    - 一个tanh层，创造新的可加入cell状态的备选值$$\hat{C_t}$$  

    $$i_t = \sigma(W_i[h_{t-1}, x_t] + b_i)$$  
    $$\hat{C_t} = tanh(W_c[h_{t-1}, x_t] + b_c)$$  
3. 我们将以上操作整合，将就状态$$C_{t-1}$$乘以$$f_t$$以遗忘一些信息，加上$$i_t*\hat{C_t}$$，也就是要更新的信息，($$i_t$$决定了我们要更新状态的权重占比)  
$$C_t = f_t * C_{t-1} + i_t * \hat{C_t}$$  
4. 最终我们决定要输出什么，输出取决于状态$$C_t$$但要经过过滤，首先通过sigmoid层来决定cell状态的哪些部分要输出，然后我们让$$C_t$$经过tanh层将值映射至$$[-1, 1]$$，然后乘以sigmoid的输出。  
$$o_t = \sigma(W_o[h_{t-1}, x_t] + b_o)$$  
$$h_t = o_t * tanh(C_t)$$  

#### LSTM的变型 ####

1. 一个流行的变型：加入peephole connections，意味着每个sigmoid门都能看到cell状态，也即：  
$$f_t = \sigma(W_f[C_{t-1},h_{t-1},x_t]) + b_f$$  
$$i_t = \sigma(W_i[C_{t-1},h_{t-1},x_t]) + b_i$$  
$$o_t = \sigma(W_o[C_{t-1},h_{t-1},x_t]) + b_o$$  
另外，也可以使sigmoid门对于cell状态部分可见，部分不可见。  
2. 另外一个变型是同时决定哪些需要遗忘，那些要新增。  
$$C_t = f_t * C_{t-1} + (1 - f_t) * \hat{C_t}$$  
3. 一个比较大的变化的变型：GRU(Gated Recurrent Unit)