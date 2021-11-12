---
title: leetcode 57. 插入区间
date: 2021-05-26 17:12:36.777
updated: 2021-05-26 17:12:36.777
url: https://pumpkn.xyz/archives/leetcode57cha-ru-qu-jian
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/insert-interval/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-5b31ed5a55f947a79cfae0a343c7b48d.png)
# 题解
跟[leetcode 56. 合并区间](https://pumpkn.xyz/archives/leetcode56he-bing-qu-jian)思路是一样的，只需先将newInterval加入二维数组intervals中，就转化为了[leetcode 56. 合并区间](https://pumpkn.xyz/archives/leetcode56he-bing-qu-jian)问题。

# 代码
```c++
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> res ;
        //对intervals长度为0时进行特判
        if( intervals.size() == 0 ){
            intervals.push_back(newInterval) ;
            return intervals ;
        }

        intervals.push_back(newInterval) ;
        //按照区间端点升序排序
        sort( intervals.begin() , intervals.end() ) ;

        for( int i = 0 ; i < intervals.size() ; i++ ){
            int L = intervals[i][0] ;
            int R = intervals[i][1] ;
            if( !res.size() || res.back()[1] < L ){
                res.push_back( {L,R} ) ;
            }
            else{
                res.back()[1] = max( res.back()[1] , R ) ;
            }
        }

        return res ;
    }
};
```