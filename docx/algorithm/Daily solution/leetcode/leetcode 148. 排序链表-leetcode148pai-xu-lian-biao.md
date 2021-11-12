---
title: leetcode 148. 排序链表
date: 2021-05-27 00:19:45.217
updated: 2021-05-27 00:19:45.217
url: https://pumpkn.xyz/archives/leetcode148pai-xu-lian-biao
categories: 算法
tags: 算法 | 链表 | 排序
---

# 题目
##[原题链接](https://leetcode-cn.com/problems/sort-list/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-ee0ad020c7754c91858968656c53eb28.png)

# 题解
插入排序法思想，思路与[leetcode 147. 对链表进行插入排序](https://pumpkn.xyz/archives/leetcode147dui-lian-biao-jin-xing-cha-ru-pai-xu)相同。
# 代码
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        //对链表长度为0进行特判
        if(head == nullptr){
            return head ;
        }

        //创建头节点
        ListNode* dummyHead = new ListNode(0) ;
        dummyHead->next = head ;
        ListNode* lastsorted = head ;
        ListNode* curr = head->next ;
        while( curr != nullptr ){
            if( lastsorted->val <= curr->val ){
                lastsorted = curr ;
                curr = curr->next ;
            }
            else{
                ListNode* pre = dummyHead ;
                while( pre->next->val < curr->val ){
                    pre = pre->next ;
                }

                lastsorted->next = curr->next ;
                curr->next = pre->next ;
                pre->next = curr ;

                curr = lastsorted->next ;
            }

        }


        
        return dummyHead->next ;
    }
};
```