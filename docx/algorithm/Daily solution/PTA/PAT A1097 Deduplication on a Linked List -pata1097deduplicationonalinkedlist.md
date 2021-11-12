---
title: PAT A1097 Deduplication on a Linked List 
date: 2021-02-20 18:51:45.516
updated: 2021-02-20 18:53:34.673
url: https://pumpkn.xyz/archives/pata1097deduplicationonalinkedlist
categories: 算法
tags: 积累 | 算法 | 链表 | 静态链表
---

# 题目
![PAT A1097 Deduplication on a Linked List.jpg](https://pumpkn.xyz/upload/2021/02/PAT%20A1097%20Deduplication%20on%20a%20Linked%20List-30fa1580b63549abba9d889a24cfc162.jpg)
# 题意
给出N个节点的地址address，数据key，指针域next。去掉数据重复的节点(重复的节点只保留第一个节点)，根据给出的链表起始地址，输出去重后的链表，并且将删去的节点组成一个链表，将其一并输出。
# 题解
&ensp;&ensp;1.建立一个数组dup，标记key是否重复：初始化dup值为10010。</br>
&ensp;&ensp;2.遍历链表，若当前节点的key的绝对值为tmp，查询dup[tmp]是否为10010，若是，则表示该节点没有重复，将该节点添加到结果链表res中，并将dup[tmp]的值设为1，表示该节点已存在；若不等于10010，说明当前节为重复节点，将当前节点加入删除链表dedup中。</br>
&ensp;&ensp;3.依次输出结果链表res，删除链表dedup。</br>
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 100010 ;
const int keyMaxn = 10010 ;

struct Node{
    int key , address , next ;
}node[maxn],res[maxn] , dedup[maxn];
//若dup[i]!= keyMaxn 则说明i重复了
int dup[keyMaxn];

int main(){
    //初始化dup
    for(int i = 0 ; i < keyMaxn ; i++ ){
        dup[i] = keyMaxn ;
    }

    int bgin , n , address ;
    cin >> bgin >> n ;
    for(int i = 0 ; i < n ; i ++){
        cin >> address ;
        node[address].address = address ;
        cin >> node[address].key >> node[address].next ;
    }

    int dCount = 0;
    int resCount = 0 ;
    int p = bgin ;
    while(p != -1){
        int tmp = abs(node[p].key) ;
        //重复
        if(dup[tmp] != keyMaxn){
            dedup[dCount++] = node[p] ;         //重复了，将该节点删除，并加入删除链表。
        }
        //没有重复
        else{
            dup[tmp] = 1 ;     //标记该key已存在
            res[resCount++] = node[p] ;     //没有重复，将该节点赋值给res链表
        }

        p = node[p].next ;
    }



    for(int i = 0 ; i < resCount ; i++){
        //当前节点不是最后一个节点
        if( i+1 < resCount){
            printf("%05d %d %05d\n" ,res[i].address , res[i].key , res[i+1].address) ;
        }else{
             printf("%05d %d -1\n" ,res[i].address , res[i].key ) ;
        }
    }

     for(int i = 0 ; i < dCount ; i++){
        //当前节点不是最后一个节点
        if( i+1 < dCount){
            printf("%05d %d %05d\n" ,dedup[i].address , dedup[i].key , dedup[i+1].address) ;
        }else{
             printf("%05d %d -1\n" ,dedup[i].address , dedup[i].key ) ;
        }
    }

    return 0 ;
}

```