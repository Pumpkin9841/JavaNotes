---
title: PAT B1029 旧键盘
date: 2021-01-03 18:33:32.373
updated: 2021-02-20 15:58:22.249
url: https://pumpkn.xyz/archives/patb1029jiu-jian-pan
categories: 算法
tags: 算法 | string
---

# 题目
![PATB1029.jpg](https://pumpkn.xyz/upload/2021/02/PATB1029-e7fea2f556cc4a35abc80d2bb3490502.jpg)
# 代码

```c++
#include<cstdio>
#include<string>
#include<algorithm>
#include<cstring>
using namespace std ;

int main(){
    char str1[100] ;   //输入字符
    char str2[100] ;   //实际接收字符
    bool HashTable[128] ={false} ;
    int i=0 , j=0;
//    gets(str1);
//    gets(str2) ;
    fgets(str1, 100, stdin);
    while (str1[i] != '\n')
        i++;
    str1[i] = '\0';
    fgets(str2 , 100 , stdin) ;
    while(str2[j] != '\n')
        j++;
    str2[j] = '\0' ;

    int len1 = strlen(str1) ;
    int len2 = strlen(str2) ;
    for(int n = 0 ; n < len1 ; n++){
        char c1 ;
        char c2 ;
        int m ;
        c1 = str1[n] ;
        for(m = 0 ; m < len2 ; m++){
            c2 = str2[m] ;
            if(c1 >='a' && c1<='z'){  //如果是小写，转化为大写
                c1 -= 32 ;
            }
            if(c2 >='a' && c2<='z'){
                c2 -= 32 ;
            }
            if(c1 == c2)
                break ;
        }
        if(m == len2 && HashTable[c1] == false){
            printf("%c" , c1) ;
            HashTable[c1] = true ;
        }
    }
    return 0 ;
}

```