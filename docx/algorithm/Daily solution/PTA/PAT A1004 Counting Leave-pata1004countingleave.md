---
title: PAT A1004 Counting Leave
date: 2021-03-03 21:18:05.601
updated: 2021-03-08 21:18:40.662
url: https://pumpkn.xyz/archives/pata1004countingleave
categories: 算法
tags: 算法 | DFS  | BFS    | 树
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805521431773184)
![image.png](https://pumpkn.xyz/upload/2021/03/image-c08cff17c502440794b25186e88beffc.png)
# 题意
给出一棵树，问每一层各有多少叶子结点。
# 题解
简单题，可用DFS思想也可用BFS。</br>
我采用的DFS思想，求出每个叶子节点的深度，每遇到一个深度为deepth的叶子节点，就将数组leaf[deepth]+1。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 110 ;

//n个节点，m个非叶子节点
int n , m ;

//第i层有叶子节点leaf[i]个
int leaf[maxn] = {0} ;
int maxDeepth = 0 ;

struct Node{
    vector<int > child ;
}node[maxn];


void DFS( int root , int deepth ){

    if( node[root].child.size() == 0 ){
        leaf[deepth] ++ ;
    }

    if( deepth > maxDeepth ){
        maxDeepth = deepth ;
    }

    for( int i = 0 ; i < node[root].child.size() ; i++ ){
        DFS( node[root].child[i] , deepth+1 ) ;
    }

}


int main(){
    cin >> n >> m ;
    for( int i = 1 ; i <= m ; i++ ){
        int id , num ;
        cin >> id >> num ;
        for( int j = 0 ; j < num ; j++ ){
            int tmp ;
            cin >> tmp ;
            node[id].child.push_back(tmp) ;
        }
    }

    DFS( 1 , 1 ) ;

    for( int i = 1 ; i <= maxDeepth ; i++ ){
        cout << leaf[i] ;
        if( i < maxDeepth ){
            cout << " " ;
        }
    }

    return 0 ;
}


```