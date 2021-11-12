---
title: leetcode 905. 按奇偶排序数组
date: 2021-08-12 12:28:05.0
updated: 2021-08-12 12:28:05.0
url: https://pumpkn.xyz/archives/leetcode905an-qi-ou-pai-xu-shu-zu
categories: 算法
tags: 学习 | 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/sort-array-by-parity/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-325e997c4cda491b87eceecdc6557188.png)
# 代码
```C++
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& nums) {
        vector<int> ans ;
        vector<int> odd ; //奇数
        for( int i = 0 ; i < nums.size() ; i++ ){
            if( nums[i] % 2 == 0 ){
                ans.push_back(nums[i]) ;
            }
            else{
                odd.push_back(nums[i]) ;
            }
        }

        for( int i = 0 ; i < odd.size() ; i++ ){
            ans.push_back(odd[i]) ;
        }
        return ans ;
    }
};
```