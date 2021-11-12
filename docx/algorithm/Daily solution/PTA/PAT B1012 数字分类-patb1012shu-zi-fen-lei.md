---
title: PAT B1012 数字分类
date: 2021-02-14 02:29:27.035
updated: 2021-03-09 02:29:49.271
url: https://pumpkn.xyz/archives/patb1012shu-zi-fen-lei
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805311146147840)
![image.png](https://pumpkn.xyz/upload/2021/03/image-42d9353f189c40cb8ea7325d92f8e983.png)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int n ;
    cin >> n ;

    int A1 = 0 ;
    vector<int > A2 ;
    int A3 = 0;
    vector<double > A4 ;
    vector<int > A5 ;

    for( int i = 0 ; i < n ; i++ ){
        int tmp ;
        cin >> tmp ;
        //A1
        if( tmp % 5 == 0 && tmp % 2 == 0 ){
            A1 += tmp  ;
        }

        //A2
        if( tmp % 5 == 1 ){
            A2.push_back(tmp) ;
        }

        //A3
        if( tmp % 5 == 2 ){
            A3++ ;
        }

        //A4
        if( tmp % 5 == 3 ){
            A4.push_back(tmp) ;
        }

        //A5
        if( tmp % 5 == 4 ){
            A5.push_back(tmp) ;
        }

    }

    if( A1 == 0){
        cout << "N "  ;
    }else{
        cout << A1 << " " ;
    }

    if( A2.size() != 0){
        int resA2 = A2[0] - A2[1] ;
        for(int i = 2 ; i < A2.size() ; i++){
            if( i % 2 == 0 ){
                resA2 +=  A2[i] ;
            }else{
                resA2 -=  A2[i] ;
            }
        }
        cout << resA2 << " ";
    }else{
        cout << "N " ;
    }


    if( A3 == 0){
        cout << "N "  ;
    }else{
        cout << A3 << " " ;
    }


    if( A4.size() != 0 ){
        double resA4 ;
        double tmpResA4 = 0 ;
        for( int i = 0 ; i < A4.size() ; i++ ){
            tmpResA4 += A4[i] ;
        }
        resA4 = tmpResA4 / A4.size() ;
        printf( "%.1f " , resA4) ;
    }else{
        cout << "N "  ;
    }


    if( A5.size() != 0 ){
        int maxValue = 0 ;
        for( int i = 0 ; i < A5.size() ; i++ ){
            if( A5[i] > maxValue ){
                maxValue = A5[i] ;
            }
        }
        cout << maxValue ;
    }else{
        cout << "N" ;
    }



    return 0 ;
}

```