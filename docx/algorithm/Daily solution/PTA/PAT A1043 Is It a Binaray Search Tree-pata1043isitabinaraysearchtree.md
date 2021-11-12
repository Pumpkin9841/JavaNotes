---
title: PAT A1043 Is It a Binaray Search Tree
date: 2021-03-04 15:35:36.0
updated: 2021-03-10 15:43:36.318
url: https://pumpkn.xyz/archives/pata1043isitabinaraysearchtree
categories: 算法
tags: 算法 | 树 | 二叉树 | BST
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805440976633856)
![image.png](https://pumpkn.xyz/upload/2021/03/image-e9b502b80c2f49c5939d2167383df4c0.png)
# 题意
给出N个正整数来作为一棵二叉排序树的结点插入顺序，问：这串序列是否是该二叉排序树的先序序列或是该二叉排序树的镜像树的先序序列。所谓镜像树是指交换二叉树的所有结点的左右子树而形成的树（也即左子树所有结点数据域大于或等于根结点，而根结点数据域小于右子树所有结点的数据域）。如果是镜像树，则输出YES，并输出对应的树的后序序列；否则，输出NO.
# 样例解释
样例1</br>
显然插入序列就是二叉排序树的先序序列
![image.png](https://pumpkn.xyz/upload/2021/03/image-9f203da9b191430fb8989cf07014485f.png)
样例2</br>
通过序列产生的二叉树第一个样例中的二叉树，其镜像树如下所示，而题目给出的序列恰好为该镜像树的先序序列。
![image.png](https://pumpkn.xyz/upload/2021/03/image-e01fe06281bb4e088c5610e434d98239.png)
样例3</br>
插入的序列显然既不是该二叉树的先序序列，也不是其镜像树的先序序列，因此输出NO.
![image.png](https://pumpkn.xyz/upload/2021/03/image-e0593fcdac904bb89e919d3674783794.png)
<font color = "red"> 此题中数据域相同插入根节点的右子树 </font>
# 题解
根据输入序列data[maxn]创建二叉查找树，将该二叉查找树的前序序列存入```vector<int > preOrders```数组中，将```preOrders```与```data```比对，若相同，则输出YES，并输出该二叉树的后序遍历序列。</br>
若不相同，生成镜像BST，重复上诉步骤。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 1010 ;
int data[maxn] ;    //存放所有节点的数据域
vector<int > preOrders ;    //存放二叉查找树的前序遍历序列
vector<int > postOrders ;    //存放二叉查找树的后序遍历序列

struct node{
    int data ;
    node* lchild ;
    node* rchild ;
};

//创建新节点
node* newNode( int x ){
    node* Node = new node ;
    Node->data = x ;
    Node->lchild = Node->rchild = NULL ;
    return Node ;
}

//在二叉树中插入一个数据域为x的新节点(注意参数root要加引用&)
void insert( node* &root , int x ){
    if( root == NULL ){
        root = newNode(x) ;
        return ;
    }

    if( root->data == x ){  //要插入的节点已存在
   //     return ;
        insert( root->rchild , x ) ;
    }else if( root->data > x ){
        insert( root->lchild , x ) ;
    }else{
        insert( root->rchild , x ) ;
    }
}

//创建二叉查找树
node* create( int data[] , int n ){
    node* root = NULL ;
    for( int i = 0 ; i < n ; i++ ){
        insert( root , data[i] ) ;
    }
    return root ;
}

//前序遍历
void preOrder( node* root ){
    node* p = root ;
    if( p == NULL ){
        return ;
    }
    preOrders.push_back( p->data ) ;
    preOrder( p->lchild ) ;
    preOrder( p->rchild ) ;
}

//生成镜像BST
void swapNode( node* &root ){
    if( root == NULL ){
        return ;
    }

    swapNode( root->lchild ) ;
    swapNode( root->rchild ) ;
    swap( root->lchild , root->rchild ) ;

}

//后序遍历
void postOrder( node* root ){
    node* p = root ;
    if( p == NULL ){
        return ;
    }
    postOrder( p->lchild ) ;
    postOrder( p->rchild ) ;
    postOrders.push_back( p->data ) ;
}




int main(){
    int n ;
    cin >> n ;
    for( int i = 0 ; i < n ; i++ ){
        cin >> data[i] ;
    }

    //创建二叉查找树
    node* root = create( data , n ) ;
    //前序遍历树
    preOrder( root ) ;

    bool flag = true ;  //true表示输入序列顺序与非镜像BST前序相同
    bool mirrFlag = true ;  //true表示输入序列顺序与镜像BST前序相同

    for( int i = 0 ; i < n ; i++ ){
        if( data[i] != preOrders[i] ){
            flag = false ;
        }
    }

    //输入序列与BST前序相同
    if( flag == true ){
        cout << "YES\n" ;
        postOrder( root ) ;
        for( int i=0 ; i < postOrders.size() ; i++ ){
            cout << postOrders[i] ;
            if( i < postOrders.size() - 1){
                cout << " " ;
            }
        }
    }else{
        swapNode( root ) ;  //生成镜像BST
        preOrders.clear() ;  //清空前序数组
        preOrder( root ) ;
        //输出后序遍历序列
        for( int i = 0 ; i < n ; i++ ){
            if( data[i] != preOrders[i] ){
                mirrFlag = false ;
            }
        }

        if( mirrFlag == true ){
            cout << "YES\n" ;
            postOrders.clear() ;       //清空后序数组
            postOrder( root ) ;
            //输出后序遍历序列
            for( int i=0 ; i < postOrders.size() ; i++ ){   
                cout << postOrders[i] ;
                if( i < postOrders.size() - 1){
                    cout << " " ;
                }
            }
        }else{      //既不是BST的前序也不是镜像BST的前序
            cout << "NO" ;
        }

    }


    return 0 ;
}

```
