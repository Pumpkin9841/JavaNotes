---
title: leetcode 349 两个数组的交集
date: 2021-03-07 11:51:01.245
updated: 2021-03-17 11:52:27.544
url: https://pumpkn.xyz/archives/leetcode349liang-ge-shu-zu-de-jiao-ji
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/intersection-of-two-arrays/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-d43fb904e2b2423981a64c9aaf883ffc.png)
# 题解
题目太简单，没啥可说的
# 代码
```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        set<int > set1 ;
        set<int > set2 ;
        for( int i = 0 ; i < nums1.size() ; i++ ){
            set1.insert(nums1[i]) ;
        }

        for( int i = 0 ; i < nums2.size() ; i++ ){
            set2.insert(nums2[i]) ;
        }

        vector<int> res ;
        for( set<int>::iterator it = set1.begin() ; it != set1.end() ; it++ ){
            for( set<int>::iterator it2 = set2.begin() ; it2 != set2.end() ; it2++ ){
                if( *it == *it2){
                    res.push_back(*it) ;
                }
            }
        }

        return res ;
    }
};
```