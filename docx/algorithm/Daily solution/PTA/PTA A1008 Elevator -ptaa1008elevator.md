---
title: PTA A1008 Elevator 
date: 2021-07-13 19:39:15.148
updated: 2021-07-13 19:39:15.148
url: https://pumpkn.xyz/archives/ptaa1008elevator
categories: 算法
tags: 
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805511923286016)
![image.png](https://pumpkn.xyz/upload/2021/07/image-6f2770a70d9149faa60edd70e4cd86a9.png)
# 题意
电梯每上升一层楼花6s,下降一层花4s，没到达一层等待5s。给一份需要到达楼层的清单，求出共花费的时间。（初始时电梯停在0层）

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int UP = 6 ;
const int DOWN = 4 ;
const int STAY = 5 ;

int main(){
    int n ;
    cin >> n ;
    vector<int> input ;
    for( int i = 0 ; i < n ; i++ ){
        int temp ;
        cin >> temp ;
        input.push_back(temp) ;
    }

    int totalTime = 0 ;  //花费的总时间
    int current = 0 ;   //当前所在层

    for( int i = 0 ; i < n ; i++ ){
        int temp = input[i] ;
        //电梯上行
        if( current < temp ){
            int sub = temp - current ;
            totalTime += ( sub*UP + STAY) ;
        }
        //电梯下行
        else if( current > temp ){
            int sub = current - temp ;
            totalTime += ( sub*DOWN + STAY) ;

        }
        else{
            totalTime += STAY ;
        }
        current = temp ;
    }

    cout << totalTime ;
    return 0 ;
}

```