---
title: PAT B1039 到底买不买
date: 2021-01-10 14:50:27.201
updated: 2021-02-20 15:56:24.106
url: https://pumpkn.xyz/archives/patb1039dao-di-mai-bu-mai
categories: 算法
tags: 积累 | 算法
---

# 题目
![PATB1039.jpg](https://pumpkn.xyz/upload/2021/02/PATB1039-8a664ef2a1d84598aca7a8fd36c18d1b.jpg)
# 代码
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<iostream>
using namespace std ;

int main(){
    char str1[1010] ;   //能买的串
    char str2[1010] ;   //需要的串

    // gets(str1) ;
    // gets(str2) ;

    int n = 0 , m = 0 ;
    fgets(str1, 1010, stdin);
    while (str1[n] != '\n')
        n++;
    str1[n] = '\0';
    fgets(str2 , 1010 , stdin) ;
    while(str2[m] != '\n')
        m++ ;
    str2[m] = '\0' ;

    int HashTable_str1[128] = {0} ;
    int HashTable_str2[128] = {0} ;
    int sum = 0 ;
    bool flag = true ;    //标记是否能买
    int len1 = strlen(str1) ;
    int len2 = strlen(str2) ;

    for(int i = 0 ; i < len1 ; i++){
        char c1 = str1[i] ;
        HashTable_str1[c1]++ ;
    }

    for(int i = 0 ; i < len2 ; i++){
        char c2 = str2[i] ;
        HashTable_str2[c2]++ ;
    }

    for(int i = 0 ; i < 128 ; i++){
        if(HashTable_str1[i] < HashTable_str2[i]){
            sum = abs(HashTable_str1[i]-HashTable_str2[i]) + sum ;
            flag = false ;
        }
    }

    if(flag){
        printf("Yes %d" , abs(len1-len2)) ;
    }else{
         printf("No %d" , sum) ;
    }

    return 0 ;
}

```