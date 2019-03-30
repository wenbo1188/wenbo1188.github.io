---
layout: post
title: NLP学习(11)之模型优化方法
date: 2019-03-28
categories: NLP
---

# 模型泛化和“0”的处理 #

与其他机器学习方法一样，语言模型的训练也会有过拟合的问题。一种解决过拟合的方法：处理好数据中的“0”  
0表示从未在训练集中出现，但在测试集中出现某个单词。  
e.g.  
训练集中：  
... denied the allegations.  
... denied the reports.  
... denied the claims.  
... denied the requests.  
$$\Rightarrow P("offer"|denied the) = 0$$  
测试集中：  
... denied the offer.  
... denied the loan.  
如何处理这种情况呢?  

# Add one smoothing #

$$Add one smoothing \Rightarrow Laplace smoothing$$  
假装我们对于每个单词的count都至少为1，即假设我们都至少见过一次，对每个情况的count都加1。  
$$P_{MLE}(w_i|w_{i-1}) = \frac{C(w{i-1},w_i)}{C(w_{i-1})}$$  
$$P_{ADD-1}(w_i|w_{i-1}) = \frac{C(w_{i-1},w_i) + 1}{C(w_{i-1} + V)}$$  
注意分母要加V，因为对于每种组合$$(w_{i-1},w_x)$$都加了1，而$$w_x的个数为V$$

# Maximum Likelihood Estimates(最大似然估计) #

调整后的count:  
$$C^*(w_{n-1},w_n) = \frac{[C(w_{n-1},w_n) + 1] C(w_{n-1})}{C(w_{n-1}) + V}$$

被修改过的Count与原有的Count数相距甚远，因此add-1不被用于N-grams：  

- 我们有更好的方法
- add-1被用于的NLP模型特点：  
    + 用于文本分类
    + 0的个数不多的情况

# backoff and Interpolation(退化和插入) #

- 有时使用更少的上下文会更有帮助  
- 在使用没有被证明优秀的模型时，可能会用到使用更少上下文的模型
- backoff
    + 如果有很好的证据说明trigram很好，那么使用
- Interplotation
    + 混合使用unigram, bigram, trigram
- Interplotation往往比backoff更好

## 线性interplotation ##

- simple interplotation  
$$\hat{P}(w_n|w_{n-2},w_{n-1}) = \lambda_1P(w_n|w_{n-2},w_{n-1}) + \lambda_2P(w_n|w_{n-1}) + \lambda_3P(w_n), 其中\Sigma_i\lambda_i = 1$$  
- 上下文条件参数：  
$$\hat{P}(w_n|w_{n-2},w_{n-1}) = \lambda_1(w^{n-1}_{n-2})P(w_n|w_{n-2},w_{n-1}) + \lambda_2(w^{n-1}_{n-2})P(w_n|w_{n-1}) + \lambda_3(w^{n-1}_{n-2})P(w_n)$$  

## 如何设置$$\lambda$$d的值 ##

- 使用预留的语料库(held-out corpus)  
整体语料库被分为三部分：  
    + train data
    + held-out data
    + test data  
其中held-out data也被称为DEV SET  
- 选择使held-out data的概率最大化的$$\lambda$$：  
$$logP(w_1,...,w_n|M(\lambda_1,...,\lambda_K)) = \Sigma_ilogP_M(\lambda_1,...,\lambda_K)(w_i|w_{i-1})$$

# 未见过的单词处理 #

大多数的任务并不限制在固定有限的词语集内，因此可能会出现这样的情况：  
在训练集中从未出现的单词，在测试集中出现：  
OOV：out of vocabulary  

解决：  
是指一个固定的大小为V的词库，在训练集中不在词库的单词被替换为<UNK>正常训练，测试出现未见过的单词时，按<UNK>的概率计算。  

# 大语料库中的n-gram #

Pruning: 只存出个数大于某个指定threshold的n-gram  
Entropy: base pruning  
提升效率的方法：  

1. 使用高效的数据结构：tries
2. Bloom filter: approximate language model
3. 将单词以索引方式存储，而不是字符串
4. 做一些量化优化(之存储概率中的某些bit而不是全部)

# 大语料库中的smoothing #

- "Stupid backoff"
- $$S(w_i|w^{i-1}_{i-k+1}) = \begin{cases}0& \text{if count(w^i_{i-k+1}) > 0}1& \text{otherwise}\end{cases}$$
$$S(w_i) = \frac{count(w_i)}{N}$$

# N-gram smoothing总结 #

- Add-1: 适用于文本分类，不适用于语言模型
- 最常用的方法：extended interpolated kneser-ney
- 超大语料库，比如web: stupid back off

# 更高级的一些smoothing方法 #

- Good-Turing
- Kneser-Ney
- Witten-Bell

用我们见过一次的事物的count，来帮助估计我们从未见过的  
Notation:  
$$N_e = Frequency of frequency c$$  
$$PGT(things with zero frequency) = \frac{N_1}{N}, C^* = \frac{(C+1)N_{C+1}}{N_C}$$