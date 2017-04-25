---
layout: post
title: "RBDS(Raspberrypi-Based Distributing System)搭建及配置"
date: 2017-3-30
categories: share
---

## RBDS(Raspberrypi-Based Distributing System)搭建及配置 ##
----------- 
### 背景介绍 ###
随着移动通信技术的进步，推动着物联网和智慧城市的迅速发展、普及，相关的应用渗透到社会领域的许多方面，成为人们日常工作生活的重要部分。由于移动节点的迅速增长、应用日益广泛，产生了巨大的数据量，同时也极大地增加了网络传输的负载。有效、快速地处理这些大数据是提升的物联网应用性能的关键环节。  
由于移动设备电池有限，计算和存储较弱，无法胜任这样大数据量的计算、存储、管理等操作，所以需要一个强大的平台来管理这些数据。云计算强大的数据存储和计算能力，把物联网等移动应用于云计算结合可以较好地解决这些问题。当前有很多物联网、智慧城市与云计算结合的研究，在这种结合模式中移动节点把数据传输到远程的数据中心，在数据中心进行集中化的管理、计算和存储，很大程度上弥补了移动节点自身弱的不足.  
但是，伴随着网络中大量的数据的传输和处理，集中化的数据中心所代表的云计算框架渐渐地暴露出了其不足之处：  
1. 传输延迟高：由于移动端数据都要传到远程数据中心去处理，需要比较高的延时，应用的相应时间也会变大；
2. 带宽负荷大：大量的移动端数据都传到数据中心，给数据中心的对外网络带宽增加了很大的负荷；
3. 可扩展性弱：由于数据中心本身体量大，增加一个数据中心代价太大，可扩展性方面较弱。


为了克服以上集中式不足，分布式的边缘计算（或者雾计算）框架能够更好地适用于移动应用，弥补集中式云计算框架的不足。相比云计算：
1. 边缘计算框架能够直接服务于移动端，很多数据可以在局部直接处理掉，降低了数据的传输时延已经应用的反馈时间，对实时性应用更为重要；
2. 边缘计算节点只需要负担起本地的移动节点流量，并且可以根据应用的需求融合、吸收、过滤本地数据，大大降低了网络负载；
3. 边缘计算节点规模小，可以比较灵活地加入，可以更为广泛地分布、灵活地部署，增加了系统的可扩展性；
4. 同时，这种分布式的系统结构还能更好地服务于节点的移动漫游，降低漫游切换的时延。
研究边缘计算与物联网、智慧城市的结合能够有效提升目前大规模移动应用的性能，更好地推动物联网、智慧城市的进一步发展。


----------- 
### 基本框架结构 ###
![A logical Architecture Example of IoMDC](https://raw.githubusercontent.com/wenbo1188/wenbo1188.github.io/master/static/img/1.png)
![The platform of IoMDC system](https://raw.githubusercontent.com/wenbo1188/wenbo1188.github.io/master/static/img/2.png)
![The platform of IoMDC system](https://raw.githubusercontent.com/wenbo1188/wenbo1188.github.io/master/static/img/3.png)

----------- 
### 准备工作 ### 
+ PC端(用于开发) 
	* 操作系统: Linux(Ubuntu, Archlinux等均可), 我自己用的是Ubuntu14.04LTS, 以下均按照Ubuntu系统为例说明
	* Python版本:python2.x 
	* 安装及配置git: [github简明教程](http://www.runoob.com/w3cnote/git-guide.html) 
	* 安装Flask: 
		```bash 
		$ sudo apt-get install python-pip
		$ sudo pip install Flask
		``` 
	* 安装docker: 
		```bash
		$ sudo apt-get install docker.io
		``` 
	* 获取源代码: 
		```bash 
		$ sudo git clone https://github.com/wenbo1188/distribute.git project_path
		``` 
+ 树莓派端(用于部署应用, Raspberry Pi 3 Model B) 
	* 操作系统: ubuntu-mate([树莓派官网](https://www.raspberrypi.org/downloads/)上可以下载ubuntu-mate的镜像, 可以用[imagewriter](https://sourceforge.net/projects/win32diskimager/)把镜像写入microSD(此软件是在windows下的=_=).
	* 树莓派账户及密码: raspberry1(2-6)(根据盒子上的标号分别为1-6), 密码均为raspberrypi
	* [安装docker](http://hugozhu.myalert.info/2015/04/12/60-run-docker-on-raspberry-pi.html): 这篇教程讲的是在树莓派上配置archlinux系统后安装docker的过程, 仅供参考, 其实用ubuntu-mate也可以安装docker, 方法见PC端.
	* 部署应用: 
	```bash 
	$ sudo git clone https://github.com/wenbo1188/distribute.git project_path 
	$ cd project_path
	$ sudo pip install -e . 
	``` 
	* 使用方法及模块介绍:  
	[https://github.com/wenbo1188/distribute/blob/master/README.md](https://github.com/wenbo1188/distribute/blob/master/README.md)