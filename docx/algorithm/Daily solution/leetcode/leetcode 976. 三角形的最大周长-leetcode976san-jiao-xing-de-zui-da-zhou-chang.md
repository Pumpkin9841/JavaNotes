---
title: leetcode 976. 三角形的最大周长
date: 2021-03-10 21:17:04.0
updated: 2021-03-17 21:19:13.67
url: https://pumpkn.xyz/archives/leetcode976san-jiao-xing-de-zui-da-zhou-chang
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/largest-perimeter-triangle/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-a618d20eaae4465eb2f7980bb00959b2.png)
# 题解
> 为什么只需要判断数组中相邻的三个数？

在固定最后一个数 A[i] 时，前两个数需不需要再往前找呢？</br>

如果 A[i-2] + A[i-1] <= A[i] ，这三个数一定不能构成三角形，而A[i-3]以及更往前的数，都小于等于A[i-2]，所以再往前取任何两个数只会让相加的值更小，就更不能满足 A[j] + A[k] > A[i]了 (j<i-2, k<i-1, j<k)。所以如果相邻的数构不成三角形，就不需要再固定第三个数并往前找两个数了。</br>

如果 A[i-2] + A[i-1] > A[i]j，这三个数可以构成三角形，再往前找只会让周长变短，所以也不用再往前了。</br>

综上，只需要判断相邻的三个数。</br>

# 代码
```c++
class Solution {
public:
    int largestPerimeter(vector<int>& nums) {
            sort(nums.begin() , nums.end()) ;
            for(int i = nums.size()-1 ; i >= 2 ; --i){
                if( nums[i-2] + nums[i-1] > nums[i] )
                return nums[i] + nums[i-1] + nums[i-2] ;
            }
          return 0 ;
    }
};
```