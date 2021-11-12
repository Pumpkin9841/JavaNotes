---
title: PAT A1054 The Dominant Color
date: 2021-02-10 19:15:18.245
updated: 2021-02-20 15:53:01.782
url: https://pumpkn.xyz/archives/pata1054thedominantcolor
categories: 算法
tags: 积累 | 算法 | map   
---

# 题目
![PATA1054.jpg](https://pumpkn.xyz/upload/2021/02/PATA1054-b4e9de86997248ccb31d24d169b566cc.jpg)
# 题意
给出N行M列的数字整列，求其中超过半数的出现次数的数字，题目保证该数一定存在且唯一。
# 题解
题目很简单，值得注意的是题目中颜色的范围为[0,2^24)，用int到int的映射即数组就能表示，但是如果范围改为[0,2^31)或更大，用数组是不可行的，所以想到string到int的映射。本题题解采用steing到int的映射。
***
<font color="red">map的find()方法，返回迭代器it，若it != mp.end() 表示找到,否则，没找到</font>

# 代码
```c++
#include<cstdio>
#include<string>
#include<map>
#include<iostream>
#include<vector>
using namespace std ;


int main(){
    int M , N ;
    cin >> M >> N ;
    map<string , int> mp ;
    for(int i = 0 ; i < N ; i ++){
        for(int j = 0 ; j < M ; j++){
            string tmp ;
            cin >> tmp ;
            map<string , int >::iterator it = mp.find(tmp) ;
            //如果mp中已经存在该颜色，将数量加1
            if(it != mp.end()){
                int num = it->second  ;
                mp[tmp] = num + 1 ;
            }
            //如果mp不存在改颜色，添加该颜色，并将数量设置为1
            else{
                mp[tmp] = 1 ;
            }
        }
    }
    string res ;
    int maxColor = 0 ;
    for(map<string , int>::iterator it = mp.begin() ; it != mp.end() ; it++ ){
        if(it->second > maxColor){
            res = it->first ;
            maxColor = it->second ;
        }

    }

    cout << res ;


    return 0 ;
}

```