---
title: PAT B1091 N-自守数
date: 2021-01-31 23:10:01.327
updated: 2021-03-03 23:10:55.198
url: https://pumpkn.xyz/archives/patb1091n--zi-shou-shu
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/1071785664454127616)
![image.png](https://pumpkn.xyz/upload/2021/03/image-6592fc8889e34967a03edabe930434e2.png)

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int m ;
    cin >> m ;
    for(int i = 0 ; i < m ; i++){
        int tmp ;
        cin >> tmp ;

        stringstream ss ;
        ss << tmp ;
        string strtmp = ss.str() ;

        int j ;
        for(j = 1 ; j < 10 ; j++){
            int ans = tmp*tmp*j ;
            ss << ans ;
            string strans = ss.str() ;
            strans = strans.substr(strans.length()-strtmp.length() , strans.length()-1) ;
            if( strtmp == strans){
                cout << j << " " << ans <<endl ;
                break ;
            }

        }

        if( j == 10){
                cout << "No" <<endl ;
        }

    }

    return 0 ;
}

```