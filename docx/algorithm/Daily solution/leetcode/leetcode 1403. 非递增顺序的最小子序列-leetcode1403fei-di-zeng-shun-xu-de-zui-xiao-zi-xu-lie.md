---
title: leetcode 1403. 非递增顺序的最小子序列
date: 2021-05-25 21:56:39.872
updated: 2021-05-25 21:56:39.872
url: https://pumpkn.xyz/archives/leetcode1403fei-di-zeng-shun-xu-de-zui-xiao-zi-xu-lie
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/minimum-subsequence-in-non-increasing-order/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-f0449483454147609a9bcd3360c7008b.png)

# 题解
**贪心算法**：因为给的 nums 数组中的元素不是连续的，而选择的子序列要满足该子序列的元素之和 **严格** 大于未包含在该子序列中的各元素之和。所以每次选择的元素值应是当前数组还未选择中的所有元素中最大的元素。</br>
开始时，要将 nums 数组进行排序，每次选择最大的元素。

# 代码
```c++
class Solution {
public:

    static bool cmp( int a , int b ){
        return a > b ;
    }

    vector<int> minSubsequence(vector<int>& nums) {
        sort( nums.begin() , nums.end() ) ;
        int maxIndex = nums.size() - 1 ;
        vector<int> res ;   //子序列
        int resSum = 0 ;    //子序列和
        vector<int> left ;  //剩余序列
        int leftSum = 0 ;  //剩余序列和
        res.push_back( nums[maxIndex] ) ;
        nums.pop_back() ;
        resSum += nums[maxIndex] ;
        for(int i = 0 ; i < nums.size() ; i++ ){
            left.push_back( nums[i] ) ;
            leftSum += nums[i] ;
        }
       
        while( resSum <= leftSum ){
            int last = nums.back() ;
            nums.pop_back() ;
            resSum += last ;
            leftSum -= last ;
            res.push_back(last) ;
            left.pop_back() ;
        }
        
        return res ;
    }
};
```