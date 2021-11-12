---
title: PAT B1051 复数乘法
date: 2021-01-24 21:02:10.286
updated: 2021-02-25 21:02:12.777
url: https://pumpkn.xyz/archives/patb1051fu-shu-cheng-fa
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805274496319488)
![image.png](https://pumpkn.xyz/upload/2021/02/image-8b6ed7c30da1453bb9fbd97570ff236c.png)
# 代码
```c++
#include <stdio.h>
#include <math.h>

int main()
{
    double R1, P1, R2, P2, a, b;
    int check_a = 1, check_b = 2;
    scanf("%lf %lf %lf %lf", &R1, &P1, &R2, &P2);
    a = R1 * R2 * cos(P1 + P2);
    b = R1 * R2 * sin(P1 + P2); // 计算常规形式的两个系数a和b
    if (fabs(a) < 0.01) check_a = 0;
    if (fabs(b) < 0.01) check_b = 0; // 检查a和b是否为0

    if (check_a == 0 && check_b == 0) // a和b同时为0的情况，只输出一个0
        printf("0");
    else if (check_a == 0 && check_b != 0) // 其余情况实部和虚部均要输出保留两位小数的数字，包括0
        printf("0.00%+.2fi", b);
    else if (check_a != 0 && check_b == 0)
        printf("%.2f+0.00i", a);
    else
        printf("%.2f%+.2fi",a, b);

    return 0;
}
```