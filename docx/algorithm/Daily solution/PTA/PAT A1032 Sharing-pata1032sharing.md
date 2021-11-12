---
title: PAT A1032 Sharing
date: 2021-02-19 14:25:00.442
updated: 2021-02-20 15:43:23.377
url: https://pumpkn.xyz/archives/pata1032sharing
categories: 算法
tags: 积累 | 算法 | 链表 | 静态链表
---

# 题目
## [题目链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805460652113920)
![PAT A1032 Sharing.jpg](https://pumpkn.xyz/upload/2021/02/PAT%20A1032%20Sharing-8cd99474826547c299d8aaadcdf44ba9.jpg)

# 题意
给出两条链表的首地址以及N个节点的地址、数据、下一个节点的地址。若两条链表有公共后缀，则输出公共后缀第一个节点的地址(注意格式，不足五位前补0),若没有，则输出-1.
# 题解
&ensp;&ensp;1.由于地址范围很小，采用静态链表。</br>
&ensp;&ensp;2.使用快慢指针，快慢指针初始化为各链表的起始节点的地址。</br>
&ensp;&ensp;3.计算两条链表长度之差step,将快指针指向链表长度较长的一条，并先向前移动step个节点，然后快慢指针同时移动，每移动一位，比较节点是否相同，若相同，则停止循环，该节点则会公共后缀，返回该节点地址；若不相同，继续向前移动，直到链表结束。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 100010 ;

struct Node{
    int address , next , order ;
    char data ;
}node[maxn];

int main(){
    int word1_bgin  , word2_bgin , total , address ;
    cin >> word1_bgin >> word2_bgin >> total ;
    for(int i = 0 ; i < total ; i++){
        cin >> address ;
        node[address].address = address ;
        cin >> node[address].data >> node[address].next ;
    }

    int w1 = word1_bgin , w2 = word2_bgin ;
    int w1_len = 0 , w2_len = 0 ;
    //计算单词1的长度
    while(w1 != -1){
        w1_len ++ ;
        w1 = node[w1].next ;
    }

    //计算单词2的长度
     while(w2 != -1){
        w2_len ++ ;
        w2 = node[w2].next ;
    }

    //快指针比慢指针快的步数
    int step = abs(w2_len - w1_len) ;


    //单词1比单词2长，则快指针指向单词1 ,慢指针指向单词2
    if(w1_len >= w2_len){

        int fast = word1_bgin ;  //快指针
        int slow = word2_bgin ;     //慢指针

	//快指针先向前移动step个节点
        while( step > 0){
            step -- ;
            fast = node[fast].next ;
        }
	
	//若链表未到末尾且当前节点不是公共后缀节点，指针继续向前移动
        while( fast != -1 && node[fast].address != node[slow].address ){
            fast = node[fast].next ;
            slow = node[slow].next ;
        }

       // cout << fast << endl ;
        // printf("%05d\n" , fast) ;
         if(fast == -1){
            cout << fast << endl ;
         }else{
            printf("%05d\n" , fast) ;
         }
    }
    //否则快指针指向单词2
    else{

        int fast = word2_bgin ;  //快指针
        int slow = word1_bgin ;     //慢指针

	//快指针先向前移动step个节点
        while( step > 0){
            step -- ;
            fast = node[fast].next ;
        }

	//若链表未到末尾且当前节点不是公共后缀节点，指针继续向前移动
        while( fast != -1 && node[fast].address != node[slow].address ){
            fast = node[fast].next ;
            slow = node[slow].next ;
        }

       if(fast == -1){
            cout << fast << endl ;
         }else{
            printf("%05d\n" , fast) ;
         }
    }


    return 0 ;
}

```