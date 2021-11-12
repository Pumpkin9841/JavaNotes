---
title: PAT B1033 旧键盘打字
date: 2021-01-06 11:42:32.415
updated: 2021-02-20 15:57:44.314
url: https://pumpkn.xyz/archives/patb1033jiu-jian-pan-da-zi
categories: 算法
tags: 积累 | 算法
---

# 题目
![PATB1033.jpg](https://pumpkn.xyz/upload/2021/02/PATB1033-988fe99dd6694908949bcdac82522799.jpg)
# 代码
```c++
#include<stdio.h>
#include<string>
#include<cstring>
#include<algorithm>
using namespace std ;

int main(){
    char str1[100010] ;
    char str2[100010] ;
    bool HashTable[128] = {false} ;   //记录是否输出
    bool flag = false ;   //上档键是否坏了
    int i = 0 , j = 0;
    fgets(str1, 100010, stdin);
    while (str1[i] != '\n')
        i++;
    str1[i] = '\0';
    fgets(str2 , 100010 , stdin) ;
    while(str2[j] != '\n')
        j++ ;
    str2[j] = '\0' ;

    int len1 = strlen(str1) ;
    int len2 = strlen(str2) ;

    for(int n = 0 ; n < len1 ; n++){
        if(str1[n] == '+'){
            flag = true ;       //上档键坏了为true,没坏为false
        }
    }

    if(flag){   //上档键坏了的情况
        for(int n = 0 ; n < len1 ; n++){
            int m  ;
            char c1 ;
            char c2 ;
            c1 = str1[n] ;
            for(m = 0 ; m < len2 ; m++){
                c2 = str2[m] ;
                if(c1 == c2 || (c1+32) == c2){
                    HashTable[c1] = true ;
                    HashTable[c1+32] = true ;
                }else if(c2 >= 'A' && c2 <='Z'){
                    HashTable[c2] = true ;
                }else{
                    //什么也不做
                }
            }
        }
    }else{
         for(int n = 0 ; n < len1 ; n++){
            int m  ;
            char c1 ;
            char c2 ;
            c1 = str1[n] ;
            for(m = 0 ; m < len2 ; m++){
                c2 = str2[m] ;
                if(c1 >= 'A' && c1 <='Z'){
                    if(c1 == c2 || (c1+32) == c2){
                        HashTable[c1] = true ;
                        HashTable[c1+32] = true ;
                    }

                }else {
                    if( c1 == c2){
                        HashTable[c1] = true ;
                    }else{
                        //什么也不做
                    }
                }
            }
        }
    }

    for(int n = 0 ; n < len2 ; n++){
        char c3 ;
        c3 = str2[n] ;
        if(HashTable[c3] == false){
            printf("%c" ,c3) ;
        }
    }



    return 0 ;
}

```