---
title: PTA A1015 Reversible Primes 
date: 2021-07-14 21:07:40.832
updated: 2021-07-14 21:51:26.851
url: https://pumpkn.xyz/archives/ptaa1015reversibleprimes
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805495863296000)
![image](https://pumpkn.xyz/upload/2021/07/image-26f39667edd945a38121ae4ad9bc6833.png)
# 题意
给出正整数N和进制radix，如果N是素数，且N在radix进制下反转后的数在十进制下也是素数，则输出Yes，否则输出No。

# 题解
将n转换成radix进制的数(倒序排放)
```c++
vector<int> getRadix( int n , int radix ){
    vector<int> ans ;
    do{
        ans.push_back(n % radix)   ;
        n /= radix ;
    }while( n != 0 ) ;
    return ans ;
}
```


# 代码
```c++
#include<bits/stdc++.h>
using namespace std;

vector<int> getRadix( int n , int radix ){
    vector<int> ans ;
    do{
        ans.push_back(n % radix)   ;
        n /= radix ;
    }while( n != 0 ) ;
    return ans ;
}

int getDecimal( vector<int> reverseN , int radix ){
    int ret = 0 ;
    for( int i = reverseN.size()-1 ,  j = 0 ; i >= 0 ; i-- , j++ ){
        ret += reverseN[i]*pow( radix , j ) ;
    }
    return ret ;
}


bool isPrime( int n ){
    if( n <= 1 ){     //特判
        return false ;
    }
    for( int i = 2 ; i*i < n ; i++ ){
        if( n % i == 0 ){
            return false ;
        }
    }
    return true ;
}


int main(){
    int temp = 1 ;
    vector< vector<int> > input ;
    vector<int> tempInput ;
    while( temp > 0 ){
        cin >> temp ;
        if( tempInput.size() < 2 ){
            tempInput.push_back(temp) ;
        }
        else{
            input.push_back( tempInput ) ;
            tempInput.clear() ;
            tempInput.push_back(temp) ;
        }

    }


    for( int i = 0 ; i < input.size() ; i++ ){
        int n = input[i][0] ;

        if( n == 1 ){
            cout <<"No" << endl ;
            continue ;
        }

        int radix = input[i][1] ;
        if( isPrime(n) == false  ){
            continue ;
        }
        else{
            //获取按radix进制转换为翻转的数
            vector<int> reverseN = getRadix(n , radix) ;

//            for( int j = 0 ; j < reverseN.size() ; j++ ){
//                cout << reverseN[j] << " " ;
//            }
//            cout <<endl ;
            int ret = getDecimal( reverseN , radix ) ;
         //   cout << "ret  " <<ret <<endl ;
            if( isPrime(ret) == true ){
                cout << "Yes" <<endl ;
            }
            else{
                cout << "No" <<endl ;
            }
        }

    }


    return 0 ;
}

```