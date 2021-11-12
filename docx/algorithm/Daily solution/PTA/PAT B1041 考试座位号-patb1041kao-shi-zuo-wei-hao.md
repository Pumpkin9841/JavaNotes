---
title: PAT B1041 考试座位号
date: 2021-01-21 22:03:42.005
updated: 2021-02-24 22:03:46.759
url: https://pumpkn.xyz/archives/patb1041kao-shi-zuo-wei-hao
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805281567916032)
![image.png](https://pumpkn.xyz/upload/2021/02/image-63658789e3df4b86b481c5a1de63bb59.png)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

struct Student{
    char ID[16] ;
    int shiji ;
    int kaoshi ;
}stu[1010];

int main(){
    int n ;
    cin >> n ;
    for(int i = 0 ; i < n ; i++ ){
        for(int j = 0 ; j < 16 ; j++){
            cin >> stu[i].ID[j] ;
        }
        cin >> stu[i].shiji >> stu[i].kaoshi ;
    }

    int queryNum ;
    cin >> queryNum ;
    int queryShiji[queryNum] ;
    for(int i = 0 ; i < queryNum ; i++){
        cin >> queryShiji[i] ;
    }

    for(int i = 0 ; i < queryNum ; i++){
        for(int j = 0 ; j < n ; j++){
            if(queryShiji[i] == stu[j].shiji){
               for(int x = 0 ; x<16 ; x++ ){
                    printf("%c" ,stu[j].ID[x]) ;
               }
                printf(" %d" , stu[j].kaoshi) ;
            }
        }
        printf("\n") ;
    }

    return 0 ;
}

```