---
title: leetcode 1370. 上升下降字符串
date: 2021-03-19 00:20:58.693
updated: 2021-03-19 00:20:58.693
url: https://pumpkn.xyz/archives/leetcode1370shang-sheng-xia-jiang-zi-fu-chuan
categories: 算法
tags: 算法 | STL
---

# 题目
##  [原题链接](https://leetcode-cn.com/problems/increasing-decreasing-string/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-92b51fb686364f1eb2ee98a3da1a448d.png)

# 代码

```c++
class Solution {
public:
    string sortString(string s) {
        vector<int> num(26);
        for (char &ch : s) {
            num[ch - 'a']++;
        }

        string ret;
        while (ret.length() < s.length()) {
            for (int i = 0; i < 26; i++) {
                if (num[i]) {
                    ret.push_back(i + 'a');
                    num[i]--;
                }
            }
            for (int i = 25; i >= 0; i--) {
                if (num[i]) {
                    ret.push_back(i + 'a');
                    num[i]--;
                }
            }
        }
        return ret;
    }
};
```