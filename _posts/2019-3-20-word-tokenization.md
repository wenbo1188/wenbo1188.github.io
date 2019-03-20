---
layout: post
title: NLP学习之词语划分
date: 2019-03-20
categories: NLP
---

## 次数统计 ##

### 概念 ###
> I do uh main- mainly business data processing.  

main-: fragments(分片)  
uh: filled pauses(语间停顿填充词)  

lemma: 相同词根、相同成分、相同模糊词义  
wordform: cat和cats属于不同的wordform  
type: 语句中出现的词语集合中的某个元素(统计时不重复计数)  
token: 与剧中的词语实体(统计时可重复计数)  
N = token个数  
V = type的集合, |V|表示该集合的大小

分词或词数统计在中文里比英语等有空格隔开单词的语言更复杂。

