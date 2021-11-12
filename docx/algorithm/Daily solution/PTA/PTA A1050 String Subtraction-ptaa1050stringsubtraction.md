---
title: PTA A1050 String Subtraction
date: 2021-06-07 14:39:52.25
updated: 2021-06-07 14:39:52.25
url: https://pumpkn.xyz/archives/ptaa1050stringsubtraction
categories: 算法
tags: 算法 | map   
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805429018673152)
![image.png](https://pumpkn.xyz/upload/2021/06/image-deb80a9412ef4d5c85129684615dadfc.png)
# 题意
给出两个字符串，在第一个字符串中删去第二个字符串中出现过的所有字符并输出。
# 题解
小写字母转成大写字母
```c++
char a = 'a' ;
char A = 'a' - 32 ;
```

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    string s1 ;
    string s2 ;
    getline( cin , s1) ;
    getline( cin , s2 ) ;

    map<char , int> mp ;

    //初始化
    for( int i = 'a' - '0' ; i <= 'z' - '0' ; i++ ){
        mp[i+'0'] = 0 ;
        char c = i+'0'-32 ;
        mp[c] = 0 ;
    }

    for( int i = 0 ; i < s2.length() ; i++ ){
        mp[ s2[i] ] ++ ;
    }

    for( int i = 0 ; i < s1.length() ; i++ ){
        char c = s1[i] ;
        if( mp[c] != 0 ){
            continue ;
        }
        cout << c ;
    }


    return 0 ;
}

```