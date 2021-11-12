---
title: PAT B1016 部分A+B
date: 2021-01-17 01:46:49.066
updated: 2021-02-24 01:47:32.39
url: https://pumpkn.xyz/archives/patb1016bu-fen-ab
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805306310115328)
![image.png](https://pumpkn.xyz/upload/2021/02/image-9296db0d315d444bbeb9c47f094223f6.png)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    long A , B ,Da ,Db ;
    long  Daa = 0 , Dbb = 0 ;
    cin >> A >> Da >> B >> Db ;
    stringstream ss ;
    ss << A ;
    string strA = ss.str() ;
    for(int i = 0 ; i < strA.length() ; i++){
        int tmp = strA[i] - '0' ;
        if(tmp == Da){
            Daa++ ;
        }
    }

    stringstream s ;
    s << B ;
    string strB = s.str() ;
    for(int i = 0 ; i < strB.length() ; i++){
        int tmp = strB[i] - '0' ;
        if(tmp == Db){
            Dbb++ ;
        }
    }

    long resA = 0 , resB = 0 , res = 0 ;
    while(Daa > 0){
        resA += pow(10,Daa-1)*Da ;
        Daa-- ;
    }


     while(Dbb > 0){
        resB += pow(10,Dbb-1)*Db ;
        Dbb-- ;
    }

  //  cout << resA << " " << resB ;

    cout <<resA + resB ;

    return 0 ;
}

```