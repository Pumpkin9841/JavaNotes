---
title: PAT B1047 编程团体赛
date: 2021-01-14 19:03:12.917
updated: 2021-02-20 15:55:41.609
url: https://pumpkn.xyz/archives/patb1047bian-cheng-tuan-ti-sai
categories: 算法
tags: 积累 | 算法
---

# 题目
![PATB1047.jpg](https://pumpkn.xyz/upload/2021/02/PATB1047-a92e574ad5bb4d19be6a667648c964b4.jpg)
# 代码
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std ;

int main(){
    int N = 0 ;
    int team = 0 ;
    int num = 0 ;
    int score = 0 ;
    int HashTable[1010] = {0} ;
    scanf("%d" , &N) ;
    while(N > 0 ){
        scanf("%d-%d %d" , &team , &num , &score) ;
        HashTable[team] += score ;
        N -- ;
    }
    int maxScore = 0 ;
    int champin = 0 ;
    for(int i = 0 ; i < 1010 ; i++){
        if(HashTable[i] > maxScore){
            maxScore = HashTable[i] ;
            champin = i ;
        }
    }

    printf("%d %d" , champin ,maxScore ) ;


    return 0 ;
}

```