---
title: PAT B1056 组合数的和
date: 2021-01-23 20:57:11.028
updated: 2021-02-25 20:57:54.89
url: https://pumpkn.xyz/archives/patb1056zu-he-shu-de-he
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805271455449088)
![image.png](https://pumpkn.xyz/upload/2021/02/image-d9740eea7a6c40778c9b1254b2411040.png)

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int n ;
    cin >> n ;
    int num[n] ;
    for(int i = 0 ; i < n ; i++){
        cin >> num[i] ;
    }

    vector<int > zuhe ;
    for(int i = 0 ; i < n ; i++){
        for(int j = 0 ; j < n ; j ++){
            if( num[i] != num[j]){
                int tmp = num[i]*10 + num[j] ;
                zuhe.push_back(tmp) ;
            }
        }
    }

    int sum = 0 ;
    for(int i = 0 ; i < zuhe.size() ; i++){
        sum += zuhe[i] ;
    }
    cout << sum ;

    return 0 ;
}

```