---
title: PTA A1063 Set Similarity
date: 2021-07-19 16:44:19.77
updated: 2021-07-19 16:44:19.77
url: https://pumpkn.xyz/archives/ptaa1063setsimilarity
categories: 算法
tags: 算法 | STL | set
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805409175420928)
![image.png](https://pumpkn.xyz/upload/2021/07/image-5097fcf32cc348449e13f2bbe1078b6a.png)
# 题意
给出N个集合，给出的集合中可能含有相同的值。然后要求M个查询，每个查询给出两个集合的编号x,y,求集合x和y的相同元素率，即将两个集合的交集与并集(均需去重)的元素个数的比率。
# 题解
设置N个set集合，在读入时将元素放入对应的set中，这样就可以消除同一个集合中的相同元素。之后对每一个查询(查询集合编号为x和y)，设置两个int型变量nt和nc(初始值分别为y集合元素个数和0)，分别表示不同元素个数（并集）与相同元素个数（交集）。然后枚举x集合中的元素，判断是否在集合y中出现，若出现则交集个数nc++，若没有出现，则意味出现了新元素，并集个数加1即nt++。当遍历完x集合后，计算nc*100.0/nt即为所求比率。
# 代码

```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 55 ;

int main(){
    set< int > inputs[maxn] ;
    int n ;
    cin >> n ;
    for( int i = 1 ; i <= n ; i++ ){
        int temp ;
        cin >> temp ;
        for( int j = 0 ; j < temp ; j++ ){
            int x ;
            cin >> x ;
            inputs[i].insert(x) ;
        }
    }


    int k ;
    cin >> k ;
    for( int i = 0 ; i < k ; i++ ){
        int cmp1 , cmp2 ;
        cin >> cmp1 >> cmp2 ;
        int nt = inputs[cmp2].size() ;
        int nc = 0 ;
        for( auto it = inputs[cmp1].begin() ; it != inputs[cmp1].end() ; it++ ){
           int temp = *it ;
           //没找到，则发现新元素，并集个数加1
           if( inputs[cmp2].find(temp) == inputs[cmp2].end() ){
                nt ++ ;
           }
           else{
                nc ++ ;
           }
        }

        double similarity = nc*100.0/nt ;
        printf("%.1f%\n",similarity) ;
    }





    return 0 ;
}

```