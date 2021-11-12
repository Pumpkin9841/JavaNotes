---
title: PAT B1066 图像过滤
date: 2021-01-26 22:15:49.391
updated: 2021-02-25 22:16:10.321
url: https://pumpkn.xyz/archives/patb1066tu-xiang-guo-lv
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805266514558976)
![image.png](https://pumpkn.xyz/upload/2021/02/image-5aea940dfce34a0fbf5c15d5abdeb2a7.png)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    //分辨率m*n的图像，灰度值区间[l,r] ， 替换值rep
    int m , n , l , r , rep ;
    cin >> m >> n >> l >> r >> rep ;

    int pic[m][n] ;

    for(int i = 0 ; i < m ; i++){
        for(int j = 0 ; j < n ; j++){
            cin >> pic[i][j] ;
            if( pic[i][j] <= r && pic[i][j] >= l ){
                pic[i][j] = rep ;
            }
        }
    }

    for(int i = 0 ; i < m ; i++){
        for(int j = 0 ; j < n ; j++){
            printf("%03d" ,pic[i][j]) ;
            if( j < n-1){
                printf(" ");
            }
        }
        printf("\n") ;
    }



    return 0 ;
}

```