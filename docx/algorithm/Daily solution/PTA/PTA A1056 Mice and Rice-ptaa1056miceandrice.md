---
title: PTA A1056 Mice and Rice
date: 2021-07-22 17:14:59.707
updated: 2021-07-22 17:16:28.553
url: https://pumpkn.xyz/archives/ptaa1056miceandrice
categories: 算法
tags: 算法 | STL | queue
---

# 题目
## [原题链接](链接)
![image.png](https://pumpkn.xyz/upload/2021/07/image-3bb1f41cce7944fa86471306c321207d.png)

# 题意
![image.png](https://pumpkn.xyz/upload/2021/07/image-153e07eace9346e1aab6d6348a4d3205.png)

# 样例解释
![image.png](https://pumpkn.xyz/upload/2021/07/image-1be0b68cb96b41d48430059d2b8c4ae9.png)

# 题解
![image.png](https://pumpkn.xyz/upload/2021/07/image-1ca49c5caae84315a69bbfc2823bf68c.png)
![image.png](https://pumpkn.xyz/upload/2021/07/image-40bff1215b2a46a3ad429a003b26df3c.png)


# 代码

```c++
#include<cstdio>
#include<iostream>
#include<queue>
using namespace std ;

const int maxn = 1010 ;

struct Mice{
    int rank ;
    int weight ;
}mice[maxn];


int main(){

    int np , ng ;
    cin >> np >> ng ;
    for( int i = 0 ; i < np ; i++ ){
        int weight ;
        cin >> weight ;
        mice[i].weight = weight ;
    }

    int order ;
    queue<int> q ;
    for( int i = 0 ; i < np ; i++ ){
        cin >> order ;
        q.push(order) ;   //按顺序把老鼠们入队
    }



    int temp = np ,group ;  //temp为当前轮比赛总老鼠数，group为组数
    while( q.size() != 1 ){
        //能刚好分成group组
        if( temp % ng == 0 ){
            group = temp / ng ;
        }
        else{
            group = temp / ng + 1 ;
        }

        //枚举每一组，选出质量最大的
        for( int i = 0 ; i < group ; i++ ){
            int k = q.front() ;   //k存放该组质量最大的老鼠编号
            for( int j = 0 ; j < ng ; j++ ){
                //在最后一组老鼠不足ng时起作用
                if( i * ng + j >= temp ){
                    break ;
                }
                int front = q.front() ;  //队首老鼠编号

                if( mice[front].weight > mice[k].weight ){
                    k = front ;   //找出质量最大的
                }

                mice[front].rank = group + 1 ;  //该轮老鼠排名为group+1
                q.pop() ;   //出队这只老鼠
            }
            q.push(k) ;  //把该轮胜利的老鼠加入队列
        }
        temp = group ;   //group只老鼠晋级，因此下轮总老鼠数为group
    }
    mice[q.front()].rank = 1 ;

    for( int i = 0 ; i < np ; i++ ){
        cout << mice[i].rank ;
        if( i < np - 1 ){
            cout << " " ;
        }
    }

    return 0 ;
}

```