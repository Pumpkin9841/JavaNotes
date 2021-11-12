---
title: leetcode 345. 反转字符串中的元音字母
date: 2021-08-19 13:20:47.995
updated: 2021-08-19 13:20:47.995
url: https://pumpkn.xyz/archives/leetcode345fan-zhuan-zi-fu-chuan-zhong-de-yuan-yin-zi-mu
categories: 算法
tags: 积累 | 算法 | 双指针
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-8bd88968916549d687bb6a7cb9750d08.png)

# 题解

- 双指针思想：

	- 同时定义两个指针i,j，分别指向字符串s的首位和末尾，如果i和j所指的字符都是元音字符，则交换，然后将i向右走1位，j向左走1位。
	- 如果i所指不是元音字母，直接将i向右走1位
	- 如果j所指不是元音字母，直接将j向左走1位
	- 直到i > j 

注意元音字母包括大小写。


# 代码
```C++
class Solution {
public:

    vector<char> vowel{'a' , 'e' ,'i' ,'o' ,'u' } ; 
    bool isVowel( char c ){
        for( int i = 0 ; i < vowel.size() ; i++ ){
            if( c <= 'Z' && c >= 'A' ){
                c = c + 32 ;
            }

            if( vowel[i] == c ){
                return true ;
            }
        }
        return false ;
    }
    string reverseVowels(string s) {
        int i = 0 ;  //指向开头
        int j = s.size() - 1 ;  //指向末尾
        while( i < j ){
             if( isVowel (s[i]) && isVowel (s[j]) ){
                 swap(s[i] , s[j]) ;
                 i ++ ;
                 j -- ;
                 continue ;
             }

            if( !isVowel (s[i]) ){
                i ++ ;
            }

            if( !isVowel (s[j]) ){
                j-- ;
            }
        }
        return s ;

    }
};
```