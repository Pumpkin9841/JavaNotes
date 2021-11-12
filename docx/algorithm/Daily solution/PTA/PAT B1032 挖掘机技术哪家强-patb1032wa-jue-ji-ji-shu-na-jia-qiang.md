---
title: PAT B1032 挖掘机技术哪家强
date: 2021-01-05 14:39:59.151
updated: 2021-02-20 15:57:55.837
url: https://pumpkn.xyz/archives/patb1032wa-jue-ji-ji-shu-na-jia-qiang
categories: 算法
tags: 算法
---

# 题目
![PATB1032.jpg](https://pumpkn.xyz/upload/2021/02/PATB1032-99328441053c47bcadd5e6be9a04b566.jpg)
# 代码

```c++
#include<stdio.h>
#define MAX 100000

int main(){
    int n = 0 ;
    int schScore[MAX] = {0} ;
    int schID = 0 ;
    int grade = 0 ;
    scanf("%d" ,&n) ;
    while(n>0){
        scanf("%d %d" ,&schID,&grade) ;
        schScore[schID] += grade ;
        n-- ;
    }
    int maxNum = 0 ;
    int schoolId = 0 ;
    for(int i = 0 ; i <MAX ; i++ ){
        if(schScore[i] > maxNum){
            maxNum = schScore[i] ;
            schoolId = i ;
        }
    }
    printf("%d %d" ,schoolId ,maxNum) ;

}
```