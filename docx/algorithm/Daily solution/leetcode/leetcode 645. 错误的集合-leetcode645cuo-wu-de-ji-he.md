---
title: leetcode 645. 错误的集合
date: 2021-08-07 12:03:51.553
updated: 2021-08-07 12:03:51.553
url: https://pumpkn.xyz/archives/leetcode645cuo-wu-de-ji-he
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/set-mismatch/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-9c319e5d12104f3a9d145fe19670db47.png)
# 代码
```C++
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        int vis[10010] ;
        fill(vis, vis+10010 , 0) ;
        for( int i = 0 ; i < nums.size() ; i++ ){
             vis[ nums[i] ] ++ ;
        }
        vector<int> ans ;
        int k ;
        for( int i = 1 ; i <= nums.size() ; i++ ){
            if( vis[i] == 2 ){
                ans.push_back(i) ;
            }
            if( vis[i] == 0 ){
                k = i  ;
            }
        }
        ans.push_back(k) ;
        return ans ;
    }
};
```