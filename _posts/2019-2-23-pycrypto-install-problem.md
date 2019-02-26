---
layout: post
title: pycrypto库安装问题解决记录
date: 2019-02-23
categories: python
---

## 问题描述 ##  
在ozone的配套库中，有一个安装不是很顺利的库--pycrypto，记录一下解决方案，解决方案在linux下测试有效，windows下未找到好的解决方案。
问题在于执行命令：  
```shell  
pip install pycrypto
```  
后出现错误提示，原因在于缺少C编译的工具。

## 解决方案 ##
安装缺少的相关工具即可。
运行命令：
```shell  
sudo apt-get install python3-dev (for python3)
sudo apt-get install python-dev (for python2)
```  