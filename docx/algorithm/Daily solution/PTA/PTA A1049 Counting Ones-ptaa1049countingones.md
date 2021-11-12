---
title: PTA A1049 Counting Ones
date: 2021-07-13 19:53:30.846
updated: 2021-07-13 19:53:30.846
url: https://pumpkn.xyz/archives/ptaa1049countingones
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805430595731456)
![image.png](https://pumpkn.xyz/upload/2021/07/image-5266d7bc22f7486796e8003b2c7f494b.png)
# 题意
给定一个整数N，计算从1到N的所有数中，包含数字'1'的个数。如12，从1到12的所有数中，含1的有1、10、11、12，即出现了5个1，所以输出结果5。

# 题解
遍历1到N的每一个数i，将i转换成字符串格式，然后遍历字符串的每一位。

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int n ;
    cin >> n ;
    int num = 0 ;
    for( int i = 1 ; i <= n ; i++ ){
        stringstream ss ;
        ss << i ;
        string str = ss.str() ;
        for( int j = 0 ; j <str.length() ; j++ ){
            if( str[j] == '1' ){
                num ++ ;
            }
        }

    }

    cout << num ;

    return 0 ;
}

```