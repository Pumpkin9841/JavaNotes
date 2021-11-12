---
title: leetcode 554. 砖墙
date: 2021-05-02 15:23:25.859
updated: 2021-05-02 15:23:25.859
url: https://pumpkn.xyz/archives/leetcode554zhuan-qiang
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/brick-wall/)

![image.png](https://pumpkn.xyz/upload/2021/05/image-bdea32a940d948c587635e1942fa14a5.png)
#题解
使用 <font color = "red">**哈希表**</font></br>
题目要求穿过的砖块数量最少，等效于通过的间隙最多。</br>

我们可以使用「哈希表」记录每个间隙的出现次数，最终统计所有行中哪些间隙出现得最多，使用「总行数」减去「间隙出现的最多次数」即是答案。</br>

如何记录间隙呢？直接使用行前缀记录即可。</br>

![image.png](https://pumpkn.xyz/upload/2021/05/image-32ac4a3b4f314818a9adceee042aefb7.png)

第 1 行的间隙有 [1,3,5]</br>
第 2 行的间隙有 [3,4]</br>
第 3 行的间隙有 [1,4]</br>
第 4 行的间隙有 [2]</br>
第 5 行的间隙有 [3,4]</br>
第 6 行的间隙有 [1,4,5]</br>
对间隙计数完成后，遍历「哈希表」找出出现次数最多间隙 4，根据同一个间隙编号只会在单行内被统计一次，用总行数减去出现次数，即得到「最少穿过的砖块数」。

# 代码
```c++
class Solution {
public:
    int leastBricks(vector<vector<int>>& wall) {
        map< int , int > mp ;
        for( int i = 0 ; i < wall.size() ; i++ ){
            int tmp = 0 ;
            for( int j = 0 ; j < wall[i].size() ; j++ ){
                tmp += wall[i][j] ;
                mp[tmp] ++ ;
            }
        }

        int length = 0 ;
        for( int i = 0 ; i < wall[0].size() ; i++ ){
            length += wall[0][i] ;
        }

        mp.erase(length) ;

        int max = 0 ;
        for( auto it = mp.begin() ; it != mp.end() ; it++ ){
         //   cout << it->first  << "  "  << it->second <<endl ;
          if( it->second > max ){
                max = it->second ;
            }
        }
        return  wall.size() - max   ;
    }
};
```