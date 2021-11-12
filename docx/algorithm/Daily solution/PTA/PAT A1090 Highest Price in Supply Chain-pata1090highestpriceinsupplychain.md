---
title: PAT A1090 Highest Price in Supply Chain
date: 2021-02-28 17:44:45.011
updated: 2021-03-08 17:45:20.561
url: https://pumpkn.xyz/archives/pata1090highestpriceinsupplychain
categories: 算法
tags: DFS  | BFS    | 树
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805376476626944)
![image.png](https://pumpkn.xyz/upload/2021/03/image-109d4ed5b3fe44c194b6b4c6e856f709.png)
# 题意
给出一棵销售供应的树，树根唯一。在树根处货物的价格为P，然后从根结点开始每往子结点走一层，该层的货物价格将会在父亲结点的价格上增加r%。求所有叶结点中的最高价格以及这个价格的叶结点个数。
# 样例解释
第二行的输入S[i]：第i个节点的父节点为S[i] 
## 样例构成的树
![image.png](https://pumpkn.xyz/upload/2021/03/image-a14210c6f6234f3b827ecca0c7aaff00.png)

# 题解
由于每层的价格都在上一层的基础上乘以（1 +r）（r已去除百分号，即已经在题目输入的基础上除以了100，下同），因此只要计算深度最深的结点即可。由于不用考虑结点的点权，因此可以直接以```vectorc<int> child[maxn]```来存放树。接下来就可以使用DFS或BFS来获取这棵树的最深深度（设根结点的深度为0），此处以DFS为例。</br>
在全局变量中设置int型maxDepth以表示最大深度，以num表示最大深度的叶结点个数，初值均为0。对本题的DFS函数来说，参数只需要设置当前访问结点index与当前深度depth.</br>
下面分别考虑递归边界和递归式</br>
①递归边界。当结点index的子结点个数为0时，表示到达了叶结点。此时判断当前深度depth是否大于最大深度maxDepth，如果大于，则更新maxDepth并重置num为1：如果不大于，则判断depth是否等于maxDepth，如果depth等于maxDepth，则令numt+，表示最大深度的结点个数加1。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 100010 ;
//节点总数
int n ;
//价格P ， 增长率r
double p , r ;

struct Node{
    vector<int > child ;
}node[maxn];

vector<double > price ;

//降序排序
bool cmp(double a , double b){
    return a > b ;
}


void DFS(int root , int deepth ){

   //为叶子节点
   if( node[root].child.size() == 0 ){
        double total = p * ( pow( 1+ r/100 , deepth ) ) ;
        price.push_back(total) ;
   }

   for(int i = 0 ; i < node[root].child.size() ; i++ ){
        DFS( node[root].child[i] , deepth + 1 ) ;
   }

}


int main(){
    int root ; //记录根节点位置
    cin >> n >> p >> r ;
    for(int i = 0 ; i < n ; i++){
        int tmp ;
        cin >> tmp ;
        // tmp为-1表示根节点
        if( tmp != -1){
            node[tmp].child.push_back(i) ;
        }else{
            root = i ;
        }
    }

    DFS(root , 0) ;

    sort( price.begin() , price.end() , cmp ) ;

    int count = 0 ;
    for(int i = 0 ; i < price.size() ; i++ ){
            if( price[0] == price[i] ){
                count ++ ;
            }

    }

    printf("%.2f %d" , price[0] , count) ;

    return 0 ;
}

```