---
title: PTA A1078 Hashing
date: 2021-07-14 21:51:17.752
updated: 2021-07-14 21:51:17.752
url: https://pumpkn.xyz/archives/ptaa1078hashing
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805389634158592)
![image.png](https://pumpkn.xyz/upload/2021/07/image-1c083f04b21b4a3cb6cd668da846417d.png)
# 题意
给出散列表长Tsize和欲插入的元素，将这些元素按读入的顺序插入散列表中，其中散列函数为H(key) = key % Tsize，解决冲突采用只往正向增加的二次探查法（即二次方探查法）。另外，如果题目给出的Tsize不是素数，那么需要将Tsize重新赋值为第一个比Tsize大的素数再进行元素插入。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

bool isPrime( int n ){
    if( n <= 1 ){
        return false ;
    }
    for( int i = 2 ; i*i <= n ; i++ ){
        if( n % i == 0 ){
            return false ;
        }
    }
    return true ;
}

int getMinPrime( int n ){
    for( int i = n ; i < 10000 ; i++ ){
        if( isPrime(i) == true ){
            return i ;
        }
    }
    return -1 ;
}


int main(){
    int t , n ;
    cin >> t >> n ;
    if( isPrime(t) != true ){
        t = getMinPrime(t) ;
    }

    bool isUsed[t] ;
    for( int i = 0 ; i < t ; i++ ){
        isUsed[i] = false ;
    }

    for( int i = 0 ; i < n ; i++ ){
        int temp ;
        cin >> temp ;
        int H = temp % t ;
        if( isUsed[H] == false ){
            cout << H ;
            isUsed[H] = true ;
            if( i < n-1 ){
                cout << " " ;
            }
            continue ;
        }
        else{
            bool flag = false ;
            for( int j = 1 ; j < t ; j++ ){
                H = (temp + j*j) % t ;
                if( isUsed[H] == false ){
                    cout << H ;
                    isUsed[H] = true ;
                    flag = true ;
                    if( i < n-1 ){
                        cout << " " ;
                    }
                    break ;
                }
            }
            if( flag == false ){
                cout << "-" ;
                if( i < n-1 ){
                    cout << " " ;
                }
            }
        }


    }

    return 0 ;
}

```