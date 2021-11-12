---
title: PTA A1005 Spell It Right
date: 2021-05-31 23:49:30.212
updated: 2021-05-31 23:49:30.212
url: https://pumpkn.xyz/archives/ptaa1005spellitright
categories: 算法
tags: 算法 | string
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805519074574336)
![image.png](https://pumpkn.xyz/upload/2021/05/image-2763e70f25fb4c3c82a4155820faa682.png)
# 题解
无
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const string mp[10] = { "zero" , "one" , "two" , "three" , "four" , "five" , "six" , "seven" , "eight" ,"nine"  } ;


int main(){
    string str ;
    getline(cin , str) ;
    int sum = 0 ;
    for( int i = 0 ; i < str.length() ; i++ ){
        sum += str[i] - '0' ;
    }

    stringstream ss ;
    ss << sum ;
    string strRes = ss.str() ;

    for( int i = 0 ;i < strRes.length() ; i++ ){
        cout << mp[ strRes[i] - '0' ] ;
        if( i != strRes.length()-1 ){
            cout << " " ;
        }
    }

    return 0 ;
}

```