---
title: PATB1009 说反话
date: 2020-12-27 13:29:47.372
updated: 2021-02-20 15:59:40.456
url: https://pumpkn.xyz/archives/patb1009shuo-fan-hua
categories: 算法
tags: 算法
---

# 题目
![PATB1009.png](https://pumpkn.xyz/upload/2021/02/PATB1009-f9722abb398e4c54b81a42669f04639b.png)
# 代码

```c++
#include<cstdio>
#include<cstring>
int main(){
    char str[90] ;
     int i=0;
    fgets(str, 90, stdin);
    while (str[i] != '\n')
        i++;
    str[i] = '\0';
    int len = strlen(str) ;
    char ans[90][90] ;
    int r=0 , c = 0 ;
    for(int i = 0 ; i < len ; i++ ){
        if(str[i] != ' '){
            ans[r][c++] = str[i] ;
        }else{
            ans[r][c] = '\0' ;
            r ++ ;
            c = 0 ;
        }
    }

    for(int i = r ; i>=0 ; i-- ){
        printf("%s" , ans[i]) ;
        if(i>0){
            printf(" ") ;
        }
    }


    return 0 ;
}

```