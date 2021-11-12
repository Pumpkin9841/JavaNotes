---
title: leetcode 561. 数组拆分 I
date: 2021-08-06 13:04:52.589
updated: 2021-08-06 13:04:52.589
url: https://pumpkn.xyz/archives/leetcode561shu-zu-chai-fen-i
categories: 算法
tags: 学习 | 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/array-partition-i/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-788b5758237c47c3a1db854a09dbc431.png)

# 题解
先将数组升序排序，再依次两两分为一组，将每组的最小值累计求和即可。

# 代码
```C++
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        sort( nums.begin() , nums.end() ) ;

        int group = nums.size() / 2 ;
        int sum = 0 ;
        int index = 0 ;
        for( int i = 0 ; i < group ; i++ ){
            sum += min( nums[index] , nums[++index] ) ;
            index ++ ;
        }
        return sum ;
    }
};
```