---
title: leetcode 268. 丢失的数字
date: 2021-08-06 11:56:05.702
updated: 2021-08-06 11:56:05.702
url: https://pumpkn.xyz/archives/leetcode268diu-shi-de-shu-zi
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](链接)

![image.png](https://pumpkn.xyz/upload/2021/08/image-8e265e6355f847b3920ac0b6712b9bb4.png)
# 代码
```C++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
       int n = nums.size() ;
       sort( nums.begin() , nums.end() ) ;
       for(int i = 0 ; i < nums.size() ; i++){
           if( i != nums[i] ){
               return i ;
           }
       }
       return n ;
    }
};
```