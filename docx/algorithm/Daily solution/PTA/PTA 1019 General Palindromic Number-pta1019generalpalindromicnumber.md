---
title: PTA 1019 General Palindromic Number
date: 2021-05-29 22:49:09.677
updated: 2021-05-29 22:50:37.227
url: https://pumpkn.xyz/archives/pta1019generalpalindromicnumber
categories: 算法
tags: 算法 | 进制转换
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805487143337984)
![image.png](https://pumpkn.xyz/upload/2021/05/image-75260554659b4552984c31452ee6b269.png)

# 题解
将十进制数n转化成b进制，代码模板
```c++
int n , b ;
vector<int> res ; //用来存储b进制数
while( n != 0 ){
    res.push_back( n % b );
    n /= n ;
}

```

然后将保存b进制数的res分别按照顺序和逆序存储到ASC，DESC数组中，若
```c++
ASC == DESC
```
则该b进制数为回文数，返回Yes</br>
否则返回No。

# 代码

```c++
#include<bits/stdc++.h>
using namespace std ;


void print(vector<int> tmp){
    for( int i = 0 ; i < tmp.size() ; i++ ){
        cout << tmp[i] ;
        if( i != tmp.size()-1 ){
            cout << " " ;
        }
    }
}

int main(){
    int n , b ;
    cin >> n >> b ;
    vector<int> res ;

    while( n != 0 ){
        res.push_back( n % b ) ;
        n /= b ;
    }

    vector<int> ASC , DESC ;
    for( int i = 0 ; i < res.size() ; i++ ){
        ASC.push_back(res[i]) ;
    }

    for( int i = res.size()-1 ; i>=0 ; i-- ){
        DESC.push_back(res[i]) ;
    }

    if( ASC == DESC ){
        cout << "Yes" <<endl ;
        print(DESC) ;
    }else{
        cout << "No" <<endl ;
        print(DESC) ;
    }

    return 0 ;
}

```