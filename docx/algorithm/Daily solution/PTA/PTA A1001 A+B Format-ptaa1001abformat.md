---
title: PTA A1001 A+B Format
date: 2021-05-31 17:12:46.902
updated: 2021-05-31 17:12:46.902
url: https://pumpkn.xyz/archives/ptaa1001abformat
categories: 算法
tags: 算法 | string
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805528788582400)
![image.png](https://pumpkn.xyz/upload/2021/05/image-6ac75dc7db8645f88187376f15c7262e.png)

# 题解
先求出A+B的和sum，并根据sum的正负判断应该输出的符号。</br>
将sum转成string型，从右往左每隔3个字符就输出一个','(从右往左的原因：若sum为5位数字12345 ,正向标准化则为123,45。显然这是错误的格式，正确应为12,345) , 若已经隔了3个字符但当前字符已为最后一个字符也不输出',' ,最后将逆序输出res即可。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int a, b ;
    cin >> a >>b ;
    int sum = a + b ;
    if( sum < 0 ){
        cout << "-" ;
    }
    sum = abs(sum) ;

    stringstream ss ;
    ss << sum ;
    string str = ss.str() ;

    vector<char> res ;

    int j = 1 ;
    for( int i = str.length()-1 ; i >= 0 ; i-- ){
        res.push_back(str[i]) ;

        if( j % 3 == 0 && i != 0){
            res.push_back(',') ;
        }
        j++ ;
    }

    reverse(res.begin() , res.end()) ;
    for( int i = 0 ; i < res.size() ; i++ ){

        cout << res[i] ;
    }


    return 0 ;
}

```