---
title: PTA A1099 Build A Binary Search Tree
date: 2021-07-26 17:25:22.618
updated: 2021-07-26 17:25:22.618
url: https://pumpkn.xyz/archives/ptaa1099buildabinarysearchtree
categories: 算法
tags: 学习 | 算法 | 树 | 二叉树 | BST
---

# 题目
## [原题链接](链接)
![image.png](https://pumpkn.xyz/upload/2021/07/image-356b7222e4a945e38f0932097dcf46a3.png)

# 题意
给出一个二叉树的结构，再给出一个整数序列，将整数序列中的数字填入之前构造的二叉树，使其成为一颗二叉查找树。输出二叉查找树的层序遍历序列。题目保证构成的二叉查找树唯一。
# 题解
<font color="red">由于二叉构造树的中序遍历序列是从小到大的有序序列，所以很容易想到，将题目给出序列seq升序排序后，按构造的二叉树的中序遍历序列插入权值，即可得到构造的二叉查找树，最后输出二叉查找树的层序遍历序列即可。</font>


# 代码

```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 110 ;
int n , num = 0; //n为总结点树，num为已经输出的结点个数
struct Node{
    int data ;
    int lchild ;
    int rchild ;
}node[maxn];

int j = 0 ;
void inOrder( int root , vector<int> seq ){
    if( root == -1 ){
        return ;
    }
    inOrder( node[root].lchild  , seq ) ;
    node[root].data = seq[j++] ;
    inOrder( node[root].rchild  , seq ) ;
}


void levelOrder( int root ){
    queue<int> q ;
    q.push(root) ;

    while( !q.empty() ){
        int top = q.front() ;
        q.pop() ;
        cout << node[top].data  ;
        num++ ;
        if( num < n ){
            cout << " " ;
        }
        if( node[top].lchild != -1 ){
            q.push( node[top].lchild ) ;
        }

        if( node[top].rchild != -1 ){
            q.push( node[top].rchild ) ;
        }
    }
}

int main(){
    cin >> n ;
    for( int i = 0 ; i < n ; i ++ ){
        cin >> node[i].lchild >> node[i].rchild ;
    }

    vector<int> seq ;
    for( int i = 0 ; i < n ; i++ ){
        int temp ;
        cin >> temp ;
        seq.push_back(temp) ;
    }

    sort( seq.begin() , seq.end() ) ;

    inOrder( 0 , seq ) ;
    levelOrder(0) ;

    return 0 ;
}

```