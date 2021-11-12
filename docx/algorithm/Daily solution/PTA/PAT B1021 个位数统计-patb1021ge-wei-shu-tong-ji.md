---
title: PAT B1021 个位数统计
date: 2021-01-18 01:59:17.288
updated: 2021-02-24 01:59:33.956
url: https://pumpkn.xyz/archives/patb1021ge-wei-shu-tong-ji
categories: 算法
tags: 
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805300404535296)
![image.png](https://pumpkn.xyz/upload/2021/02/image-40be29939ea946fca06ec9be3febea95.png)

#代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int res[20] = {0} ;
    string str ;
    getline(cin , str) ;
    for(int i = 0 ; i < str.length() ; i++){
        int tmp = str[i]-'0' ;
        for(int j = 0 ; j < 10 ; j++){
            if(tmp == j){
                res[tmp]++ ;
            }
        }
    }

    for(int i = 0 ; i < 10 ; i ++){
        if(res[i] != 0){
            cout << i << ":" << res[i] <<endl  ;
        }
    }

    return 0 ;
}

```