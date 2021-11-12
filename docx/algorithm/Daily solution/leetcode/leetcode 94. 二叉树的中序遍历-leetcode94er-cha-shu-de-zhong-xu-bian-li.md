---
title: leetcode 94. 二叉树的中序遍历
date: 2021-08-17 11:04:15.985
updated: 2021-08-17 11:04:15.985
url: https://pumpkn.xyz/archives/leetcode94er-cha-shu-de-zhong-xu-bian-li
categories: 
tags: 
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
![image.png](https://pumpkn.xyz/upload/2021/08/image-d3208d24c52c43878e3a6b3ba6aa3f82.png)

# 代码

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> ans ;
    vector<int> inorderTraversal(TreeNode* root) {
        if( root == nullptr ){
            return ans ;
        }

        inorderTraversal(root->left) ;
        ans.push_back(root->val) ;
        inorderTraversal(root->right) ;
        return ans ;
    }
};
```