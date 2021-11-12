---
title: leetcode 461. 汉明距离
date: 2021-05-27 18:09:21.092
updated: 2021-05-27 18:10:02.761
url: https://pumpkn.xyz/archives/leetcode461han-ming-ju-li
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/hamming-distance/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-6458d12f59c14787b554e9e3a0adab36.png)
# 题解
![image.png](https://pumpkn.xyz/upload/2021/05/image-ba2e36f1540f48fab94db512e819f683.png)


# 代码
```c++
class Solution {
public:
    int hammingDistance(int x, int y) {
        return __builtin_popcount(x ^ y);
    }
};
```

![image.png](https://pumpkn.xyz/upload/2021/05/image-e1b5b9b05aed4cfd8f01dbafbe2dc74a.png)

```c++

class Solution {
public:
    int hammingDistance(int x, int y) {
        int s = x ^ y, ret = 0;
        while (s) {
            ret += s & 1;
            s >>= 1;
        }
        return ret;
    }
};

```