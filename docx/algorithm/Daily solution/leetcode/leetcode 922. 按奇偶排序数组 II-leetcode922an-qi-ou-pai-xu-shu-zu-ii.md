---
title: leetcode 922. 按奇偶排序数组 II
date: 2021-03-09 13:27:43.236
updated: 2021-03-17 13:28:00.67
url: https://pumpkn.xyz/archives/leetcode922an-qi-ou-pai-xu-shu-zu-ii
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/sort-array-by-parity-ii/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-b67607c223be4a5ea860e383a6cbeae0.png)

# 题解
将原数组中奇数单独存放在一个数组odd中，偶数单独存放在数组even中，对应着 存进res中返回即可。

#代码
```c++
class Solution {
public:
    vector<int> sortArrayByParityII(vector<int>& nums) {
          vector<int> od ;  //奇数
        vector<int> even ;  //偶数
        vector<int> res ;
        for( int i = 0 ; i < nums.size() ; i++ ){
            if( nums[i] % 2 == 0  ){
                even.push_back(nums[i]) ;
            }else{
                od.push_back(nums[i]) ;
              
            }
        }

        for(int i = 0 ; i < nums.size() ; i++){
            if( i % 2 == 0 ){
                res.push_back( even.back() ) ;
                even.pop_back() ;
            }else{
                res.push_back(od.back()) ;
                od.pop_back() ;
            }
        }
        return res ;

    }
};
```