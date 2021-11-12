---
title: PAT B1026 程序运行时间
date: 2021-01-19 13:00:33.499
updated: 2021-02-24 13:00:53.612
url: https://pumpkn.xyz/archives/patb1026cheng-xu-yun-xing-shi-jian
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805295203598336)
![image.png](https://pumpkn.xyz/upload/2021/02/image-c90ed630f44d47a1964084b53d5922e2.png)

# 题解
|函数名称|函数说明|
|-------|-------|
|floor()|不大于自变量的最大整数|
|ceil()|不小于自变量的最大整数|
|round()|四舍五入到最邻近的整数|

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int CLK = 100 ;

int main(){
    double c1 , c2 ;
    cin >> c1 >> c2 ;
    //round() 四舍五入到临近的整数
    double c3 = (c2 - c1)/CLK ;


    int hours = c3/3600 ;
    int minute = (c3 - (hours*3600))/60 ;
    int second = round(c3 - (hours*3600) - (minute*60)) ;
    printf("%02d:%02d:%02d" , hours , minute , second) ;

    return 0 ;
}

```