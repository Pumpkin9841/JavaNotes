---
title: PTA A1021 Deepest Root 
date: 2021-08-04 16:25:49.195
updated: 2021-08-04 16:25:49.195
url: https://pumpkn.xyz/archives/ptaa1021deepestroot
categories: 算法
tags: 学习 | 算法 | 并查集 | 图
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805482919673856)
![image.png](https://pumpkn.xyz/upload/2021/08/image-7cb0f98375c341508ba9bcebd94a1d16.png)

# 题意
给出N个结点与N-1条边，它们能否形成一棵N个结点的树，如果能，则从中选出树高度最大的根结点，若有多个则按从小到大全部输出。否则输出Error: k components，k为连通块数。


## 求连通块数 有两种方法。

# 并查集求连通块数
```c++
const int maxn = 1010 ;
int Father[maxn] ;
bool isRoot[maxn] //记录每个结点是否作为某个集合的根结点

//查找x所在的集合根结点
int findFather( int x ){
    while( x != Father[x] ){
        x = Father[x] ;
    }
    return x ;
}

//合并a和b在的集合
void Union( int a, int b ){
    int faA = findFather(a) ;
    int faB = findFather(b) ;
    if( faA != faB ){
        Father[faA] = faB ;
    }
}

//初始化并查集
void init( int n ){
    for( int i = 1 ; i <= n ; i++ ){
        Father[i] = i ;
    }
}

//计算连通块数
int calBlock( int n ){
    int Block = 0 ;
    for( int i = 1 ; i <= n ; i++ ){
        isRoot[ findFather(i) ] = true ; //i的根结点是findFather(i)
    }
    for( int i = 1 ; i <= n ; i++ ){
        Block += isRoot[i] ; //累加根结点个数
    }
    return Block ;
}

int main(){
    
    init(n) ; //初始化并查集
    for( int i = 1 ; i < n ; i++ ){
        //输入图 边的信息
        G[a].push_back(b) // 边a->b 
        G[b].push_back(a)  //边b->a
        Union(a,b) ; //合并a和b所在集合
        int Block  = calBlock(n) ; //计算连通块数
        
        //后续操作
        
    }
    
    return 0 ;
}
```

# DFS遍历求连通块数

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 1010 ;

struct Node{
    int value ;
    int depth ;
};

vector<Node> G[maxn] ;
int n ;
bool vis[maxn]  ;
int block ; //记录连通块数
int maxDepth  ;
void DFS( int u , int depth ){ //depth为深度
    vis[u] = true ;

    if( depth > maxDepth ){
        maxDepth = depth ;
    }
    for( int i = 0 ; i < G[u].size() ; i++ ){
        int v = G[u][i].value ; //若u与v之间存在边
        if( vis[v] == false ){
            DFS(v , depth+1) ;  //递归遍历，每递归一次深度+1
        }
    }
}

void DFSTrave( int u ){
    //特判
    if( vis[u] == false ){
        DFS(u , 1) ;
        block ++ ;
    }

    for( int i = 1 ; i <= n ; i++ ){
        if( i != u && vis[i] == false ){
            DFS( i , 1 ) ;
            block ++ ;
        }
    }
}


int main(){
    cin >> n ;
    for( int i = 0 ; i < n - 1 ; i++ ){
        int a , b ;
        cin >> a >> b ;
        Node one ;
        one.value = a ;
        one.depth = 1 ;
        Node two ;
        two.value = b ;
        two.depth = 1 ;

        G[a].push_back(two) ; //边a->b
        G[b].push_back(one) ;  //边b->a
    }

    vector<int> ans ;
    ans.push_back(0) ;
    bool flag = false ;
    for( int u = 1 ; u <= n ; u++ ){
        block = 0 ;
        maxDepth = 0 ;
        memset(vis , false , sizeof(vis)) ;
        DFSTrave(u) ;
        if( block > 1 ){
            cout << "Error: "<< block << " components" ;
            flag = true ;
            break ;
        }
        else{
            ans.push_back(maxDepth) ;
        }
    }

    if( flag == false ){
        vector<int> temp = ans ;
        sort( temp.begin() , temp.end() ) ;
        int maxVlue = temp[temp.size()-1] ;
        for( int i = 1 ; i<ans.size() ; i++ ){
            if( ans[i] == maxVlue ){
                cout << i  ;
                if( i < ans.size()-1 ){
                 cout << endl ;
                }
            }

        }
    }
    return 0 ;
}

```