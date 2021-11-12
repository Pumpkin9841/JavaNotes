---
title: PAT A1094 The Largest Generation
date: 2021-03-01 18:22:49.887
updated: 2021-03-08 18:23:18.541
url: https://pumpkn.xyz/archives/pata1094thelargestgeneration
categories: 算法
tags: DFS  | BFS    | 树
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805372601090048)
![image.png](https://pumpkn.xyz/upload/2021/03/image-220badf563d64c439482ec943a2a9c03.png)
# 题意
输入树的结点个数N（结点编号为1-N）、非叶子结点个数M，然后输入M个非叶子结点各自的孩子结点编号，求结点个数最多的一层（层号是从整体来看的，根结点层号为1），输出该层的结点个数以及层号。

# 样例解释
显然第4层有9个节点，是最多的
![image.png](https://pumpkn.xyz/upload/2021/03/image-a56341b31b8f406999066e893ffeaa6a.png)

# 题解
题意很明确，题目也相当简单，先通过层序遍历对节点的层号进行求解，再通过先序遍历将每一代有多少人存入数组generation[maxn]中，下标表示层数也即是哪一代人，值表示人数。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 110 ;

struct Node{
    int layer ;  //表示第几代人
    vector<int > child ;
}node[maxn];

//记录第i代人共有generation[i]人
int generation[maxn] = {0};

//一个家族n个人，有m个人有孩子
int n , m ;

//层序遍历并确认generation
void lay( int root ){
    queue<int > q ;
    node[root].layer = 1 ;
    q.push(root) ;
    while( !q.empty() ){
        int top = q.front() ;
        q.pop() ;
        for( int i = 0 ; i < node[top].child.size() ; i++ ){
            int child = node[top].child[i] ;
            node[child].layer = node[top].layer + 1 ;
            q.push(child) ;
        }

    }

}

//先序遍历
void preOrder(int root){
    generation[ node[root].layer ] ++ ;

    for(int i = 0 ; i < node[root].child.size() ; i++){
        preOrder( node[root].child[i] ) ;
    }

}




int main(){
    cin >> n >> m ;
    for(int i = 1 ; i <= m ; i++ ){
        int id , num ;
        cin >> id >> num ;
        for( int j = 0 ; j < num ; j++ ){
            int tmp ;
            cin >> tmp ;
            node[id].child.push_back(tmp) ;
        }
    }

    lay(1) ;

    preOrder(1) ;

    int maxNum = 0 ;
    int gen ;

    for(int i = 0 ; i < maxn ; i++ ){
        if( generation[i] > maxNum ){
            maxNum = generation[i] ;
            gen = i ;
        }
    }

    cout << maxNum << " "  << gen ;

    return 0 ;
}

```