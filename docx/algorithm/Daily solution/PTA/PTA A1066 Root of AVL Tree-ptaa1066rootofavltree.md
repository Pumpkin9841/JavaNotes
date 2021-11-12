---
title: PTA A1066 Root of AVL Tree
date: 2021-07-27 14:01:11.092
updated: 2021-07-27 14:02:27.725
url: https://pumpkn.xyz/archives/ptaa1066rootofavltree
categories: 算法
tags: 学习 | 算法 | 树 | 二叉树 | AVL
---

# 题目
## [原题链接](链接)
![image.png](https://pumpkn.xyz/upload/2021/07/image-78190cc3709948c6afa70e840b2e94b6.png)
# 题意
给出一组序列，将其构成平衡二叉树，并输出构成的平衡二叉树的根结点的权值。（题目保证构成的二叉树唯一）
# 题解
 [根据AVL定义即可](https://pumpkn.xyz/archives/-suan-fa-bi-ji-95ping-heng-er-cha-shu-avl)
# 代码
```C++
#include<bits/stdc++.h>
using namespace std ;

struct Node{
    int data ;
    int height ;
    Node* lchild ;
    Node* rchild ;
};

int getHeight( Node* root ){
    if( root == NULL ){
        return 0 ;
    }
    return root->height ;
}

//更新结点高度
void updateHeight( Node* &root ){
    root->height = max( getHeight(root->lchild) , getHeight( root->rchild ) ) + 1 ;
}

//获取平衡因子
int getBalanceFactor( Node* root ){
    return getHeight( root ->lchild ) - getHeight(root->rchild) ;
}

Node* newNode( int x ){
    Node* node = new Node ;
    node->data = x ;
    node->height = 1 ;
    node->lchild = node->rchild = NULL ;
    return node ;
}

//左旋
void L(Node* &root ){
    Node* temp = root->rchild ;
    root->rchild = temp->lchild ;
    temp->lchild = root ;
    updateHeight(root) ;
    updateHeight(temp) ;
    root = temp ;
}

//右旋
void R(Node* &root){
    Node* temp = root->lchild ;
    root->lchild = temp->rchild ;
    temp->rchild = root ;
    updateHeight(root) ;
    updateHeight(temp) ;
    root = temp ;
}


//插入结点
void insert( Node* &root , int x ){
    if( root == NULL ){
        root = newNode(x) ;
        return ;
    }

    if( root->data > x ){
        insert( root->lchild , x ) ;
        updateHeight( root ) ;
        if( getBalanceFactor(root) == 2 ){
            if( getBalanceFactor(root->lchild) == 1 ){ //LL型
                R(root) ;
            }
            else if( getBalanceFactor(root->lchild) == -1 ){  //LR型
                L(root->lchild) ;
                R(root) ;
            }
        }
    }
    else{
        insert( root->rchild ,x ) ;
        updateHeight( root ) ;
        if( getBalanceFactor(root) == -2 ){
            if( getBalanceFactor(root->rchild) == -1 ){  //RR
                L(root) ;
            }
            else if( getBalanceFactor(root->rchild) == 1 ){  //RL
                R(root->rchild) ;
                L(root) ;
            }
        }
    }
}

//创建
Node* create( vector<int> seq , int n ){
    Node* root = NULL ;
    for( int i = 0 ; i < n ; i++ ){
        insert( root , seq[i] ) ;
    }
    return root ;
}


int main(){
    int n ;
    cin >> n ;
    vector<int> seq ;
    for( int i = 0 ; i < n ; i++ ){
        int temp ;
        cin >> temp ;
        seq.push_back(temp) ;
    }

     Node* root = create( seq , n ) ;
     cout << root->data;

    return 0 ;
}

```