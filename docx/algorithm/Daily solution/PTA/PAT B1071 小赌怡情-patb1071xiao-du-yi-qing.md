---
title: PAT B1071 小赌怡情
date: 2021-01-27 21:49:16.917
updated: 2021-03-03 21:49:58.34
url: https://pumpkn.xyz/archives/patb1071xiao-du-yi-qing
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805264312549376)
![image.png](https://pumpkn.xyz/upload/2021/03/image-1843a3021d3b406faa7a38fb41127f89.png)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int n , k ;
    int n1 , b ,t ,n2 ;
    bool flag = true ;
    cin >> n >> k ;
    for(int i = 0 ; i < k ; i++){
        cin >> n1 >> b >> t >> n2 ;

        if( n <= 0 && flag == true){
            flag = false ;
            cout << "Game Over."  ;
        }

        if( n > 0){
            if(t > n ){
                cout << "Not enough tokens.  Total = " << n << "." << endl ;
            }else{
                if( (n2 < n1 && b == 0) || ( n2 > n1 && b == 1 )){
                    n = n + t ;
                    cout << "Win " << t << "!  Total = " << n <<"." <<endl ;
                }else{
                    n = n - t ;
                    cout << "Lose " << t << ".  Total = " << n << "." <<endl  ;
                }
            }
        }
    }

    return 0 ;
}

```