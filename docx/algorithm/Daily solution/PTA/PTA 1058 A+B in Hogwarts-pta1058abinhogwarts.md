---
title: PTA 1058 A+B in Hogwarts
date: 2021-05-30 23:02:35.811
updated: 2021-05-30 23:04:20.115
url: https://pumpkn.xyz/archives/pta1058abinhogwarts
categories: 算法
tags: 算法 | 进制转换
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805416519647232)
![image.png](https://pumpkn.xyz/upload/2021/05/image-4c83f6d5d85c4789b30b43ecc773684a.png)
# 题解
先将A+B转化为Knut形式并求和sum，再将sum转化为Galleon.Sickle.Knut格式即可。
# 注意
A+B的和sum可能超出int范围，使用long long类型。</br>

<font color = 'red'> ```%lld``` **为scanf或printf时long long 类型的通配符**  </font>
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int Galleon = 17*29 ;
const int Sickle = 29 ;

int main(){

    long long a1 , b1 , c1 ;
    long long a2 , b2 , c2 ;
    scanf("%lld.%lld.%lld %lld.%lld.%lld" , &a1 , &b1 , &c1 , &a2 , &b2 , &c2) ;

    long long sum = ( a1+a2 )*Galleon + ( b1+b2 )*Sickle + c1 + c2 ;

    printf("%lld.%lld.%lld" , sum / Galleon , sum % Galleon / Sickle , sum % Sickle) ;


    return 0 ;
}

```