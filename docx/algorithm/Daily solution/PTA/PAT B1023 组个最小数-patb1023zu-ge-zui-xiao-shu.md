---
title: PAT B1023 组个最小数
date: 2021-01-02 18:31:03.019
updated: 2021-02-20 15:58:32.633
url: https://pumpkn.xyz/archives/patb1023zu-ge-zui-xiao-shu
categories: 算法
tags: 算法
---

# 题目
![PATB1023.jpg](https://pumpkn.xyz/upload/2021/02/PATB1023-6feadec65a404c9e9a4b6ae209a323ca.jpg)
# 代码
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std ;

bool cmp(int a , int b){
    return a < b ;
}


int main(){
    int a[15] ;
    int ret[80] ;
    int n = 0 ;
    for(int i = 0 ; i < 10 ; i ++){
        scanf("%d" ,&a[i]) ;
    }
    for(int i = 0 ; i < 10 ; i++){
        int tmp = a[i] ;
        for(int j = 0 ; j < tmp ; j++){
            ret[n++] = i ;
        }
    }

    int notZero ;   //第一个不为0的数的下标
    for(int i = 0 ; i < n ; i ++){
        if(ret[i] != 0){
            notZero = i ;
            break ;
        }
    }

    if(notZero == 0){
        for(int i = 0 ; i < n ; i++){
            printf("%d" , ret[i]) ;
        }
    }else{
        swap(ret[0] , ret[notZero]) ;
        for(int i = 0 ; i < n ; i++){
            printf("%d" , ret[i]) ;
        }
    }

//     int temple = ret[0] ;
//        ret[0] = ret[notZero] ;
//        ret[notZero] = temple ;

    return 0 ;
}

```