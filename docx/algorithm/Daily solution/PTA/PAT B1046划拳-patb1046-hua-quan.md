---
title: PAT B1046划拳
date: 2021-01-22 22:14:08.243
updated: 2021-02-24 22:15:01.287
url: https://pumpkn.xyz/archives/patb1046-hua-quan
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805277847568384)
![image.png](https://pumpkn.xyz/upload/2021/02/image-8e55033a007b4f1eb57acca826eec2ee.png)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int n ;
    int resA = 0 , resB = 0 ;
    cin >> n ;
    for(int i = 0 ; i < n ; i++){
        int Ahan , Ahua , Bhan , Bhua ;
        cin >> Ahan >> Ahua >> Bhan >> Bhua ;
        int han_sum = Ahan+Bhan ;
        //甲划出的数等于两人喊出的数之和并且乙划出的数不等于两人喊出的数，甲赢
        if(Ahua == han_sum && Bhua != han_sum){
            resB++ ;
        }
        //乙赢
        if(Bhua == han_sum && Ahua != han_sum){
            resA++ ;
        }
    }

    cout << resA << " " <<resB ;


    return 0 ;
}

```