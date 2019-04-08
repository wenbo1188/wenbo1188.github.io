---
layout: post
title: NLP学习(13)之概率和信息论复习
date: 2019-04-08
categories: NLP
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# 常用分布 #

## 均匀分布 ##

均匀分布(uniform distribution), $$X \sim U(a, b)$$表示$$X$$在$$[a, b]$$上是均匀分布的。

## Bernoulli分布 ##

由单个参数$$\phi \in [0, 1]$$控制，$$\phi$$给出了随机变量等于1的概率。  
$$P(X=1) = \phi$$  
$$P(X=0) = 1 - \phi$$  
$$EX = \phi$$  
$$DX = \phi (1 - \phi)$$

## Multinoulli分布 ##

Multinoulli分布是Bernoulli分布的一个推广形式，Multinoulli分布能在单次试验中产生k种结果，当实验结果为第i个时，随机向量中的对应值为1，其余值均为0，随机向量记为$$X = [X_1, X_2, ..., X_k]$$(相当于独热编码)。

## 多维正态分布 ##

它的参数是一个正定对称矩阵$$\Sigma$$:  
$$N(x;\mu,\Sigma) = \sqrt \frac{1}{(2\pi)^ndet(\Sigma)}e^{-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)}$$

## 指数分布和Laplace分布 ##

### 指数分布 ###

$$P(x;\lambda) = \lambda e^{-\lambda x}, x \ge 0 = \lambda_{1_{x \ge 0}}e^{-\lambda x}, 其中指示函数1_{x \ge 0}来使得当x取负值时的概率为0$$

### Laplace分布 ###

一个联系紧密的概率分布是laplace分布，它允许我们在任意一点$$\mu$$出设置概率质量的峰值，$$Laplace(x;\mu, \gamma) = \frac{1}{2\gamma}e^{-\frac{|x-\mu|}{\gamma}}$$

## Dirac分布和经验分布 ##

在一些情况下，我们希望概率分布中的所有质量都集中在一个点上，可以通过Dirac delta函数$$\delta (x)$$定义概率密度函数来实现。  
$$p(x) = \delta (x - \mu)$$  
$$\delta (x)$$被定义为除了0以外的所有点的值都为0，但积分为1，通过把$$p(x)$$定义成$$\delta$$函数左移$$\mu$$个单位，我们得到了一个在$$x = \mu$$处具有无限窄也无限高的峰值的概率质量。  
Dirac分布经常作为经验分布的一个组成部分出现：  
$$\hat{p}(x) = \frac{1}{m}\sum_{i=1}^m\delta (x - x^{(i)})$$  
经验分布将概率密度$$\frac{1}{m}$$赋给m个点$$x^{(1)},...,x^{(m)}$$中的每一个，这些点是给定的数据集或者采样的集合，只有定义连续型随机变量的经验分布时，Dirac delta函数才是必要的。

## 分布的混合 ##

混合分布由一些组件分布构成，每次实验，样本是由那个组件分布产生的取决于从一个Multinnoulli分布中采样的结果：  
$$P(x) = \sum_i P(c=i)P(x|c=i)$$,其中$$P(c)$$是对各组件的一个Multinoulli分布。  
一个非常强大而且常见的混合模型是高斯混合模型，它的组件p(x|c=i)是高斯分布。每个组件都有各自的参数，均值$$\mu^{(i)}$$和协方差矩阵$$\Sigma^{(i)}$$，有一些混合可以有更多的限制，例如，协方差矩阵可以通过$$\Sigma^{(i)} = \Sigma, \forall i$$的形式在组件之间共享参数。

# 常用函数的有用性质 #

## logistic sigmoid函数 ##

$$\sigma(x) = \frac{1}{1 + e^{-x}}$$  
logistic sigmoid函数通常用来产生Bernoulli分布中的参数$$\phi$$，因为它的值域为$$(0, 1)$$处在$$\phi$$的有效取值范围内。sigmoid函数在变量取绝对值非常大的正值或者负值时会有饱和的现象，函数会变得很平，并且对输入的微小改变不敏感。  

## softplus函数 ##

另外一个常见函数是softplus函数：  
$$\zeta(x) = log(1+e^x)$$  
softplus函数可以用来产生正态分布的$$\beta$$和$$\sigma$$参数，因为它的值域为$$(0, \infty)$$，当处理包含sigmoid函数的表达式时它经常出现。softplus函数是另外一个函数的平滑形式。这个函数是$$x^+ = max(0,x)$$。  

## 重要性质 ##

1. $$\sigma(x) = \frac{e^x}{e^x + e^0}$$  
2. $$\frac{d}{dx}\sigma(x) = \sigma(x)(1 - \sigma(x))$$  
3. $$1 - \sigma(x) = \sigma(-x)$$  
4. $$log\sigma(x) = -\zeta(-x)$$  
5. $$\frac{d}{dx}\zeta(x) = \sigma(x)$$  
6. $$\forall x \in (0, 1), \sigma^{-1}(x) = log(\frac{x}{1-x})$$  
7. $$\forall x>0, \zeta^{-1}(x) = log(e^x - 1)$$  
8. $$\zeta(x) = \int_{-\infty}^x\sigma(y)dy$$  
9. $$\zeta(x) - \zeta(-x) = x$$