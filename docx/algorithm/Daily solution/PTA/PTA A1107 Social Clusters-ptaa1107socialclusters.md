---
title: PTA A1107 Social Clusters
date: 2021-07-27 16:11:56.872
updated: 2021-07-27 16:11:56.872
url: https://pumpkn.xyz/archives/ptaa1107socialclusters
categories: 算法
tags: 学习 | 算法 | 并查集
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805361586847744)
![image.png](https://pumpkn.xyz/upload/2021/07/image-0ba5fb062b6d40b3a3e3817fe56105c9.png)
# 题意
有N个人，每个人喜欢若干项活动，如果两个人有任意一个活动相同，那么就称他们处于同一个社交网络（若A和B属于同一个社交网络，B和C属于同一个社交网络，那么A.B.C属于同一个社交网络），求这N个人总共形成了多少个社交网络。

# 题解
本题中判断两个人是好朋友的条件为他们有公共喜欢的活动，因此不妨开一个数组course，其中course[h]用以记录任意一个喜欢活动h的人的编号，这样的话findFather(course[h]）就是这个人所在的社交网络的根结点。于是，对当前读入的人的编号i和他喜欢的每一个活动h，只需要合并i与findFather（course[h]）即可。</br>
至于最后集合个数的计数，只需要开一个int型数组isRoot（其中isRoot[x]代表以x号人作为根结点的社交网络中有多少人，如果x不是根结点，则isRoot[x]为0），之后遍历i、让isRoot[findFather(i)]加1即可。

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int N = 1010 ;
int father[N] ;
int isRoot[N] = {0} ;
int course[N] = {0} ;

int findFather( int x ){
    while( x != father[x] ){
        x = father[x] ;
    }
    return x ;
}

void Union( int a , int b ){
    int faA = findFather(a) ;
    int faB = findFather(b) ;
    if( faA != faB ){
        father[faA] = faB ;
    }
}

void init( int n  ){
    for( int i = 1 ; i <= n ; i++ ){
        father[i] = i ;
        isRoot[i] = false ;
    }
}

bool cmp( int a , int b ){
    return a > b ;
}


int main(){
    int n , k , h ;
    cin >> n ;
    init(n) ;
    for( int i = 1 ; i<= n ; i++ ){
        scanf("%d: " , &k) ;
        for( int j = 0 ; j<k ; j++ ){
            scanf("%d" , &h) ;
            if( course[h] == 0 ){
                course[h] = i ;
            }
            Union(i , findFather(course[h])) ;
        }
    }

    for( int i = 1 ; i<=n ;i++ ){
        isRoot[ findFather(i) ] ++ ;
    }
    int ans = 0 ;
    for( int i = 1 ; i<= n ;i++ ){
        if( isRoot[i] != 0 ){
            ans ++ ;
        }
    }

    cout << ans << endl ;
    sort( isRoot + 1 , isRoot + n + 1 , cmp ) ;
    for( int i = 1 ; i <=ans ; i++ ){
        cout << isRoot[i] ;
        if( i < ans  ){
            cout << " " ;
        }
    }

    return 0;
}

```