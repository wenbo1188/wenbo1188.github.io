---
layout: post
title: NLP学习(15)之约束优化
date: 2019-04-11
categories: NLP
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# 约束优化 #

有时候，在$$x$$的所有可能值下最大化或最小化一个函数$$f(x)$$不是我们所希望的。相反，我们可能希望在$$x$$的某些集合$$\mathbb{S}$$中找$$f(x)$$的最大值或最小值。这被称为约束优化。在约束优化术语中，集合$$\mathbb{S}$$内的点$$x$$被称为可行点。  

范数约束，如$$\parallel x \parallel \le 1$$是一个常见的约束条件。  

Karush–Kuhn–Tucker(KKT)方法是针对约束优化非常通用的解决方案。为介绍KKT方法，我们引入一个称为广义Lagrange函数的新函数。我们希望通过$$m$$个函数$$g^{(i)}$$和$$n$$个函数$$h^{(j)}$$描述$$\mathbb{S}$$，那么$$\mathbb{S}$$可以表示为$$\mathbb{S} = \{x | \forall i, g^{(i)}(x) = 0 and \forall j, h^{(j)}(x) \le 0\}$$。其中涉及$$g^{(i)}$$的等式称为等式约束，涉及$$h^{(j)}$$的不等式称为不等式约束。  
广义
Lagrangian可以如下定义：  
$$L(x, \lambda, \alpha) = f(x) + \sum_i\lambda_ig^{(i)}(x) + \sum_j\alpha_jh^{(j)}(x)$$  

我们可以通过优化无约束的广义Lagrangian解决约束最小化问题。只要存在至少一个可行点且$$f(x)$$不允许取$$\infty$$，那么  
$$\min \limits_x \max \limits_{\lambda} \max \limits_{\alpha,\alpha \ge 0}L(x, \lambda, \alpha)$$  
与如下函数有相同的最优目标函数值和最优点集$$x$$，  
$$\min \limits_{x \in \mathbb{S}}$$  

同理，要解决约束最大化问题，我们可以构造$$-f(x)$$的广义Lagrange函数。