---
title: PAT B1081 检查密码
date: 2021-01-29 22:37:27.173
updated: 2021-03-03 22:37:41.241
url: https://pumpkn.xyz/archives/patb1081jian-cha-mi-ma
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805261217153024)
![image.png](https://pumpkn.xyz/upload/2021/03/image-7b893630ad704b17ac595fe5cdf0e8ad.png)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;


void check(string str){
    bool shuzi = false ;
    bool zimu = false ;

    //长度不够
    if(str.length() < 6){
        cout << "Your password is tai duan le." <<endl ;
        return ;
    }

    for(int i = 0 ; i< str.length()-1 ; i++ ){
        //只有英文字母数字和小数点
        if( (str[i] < 'z' && str[i] > 'a') || (str[i] > 'A' && str[i] < 'Z' )
            || ( str[i] > '0' && str[i] ) || str[i] == '.' ){
            //什么也不做
        }
        else{
            cout << "Your password is tai luan le." <<endl ;
            return ;
        }
    }

    for(int i = 0 ; i< str.length()-1 ; i++ ){
       //有字母
       if( (str[i] > 'a' && str[i] < 'z') || (str[i] > 'A' && str[i] <'Z') ){
         zimu = true ;
       }

       //有数字
       if( str[i] > '0' && str[i] < '9' ){
         shuzi = true ;
       }

    }

    if(shuzi == false){
        cout << "Your password needs shu zi." <<endl ;
        return ;
    }

    if(zimu == false ){
        cout << "Your password needs zi mu." << endl ;
        return ;
    }

    cout << "Your password is wan mei." <<endl ;

}

int main(){
    int n ;
    cin >> n ;
    getchar() ;
    string str[n] ;
    for(int i = 0 ; i < n ; i++){
        getline(cin , str[i]) ;
    }

    for(int i = 0 ; i < n ; i++){
        check(str[i]) ;
    }


    return 0 ;
}

```