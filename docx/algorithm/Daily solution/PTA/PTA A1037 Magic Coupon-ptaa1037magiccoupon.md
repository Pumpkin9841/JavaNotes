---
title: PTA A1037 Magic Coupon
date: 2021-07-02 23:56:03.96
updated: 2021-07-02 23:56:03.96
url: https://pumpkn.xyz/archives/ptaa1037magiccoupon
categories: 算法
tags: 积累 | 算法 | 贪心算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805451374313472)
![image.png](https://pumpkn.xyz/upload/2021/07/image-63894e701e014dc295bf16f8e36a8841.png)
# 题意
给出两个集合，从这两个集合中分别选取相同数量的元素进行一对一相乘，问能得到的乘积之和最大是多少。
# 题解
从直观上可以很容易想到贪心策略：</br>
对于每一个集合，将正数与负数分开存放，正数升序排序，负数降序排序(便于后面删除每个集合最后一个元素的操作)。若两个集合中均还有正数，将两个集合中最大的正数相乘并累加到ret；若没有正数，将两个集合中最大的负数相乘并累加到ret。

# 代码

```c++
#include<bits/stdc++.h>
using namespace std ;

bool cmp( int a , int b ){
    return a > b ;
}

int main(){
    int c , p ;
    vector<int> nc , np ;
    cin >> c  ;
    for( int i = 0 ; i < c ; i++ ){
        int tmp ;
        cin >> tmp ;
        nc.push_back(tmp) ;
    }

    cin >> p ;
    for( int i = 0 ; i < p ; i++ ){
        int tmp ;
        cin >> tmp ;
        np.push_back(tmp) ;
    }

    //将nc中正负数分开存放
    vector<int> zNc , fNc ;
    for( int i = 0 ; i < c ; i++ ){
        if( nc[i] > 0 ){
            zNc.push_back(nc[i]) ;
        }
        else if( nc[i] < 0 ){
            fNc.push_back(nc[i]) ;
        }
    }

    //将np中正负数分开存放
    vector<int> zNp , fNp ;
    for( int i = 0 ; i < p ; i++ ){
        if( np[i] > 0 ){
            zNp.push_back(np[i]) ;
        }
        else if( np[i] < 0 ){
            fNp.push_back(np[i]) ;
        }
    }

    sort( zNc.begin() , zNc.end() ) ;
    sort( fNc.begin() , fNc.end() , cmp ) ;
    sort( zNp.begin() , zNp.end() ) ;
    sort( fNp.begin() , fNp.end() , cmp ) ;

//    for( int i = 0 ; i < zNp.size() ; i++ ){
//        cout << "zNp: " <<  zNp[i] << endl ;
//
//    }

    int ret = 0 ;
    for( int i = 0 ; i < min( c , p )  ; i++  ){
        int a , b , ans ;
        if( (zNc.size() > 0) && (zNp.size() > 0) ){
            a = zNc.back() ;
            zNc.pop_back() ;
            b = zNp.back() ;
            zNp.pop_back() ;
      //      cout << "zNp的大小" << zNp.size() <<endl ;
            ans = a * b ;
       //     cout << "a: " << a << " b " << b <<endl ;
       //     cout << "ans " << ans << endl ;
            ret += ans ;
//            cout << "ret " << ret <<endl ;
        }
        else if( fNc.size() > 0 && fNp.size() > 0 ){
            a = fNc.back() ;
            fNc.pop_back() ;
            b = fNp.back() ;
            fNp.pop_back() ;
            ans = a * b ;
//            cout << "a: " << a << " b " << b <<endl ;
//            cout << "ans " << ans << endl ;
            ret += ans ;
//            cout << "ret " << ret << endl;
        }

    }

    cout << ret ;

    return 0 ;
}

```