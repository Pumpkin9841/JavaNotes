---
title: PTA A1048 Find Coins
date: 2021-06-08 23:53:56.757
updated: 2021-06-08 23:53:56.757
url: https://pumpkn.xyz/archives/ptaa1048findcoins
categories: 算法
tags: 算法 | map   
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805432256675840)
![image.png](https://pumpkn.xyz/upload/2021/06/image-b0dffd65cd4548da8ccb45d2978b2181.png)

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int n , m ;
    cin >> n >> m ;
    vector<int> coins ;

    for( int i = 0 ; i < n ; i++ ){
        int tmp ;
        cin >> tmp ;
        coins.push_back(tmp) ;
    }
    sort(coins.begin() , coins.end() ) ;
    vector<int> res ;
    for( int i = 0 ; i < n ; i++ ){
        int value1 = coins[i] ;
        int value2 = m - value1 ;
        for( int j = i+1 ; j < n ; j++ ){
            if( coins[j] == value2 ){
                cout << value1 << " " <<value2 ;
                return 0 ;
            }
        }
    }

    cout <<"No Solution" ;

    return 0 ;
}

```