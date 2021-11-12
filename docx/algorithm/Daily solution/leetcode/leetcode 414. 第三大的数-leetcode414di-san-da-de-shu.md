---
title: leetcode 414. 第三大的数
date: 2021-08-06 12:16:32.06
updated: 2021-08-06 12:16:32.06
url: https://pumpkn.xyz/archives/leetcode414di-san-da-de-shu
categories: 算法
tags: 学习 | 积累 | 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/third-maximum-number/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-f345874907c140448e1729f146c036f6.png)

# 代码
```C++
class Solution {
public:
    int thirdMax(vector<int>& nums) {

        set<int> st ;
        for( int i = 0 ; i < nums.size() ; i++ ){
            st.insert( nums[i] ) ;
        }

        if( st.size() < 3 ){
            auto it = st.end() ;
            it -- ;
            return  *( it )  ;
        }

        auto it = st.end() ;
        it -- ;
        it -- ;
        it -- ;
        return *(it) ;
    }
};
```