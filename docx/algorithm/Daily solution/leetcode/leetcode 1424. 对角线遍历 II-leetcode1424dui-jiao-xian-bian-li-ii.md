---
title: leetcode 1424. 对角线遍历 II
date: 2021-04-07 19:51:32.653
updated: 2021-04-07 19:51:32.653
url: https://pumpkn.xyz/archives/leetcode1424dui-jiao-xian-bian-li-ii
categories: 算法
tags: 算法
---

#题目
## [原题链接](https://leetcode-cn.com/problems/diagonal-traverse-ii/)
![image.png](https://pumpkn.xyz/upload/2021/04/image-71a77a1fa07749fbb7fa962c0017b3e1.png)

# 题解
因为对角线上的数坐标i+j总是相同，直接遍历Nums，将i+j相同的数加入数组ans,又因为是从左到右，从上到下遍历ans的，所以得到的对角线上的顺序是反的，逆序加入res中即可。
# 代码
```c++
class Solution {
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& nums) {
        vector<int> ans[200000] ;
        for( int i = 0 ; i < nums.size() ; i++ ){
            for( int j = 0 ; j < nums[i].size() ; j++ ){
                int tmp = i + j ;
                ans[tmp].push_back(nums[i][j]) ;
            }
        } 
        vector< int > res ;
        for( int i = 0 ; i < 200000 ; i++ ){
            if( ans[i].size() == 0 ){
                break ; 
            }
            else{
                reverse(ans[i].begin() , ans[i].end()) ;
                for( int j = 0 ; j < ans[i].size() ; j++ ){
                    res.push_back(ans[i][j]) ;
                }
            }
        }
        return res ;
    }
};


```