---
title: leetcode 56. 合并区间
date: 2021-05-26 12:28:08.839
updated: 2021-05-26 12:28:08.839
url: https://pumpkn.xyz/archives/leetcode56he-bing-qu-jian
categories: 算法
tags: 算法 | 排序
---

# 题目
# [原题链接](https://leetcode-cn.com/problems/merge-intervals/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-ed523e08cc3849aa85ed88c66b5fb941.png)

# 题解
![image.png](https://pumpkn.xyz/upload/2021/05/image-3ddbe1ae85ad43be876ad609fec38e10.png)
![image.png](https://pumpkn.xyz/upload/2021/05/image-236b6ac5dfe94ed59e9e62db8f86d47f.png)
*[转至leetcode官方题解](https://leetcode-cn.com/problems/merge-intervals/solution/he-bing-qu-jian-by-leetcode-solution/)*
# 代码
```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        //长度为0时进行特判
        if( intervals.size() == 0 ){
            return {} ;
        }

        sort( intervals.begin() , intervals.end() ) ;
        vector<vector<int>> res ;
        for( int i = 0 ; i < intervals.size() ; i++ ){
            int L = intervals[i][0] ;
            int R = intervals[i][1] ;

            if(  !res.size() || res.back()[1] < L ){
                res.push_back( {L,R} ) ;
            }
            else{
                res.back()[1] = max( res.back()[1] , R ) ;
            }


        }
        return res ;
    }
};
```

