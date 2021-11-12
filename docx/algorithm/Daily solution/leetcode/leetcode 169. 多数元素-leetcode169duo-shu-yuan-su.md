---
title: leetcode 169. 多数元素
date: 2021-08-05 22:28:43.081
updated: 2021-08-05 22:28:43.081
url: https://pumpkn.xyz/archives/leetcode169duo-shu-yuan-su
categories: 算法
tags: 学习 | 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/majority-element/submissions/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-c194fcf9675e449495625670fc8c6943.png)
# 题解
将数组排序，用num记录当前元素出现的个数，maxn记录出现最多的次数，k记录出现次数最多的元素值，从左往右依次读取数组元素，每遍历一个元素，若当前元素num[i]与上一个元素值相同则num+1 ，判断若num大于maxn，则执行k=num[i]。若与上一个元素值不同，再次做上诉判断。然后将temp更新为当前元素的值，num 设为1 

# 代码
```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort( nums.begin() , nums.end() ) ;
        
        int temp = nums[0] ;
        int num = 1 ;
        int k = nums[0] ;
        int maxNum = 0 ;
        for( int i = 1 ; i < nums.size() ; i++ ){
            if( nums[i] == temp ){
                num++ ;
                if( num > maxNum ){
                    maxNum = num ;
                    k = temp ;
                }
            }
            else{
                if( num > maxNum ){
                    maxNum = num ;
                    k = temp ;
                }
                temp = nums[i] ;
                num = 1 ; 
            }
        }

        return k ;
    }
};
```