---
title: leetcode 242 有效的字母异位词
date: 2021-03-06 01:09:24.211
updated: 2021-03-17 01:09:48.146
url: https://pumpkn.xyz/archives/leetcode242you-xiao-de-zi-mu-yi-wei-ci
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/valid-anagram/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-de1974c8dece4c18b1881d31702656cf.png)
```c++
class Solution {
public:


    bool isAnagram(string s, string t) {
        bool flag = true ;

        int leng_s = s.length() ;
        int leng_t = t.length() ;

        if( leng_s != leng_t ){
            return false ;
        }

        char cs[leng_s] ;
        char ct[leng_t] ;
        for( int i = 0 ; i < leng_s ; i++ ){
            cs[i] = s[i] ;
        }
        for( int i = 0 ; i < leng_t ; i++ ){
            ct[i] = t[i] ;
        }


        sort( cs , cs+leng_s ) ;
        sort(ct , ct+leng_t ) ;


        for( int i = 0 ; i < s.length() ; i++ ){
            if( cs[i] != ct[i] ){
                flag = false ;
                break ;
            }
        }

        return flag ;
    }
};
```