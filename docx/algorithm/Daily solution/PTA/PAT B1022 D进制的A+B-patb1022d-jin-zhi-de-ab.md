---
title: PAT B1022 D进制的A+B
date: 2021-01-01 15:25:06.809
updated: 2021-02-20 15:58:41.022
url: https://pumpkn.xyz/archives/patb1022d-jin-zhi-de-ab
categories: 算法
tags: 积累 | 算法
---

# 题目
![PATB1022.jpg](https://pumpkn.xyz/upload/2021/02/PATB1022-d146b5b809c64018a08b73ec0d639a83.jpg)
# 代码
```c++
#include<cstdio>
int main(){
    int A , B , D = 0 ;
    int sum = 0 ;
    scanf("%d%d%d" , &A , &B ,&D) ;
    sum = A + B ;
    int num = 0 ;
    int ans[31] ;
    do{
        ans[num++] = sum % D ;
        sum /= D ;
    }while(sum != 0) ;
    for(int i = num - 1 ; i>=0 ; i--){
        printf("%d" ,ans[i]) ;
    }

    return 0 ;
}

```