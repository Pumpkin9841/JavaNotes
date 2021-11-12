---
title: PAT B1042 字符统计
date: 2021-01-11 18:52:24.609
updated: 2021-02-20 15:56:07.168
url: https://pumpkn.xyz/archives/patb1042zi-fu-tong-ji
categories: 算法
tags: 积累 | 算法 | string
---

# 题目
![PATB1042.jpg](https://pumpkn.xyz/upload/2021/02/PATB1042-5144df9ab7714a67810e3e148a2dadb6.jpg)
# 代码
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<stdlib.h>
using namespace std ;

struct StrList{
    int c ;    //字符
    int num ;   //出现次数
}str[1010];

bool cmp(StrList a , StrList b){
    if(a.num != b.num){
        return a.num >b.num ;
    }else{
        return a.c < b.c ;
    }
}

int main(){
    char input_str[1010] ;
    int i = 0 ;
    int HashTable[128] = {0} ;
    fgets(input_str , 1010 , stdin) ;
    while(input_str[i] != '\n')
        i++;
    input_str[i] = '\0' ;

    int len = strlen(input_str) ;
    for(int j = 0 ; j < len ; j++){
        char c1 = input_str[j] ;
        if(c1 >= 'A' && c1<='Z'){
            c1 += 32 ;
            HashTable[c1]++ ;
        }else if( c1 >= 'a' && c1 <='z'){
              HashTable[c1]++ ;
        }

    }

    int m = 0 ;
    for(int j = 0 ; j < 128 ; j++ ){
        if(HashTable[j] != 0){
            str[m].c = j ;
            str[m].num = HashTable[j] ;
            m++;
        }
    }

    sort(str ,str+m ,cmp) ;
    printf("%c %d" , str[0].c , str[0].num) ;


    return 0 ;
}

```