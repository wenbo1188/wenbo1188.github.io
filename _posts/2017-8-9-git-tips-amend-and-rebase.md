---
layout: post
title: Git使用Tips之amend与rebase
date: 2017-08-09
categories: git
---

## amend and rebase ##
最近全面系统的学习了一下git，并且在使用过程中也总结了一些小Tips。  

### 修补提交amend ###
如果当我们向暂存区中进行了一次commit以后发现commit中有一些错误，需要马上进行修改，并且你不想让其他人知道你的这次改动，那么以下命令能够对最近的一次提交进行修补：  
```
do some changing...
git commit --amend
```

### rebase进行提交顺序调整 ###
如果这个错误过了一会你才发现，而这时已经有新的commit了，这时你仍然想在原来的commit上进行补救，可以使用以下的命令：  
```
git rebase -i commit-ID^ #调整从commit-ID到最新提交的内容
```
或者
```
git rebase -i HEAD~n^ #调整从HEAD上溯的n代父提交到最新提交的内容
```
执行命令之后，会出现文字编辑器, 如下:  
```
# xxxxxxx
pick commitID1
pick commitID2
......
```
将需要修改的提交前面对应的pick改为edit, 保存。此时就可以进行修改了，修改完成后执行：  
```
git rebase --continue
```
就能够保存修改。