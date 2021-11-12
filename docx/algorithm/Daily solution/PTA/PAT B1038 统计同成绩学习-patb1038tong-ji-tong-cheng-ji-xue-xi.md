---
title: PAT B1038 统计同成绩学习
date: 2021-01-09 14:48:33.338
updated: 2021-02-21 12:36:03.201
url: https://pumpkn.xyz/archives/patb1038tong-ji-tong-cheng-ji-xue-xi
categories: 算法
tags: 积累 | 算法
---

# 题目
![PATB1038.jpg](https://pumpkn.xyz/upload/2021/02/PATB1038-0e6014937c064cef987287b2a223c52f.jpg)
# 代码
```c++
#include<cstdio>
#include<string>
#include<algorithm>
#include<cstring>

int main(){

    int N , K ;
    int grade[110]= {0} ;
    int score ;
    int searchGrade[K] ;
    scanf("%d" , &N) ;
    for(int i = 0 ; i < N ; i++){
        scanf("%d", &score) ;
        grade[score]++ ;
    }

    scanf("%d" , &K) ;
    for(int i = 0 ; i < K ; i++){
        scanf("%d" , &score) ;
        printf("%d" , grade[score]) ;
        if(i < K-1){
            printf(" ") ;
        }
    }
}

```