---
title: PAT B1043 输出PATest
date: 2021-01-12 18:54:01.758
updated: 2021-02-20 15:55:56.178
url: https://pumpkn.xyz/archives/patb1043shu-chu-patest
categories: 算法
tags: 积累 | 算法
---

# 题目
![PATB1043.jpg](https://pumpkn.xyz/upload/2021/02/PATB1043-514b20f7fe7c4377b877e92291489b24.jpg)
# 代码
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<string>
using namespace std ;

int main(){
    char str[10010] ;
    int i = 0 ;
    int HashTable[128] = {0} ;
    fgets(str , 10010 , stdin) ;  //输入str
    while(str[i] != '\n')
        i++ ;
    str[i] = '\0' ;
    int len = strlen(str) ;
    for(int j = 0 ; j < len ; j++){
        char c = str[j] ;
        if(c == 'P' || c == 'A' || c == 'T' || c == 'e' || c == 's' || c == 't'){
            HashTable[c]++ ;
        }
    }

   while(HashTable['P'] !=0 ){
        printf("P") ;
            HashTable['P']-- ;
        if(HashTable['A'] !=0 ){
           printf("A") ;
            HashTable['A']-- ;
        }
        if(HashTable['T'] !=0 ){
           printf("T") ;
           HashTable['T']-- ;
        }
        if(HashTable['e'] !=0 ){
           printf("e") ;
           HashTable['e']-- ;
        }
        if(HashTable['s'] !=0 ){
           printf("s") ;
           HashTable['s']-- ;
        }
        if(HashTable['t'] !=0 ){
           printf("t") ;
           HashTable['t']-- ;
        }
    }

    while(HashTable['A'] !=0 ){
        printf("A") ;
        HashTable['A']-- ;
        if(HashTable['T'] !=0 ){
           printf("T") ;
           HashTable['T']-- ;
        }
        if(HashTable['e'] !=0 ){
           printf("e") ;
           HashTable['e']-- ;
        }
        if(HashTable['s'] !=0 ){
           printf("s") ;
           HashTable['s']-- ;
        }
        if(HashTable['t'] !=0 ){
           printf("t") ;
           HashTable['t']-- ;
        }
    }

    while(HashTable['T'] !=0 ){
        printf("T") ;
        HashTable['T']-- ;
        if(HashTable['e'] !=0 ){
           printf("e") ;
           HashTable['e']-- ;
        }
        if(HashTable['s'] !=0 ){
           printf("s") ;
           HashTable['s']-- ;
        }
        if(HashTable['t'] !=0 ){
           printf("t") ;
           HashTable['t']-- ;
        }
    }

    while(HashTable['e'] !=0 ){
        printf("e") ;
        HashTable['e']-- ;
        if(HashTable['s'] !=0 ){
           printf("s") ;
           HashTable['s']-- ;
        }
        if(HashTable['t'] !=0 ){
           printf("t") ;
           HashTable['t']-- ;
        }
    }
    while(HashTable['s'] !=0 ){
        printf("s") ;
        HashTable['s']-- ;
        if(HashTable['t'] !=0 ){
           printf("t") ;
           HashTable['t']-- ;
        }
    }
    while(HashTable['t'] !=0 ){
        printf("t") ;
        HashTable['t']-- ;
    }

    return 0 ;
}

```