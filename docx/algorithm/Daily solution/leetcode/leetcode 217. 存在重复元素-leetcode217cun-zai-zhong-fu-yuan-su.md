---
title: leetcode 217. 存在重复元素
date: 2021-08-06 00:56:48.417
updated: 2021-08-06 00:56:48.417
url: https://pumpkn.xyz/archives/leetcode217cun-zai-zhong-fu-yuan-su
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](链接)
![image.png](https://pumpkn.xyz/upload/2021/08/image-5e333fe83a134b48858a72a4111a6aaf.png)

# 题解
将nums排序，后一个元素与前一个元素相比，若存在相同的情况，则返回true，若没有，则返回fasle

# 代码
```C++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        sort( nums.begin() , nums.end() ) ;
        int pre = nums[0] ;
        for( int i = 1 ; i < nums.size() ; i++ ){
            if( pre != nums[i] ){
                pre = nums[i] ;
            }
            else{
                return true ;
            }
        }
        return false ;
    }
};
```