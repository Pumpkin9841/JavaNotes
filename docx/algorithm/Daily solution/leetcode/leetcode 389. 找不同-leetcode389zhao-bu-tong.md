---
title: leetcode 389. 找不同
date: 2021-08-06 01:06:07.703
updated: 2021-08-06 01:06:07.703
url: https://pumpkn.xyz/archives/leetcode389zhao-bu-tong
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/find-the-difference/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-52545f73afd146e28438d9961495ac4e.png)
# 代码
```C++
class Solution {
public:
    char findTheDifference(string s, string t) {
        sort( s.begin() , s.end()) ;
        sort( t.begin() , t.end() ) ;
        int i = 0 ;
        int j = 0 ;
        while( i < s.length() && j < t.length() ){
            if( s[i] != t[j] ){
                return t[j] ;
            }
            i++ ;
            j++ ;
        }
        return t[ t.length()-1 ] ;
    }
};
```