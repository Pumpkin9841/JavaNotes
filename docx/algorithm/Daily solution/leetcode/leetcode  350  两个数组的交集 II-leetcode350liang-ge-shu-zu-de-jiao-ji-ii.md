---
title: leetcode  350  两个数组的交集 II
date: 2021-03-08 12:48:40.222
updated: 2021-03-17 12:48:57.978
url: https://pumpkn.xyz/archives/leetcode350liang-ge-shu-zu-de-jiao-ji-ii
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-81f839b742b243f99e3d20b7ac844186.png)

# 题解
没有另开数组保存值。
# 代码
```c++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        sort( nums1.begin() , nums1.end() ) ;
        sort( nums2.begin() , nums2.end() ) ;
        vector<int> res ;
        
        int len_1 = nums1.size() ;
        int len_2 = nums2.size() ;
        
        if( len_1 >= len_2){
            int j = 0 ;
            for( int i = 0 ; i < len_1 ; i++ ){
                if( j >= len_2){
                    break;
                }else{
                    if( nums1[i] == nums2[j]){
                        res.push_back(nums1[i]) ;
                        j++ ;
                    }else if( nums1[i] > nums2[j] ){
                        j++ ;
                        i--;
                    }else{
                        continue ;
                    }
                }

            }
        }else{
            int i = 0 ;
            for( int j = 0 ; j < len_2 ; j++ ){
                if( i >= len_1){
                    break;
                }else{
                    if( nums1[i] == nums2[j]){
                        res.push_back(nums2[j]) ;
                        i++ ;
                    }else if( nums1[i] > nums2[j] ){
                        continue ;
                    }else{
                        i++ ;
                        j--;
                    }
                }

            }            
        }



        return res ;
    }
};
```