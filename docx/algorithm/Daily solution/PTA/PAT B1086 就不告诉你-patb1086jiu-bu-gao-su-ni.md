---
title: PAT B1086 就不告诉你
date: 2021-01-30 22:50:38.79
updated: 2021-03-03 22:50:53.078
url: https://pumpkn.xyz/archives/patb1086jiu-bu-gao-su-ni
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/1038429065476579328)
![image.png](https://pumpkn.xyz/upload/2021/03/image-683b840a66b6401b9511c164a3571c40.png)
# 代码

```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int a ,b ;
    cin >> a >> b ;
    int tmp = a*b ;
    stringstream ss ;
    ss << tmp ;
    string str = ss.str() ;
    for(int i = str.length()-1 ; i >=0 ; i--){
        if( str.length() > 1 ){
                if( str[i] != '0'){
                cout << str[i] ;
            }
        }else{
            cout << str[i] ;
        }
    }

    return 0 ;
}

```