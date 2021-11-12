---
title: PAT A1102 Invert a Binary Tree
date: 2021-02-25 00:35:06.022
updated: 2021-03-07 00:35:40.127
url: https://pumpkn.xyz/archives/pata1102invertabinarytree
categories: 算法
tags: 树 | 二叉树
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805365537882112)
![image.png](https://pumpkn.xyz/upload/2021/03/image-b143d7a0e486476ca842c3ad373b1e07.png)
# 题意
二叉树有N个节点，给出每个节点的左右孩子节点编号，把该二叉树反转(即把每个节点的左右子树都相互交换)，输出反转后的二叉树的层序遍历和中序遍历。
![image.png](https://pumpkn.xyz/upload/2021/03/image-b6f61118f3a1421b8362625ef144713b.png)
# 题解
### 步骤一：</br>
**获取根节点**: 开一个bool型数组notRoot[maxn]，false表示为根节点，true表示不是根节点，默认值为fasle。找到一个节点，他不是任何节点的孩子，则该节点就是根节点。
### 步骤二: </br>
**反转二叉树**: <font color ="red">在后序遍历中交换左右孩子节点，则可以得到反转的二叉树 </font>
### 步骤三: </br>
依次中序遍历和层序遍历。 
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 110 ;

struct Node{
    int data ;
    int lchild , rchild ;
}node[maxn];

bool notRoot[maxn] = {false} ;  //fasle表示为根节点，true表示不是根节点
int inNum = 0 ;     //记录中序遍历输出节点个数
int levelNum = 0 ;  //记录层序遍历输出节点个数


int strToInt(char c){
    if( c == '-' ){
        return -1 ;
    }
    //为某个节点的孩子节点，说明该节点不是根节点
    else{
        notRoot[c-'0'] = true ;
        return c-'0' ;
    }
}

//寻找根节点
int getRoot(int n){
    for(int i = 0 ; i <n ; i++){
        if( notRoot[i] == false ){
            return i ;
        }
    }

}

//反转二叉树
void  postOrderToInvert(int root){

    if(root == -1){
        return ;
    }

    postOrderToInvert(node[root].lchild) ;
    postOrderToInvert(node[root].rchild) ;
    //交换孩子节点，实现反转二叉树
    swap(node[root].lchild , node[root].rchild) ;
}

//中序遍历
void Inorder(int root , int n){
    if( root == -1 ){
        return ;
    }
    Inorder( node[root].lchild , n ) ;

    printf("%d" , root) ;
    inNum++ ;
    if(inNum < n){
        cout << " " ;
    }

    Inorder( node[root].rchild , n ) ;


}

//层序遍历
void LevelOrder(int root , int n){
    queue<int > q  ;
    q.push(root) ;  //根节点入队
    //队非空
    while( !q.empty() ){
        int top = q.front() ;       //取出队头元素
        q.pop() ;       //出队

        cout << top ;
        levelNum++ ;
        if( levelNum < n ){
            cout << " " ;
        }

        if( node[top].lchild != -1 ){
            q.push(node[top].lchild) ;
        }
        if( node[top].rchild != -1 ){
            q.push(node[top].rchild) ;
        }

    }


}


int main(){
    int n ;
    cin >> n ;
    getchar() ;
    for(int i = 0 ; i < n ; i++){
        char a , b ;
        scanf("%c %c" , &a ,&b) ;
        getchar() ;
        node[i].lchild = strToInt(a) ;
        node[i].rchild = strToInt(b) ;
    }

    //获取根节点
    int root = getRoot(n) ;
    //反转二叉树
    postOrderToInvert(root) ;

    //层序遍历
    LevelOrder(root , n) ;

    cout << "\n" ;

    //中序遍历
    Inorder(root , n) ;

    return 0 ;
}

```