---
title: leetcode 1122. 数组的相对排序
date: 2021-03-11 21:58:21.151
updated: 2021-03-17 21:58:38.263
url: https://pumpkn.xyz/archives/leetcode1122shu-zu-de-xiang-dui-pai-xu
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/relative-sort-array/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-bf762a08cc3c4ac29d93ce318e217990.png)

# 题解
利用空间换时间，另外数组用下标和值表示arr1与arr2的交集。

# 代码
```c++
class Solution {
public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
            vector<int> res ;
            vector<int> left ;
            int tmp[1010] ;
            int tmp_arr2[1010] = {0} ;
            for( int i = 0 ; i < 1009 ; i++ ){
                tmp[i] = 0 ;
            }

            for( int i = 0 ; i < arr1.size() ; i++){
                tmp[arr1[i]] ++ ;
            }
            for( int i = 0 ; i < arr2.size() ; i++ ){
                int t = tmp[arr2[i]] ;
                if( t != 0 ){
                    for( int j = 0 ; j < t ; j++ ){
                        res.push_back(arr2[i]) ;
                    }
                }
            }

            for( int i = 0 ; i < arr2.size() ; i++ ){
                tmp_arr2[arr2[i]] ++ ;
            }

            for( int i = 0 ; i < arr1.size() ; i++ ){
                if( tmp[arr1[i]] != 0 && tmp_arr2[arr1[i]] == 0  ){
                    left.push_back(arr1[i]) ;
                }
            }

            sort(left.begin() , left.end()) ;
            for( int i = 0 ; i < left.size() ; i++ ){
                res.push_back(left[i]) ;
            }
            
            return res ;
           
    }
};
```