---
title: leetcode 1636. 按照频率将数组升序排序
date: 2021-03-13 00:40:22.246
updated: 2021-03-18 00:43:29.781
url: https://pumpkn.xyz/archives/leetcode1636an-zhao-pin-lv-jiang-shu-zu-sheng-xu-pai-xu
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/sort-array-by-increasing-frequency/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-9120ead8768b4e7fa76dec75a8ad84ae.png)

# 题解
nums的区间位[-100 , 100] ,用数组存放会出现下标为负的情况。我的做法是将整个区间向右平移100个单位到[0,200]。这样就解决了下标出现负的错误，输出时只需将数据-100即可。
# 代码
```c++
class Solution {
public:

    struct  Node{
        int frenquency ; //频率
        int data ;  
    }node[210];

    static bool cmp( Node a , Node b ){
        if( a.frenquency == b.frenquency ){
            return a.data > b.data ;
        }else{
            return a.frenquency < b.frenquency ;
        }
    } ;
    vector<int> frequencySort(vector<int>& nums) {
        //初始化结构体
        for( int i = 0 ; i < 210 ; i++ ){
            node[i].frenquency = 0 ;
            node[i].data = 0 ;
        }

        for( int i = 0 ; i < nums.size() ; i++ ){
            int tmp = nums[i]+100 ;
            node[tmp].data = tmp ;
            node[tmp].frenquency ++ ;
        }

        sort(node , node+210 , cmp) ;
        vector<int> res ;
        for( int i = 0 ; i < 210 ; i++ ){
            if( node[i].frenquency != 0 ){
                for( int j = 0 ; j < node[i].frenquency ; j++ ){
                    res.push_back(node[i].data-100) ;
                }
            }
        }
        return res ;
    }
};
```