---
title: leetcode 1859. 将句子排序
date: 2021-05-24 18:33:27.845
updated: 2021-05-24 18:33:27.845
url: https://pumpkn.xyz/archives/leetcode1859jiang-ju-zi-pai-xu
categories: 算法
tags: 算法 | map    | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/sorting-the-sentence/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-b072218e537d4abfa07206fc956bf7e6.png)

# 题解
将字符串以" "切割，用一个int到string的映射map<int , string>保存切割后的数据，同时将序号存入数组vector<int>中，只需将vector排序，输出map对应的值即可。
# 注意
char 转换为 int只需要
```
char c ;
int i = c - '0'
```
int 转换为 char 只需要+'0'即可。</br>
string.substr(start , end)方法截取的是[start , end),即左闭右开的范围。

# 代码
```c++
class Solution {
public:
    string sortSentence(string s) {
        map<int , string> mp ;
        vector<int> res ;
        while( s.find(" ") != string::npos ){
            int space = s.find(" ") ;
            string tmpStr = s.substr(0,space) ;
            s = s.substr(space+1 , s.length()) ;
            int len = tmpStr.length() ;
            int num = tmpStr[len-1] - '0' ;
            string ans = tmpStr.substr(0 , len-1) ;
            mp[num] = ans ;
            res.push_back(num) ;
        }
        int len = s.length() ;
        int num = s[len-1] - '0' ;
        string ans = s.substr(0 , len-1) ;
        mp[num] = ans ;
        res.push_back(num) ;
        sort(res.begin() , res.end()) ;

        string x ;
        for( int i = 1 ; i <= res.size() ; i++ ){
            x += mp[i] ;
            if( i != res.size() ){
               x += " " ;
            }
        }
        return x ;
    }
};
```