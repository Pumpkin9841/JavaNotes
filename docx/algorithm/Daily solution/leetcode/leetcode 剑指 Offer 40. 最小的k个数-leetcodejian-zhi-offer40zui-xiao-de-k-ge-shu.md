---
title: leetcode 剑指 Offer 40. 最小的k个数
date: 2021-03-21 23:36:06.063
updated: 2021-05-25 23:36:41.985
url: https://pumpkn.xyz/archives/leetcodejian-zhi-offer40zui-xiao-de-k-ge-shu
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-f95034ac015746a482925acd620e93a8.png)
# 题解
题目太简单，没有题解。

# 代码
```c++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
            vector<int> res ;
            sort(arr.begin() , arr.end() ) ;
            for( int i = 0 ; i < k ; i++ ){
                res.push_back(arr[i]) ;
            }
            return res ;
    }
};
```