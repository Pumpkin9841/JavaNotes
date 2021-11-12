---
title: PAT B1076 WIFI密码
date: 2021-01-28 22:08:20.0
updated: 2021-03-03 22:09:34.398
url: https://pumpkn.xyz/archives/patb1076wifi-mi-ma
categories: 算法
tags: 算法
---

# 题目
##  [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805262622244864)
![image.png](https://pumpkn.xyz/upload/2021/03/image-5cefc840b58d45b88dcbd50c7885c1a4.png)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int n ;
    vector<int > res ;
    map<char ,int>mp ;
    mp['A'] = 1 ;
    mp['B'] = 2 ;
    mp['C'] = 3 ;
    mp['D'] = 4 ;
    cin >> n ;
    getchar() ;
    for(int i = 0 ; i < n ; i++){
        for(int j = 0 ; j < 4 ; j++){
            char que , ans ;
            scanf("%c-%c" ,&que , &ans) ;
            getchar() ;
            if( ans == 'T' ){
                res.push_back(mp[que]) ;
            }
        }
    }

    for(int i = 0 ; i < res.size() ; i++){
        cout << res[i] ;
    }

    return 0 ;
}

```