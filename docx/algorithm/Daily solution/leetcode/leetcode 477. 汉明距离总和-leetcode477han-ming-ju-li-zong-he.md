---
title: leetcode 477. 汉明距离总和
date: 2021-05-28 16:18:38.523
updated: 2021-05-28 16:18:38.523
url: https://pumpkn.xyz/archives/leetcode477han-ming-ju-li-zong-he
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/total-hamming-distance/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-cb6acf2893284623b4ddc53d484ae62d.png)
# 题解
## [宫水三叶题解](https://leetcode-cn.com/problems/total-hamming-distance/solution/gong-shui-san-xie-ying-yong-cheng-fa-yua-g21t/)

# 代码
```c++
class Solution {
public:
 
    int totalHammingDistance(vector<int>& nums) {
        int res = 0 ;
        for( int i = 31 ; i >= 0 ; i-- ){
            int s0 = 0 ;
            int s1 = 0 ;
            for( int x : nums ){
                if( ( x >> i ) & 1 ){
                    s1 ++ ;
                }
                else{
                    s0 ++ ;
                }
            }
            res += s0 * s1 ;    
        }
        return res ;
    }

};
```