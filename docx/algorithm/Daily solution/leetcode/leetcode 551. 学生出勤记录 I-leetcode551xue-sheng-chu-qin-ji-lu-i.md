---
title: leetcode 551. 学生出勤记录 I
date: 2021-08-17 10:48:38.22
updated: 2021-08-17 10:48:38.22
url: https://pumpkn.xyz/archives/leetcode551xue-sheng-chu-qin-ji-lu-i
categories: 算法
tags: 学习 | 积累 | 算法
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/student-attendance-record-i/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-d857043a724d4ec4b108dbbbf726d7de.png)

## 代码

```C++
class Solution {
public:
    bool checkRecord(string s) {
        int Anums = 0 ;
        int LContinueNums = 0 ;

        for( int i = 0 ; i < s.length() ; i++ ){
            //如果迟到天数不是连续且少于3天，则置0
            if( s[i] != 'L' && LContinueNums < 3 ){
                LContinueNums = 0 ;
            }

            if( s[i] == 'L' ){
                LContinueNums ++ ;
            }

            if( s[i] == 'A' ){
                Anums ++ ;
            }

        }
        //缺勤少于两天并且迟到小于三天，返回true
        if( Anums < 2 && LContinueNums < 3 ){
            return true ;
        }
        else{
            return false ;
        }
    }
};
```