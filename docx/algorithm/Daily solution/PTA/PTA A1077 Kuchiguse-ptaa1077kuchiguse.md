---
title: PTA A1077 Kuchiguse
date: 2021-06-01 18:37:49.8
updated: 2021-06-01 18:37:49.8
url: https://pumpkn.xyz/archives/ptaa1077kuchiguse
categories: 算法
tags: 算法 | string
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805390896644096)
![image.png](https://pumpkn.xyz/upload/2021/06/image-0e8c379415c24fa88dba71ef60f882f3.png)
# 题意
给几个字符串，输出相同后缀，无相同后缀输出nai
# 题解
先将输入的字符串全部反转，然后从左到右依次比较每个字符串该位是否相同，相同则存入vector<char> res数组中，若res.size()为0，表示无相同后缀，输出nai, 否则逆序输出res。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int n ;
    cin >> n ;
    getchar() ;
    vector< string > spoken ;
    for( int i = 0 ; i < n ; i++  ){
        string str ;
        getline(cin , str ) ;
        spoken.push_back(str) ;
    }

    for( int i = 0 ; i < spoken.size() ; i++ ){
        reverse(spoken[i].begin() , spoken[i].end() ) ;
    }



    vector<char> res ;


    int minLen = 200000 ;
    for( int i = 0 ; i < spoken.size() ; i++ ){
        if( spoken[i].length() < minLen ) {
            minLen = spoken[i].length() ;
        }
    }

    bool flag = true ;
    for( int i = 0 ; i < minLen ; i++ ){
        int x = 0 ;

        for( int j = 1 ; j < spoken.size() ; j++ ){
            if( spoken[j][i] != spoken[x][i] ){
                flag = false ;
                break ;
            }
        }


        if( flag ){
            res.push_back( spoken[x][i] ) ;
        }
        else{
            break ;
        }


    }

    reverse( res.begin() , res.end() ) ;




    if( res.size() == 0 ){
        cout << "nai" ;
    }
    else{
        if( res[0] == ' ' ){
            for( int i = 1 ; i <res.size() ;i++ ){
                cout << res[i]  ;
            }
        }
        else{
            for( int i = 0 ; i <res.size() ;i++ ){
                cout << res[i]  ;
            }
        }
    }


    return 0 ;
}

```