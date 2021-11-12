---
title: leetcode 1491. 去掉最低工资和最高工资后的工资平均值
date: 2021-03-16 22:58:49.69
updated: 2021-03-18 23:00:59.636
url: https://pumpkn.xyz/archives/1491qu-diao-zui-di-gong-zi-he-zui-gao-gong-zi-hou-de-gong-zi-ping-jun-zhi
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/average-salary-excluding-the-minimum-and-maximum-salary/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-51f39abcb94643b5b5acb6dfa5847508.png)
# 代码

```c++
class Solution {
public:
    double average(vector<int>& salary) {
        sort( salary.begin() , salary.end() ) ;
        int sum = 0 ;
        for( int i = 1 ; i < salary.size() - 1 ; i++ ){
            sum += salary[i] ;
        }
        return sum / ( (salary.size()-2)*1.0 ) ;
    }
};
```