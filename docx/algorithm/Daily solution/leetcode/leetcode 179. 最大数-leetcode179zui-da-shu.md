---
title: leetcode 179. 最大数
date: 2021-05-27 19:33:17.175
updated: 2021-05-27 19:33:17.175
url: https://pumpkn.xyz/archives/leetcode179zui-da-shu
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/largest-number/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-1ee1c452fa974e0cba516b19e11caa45.png)
# 题解
需要比较输入数组的每个元素的最高位，最高位相同的时候比较次高位只需要比较 
 ```c++
string a ;
string b ; 
a + b > b + a  ;
```
即可
# 代码
```c++
class Solution {
public:

    /**需要比较输入数组的每个元素的最高位，最高位相同的时候比较次高位
       只需要比较 
       string a , string b ; 
       a + b > b + a  即可
    */
    static bool cmp( long a , long b){
        string strA = to_string(a) ;
        string strB = to_string(b) ;
        return strA+strB > strB+strA ;
    }

    string largestNumber(vector<int>& nums) {
        
        long sum = 0 ;
        //对nums为全0的情况进行特判
        for( int i = 0 ; i < nums.size() ; i++ ){
            sum += nums[i] ;
        }
        if( sum == 0 ){
            return "0" ;
        }
 

        sort( nums.begin() , nums.end() , cmp ) ;
        string res ;
        for( int i = 0 ; i < nums.size() ; i++ ){
            string tmpStr = to_string(nums[i]) ;
            res += tmpStr ;
        }
        return res ;
    }
};
```