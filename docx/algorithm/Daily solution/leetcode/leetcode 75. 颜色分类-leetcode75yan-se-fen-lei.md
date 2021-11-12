---
title: leetcode 75. 颜色分类
date: 2021-03-22 17:26:04.579
updated: 2021-05-26 17:26:10.808
url: https://pumpkn.xyz/archives/leetcode75yan-se-fen-lei
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/sort-colors/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-c9c9a796c999448cbc183cf64a01de6e.png)

# 题解
sort() 永远的神!
# 代码
```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        sort( nums.begin() , nums.end() ) ;
    }
};
```