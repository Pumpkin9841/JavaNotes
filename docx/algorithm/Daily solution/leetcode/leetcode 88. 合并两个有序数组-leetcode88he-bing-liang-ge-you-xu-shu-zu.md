---
title: leetcode 88. 合并两个有序数组
date: 2021-08-05 21:47:02.889
updated: 2021-08-05 21:47:02.889
url: https://pumpkn.xyz/archives/leetcode88he-bing-liang-ge-you-xu-shu-zu
categories: 算法
tags: 算法 | 双指针
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/merge-sorted-array/)

# 题解
[归并笔记](https://pumpkn.xyz/archives/-suan-fa-bi-ji-46twopointers)

# 代码
```C++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        vector<int> temp1 = nums1 ;
        nums1.clear() ;
        int i = 0 ; //指向nums1的left 初始为0 区间为[0 , m-1]
        int j = 0 ; //指向nums2的left 初始为0 区间为[0 , n-1]
        while(  (i <= m-1 ) && ( j <= n-1 ) ){
            if( temp1[i] <= nums2[j] ){
                nums1.push_back(temp1[i++]) ;
            }
            else{
                nums1.push_back(nums2[j++]) ;
            }
        }

        while( i <= m-1 ){
            nums1.push_back( temp1[i++] ) ;
        }

        while( j <= n-1 ){
            nums1.push_back( nums2[j++] ) ;
        }

    }
};
```