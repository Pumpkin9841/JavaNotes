---
title: PAT B1030 完美数列
date: 2021-01-04 18:36:23.0
updated: 2021-02-20 15:58:06.57
url: https://pumpkn.xyz/archives/patb1030wan-mei-shu-lie
categories: 算法
tags: 算法
---

# 题目
![PATB1030.jpg](https://pumpkn.xyz/upload/2021/02/PATB1030-7dd583a5b75a4277b8b8e0130a17dd70.jpg)
#代码
```c++
#include<cstdio>
#include<algorithm>
using namespace std ;

int main(){
    int N , p , a[100010] ;
    scanf("%d %d" , &N ,&p) ;
    for(int i = 0 ; i < N ; i++){
        scanf("%d" , &a[i]) ;
    }

    sort(a ,a+N ) ;

    int ret = 1 ;
    for(int i = 0 ; i<N ; i++){
        int j = upper_bound(a+i+1 , a+N , (long long)a[i]*p) - a ;
        ret = max(ret , j-i ) ;
    }
    printf("%d" ,ret) ;

    return 0 ;
}

```