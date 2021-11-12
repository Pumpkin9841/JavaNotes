---
title: leetcode 7. 整数反转
date: 2021-05-03 13:56:00.345
updated: 2021-05-03 13:56:00.345
url: https://pumpkn.xyz/archives/leetcode7zheng-shu-fan-zhuan
categories: 算法
tags: 积累 | 算法
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/reverse-integer/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-3d9006337b4f447b8121c4d037b90640.png)
# 题解
特判负数情况，直接写出MAX_INT，进行特判。
# 代码
```c++
class Solution {
public:
    
    int reverseAll( int x ){
        stringstream ss ;
        ss << x ;
        string s = ss.str() ;
        string ans ;
        for( int i = s.length()-1; i >=0  ; i--){
            ans += s[i] ;
        }
        
        stringstream stmp ;
        stmp << ans ;
        int tmp ;
        stmp >> tmp ;

        if( 2147483647 == tmp ){
            return 0 ;
        }
        return tmp ;
    }

    int reverse(int x) {
        
        if( x < 0 ){
            return -reverseAll( abs(x) ) ;
        }else{
            return reverseAll(x) ;
        }
        
    }
};
```