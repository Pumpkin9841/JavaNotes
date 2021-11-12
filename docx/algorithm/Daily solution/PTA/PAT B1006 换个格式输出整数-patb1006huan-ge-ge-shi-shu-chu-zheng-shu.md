---
title: PAT B1006 换个格式输出整数
date: 2021-01-15 01:31:27.0
updated: 2021-02-23 01:33:42.679
url: https://pumpkn.xyz/archives/patb1006huan-ge-ge-shi-shu-chu-zheng-shu
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805318855278592)
![PAT B1006.jpg](https://pumpkn.xyz/upload/2021/02/PAT%20B1006-52f97a7e478b442491c79bcf3b08d61d.jpg)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int n ;
    cin >> n ;
    int bai = n/100 ;
    int shi = (n - bai*100)/10 ;
    int ge = n - bai*100 - shi * 10 ;

    for(int i = 0 ; i < bai ; i++){
        cout << "B" ;
    }

    for(int i = 0 ; i < shi ; i++){
        cout << "S" ;
    }

    for(int i = 1 ; i <= ge ; i++ ){
        cout << i ;
    }

    return 0 ;
}

```