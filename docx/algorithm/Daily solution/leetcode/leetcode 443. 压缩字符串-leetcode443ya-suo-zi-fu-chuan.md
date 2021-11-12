---
title: leetcode 443. 压缩字符串
date: 2021-08-21 12:46:30.485
updated: 2021-08-21 12:46:30.485
url: https://pumpkn.xyz/archives/leetcode443ya-suo-zi-fu-chuan
categories: 算法
tags: 学习 | 算法 | string
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/string-compression/)

![image.png](https://pumpkn.xyz/upload/2021/08/image-3e392530bb3744e3b7279082ec5c20bc.png)

# 代码
```C++
class Solution {
public:
    int compress(vector<char>& chars) {
        vector<char> ans ;
       // sort( chars.begin() , chars.end() ) ;
        chars.push_back(' ');
        char pre = chars[0] ;
        int num = 1 ;
        for( int i = 1 ; i < chars.size() ; i++ ){
            if( chars[i] == pre ){
                num ++ ;
            }
            else{
                if( num >= 10 ){
                    stringstream ss ;
                    ss << num ;
                    string tempNum ;
                    tempNum = ss.str() ;
                    ans.push_back(pre) ;
                    for( int j = 0 ; j < tempNum.length() ; j++ ){
                        ans.push_back(tempNum[j]) ;
                    }
                }
                else if( num == 1 ){
                    ans.push_back(pre) ;
                   // ans.push_back(num + '0') ;
                }
                else{
                    ans.push_back(pre) ;
                    ans.push_back(num + '0') ;
                }
                pre = chars[i] ;
                num = 1 ;
            }

        }
        // for( int i = 0 ; i < ans.size() ; i++ ){
        //     cout << ans[i] ;
        // }

        chars = ans ;
        return ans.size() ;
    }
};
```