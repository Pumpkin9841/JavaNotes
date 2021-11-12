---
title: leetcode 1502. 判断能否形成等差数列
date: 2021-03-18 23:24:15.731
updated: 2021-03-18 23:24:33.077
url: https://pumpkn.xyz/archives/leetcode1502pan-duan-neng-fou-xing-cheng-deng-cha-shu-lie
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/can-make-arithmetic-progression-from-sequence/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-8459f0ae1fa344c289c12a82fadec77d.png)

# 代码
```c++
class Solution {
public:
    bool canMakeArithmeticProgression(vector<int>& arr) {
        sort(arr.begin() , arr.end() ) ;
        int tmp = arr[1] - arr[0] ;
        for( int i = 1 ; i < arr.size() ; i++ ){
            if( i < arr.size()-1 ){
                if( arr[i] + tmp != arr[i+1] )
                    return false ;
            }
        }
        return true ;
    }
};
```