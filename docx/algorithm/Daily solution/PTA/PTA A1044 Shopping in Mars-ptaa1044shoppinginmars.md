---
title: PTA A1044 Shopping in Mars
date: 2021-07-10 19:25:55.587
updated: 2021-07-10 20:15:20.49
url: https://pumpkn.xyz/archives/ptaa1044shoppinginmars
categories: 算法
tags: 学习 | 积累 | 算法 | 二分
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805439202443264)
![image.png](https://pumpkn.xyz/upload/2021/07/image-5da57ca8b0714c9eac0c6c8b60e184d1.png)
# 题意
给出一个数字序列与一个数字m，在数字序列中求出所有和值为m的连续子序列(左端点小的先输出，左端点相同时右端点小的先输出)。若没有这样的序列，求出和值恰好大于m的子序列（即在所有和值大于m的子序列中和值最接近m）。假设下标从1开始。

# 题解
令sum[i]表示a[1]+a[2]+...a[i]的和,可知sum[]递增。这样做的好处是在求连续子序列a[i]+a[i+1]+...a[j]的和时只需将sum[j]-sum[i-1]即可得到。例如原序列A为{1,2,3,4,5}时，对应的sum数组为{1,3,6,10,15}。此时如果要求A[3]到A[5]的和值，则只要计算sum[5]-sum[3-1]=15-3=12。</br>
数组sum严格单调递增，那就可以使用<font color = "red">二分法</font>来做这道题。假设需要在序列A[1]~A[n]中寻找和值为m的连续子序列，可以枚举左端点(1<=i<=n)，然后再数组sum的[i,n]范围内查找值为<font color="red">sum[i-1]+m的元素（由sum[j]-sum[i-1]=m推得）</font>是否存在：如果存在，则把对应的下标作为右端点j；如果不存在，找到第一个使和值超过m的右端点j。
# 代码

```c++
#include<bits/stdc++.h>
using namespace std ;

const int Max = 100010 ;
int sum[Max] ;

//初始化数组
void initSum( int sum[] ){
    for( int i = 0 ; i < Max ; i++ ){
        sum[i] = 0 ;
    }
}

int upper_Bound( int left , int right , int x ){
    int mid ;
    while( left < right ){
        mid = ( left + right ) / 2 ;
        if( sum[mid] > x ){
            right = mid ;
        }
        else{
            left = mid+1 ;
        }
    }
    return left ;
}


int main(){
    int n , m , nearM = 100000010;
    int a[Max] ;
    scanf("%d %d" , &n , &m) ;
    for( int i = 1 ; i <= n ; i++ ){
        scanf("%d" , &sum[i]) ;
        sum[i] += sum[i-1] ;
    }

    //求出每一个sum[i];  sum[i] = a[1] + a[2] + ... a[i]
    //该题这么做会导致超时
//    for( int i = 1 ; i <= n; i++ ){
//        for( int j = 1 ; j<= i ; j++ ){
//            sum[i] += a[j] ;
//        }
//    }




    for( int i = 1 ; i <= n ; i++ ){
        //a[2]+a[3]+a[4]的和可由sum[4]-sum[1]得到，sum[i-1]+m由sum[j]-sum[i-1]=m推得
        int j = upper_Bound( i , n+1 , sum[i-1]+m ) ;
        if( sum[j-1] - sum[i-1] == m ){
            nearM = m ;
            break ;
        }
        else if( j <= n && sum[j] - sum[i-1] < nearM ){
            nearM = sum[j] - sum[i-1] ;
        }

    }

    for( int i = 1 ; i <= n ; i++ ){
        int j = upper_Bound( i , n+1 , sum[i-1]+nearM ) ;
        if( sum[j-1] - sum[i-1] == nearM ){
            printf("%d-%d\n" , i , j-1) ;
        }
    }
    return 0 ;
}

```