---
title: PAT A1020 Tree Traversals
date: 2021-02-23 17:08:18.291
updated: 2021-02-23 17:09:18.055
url: https://pumpkn.xyz/archives/pata1020treetraversals
categories: 算法
tags: 树 | 二叉树
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805485033603072)
![PAT A1020.jpg](https://pumpkn.xyz/upload/2021/02/PAT%20A1020-cb571169cfa54d168286aa08ff9286be.jpg)
# 题意
给出一棵二叉树的后序遍历和中序遍历，求这棵二叉树的层序遍历。
# 代码
```C++
#include<bits/stdc++.h>
using namespace std ;

int posOrder[50] , inOrder[50] ;
int n ;
struct node{
    int data ;
    node* lchild ;
    node* rchild ;
};

//后序遍历区间为[posL,posR] ,中序遍历区间为[inL , inR]
node* create(int posL , int posR , int inL , int inR){
    //如果后续遍历区间 小于等于0 ，直接返回
    if(posL > posR){
        return NULL;
    }
    //新建一个节点，用来存放二叉树的根节点
    node* root = new node ;
    root->data = posOrder[posR] ;
    int k ;
    for(int i = inL ; i <= inR ; i++){
        if( posOrder[posR] == inOrder[i]){
            k = i ;
            break ;
        }
    }
    //左子树的节点个数
    int leftNum = k - inL ;

    root->lchild = create(posL , posL + leftNum - 1 , inL , k-1) ;

    root->rchild = create(posL + leftNum , posR-1 , k+1 , inR ) ;

    return root ;

}


int num = 0 ;
void layOrder(node* root){
   queue<node*> q ;
   q.push(root) ;
   while(!q.empty()){
    node* now = q.front() ;
    q.pop() ;
    cout << now->data  ;
    num++ ;
    if( num < n ){
        cout <<" " ;
    }
    if( now->lchild != NULL){
        q.push(now->lchild) ;
    }
    if( now->rchild != NULL){
        q.push(now->rchild) ;
    }
   }
}

int main(){

    cin >> n ;
    for(int i = 0 ; i < n ; i++ ){
        cin >> posOrder[i] ;
    }

    for(int i = 0 ; i < n ; i ++ ){
        cin >> inOrder[i] ;
    }

    layOrder(create(0,n-1,0,n-1)) ;

    return 0 ;
}


```