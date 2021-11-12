---
title: PAT B1011 A+B>C
date: 2021-01-16 01:19:58.198
updated: 2021-02-24 01:19:58.143
url: https://pumpkn.xyz/archives/patb1011abc
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805312417021952)
![PAT B1011.jpg](https://pumpkn.xyz/upload/2021/02/PAT%20B1011-bea8f1e535c34b90ac3b3ebeaf7258fc.jpg)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int t ;
    cin >> t ;
    for(int i = 1 ; i<=t ; i++){
        long A ,B ,C ;
        cin >> A >> B >> C ;
        if( (A+B) > C){
            cout << "Case #" << i <<": " << "true" << endl ;
        }else{
            cout << "Case #" << i <<": " << "false" << endl ;
        }
    }

    return 0 ;
}

```