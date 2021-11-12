---
title: PAT A1106 Lowest Price in Supply Chain
date: 2021-03-02 20:07:45.0
updated: 2021-03-08 20:13:37.608
url: https://pumpkn.xyz/archives/pata1106lowestpriceinsupplychain
categories: 算法
tags: 算法 | DFS  | BFS    | 树
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805362341822464)
![image.png](https://pumpkn.xyz/upload/2021/03/image-2d2deb34af8e47b18d0d4dd76c43c9f2.png)
# 题意
给出一棵销售供应的树，树根唯一。在树根处货物的价格为P，然后从根结点开始每往子结点走一层，该层的货物价格将会在父亲结点的价格上增加r%。求叶子结点处能获得的最低价格以及能提供最低价格的叶子结点的个数。
# 样例解释
共有4个叶结点，其中4号和7号的深度为2（即从根结点扩散两层可以到达），8号和9号的深度为3，因此有两个叶子结点能得到最低价格，最低价格多1.80 x（1 + 0.01）=1.83618，保留4位小数后为1.8362。
![image.png](https://pumpkn.xyz/upload/2021/03/image-5643b000f046449c990a2439ab937b8c.png)
# 题解
题目很简单，利用DFS算出每个叶子节点的价格，在输出最小价格及其数量即可。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 100010 ;

struct Node{
    vector<int > child ;
}node[maxn];

// 每个零售商出售价格
vector<double > prices ;


int n  ;
double p ,r ;

void DFS(int root , int deepth ){

    //为零售商（叶子节点）
    if( node[root].child.size() == 0 ){
        prices.push_back( p * ( pow( 1+r/100 , deepth ) ) ) ;
    }

    for( int i = 0 ; i < node[root].child.size() ; i++ ){
        DFS( node[root].child[i] , deepth + 1 ) ;
    }

}

int main(){
    cin >> n >> p >> r ;
    for(int i = 0 ; i < n ; i++ ){
        int num ;
        cin >> num ;
        if( num != 0 ){
             for(int j = 0 ; j < num ; j++ ){
                int tmp ;
                cin >> tmp ;
                node[i].child.push_back(tmp) ;
            }
        }
    }

    //计算每个零售商的价格
    DFS(0 , 0) ;

    //升序排序
    sort( prices.begin() , prices.end() ) ;

    //记录最低价格零售商个数
    int count = 0 ;

    for( int i = 0 ; i < prices.size() ; i++ ){
        if( prices[0] == prices[i] ){
            count ++ ;
        }
    }

    printf("%.4f %d" , prices[0] , count ) ;

    return 0 ;
}

```