---
title: leetcode 1640. 能否连接形成数组
date: 2021-03-14 11:47:43.852
updated: 2021-03-18 11:48:10.498
url: https://pumpkn.xyz/archives/leetcode1640neng-fou-lian-jie-xing-cheng-shu-zu
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/check-array-formation-through-concatenation/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-0348849b5b7845aab81756ebce419cd1.png)

# 题解
排除所有错误的情况，返回true即可。
# 代码
```c++
class Solution {
public:
    bool canFormArray(vector<int>& arr, vector<vector<int>>& pieces) {
        int arr_p = 0 ;
        while( arr_p < arr.size() ){
            int p = 0 ;
            while( p < pieces.size() &&  pieces[p][0] != arr[arr_p] ){
                p++ ;
            }

            if( p >= pieces.size() ){
                return false ;
            }

            for( int i = 0 ; i < pieces[p].size() ; i++ ){
                if( pieces[p][i] != arr[arr_p] ){
                    return false ;
                }
                arr_p ++ ;
            }

        }
        return true ;
    }
};
```