---
layout: post
title: 	"集体智慧编程-chapter2学习笔记"
date: 2017-01-23
categories: LearningNote
excerpt: 集体智慧编程学习笔记
---

# 提供推荐 

## 协作型过滤  
协作型过滤算法通常对一大群人进行搜索, 并从中找出与我们品味相近的一小群人.  
相似度评价值:**欧几里德距离**和**皮尔逊相关度**  

---------
欧几里德距离评价:以经过人们一致评价的物品为坐标轴, 然后将与参与评价的人绘制到图上, 并考察他们彼此间的距离远近.   

#### 代码示例: 

	from math import sqrt	

	# 返回一个有关person1与person2的基于距离的相似度评价
	def sim_distance(prefs, person1, person2):
		# 得到shared_items的列表
		si = {}
		for item in prefs[person1]:
			if item in prefs[person2]:
				si[item] = 1
		
		# 如果两者没有共同之处, 则返回0
		if len(si) == 0:
			return 0

		# 计算所有差值的平方和
		sum_of_squares = sum([pow(prefs[person1][item] - prefs[person2][item], 2) for item in prefs[person1] if item in prefs[person2]])
		
		# 结果加1防止除零, 并取其倒数
		return 1/(1 + sqrt(sum_of_squares) 

-----------
皮尔逊相关度评价:该相关度是判断两组数据与某一直线拟合程度的一种度量.它在**数据不是很规范(normalized)**时(比如,影评者对影片的评价总是相对于平均水平偏离很大时), 会倾向于给出更好的结果.  

#### 代码示例:

	# 返回p1和p2的皮尔逊相关系数
	def sim_pearson(prefs, p1, p2):
		# 得到双方都曾经评价过的物品列表
		si = {}
		for item in prefs[p1]:
			if item in prefs[p2]:
				si[item] = 1
		# 得到列表元素的个数
		n = len(si)

		# 如果两者没有共同之处, 则返回1
		if n == 0:
			return 1

		# 对所有偏好求和
		sum1 = sum([prefs[p1][it] for it in si])
		sum2 = sum([prefs[p2][it] for it in si])

		# 求平方和
		sum1Sq = sum([pow(prefs[p1][it], 2) for it in si])
		sum2Sq = sum([pow(prefs[p2][it], 2) for it in si])

		# 求乘积之和
		pSum = sum([prefs[p1][it] * prefs[p2][it] for it in si])

		# 计算皮尔逊评价值
		num = pSum - (sum1 * sum2 / n)
		den = sqrt((sum1Sq - pow(sum1, 2) / n) * (sum2Sq - pow(sum2, 2) / n))
		if den == 0:
			return 0

		r = num / den

		return r
	
-------------------------
相似性度量的选择:此处介绍了两种常用的相似性评价方法, 实际上还有许多方法可以衡量两组数据的相似程度, 使用哪一种方法最优, 完全取决于具体的应用.  

------------------------
为评论者打分：我们已经有了对两个人进行比较的函数，下面我们就可以编写函数，对指定人员进行打分并找出最接近的匹配结果了

#### 代码示例:

	# 从反映偏好的字典中返回最为匹配者
	# 返回结果的个数和相似度函数均为可选参数
	
