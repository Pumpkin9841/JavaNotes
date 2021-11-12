---
title: PAT A1086 Tree Traversals Again
date: 2021-02-24 00:33:56.519
updated: 2021-02-24 00:35:08.204
url: https://pumpkn.xyz/archives/pata1086treetraversalsagain
categories: 算法
tags: 树 | 二叉树
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805380754817024)
![PAT A1086.jpg](https://pumpkn.xyz/upload/2021/02/PAT%20A1086-cffb96f9c43a4760be1717be138cccaf.jpg)
# 题意
给出N个节点的出栈和出栈次序以确定一棵唯一的树，求该树的后序遍历序列。
# 题解
入栈次序就是该树的先跟序列，遇到push久入栈，遇到pop就将栈顶出栈，即为树的中跟遍历序列。知道中跟，先跟序列就可确定唯一的树，即可求出后根遍历序列。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 50 ;

int pre[maxn] , in[maxn] ,post[maxn] ;
vector<int> tmpNum ;   //临时存放输入的data域
int n ;

struct node{
    int data ;
    node* lchild ;
    node* rchild ;
};

//通过先跟遍历序列和中跟遍历序列得到唯一的二叉树
//先根遍历区间为[preL , preR] , 中根遍历序列区间[inL , inR]
node* BFS(int preL , int preR , int inL , int inR ){
    if(preL > preR ){
        return NULL ;
    }

    node* root = new node ;
    root->data = pre[preL] ;
    int k ;
    for(k = inL ; k<=inR ; k++){
        if( pre[preL] == in[k] ){
            break ;
        }
    }

    int leftNum = k - inL ;

    //左子树的先跟序列区间[preL+1 , preL+leftNum] ,中根遍历序列区间[inL,k-1]
    root->lchild = BFS(preL+1 , preL+leftNum , inL , k-1) ;

    //右子树的先跟序列区间[preL+leftNum+1 , preR] ,中根遍历序列区间[k+1,inR]
    root->rchild = BFS(preL+leftNum+1 , preR , k+1 ,inR) ;

    return root ;

}


//后根遍历
int posNum = 0 ;
void postOrder(node* root){
    if(root == NULL ){
        return ;
    }

    postOrder(root->lchild) ;
    postOrder(root->rchild) ;
    cout << root->data ;
    posNum++ ;
    if(posNum < n ){
        cout << " " ;
    }
}



int main(){
    cin >> n ;
    getchar() ;     //接收输入n后的回车
    string str[3*n] ;
    for(int i = 0 ; i < 2*n ; i++){
        getline(cin , str[i]) ;
    }

    int preNum = 0 ;
    int inNum = 0 ;
    for(int i = 0 ; i < 2*n ; i++){
        int len = str[i].length() ;
        //存放先序遍历序列
        if(len > 3){
            string tmpStr = str[i].substr(5,len-1) ;
            stringstream ss ;
            ss << tmpStr ;
            int tmp ;
            ss >> tmp ;
            pre[preNum++] = tmp ;
            tmpNum.push_back(tmp) ;
        }

        //处理中跟遍历序列
        //如果输入pop
        if( len == 3 ){
            int tmp = tmpNum.back() ;
            tmpNum.pop_back() ;
            in[inNum++] = tmp ;
        }

    }



    postOrder(BFS(0,preNum-1,0,inNum-1)) ;


    return 0 ;
}

```