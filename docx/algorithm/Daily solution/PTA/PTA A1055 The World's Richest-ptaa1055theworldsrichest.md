---
title: PTA A1055 The World's Richest
date: 2021-06-05 14:54:29.872
updated: 2021-06-05 14:54:29.872
url: https://pumpkn.xyz/archives/ptaa1055theworldsrichest
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805421066272768)
![image.png](https://pumpkn.xyz/upload/2021/06/image-c6b66e8149bd4a0a8b7c77c2750ce8e2.png)
# 题意
给出N个人的姓名、年龄及其拥有的财富值，然后进行K次查询。每次查询要求输出年龄范围在[AgeL，AgeR]的财富值从大到小的前M人的信息。如果财富值相同，则年龄小的优先：如果年龄也相同，则姓名的字典序小的优先。
# 题解

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

struct People{
    char name[10] ;
    int age ;
    int worth ;
}rich[100010];

bool cmp( People a , People b ){
    if( a.worth != b.worth ){
        return a.worth > b.worth ;
    }
    else if( a.age != b.age ){
        return a.age < b.age ;
    }
    else{
        return strcmp( a.name , b.name ) < 0 ;
    }
}


int main(){
    int n , k ;
    cin >> n >> k ;
    for( int i = 0 ; i < n ; i++ ){
        cin >> rich[i].name >> rich[i].age >> rich[i].worth ;
    }

    int query[k][3] ;
    for( int i = 0 ; i < k ; i++ ){
        cin >> query[i][0] >> query[i][1] >> query[i][2] ;
    }

    for( int i = 0 ; i < k ; i++ ){
        cout <<"Case #" << i+1 <<":" <<endl ;
        int num = query[i][0] ;
        int minAge = query[i][1] ;
        int maxAge = query[i][2] ;
        sort( rich , rich+n , cmp ) ;
        for( int j = 0 ; j < n ; j++ ){
            if( num == 0 ){
                break ;
            }
            if( rich[j].age >= minAge && rich[j].age <= maxAge ){
                cout << rich[j].name << " " << rich[j].age << " " << rich[j].worth <<endl ;
                num -- ;
            }
        }
        if( num == query[i][0] ){
            cout <<"None" <<endl ;
        }
    }


    return 0 ;
}

```