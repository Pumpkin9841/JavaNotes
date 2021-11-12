---
title: leetcode 506. 相对名次
date: 2021-08-06 12:49:37.205
updated: 2021-08-06 12:49:37.205
url: https://pumpkn.xyz/archives/leetcode506xiang-dui-ming-ci
categories: 算法
tags: 学习 | 算法 | 排序
---

# 题目

## [原题链接](https://leetcode-cn.com/problems/relative-ranks/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-c1c9bb8c23084bd49987a0a40c6c5aea.png)
# 代码
```C++
class Solution {
public:



    static bool cmp( int a , int b ){
        return a > b ;
    }

    vector<string> findRelativeRanks(vector<int>& score) {
        vector<int> temp = score ;
        vector<string> ans ;
        sort( temp.begin() , temp.end() , cmp ) ;
        map<int , string> mp ;


        for( int i = 0 ; i < temp.size() ; i++ ){
            if( i < 3 ){
                if( i == 0 ){
                    mp[ temp[0] ] = "Gold Medal" ;
                }
                else if( i == 1 ){
                    mp[ temp[1] ] = "Silver Medal" ;
                }
                else{
                    mp[ temp[2] ] = "Bronze Medal" ;
                }
            }
            else{
                mp[ temp[i] ] = to_string( i+1 ) ;
            }
        }

        for( int i = 0 ; i < score.size() ; i++ ){
            ans.push_back( mp[ score[i] ] ) ;
        }

        return ans ;
    }
};
```