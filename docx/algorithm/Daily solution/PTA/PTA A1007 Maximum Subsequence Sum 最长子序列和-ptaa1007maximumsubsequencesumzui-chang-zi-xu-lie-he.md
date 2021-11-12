---
title: PTA A1007 Maximum Subsequence Sum 最长子序列和
date: 2021-08-05 15:59:18.95
updated: 2021-08-05 15:59:18.95
url: https://pumpkn.xyz/archives/ptaa1007maximumsubsequencesumzui-chang-zi-xu-lie-he
categories: 算法
tags: 算法 | 动态规划
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805514284679168)
![image.png](https://pumpkn.xyz/upload/2021/08/image-ec9ba8f132a44feea815f9097155a533.png)
# 代码
```C++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 10010 ;
int s[maxn] = {0} ;

int main(){
    int n ;
    cin >> n ;
    int seq[maxn] ;
    bool flag = false ;
    for( int i = 0 ; i < n ; i++ ){
        cin >> seq[i] ;
        if( seq[i] >= 0 ){
            flag = true ;
        }
    }

    if( flag == false  ){
        cout << 0 << " " <<seq[0] << " " <<seq[n-1] ;
        return 0 ;
    }

    int dp[maxn] ;
    dp[0] = seq[0] ;
    for( int i = 1 ; i < n ; i++ ){
        if( dp[i-1] + seq[i] > seq[i] ){
            dp[i] = dp[i-1] + seq[i] ;
            s[i] = s[i-1] ;
        }
        else{
            dp[i] = seq[i] ;
            s[i] = i ;
        }
    }

    int maxValue = 0 ;
    int k = 0 ;
    for( int i = 0 ; i < n ; i++ ){
        if( dp[i] > maxValue ){
            maxValue = dp[i] ;
            k = i ;
        }
    }

    int start = seq[ s[k] ] ;
    cout << maxValue  << " " << start  << " " << seq[k] ;




    return 0 ;
}

```