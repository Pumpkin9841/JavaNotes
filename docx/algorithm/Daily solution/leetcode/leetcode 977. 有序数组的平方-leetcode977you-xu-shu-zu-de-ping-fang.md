---
title: leetcode 977. 有序数组的平方
date: 2021-08-11 21:51:34.538
updated: 2021-08-11 21:51:34.538
url: https://pumpkn.xyz/archives/leetcode977you-xu-shu-zu-de-ping-fang
categories: 算法
tags: 学习 | 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-971b0ff2a5a44d778fbe38c3a56a1242.png)
# 代码
```C++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int> ans ;
        for( int i = 0 ; i < nums.size() ; i++ ){
            int temp = nums[i] * nums[i] ;
            ans.push_back( temp ) ;
        }
        sort( ans.begin() , ans.end() ) ;
        return ans ;
    }
};
```