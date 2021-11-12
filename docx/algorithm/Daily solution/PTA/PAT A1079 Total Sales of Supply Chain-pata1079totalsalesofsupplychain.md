---
title: PAT A1079 Total Sales of Supply Chain
date: 2021-02-27 22:33:53.136
updated: 2021-03-07 22:36:35.864
url: https://pumpkn.xyz/archives/pata1079totalsalesofsupplychain
categories: 算法
tags: 树
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805388447170560)
![image.png](https://pumpkn.xyz/upload/2021/03/image-19b4314265bb4958aab10e4f230ac88e.png)
## 题意
给出一棵销售供应的树，树根唯一。在树根处货物的价格为P，然后从根结点开始每往子结点走一层，该层的货物价格将会在父亲结点的价格上增加%。给出每个叶结点的货物量，求出它们的价格和。
## 样例构成的树
![image.png](https://pumpkn.xyz/upload/2021/03/image-21c83d6c254e4079a00866a42cfc67b1.png)
# 题解
题目简单，套用DFS解法即可。</br>
值得一提的是，double型的输出 %.1f，有保留小数点后1位，且小数点后四舍五入的效果。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 100010 ;
//总结点数n
int n ;
//起始价格p ,价格增长百分比r
double p , r , res = 0.0;

struct node{
    int lay  ;   //层数
    int data ;
    vector<int >  child ;
}node[maxn];

//层序遍历
void layw( int root){

    queue<int> q ;
    node[root].lay = 0 ;
    q.push(root) ;

    while( !q.empty() ){
        int top = q.front() ;
        q.pop() ;
        for(int i = 0 ; i < node[top].child.size() ; i++){
            int child = node[top].child[i] ;
            node[child].lay = node[top].lay + 1 ;
            q.push(child) ;
        }
    }


}


//先序遍历
void preOrder(int root){

    if( root > n ){
        return ;
    }

    if(node[root].child.size() == 0){
        double increase = p*( pow(r/100 + 1 , node[root].lay) )*node[root].data ;
        res = res + increase ;
   }



   for(int i = 0 ; i < node[root].child.size() ; i++){
        preOrder(node[root].child[i]) ;
   }

}


int main(){
    cin >> n >> p >> r ;

    for(int i = 0 ; i < n ; i++ ){
        int num ;
        cin >> num ;
        if(num == 0){
            int stor ;
            cin >> stor ;
            node[i].data = stor ;
        }else{
            for( int j = 0 ; j < num ; j++ ){
               int tmp ;
               cin >> tmp ;
               node[i].child.push_back(tmp) ;
            }
        }
    }

    layw(0) ;

    preOrder(0) ;



    printf("%.1f" , res) ;


    return 0 ;
}

```