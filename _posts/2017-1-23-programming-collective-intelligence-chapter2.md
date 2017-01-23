---
layout: post
title: "集体智慧编程-chapter2学习笔记"
date: 2017-01-23
categories: Programming Collective Intelligence
---

#提供推荐
##协作型过滤
 * 协作型过滤算法通常对一大群人进行搜索, 并从中找出与我们品味相近的一小群人.
 * 相似度评价值:__欧几里德距离__和__皮尔逊相关度__
  + 欧几里德距离评价:以经过人们一致评价的物品为坐标轴, 然后将与参与评价的人绘制到图上, 并考察他们彼此间的距离远近.
  + 代码示例:python		
		```python
	from math import sqrt	

	\# 返回一个有关person1与person2的基于距离的相似度评价
	def sim_distance(prefs, person1, person2):
		\# 得到shared_items的列表
		si = {}
		for item in prefs[person1]:
			if item in prefs[person2]:
				si[item] = 1

