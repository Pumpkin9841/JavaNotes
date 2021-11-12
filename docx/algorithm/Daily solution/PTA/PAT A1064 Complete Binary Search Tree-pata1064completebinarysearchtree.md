---
title: PAT A1064 Complete Binary Search Tree
date: 2021-03-05 16:40:00.964
updated: 2021-03-10 16:40:29.377
url: https://pumpkn.xyz/archives/pata1064completebinarysearchtree
categories: 算法
tags: 算法 | 树 | 二叉树 | BST
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805407749357568)
![image.png](https://pumpkn.xyz/upload/2021/03/image-886cf1d869074afaa4fdfe8937af06bd.png)
# 题意
给出N个非负整数，要用它们构建一棵完全二叉排序树。输出这棵完全二叉排序树的层序遍历序列。
# 题解
### 步骤1：
如果使用数组来存放完全二叉树，那么对完全二叉树当中的任何一个结点（设编号为x，其中根结点编号为1），其左孩子结点的编号一定是2x，而右孩子结点的编号一定是2x+1。那么就可以开一个数组CBT[maxn]，其中CBT[1]~CBT[n]按层序存放完全二叉树的n个结点，这个数组就存放了一棵完全二叉树，只不过暂时还没有为其元素进行赋值。</br>
### 步骤2：
考虑到对一棵二叉排序树来说，其中序遍历序列是递增的，那么思路就很清晰了：先将输入的数字从小到大排序，然后对CBT数组表示的二叉树进行中序遍历，并在遍历过程中将数字从小到大填入数组，最后就能得到一棵完全二叉排序树。而由于CBT数组就是按照二叉树的层序来存放结点的，因此只需要将数组元素按顺序输出即为层序遍历序列。
### 注意点
①根结点下标必须为1.</br>
②完全二叉树到达空结点的标志是当前结点root的编号大于结点个数n
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 1010 ;
int index1 = 0 ;
int data[maxn] , CBT[maxn]  , n ;

void inOrder( int root ){
    if( root > n ){
        return ;
    }
    inOrder( root * 2 ) ;
    CBT[root] = data[index1++] ;
    inOrder( root * 2 + 1 ) ;
}


int main(){
    cin >> n ;
    for( int i = 0 ; i < n ; i++ ){
        cin >> data[i] ;
    }

    sort( data , data + n ) ;

    inOrder(1) ;

    for( int i = 1 ; i <= n ; i++ ){
        cout << CBT[i] ;
        if( i < n ){
            cout << " " ;
        }
    }

    return 0 ;
}

```