---
title: leetcode 1137. 第 N 个泰波那契数
date: 2021-08-08 14:30:54.294
updated: 2021-08-08 14:30:54.294
url: https://pumpkn.xyz/archives/leetcode1137di-nge-tai-bo-na-qi-shu
categories: 算法
tags: 算法 | 递归 | 动态规划
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/n-th-tribonacci-number/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-35f6b94d2aec47059fc21bd788245e8c.png)
# 题解
直接递归会超时，使用动态规划的时想，创建一个状态数组dp用来保存已经计算过的值。
# 代码
```C++
class Solution {
public:

    int dp[40] ;
    int tribonacci(int n) {
        
        if( n == 0 ){
            return 0 ;
        }
        else if( n == 1 || n == 2 ){
            return 1 ;
        }

        if( dp[n] != 0 ){
            return dp[n] ;
        }
        else{
            dp[n] = tribonacci(n-1) + tribonacci(n-2) + tribonacci(n-3) ;
            return dp[n] ;
        }

    }
};
```