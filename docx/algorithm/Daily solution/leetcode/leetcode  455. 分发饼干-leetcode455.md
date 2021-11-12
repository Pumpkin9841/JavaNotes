---
title: leetcode  455. 分发饼干
date: 2021-08-06 12:25:39.509
updated: 2021-08-06 12:26:06.017
url: https://pumpkn.xyz/archives/leetcode455
categories: 算法
tags: 学习 | 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/assign-cookies/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-12d8b1c8d8c443548e33fe6a42fe2318.png)
# 代码
```C++
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort( g.begin() , g.end() ) ;
        sort( s.begin() , s.end() ) ;
        int num = 0 ;
        int i = 0 ;
        int j = 0 ;
        while( i < g.size() && j < s.size() ){
            if( s[j] >= g[i] ){
                j++ ;
                i++ ;
                num++ ;
            }
            else{
                j++ ;
            }
        }
        return num ;
    }
};
```