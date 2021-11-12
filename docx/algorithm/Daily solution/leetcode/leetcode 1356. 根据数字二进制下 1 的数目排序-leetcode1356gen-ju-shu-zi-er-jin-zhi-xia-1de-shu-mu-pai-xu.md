---
title: leetcode 1356. 根据数字二进制下 1 的数目排序
date: 2021-03-17 23:16:24.01
updated: 2021-03-18 23:17:03.235
url: https://pumpkn.xyz/archives/leetcode1356gen-ju-shu-zi-er-jin-zhi-xia-1de-shu-mu-pai-xu
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/sort-integers-by-the-number-of-1-bits/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-da6f33913a7b461ca705c200692b4bc9.png)

# 代码
```c++
class Solution {
public:

    struct Node{
        int data ;
        int oneNum ; 
    }node[510] ;

    int getOne( int num ){
        int oneNum = 0 ;
        while( num > 0 ){
            if( num % 2 == 1 ){
                oneNum++ ;
            }
            num /= 2 ;
        }
        return oneNum ;
    }

    static  bool cmp( Node a , Node b ){
        if( a.oneNum == b.oneNum ){
            return a.data < b.data ;
        }else{
            return a.oneNum < b.oneNum ;
        }
    }

    vector<int> sortByBits(vector<int>& arr) {
        for( int i = 0 ; i < arr.size() ; i++){
            int tmp = getOne(arr[i]) ;
            node[i].oneNum = tmp ;
            node[i].data = arr[i] ;
        }
        vector<int> res ;
        sort( node , node+arr.size() , cmp );
        for( int i = 0 ; i < arr.size() ; i++ ){
            res.push_back(node[i].data) ;
        }
        return res ;
    }
};
```