---
title: leetcode 274. H 指数
date: 2021-05-20 16:58:23.425
updated: 2021-05-28 16:58:43.924
url: https://pumpkn.xyz/archives/leetcode274hzhi-shu
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/h-index/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-23a0628f344f4edb910ff792a04501dd.png)
# 题解
脑筋急转弯题目，看个乐吧

# 代码
```c++
class Solution {
public:
    int hIndex(vector<int>& citations) {
        sort(citations.begin(), citations.end());
        int i = 0;
        while(i < citations.size() && citations[citations.size() - 1 - i] > i) i++;
        return i;
    }
};
```