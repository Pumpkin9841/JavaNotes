---
title: leetcode 147. 对链表进行插入排序
date: 2021-05-26 18:55:08.942
updated: 2021-05-26 18:55:08.942
url: https://pumpkn.xyz/archives/leetcode147dui-lian-biao-jin-xing-cha-ru-pai-xu
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/insertion-sort-list/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-a74e9547eb1c434c8f4b38d825d0a686.png)
# 题解

做关于链表题时多打草稿可以做得很轻松</br>

**插入排序**的基本思想是，维护一个有序序列，初始时有序序列只有一个元素，每次将一个新的元素插入到有序序列中，将有序序列的长度增加 11，直到全部元素都加入到有序序列中。</br>

如果是数组的插入排序，则数组的前面部分是有序序列，每次找到有序序列后面的第一个元素（待插入元素）的插入位置，将有序序列中的插入位置后面的元素都往后移动一位，然后将待插入元素置于插入位置。</br>

对于链表而言，插入元素时只要更新相邻节点的指针即可，不需要像数组一样将插入位置后面的元素往后移动，因此插入操作的时间复杂度是 O(1)O(1)，但是找到插入位置需要遍历链表中的节点，时间复杂度是 O(n)O(n)，因此链表插入排序的总时间复杂度仍然是 O(n^2),n 是链表的长度。</br>

对于单向链表而言，只有指向后一个节点的指针，因此需要从链表的头节点开始往后遍历链表中的节点，寻找插入位置。</br>

对链表进行插入排序的具体过程如下。</br>

&ensp;&ensp;&ensp;1.首先判断给定的链表是否为空，若为空，则不需要进行排序，直接返回。</br>

&ensp;&ensp;&ensp;2.创建哑节点 dummyHead，令 dummyHead.next = head。引入哑节点是为了便于在 head 节点之前插入节点。</br>

&ensp;&ensp;&ensp;3.维护 lastSorted 为链表的已排序部分的最后一个节点，初始时 lastSorted = head。</br>

&ensp;&ensp;&ensp;4.维护 curr 为待插入的元素，初始时 curr = head.next。</br>

&ensp;&ensp;&ensp;5.比较 lastSorted 和 curr 的节点值。</br>

- 若 lastSorted.val <= curr.val，说明 curr 应该位于 lastSorted 之后，将 lastSorted 后移一位，curr 变成新的 lastSorted。</br>
- 否则，从链表的头节点开始往后遍历链表中的节点，寻找插入 curr 的位置。令 prev 为插入 curr 的位置的前一个节点，进行如下操作，完成对 curr 的插入：</br>
```c++
lastSorted.next = curr.next
curr.next = prev.next
prev.next = curr
```
&ensp;&ensp;&ensp;6.令 curr = lastSorted.next，此时 curr 为下一个待插入的元素。</br>

&ensp;&ensp;&ensp;7.重复第 5 步和第 6 步，直到 curr 变成空，排序结束。</br>

&ensp;&ensp;&ensp;8.返回 dummyHead.next，为排序后的链表的头节点。</br>

*题解来源于[leetcode官方题解](https://leetcode-cn.com/problems/insertion-sort-list/solution/dui-lian-biao-jin-xing-cha-ru-pai-xu-by-leetcode-s/)*

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

    ListNode* insertionSortList(ListNode* head) {
       //对空链表情况进行特判
       if( head == nullptr ){
           return head ;
       }

        //创建哑节点
        ListNode* dummyHead = new ListNode(0) ;
        dummyHead->next = head ;
        ListNode* lastsorted = head ;
        ListNode* curr = lastsorted->next ;

        while( curr != nullptr ){
            if( lastsorted->val <= curr->val ){
                lastsorted = curr ;
                curr = curr->next ;
            }
            else{
                ListNode* pre = dummyHead ;
                //找到插入位置
                while( pre->next->val <= curr->val ){
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