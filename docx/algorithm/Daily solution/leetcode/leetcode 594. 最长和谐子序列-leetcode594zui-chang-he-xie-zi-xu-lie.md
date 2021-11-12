---
title: leetcode 594. 最长和谐子序列
date: 2021-08-06 14:05:46.66
updated: 2021-08-06 14:05:46.66
url: https://pumpkn.xyz/archives/leetcode594zui-chang-he-xie-zi-xu-lie
categories: 算法
tags: 学习 | 算法 | map   
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-51fc9cc8be8f49abafb2323c860ac99c.png)

# 题解
第一次遍历构建映射关系的，即数字到数量</br>
第二次遍历映射关系，找到比它1的数量，有的话，两者相加，取最大和就是结果

# 代码
```C++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        unordered_map<int , int> mp ;
        int n = nums.size() ;
        for( int i = 0 ; i < n ; i++ ){
            int temp = nums[i] ;
            ++ mp[temp] ;
        }

        int res = 0 ;   
        for( auto it = mp.begin() ; it != mp.end() ; it++ ){
            int current = it->first ;
            if( mp.find(current+1) != mp.end() ){
                res = max( res , it->second + mp.find(current+1)->second ) ;
            }
        }
        return res ;

    }
};
```