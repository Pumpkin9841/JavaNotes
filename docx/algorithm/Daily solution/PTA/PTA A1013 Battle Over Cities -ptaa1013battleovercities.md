---
title: PTA A1013 Battle Over Cities 
date: 2021-08-03 23:05:31.304
updated: 2021-08-03 23:27:32.346
url: https://pumpkn.xyz/archives/ptaa1013battleovercities
categories: 算法
tags: 学习 | 算法 | 图
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805500414115840)
![image.png](https://pumpkn.xyz/upload/2021/08/image-d71e80b0bb1c446e95ff74ca50d18898.png)

# 题意
给出各个城市的高速公路连接图，若有一个城市被敌人占领，则此城市不能再通过高速公路来连接其他城市，问若此城市被占领，需要添加多少条高速公路才能使剩下的城市互相连通。</br>
第一行分别输入城市个数n，现有高速公路数m，查询数k。</br>
接下来m行输入高速公路
最后一行挨个输入被占领的城市编号。

</br>

依次输出被占领城市后需要添加的高速公路数。

# 题解
其本质是给定一个无向图，当删除图中某个顶点时，连同与之连接的边也删除。求删除顶点后需增加多少条边才能使图变为连通。
</br>

- 先考虑第一个问题：给定一个无向图，如何计算需要增加的边数，使其称为连通图？

	- 假设无向图G有n个连通块b1,b2,...,bn,那么只需要在b1与b2之间添加一条边，b2与b3之间添加一条边....依次类推，即可使整个图连通，且这种做法添加的边数一定是最少的。
	- <span color = "red"><font color="red">显然，需要添加的边数等于连通块个数-1</font></span>

- 此时问题转化为求一个无向图G的连通块个数。

	- 图的遍历：图的遍历过程中总是每次访问单个连通块，并将该连通块内的所有顶点标记已访问，然后去访问下个连通块，因此可以在访问过程中同时计数遍历的连通块个数，就能得到需要添加的边数。

	 



# 代码
```C++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 1010 ;

int n , m , k;
vector<int> G[maxn] ;
bool vis[maxn] ;
int currentOccupCity ; //当前被占领的城市
int block ;  //记录连通块数

void DFS( int u  ){
    if( u == currentOccupCity ){ //当前城市为被占领的城市
        return ; //直接返回
    }
    vis[u] = true ;
    for( int i = 0 ; i < G[u].size() ; i++ ){
        int v = G[u][i] ;
        if( vis[v] == false ){
            DFS(v) ;
        }

    }
}

void DFSTrave(){
    for( int u = 1 ; u <= n ; u++ ){
        if( u != currentOccupCity && vis[u] == false ){  //城市u未被占领且未被访问
            DFS(u) ;
            block ++ ;
        }
    }
}

int main(){
    cin >> n >> m >> k ;

    for( int i = 0 ; i < m ; i++ ){
        int a , b ;
        cin >> a >> b ;
        //无向图
        G[a].push_back(b) ; // 边a->b
        G[b].push_back(a) ; //边b->a
    }

    for( int i = 0 ; i < k ; i++ ){
        int temp ;
        cin >> temp ;
        currentOccupCity = temp ;
        memset(vis , false , sizeof(vis)) ;
        block = 0 ;
        DFSTrave() ;
        cout << block - 1 << endl ;
    }


    return 0 ;
}

```