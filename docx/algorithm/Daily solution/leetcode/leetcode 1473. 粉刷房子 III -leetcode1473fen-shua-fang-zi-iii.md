---
title: leetcode 1473. 粉刷房子 III 
date: 2021-05-04 15:14:26.823
updated: 2021-05-04 15:14:26.823
url: https://pumpkn.xyz/archives/leetcode1473fen-shua-fang-zi-iii
categories: 算法
tags: 动态规划
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/paint-house-iii/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-b1f439f069d546f2b8353d7acd2e410a.png)
# 代码
```c++
class Solution {
private:
    // 极大值
    // 选择 INT_MAX / 2 的原因是防止整数相加溢出
    static constexpr int INFTY = INT_MAX / 2;

public:
    int minCost(vector<int>& houses, vector<vector<int>>& cost, int m, int n, int target) {
        // 将颜色调整为从 0 开始编号，没有被涂色标记为 -1
        for (int& c: houses) {
            --c;
        }

        // dp 所有元素初始化为极大值
        vector<vector<vector<int>>> dp(m, vector<vector<int>>(n, vector<int>(target, INFTY)));

        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (houses[i] != -1 && houses[i] != j) {
                    continue;
                }
                
                for (int k = 0; k < target; ++k) {
                    for (int j0 = 0; j0 < n; ++j0) {
                        if (j == j0) {
                            if (i == 0) {
                                if (k == 0) {
                                    dp[i][j][k] = 0;
                                }
                            }
                            else {
                                dp[i][j][k] = min(dp[i][j][k], dp[i - 1][j][k]);
                            }
                        }
                        else if (i > 0 && k > 0) {
                            dp[i][j][k] = min(dp[i][j][k], dp[i - 1][j0][k - 1]);
                        }
                    }

                    if (dp[i][j][k] != INFTY && houses[i] == -1) {
                        dp[i][j][k] += cost[i][j];
                    }
                }
            }
        }

        int ans = INFTY;
        for (int j = 0; j < n; ++j) {
            ans = min(ans, dp[m - 1][j][target - 1]);
        }
        return ans == INFTY ? -1 : ans;
    }
};

```