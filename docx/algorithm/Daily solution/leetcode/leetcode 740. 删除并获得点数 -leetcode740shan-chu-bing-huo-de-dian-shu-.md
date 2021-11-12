---
title: leetcode 740. 删除并获得点数 
date: 2021-05-05 18:45:56.186
updated: 2021-05-05 18:45:56.186
url: https://pumpkn.xyz/archives/leetcode740shan-chu-bing-huo-de-dian-shu-
categories: 算法
tags: 算法 | 动态规划
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/delete-and-earn/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-382dd75ee56e4463a8d05890355d9d19.png)

# 代码

```c++
class Solution {
private:
    int rob(vector<int> &nums) {
        int size = nums.size();
        int first = nums[0], second = max(nums[0], nums[1]);
        for (int i = 2; i < size; i++) {
            int temp = second;
            second = max(first + nums[i], second);
            first = temp;
        }
        return second;
    }

public:
    int deleteAndEarn(vector<int> &nums) {
        int maxVal = 0;
        for (int val : nums) {
            maxVal = max(maxVal, val);
        }
        vector<int> sum(maxVal + 1);
        for (int val : nums) {
            sum[val] += val;
        }
        return rob(sum);
    }
};

```