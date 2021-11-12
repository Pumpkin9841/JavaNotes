---
title: PATB 1015 德才论
date: 2020-12-30 15:18:45.736
updated: 2021-02-20 15:59:02.294
url: https://pumpkn.xyz/archives/patb1015de-cai-lun
categories: 算法
tags: 积累 | 算法
---

# 题目
![PATB1015.png](https://pumpkn.xyz/upload/2021/02/PATB1015-848615d7ab9a40c9aadadf256d1feb72.png)
# 代码
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std ;
struct Student{
    char id[10] ;
    int de , cai ,sum ;
    int flag ;   //考生类别
}stu[100010];

bool cmp(Student a , Student b ){
    if(a.flag != b.flag){
        return a.flag < b.flag ;
    }else if(a.sum != b.sum){
        return a.sum > b.sum ;
    }else if(a.de != b.de){
        return a.de > b.de ;
    }else{
        return strcmp(a.id , b.id) < 0 ;
    }
}

int main(){
    int n ,L ,H ;
    scanf("%d%d%d",&n,&L,&H) ;
    int m = n ;   //记录达标的总人数
    for(int i = 0 ; i < n ; i++){
        scanf("%s%d%d" , stu[i].id , &stu[i].de , &stu[i].cai) ;
        stu[i].sum = stu[i].de + stu[i].cai ;
        if(stu[i].de < L || stu[i].cai < L){
            stu[i].flag = 5 ;
            m-- ;
        }else if(stu[i].de >= H && stu[i].cai>=H)
            stu[i].flag = 1 ;
        else if(stu[i].de >= H && stu[i].cai < H)
            stu[i].flag = 2 ;
        else if(stu[i].de >= stu[i].cai)
            stu[i].flag =3 ;
        else
            stu[i].flag= 4 ;
    }
    sort(stu , stu+n ,cmp) ;
    printf("%d\n" , m) ;
    for(int i = 0 ; i < m ; i++){
        printf("%s %d %d\n" , stu[i].id , stu[i].de , stu[i].cai) ;
    }

    return 0 ;
}

```