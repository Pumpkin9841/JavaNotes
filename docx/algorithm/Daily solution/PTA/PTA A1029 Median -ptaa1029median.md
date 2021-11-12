---
title: PTA A1029 Median 
date: 2021-07-11 21:50:48.799
updated: 2021-07-11 21:50:48.799
url: https://pumpkn.xyz/archives/ptaa1029median
categories: 算法
tags: 学习 | 算法 | 双指针
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805466364755968)
![](https://pumpkn.xyz/upload/2021/07/image-66007426f53d407cbb7f6a9fe8b76c0b.png)
# 题意
给出两个升序序列，将两个序列合并为一个序列，再求出序列的中位数。偶数个数的中位数取下限。即若序列的大小为n，中位数下标为 mid = (n-1)/2。
# 题解
很容易想到该题使用双指针的思想，将两个有序的序列合并为一个升序序列。代码模板为：
```c++
    /*
        vector<int> a 为升序序列1
        vector<int> b 为升序序列2
        int L1 , int R1 为序列1的取值区间[L1,R1]
        int R1 , int R2 为序列2的取值区间[L2,R2]
    */
    vector<int> Merge(vector<int> a , vector<int> b , int L1 , int R1 , int R1 , int R2 ){
        int i = L1 ;
        int j = L2 ;
        vector<int> ans ; //暂存合并后的序列
        //只要其中一个序列还没有遍历结束
        while( i <= R1 && j <= R2 ){
            if( a[i] <= b[j] ){
                ans.push_back(a[i++]) ;
            }
            else{
                ans.push_back(b[j++]) ;
            }
        }
        //如果序列a还没完，将剩余的加到ans上
        while( i <= R1 ){
             ans.push_back(a[i++]) ;
        }
        //如果序列b还没完，将剩余的加到ans上
        while( j <= R2 ){
             ans.push_back(b[j++]) ;
        }
        return ans ;
    }
```
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

typedef long long LL ;

vector<LL> Merge( vector<LL> a , vector<LL> b ,int L1 , int R1 , int L2 , int R2  ){
    int i = L1 ;
    int j = L2 ;
    vector<LL> ans ;
    while( i <= R1 && j <= R2 ){
        if( a[i] <= b[j] ){
            ans.push_back(a[i++]) ;
        }
        else{
            ans.push_back(b[j++]) ;
        }
    }
    while( i <= R1 ){
        ans.push_back(a[i++]) ;
    }
    while( j <= R2 ){
        ans.push_back(b[j++]) ;
    }
    return ans ;
}


int main(){
    int n , m ;
    cin >> n ;
    vector<LL> S1 ;
    for( int i = 0 ; i < n ; i++ ){
        LL temp ;
        cin >> temp ;
        S1.push_back(temp) ;
    }

    cin >> m ;
    vector<LL> S2 ;
    for( int i = 0 ; i < m ; i++ ){
        LL temp ;
        cin >> temp ;
        S2.push_back(temp) ;
    }
    vector<LL> ans ;
    ans = Merge(S1 , S2 , 0 , S1.size()-1 , 0, S2.size()-1 ) ;
    int mid = (ans.size()-1)/2 ;
    cout << ans[mid] ;

    return 0 ;
}

```