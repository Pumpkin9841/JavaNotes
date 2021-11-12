---
title: leetcode 1528. 重新排列字符串
date: 2021-03-12 00:09:10.0
updated: 2021-03-18 00:09:55.872
url: https://pumpkn.xyz/archives/leetcode1528zhong-xin-pai-lie-zi-fu-chuan
categories: 算法
tags: 算法 | 排序
---

# 题目
## 
![image.png](https://pumpkn.xyz/upload/2021/03/image-8e36ba069340431b98b34f4cfc8266c3.png)

# 题解
利用了map存放每个字符的正确顺序。

# 代码
```c++
class Solution {
public:
    string restoreString(string s, vector<int>& indices) {

            map<int ,char> mp ;
            for(int i = 0 ; i < s.length() ; i++){
                mp[ indices[i] ] = s[i] ; 
            }

            sort(indices.begin() , indices.end()) ;
            string res ;
            for( int i = 0 ; i < indices.size() ; i++ ){
                res.push_back( mp[indices[i]] );
            }
          
            return res ;
        
    }    
};
```