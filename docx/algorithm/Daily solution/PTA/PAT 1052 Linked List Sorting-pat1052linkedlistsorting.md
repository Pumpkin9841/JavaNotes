---
title: PAT 1052 Linked List Sorting
date: 2021-02-20 17:54:41.386
updated: 2021-02-20 17:55:48.077
url: https://pumpkn.xyz/archives/pat1052linkedlistsorting
categories: 算法
tags: 积累 | 算法 | 链表 | 静态链表
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805425780670464)
![PAT A1052 Linked List Sorting.jpg](https://pumpkn.xyz/upload/2021/02/PAT%20A1052%20Linked%20List%20Sorting-bd0732a906f24ef293b67250be66f3ca.jpg)
# 题意
给出N个节点的地址address，数据key，指针域next，要求以key升序重新排序链表并输出。
# 题解
&ensp;&ensp;1.地址范围不大，采用静态链表</br>
&ensp;&ensp;2.将链表所有节点的key初始化为maxn，然后依次输入节点的address，key，next。</br>
&ensp;&ensp;3.根据节点key的值可以遍历出有效节点。将有效节点存入链表res中。</br>
&ensp;&ensp;4.以节点key升序的顺序对链表res排序。</br>
&ensp;&ensp;5.每次顺序输出res当前节点的address，key，next为当前节点下一个节点的address。</br>
&ensp;&ensp;6.<font color = "red" > 若当前节点为最后一个节点，则next直接输出-1 </font></br>
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 100010 ;

struct Node{
    int key , address , next ;
}node[maxn],res[maxn];

//以节点的key升序排序
bool cmp(Node a , Node b){
    return a.key < b.key ;
}

int main(){
    //初始化链接所有节点的key为maxn
    for(int i = 0 ; i < maxn ; i++ ){
        node[i].key = maxn ;
    }

    //输入节点总数，链表开始地址，节点地址
    int n , bgin , address ;
    cin >> n >> bgin ;

    if( n == 0 ){
        printf("0 -1") ;
        return 0 ;
    }

    for(int i = 0 ; i < n ; i++ ){
        cin >> address ;
        node[address].address = address ;
        cin >> node[address].key >> node[address].next ;
    }

    int p = bgin ;
    int resCount = 0 ;
    //将有效节点依次赋值给链表res
    while( p != -1 ){
        if( node[p].key != maxn ){
            res[resCount++] = node[p] ;
        }
        p = node[p].next ;
    }

    //以节点的key升序排序
    sort(res , res+resCount , cmp) ;

    printf("%d %05d\n" , resCount , res[0].address) ;
    for(int i = 0 ; i < resCount ; i++ ){
        //若当前节点不是最后一个节点
        if( i+1 < resCount ){
            printf("%05d %d %05d\n" ,res[i].address , res[i].key ,res[i+1].address) ;
        }else{
            printf("%05d %d -1\n" ,res[i].address , res[i].key) ;
        }
    }

    return 0 ;
}

```