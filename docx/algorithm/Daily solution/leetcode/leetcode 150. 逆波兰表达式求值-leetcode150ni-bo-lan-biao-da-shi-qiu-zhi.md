---
title: leetcode 150. 逆波兰表达式求值
date: 2021-03-20 16:06:13.816
updated: 2021-03-20 16:06:13.816
url: https://pumpkn.xyz/archives/leetcode150ni-bo-lan-biao-da-shi-qiu-zhi
categories: 算法
tags: 算法 | 逆波兰式 | 栈
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-2b2fe29e54a24c71be6699f77d89329e.png)
# 题解
初始化一个栈，用来存储操作数，遍历tokens，如果是数字，则将其压入栈中，若遇到操作符，将两个栈顶元素依次出栈top1，top2，计算```res = top2 opertor top1 ```,再将res压入栈中。直到遍历完整个tokens，最后返回栈顶元素即可。
# 代码
```c++
class Solution {
public:

    bool strIsNum(string s){
        
        if( s[0] == '-'){
            if( s.length() == 1 ){
                return false ;
            }
            for( int i = 1 ; i < s.length() ; i++ ){
                if( s[i] > '9' || s[i] < '0' ){
                    return false ;
                }
            }
        }else{
            for( int i = 0 ; i < s.length() ; i++ ){
                if( s[i] > '9' || s[i] < '0' ){
                    return false ;
                }
            }
        }
        return true ;
    }

    int evalRPN(vector<string>& tokens) {
        stack<int> number ;

        for( int i = 0 ; i < tokens.size() ; i++ ){
             string tmp = tokens[i] ;
             if(strIsNum(tmp)){
                stringstream ss ;
                ss << tmp ;
                int tmp_str ;
                ss >> tmp_str ;
                 number.push(tmp_str) ;
             }else{
                 int top1 = number.top() ;
                 number.pop() ;
                 int top2 = number.top() ;
                 number.pop() ;
                 int res ;
                 if( tmp == "+" ){
                    res = top2 + top1 ;
                    number.push(res) ;
                 }

                 if( tmp == "-" ){
                     res = top2 - top1 ;
                     number.push(res) ;
                 }

                 if( tmp == "*" ){
                     res = top2 * top1 ;
                     number.push(res) ;
                 }

                 if( tmp == "/" ){
                     res = top2 / top1 ;
                     number.push(res) ;
                 }

             }
        }
        return number.top() ;

    }
};
```
