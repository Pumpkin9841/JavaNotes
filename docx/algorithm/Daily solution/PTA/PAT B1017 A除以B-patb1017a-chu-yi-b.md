---
title: PAT B1017 A除以B
date: 2021-02-17 23:29:34.068
updated: 2021-03-09 23:29:40.816
url: https://pumpkn.xyz/archives/patb1017a-chu-yi-b
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805305181847552)
![image.png](https://pumpkn.xyz/upload/2021/03/image-408c45da4f744c59ab3fcb92fe903872.png)

# 题解
关于大数的运算，用python最方便。

# 代码
```c++
A , B = map( int , input().split() ) ;
R = A // B ;
Q = A % B ;
print( "%d %d" % (R , Q)) ;
exit(0) ;
```