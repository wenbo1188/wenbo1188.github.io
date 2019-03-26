---
layout: post
title: NLP学习(9)之评价语言模型的指标
date: 2019-03-26
categories: NLP
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# 评价语言模型的指标 #

## Perplexity(困惑度) ##

好的语言模型会在测试集中给出更准确的预测，即越可能出现的语句，好的语言模型会给出更高的概率。  
$$PP(W) = P(w_1,w_2,...,w_N)^{-\frac{1}{N}} = \sqrt[N]{\frac{1}{P(w_1,w_2,...,w_N)}}$$  
由上述公式可知：

- 降低Perplexity $$\Leftrightarrow$$ 提高概率  
- Perplexity越低 $$\Rightarrow$$ 模型越好
