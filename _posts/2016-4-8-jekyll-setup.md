---
layout: post
title:  "关于jekyll安装"
date:   2016-04-08
categories: jekyll
---

# jekyll安装
----------
前些天刚刚把jekyll在我的电脑上安装成功了,安装之路极其艰辛,所以把整个安装过程记录一下.
主要借鉴了[一篇博文](http://www.zhanxin.info/jekyll/2013-08-07-jekyll-doc-installation.html),不过在我自己电脑上实践的过程中仍然遇到了很令人头痛的问题,不知道是因为版本更新的问题还是平台不同的原因.总之我的电脑是ubuntu系统,系统版本14.04,内核版本4.4.4


## 一.安装Ruby
* 详细的安装文档，可以查看 Ruby 官方的安装介绍.我在这里列出几种简单的方法，便于快速参考.   
* 使用 RVM 安装.关于 RVM 安装的详细方法在[Installation page](https://rvm.io/rvm/install)    
*             ```shell       
$ \curl -L https://get.rvm.io | bash   
##原文处这里用的是1.9.2版本但是在我的电脑上要求是版本>=2.0.0     
$ rvm install 2.0.0     
$ rvm use 2.0.0     
```     

* Linux 下的安装方法.在终端上执行：    
*          ```shell	 	
$ sudo apt-get install ruby2.0     
##检查是否安装成功     
$ ruby -v     
##返回如下结果表示安装成功(注意这里要求必须ruby版本>=2.0否则后面会出错)    
$ ruby 2.1.2p95 (2014-05-08 revision 45877) [x86_64-linux]     
```     

## 二.安装RubyGems   
* ubuntu下应该系统自带RubyGems, 否则下载安装包,安装到本地后检查更新   
*            ```    
$ sudo gem update --system   
$ gem -v     
```     

## 三.安装Jekyll     
* 最好的安装方法应该是通过 RubyGems 来安装，在终端输入：         
*                ```       
$ sudo  gem install jekyll      
##检查版本号(我的是3.1.2)
$ jekyll -v       
```       
* 这里可能需要修改gem的源,运行:
*                ```
$ gem source -r https://rubygems.org/       
$ gem source -a http://ruby.taobao.org/ or https://ruby.taobao.org/          
```       

至此,jekyll基本安装成功.
