---
layout: post
title: NLP学习之Min-Edit-Distance
date: 2019-03-20
categories: NLP
---

# 判断两个字符串的相似度的场景 #  

- 拼写检查纠错:  
    用户输入: graffe  
    哪个更接近: graf, graft, grail, giraffe  
- 计算生物学:
    基因对齐比对  

# Edit Distance #  

- 两个字符串最小的edit distance  
- 是最少的，使两个字符串变相同的操作数 :
    + 插入  
    + 删除  
    + 替换  

# 如何求Min Edit Distance #  

等价于找到一个从初始字符串到目标字符串的最短变化路径  

- 初始状态: 我们要转化的字符串
- 操作: 插入、删除、替换
- 目标状态: 目标字符串
- 路径花费: 操作数量

削减搜索空间方法:对于每个状态只保存最短路径  

# 定义Min Edit Distance #  

初始两个字符串:  
X -- 长度为m  
Y -- 长度为n  
定义D[i][j] = X[1...i]和Y[1...j]的edit distance,则X和Y的Min Edit Distance为D(n, m)  

计算方法: Dynamic Programming  

```C++
//初始化:
D[i][0] = i;
D[0][j] = j;

//循环:
for (int i = 1;i <= m;++i)
{
    for (int j = 1;j <= n;++j)
    {
        int sub = (X[i] != Y[i]) ? D[i - 1][j - 1] + 2 : D[i - 1][j - 1];
        D[i][j] = min(min(D[i - 1][j] + 1, D[i][j - 1] + 1), sub);
    }
}

//返回:
return D[n][m];
```

# 计算对齐(Computing alignments) #  

我们不仅需要知道Min Edit Distance, 还需要知道为了获得Min Edit Distance需要的操作步骤, 因此需要在上面算法中加入backtrace  

```C++
//初始化:
D[i][0] = i;
D[0][j] = j;

//循环:
for (int i = 1;i <= m;++i)
{
    for (int j = 1;j <= n;++j)
    {
        int sub = (X[i] != Y[i]) ? D[i - 1][j - 1] + 2 : D[i - 1][j - 1];
        D[i][j] = min(min(D[i - 1][j] + 1, D[i][j - 1] + 1), sub);
        if (insertion)
            ptr[i][j] = LEFT;
        else if (deletion)
            ptr[i][j] = DOWN;
        else if (substitution)
            ptr[i][j] = DIAG;
    }
}

//返回:
return D[n][m];
```

# 带权重的Weighted Edit Distance #  

需要权重的原因: 在拼写检查中，一些字母比其他字母更容易出错  
算法:

```C++
//初始化:
D[0][0] = 0;
D[i][0] = D[i - 1][0] + del[X[i]];
D[0][j] = D[0][j - 1] + ins[Y[i]];

//循环:
for (int i = 1;i <= m;++i)
{
    for (int j = 1;j <= n;++j)
    {
        D[i][j] = min(min(D[i - 1][j] + del[X[i]], D[i][j - 1] + ins[Y[i]]), D[i - 1][j - 1] + sub[X[i], Y[i]]);
    }
}

//返回:
return D[n][m];
```