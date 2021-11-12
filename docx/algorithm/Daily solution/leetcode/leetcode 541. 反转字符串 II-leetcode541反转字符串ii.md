---
title: leetcode 541. 反转字符串 II
date: 2021-08-20 13:19:20.204
updated: 2021-08-20 13:51:50.323
url: https://pumpkn.xyz/archives/leetcode541反转字符串ii
categories: 算法
tags: 积累 | 算法 | string | 二分
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/reverse-string-ii/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-6684e015f4ab4fb6918a7ed0e8465dd3.png)
# 题解

- 运用双指针的思想：

	- 定义变量current指向当前未反转的第一个字符的位置
	- 定义变量LeftLength表示未经过反转操作的字符长度
	- 循环反转操作

		- 若LeftLength小于k，则将剩余字符全部反转，并退出循环
		- 若LeftLength大于等于k并且LeftLength小于2k，反转当[current,current+k)范围内的字母，并退出循环
		- 否则，每次将[current,current+k)范围内的字母反转，并将current指向下一次反转的首字母，即current = current + 2*k ，同时 LeftLength减少2*k。


# 代码

## C++版
```C++
class Solution {
public:
    string reverseStr(string s, int k) {
        int double_K = 2 * k ;
        int LeftLength = s.length() ;
        int current = 0 ;
        while( true ){

            if( LeftLength < k ){
                reverse(s.begin()+current , s.begin()+LeftLength+current ) ;
                break ;
            }

            if( LeftLength < double_K && LeftLength >= k ){
                reverse( s.begin()+current , s.begin()+current+k ) ;
                break ;
            }



            reverse( s.begin()+current , s.begin()+current+k ) ;
            current = current + double_K ;
            LeftLength = LeftLength - double_K ; 
        }

        return s ;

    }
};
```


## Java版
```Java
class Solution {
    public String reverseStr(String s, int k) {
        int double_k = 2 * k ;
        StringBuilder sb = new StringBuilder(s) ;
        StringBuilder ans = new StringBuilder() ;
        int LeftLength = sb.length() ;
        int current = 0 ;

        while( true ){
            if( LeftLength < k ){
                sb.reverse() ;
                ans.append(sb) ;
                break ;
            }

            if( LeftLength >= k && LeftLength < double_k ){
                String temp = sb.substring( current , current+k ) ;
                StringBuilder TempSb = new StringBuilder(temp) ;
                TempSb.reverse() ;
                ans.append(TempSb.toString()) ; 
                String leftTemp = sb.substring( current+k , current+LeftLength ) ;
                ans.append(leftTemp) ;
                break ;

            }

            String temp = sb.substring( current , current+k ) ;
            StringBuilder TempSb = new StringBuilder(temp) ;
            TempSb.reverse() ;
            ans.append(TempSb.toString()) ; 
            String leftTemp = sb.substring( current+k , current + double_k  ) ;
            ans.append(leftTemp) ;
            sb.delete( current , current+double_k ) ;
           // current = current + double_k ;
            LeftLength = LeftLength - double_k ;

        }
        return ans.toString() ;
    }
}
```