---
title: PAT B1036 跟奥巴马一起编程
date: 2021-01-08 16:46:33.057
updated: 2021-02-20 15:57:12.547
url: https://pumpkn.xyz/archives/patb1036gen-ao-ba-ma-yi-qi-bian-cheng
categories: 算法
tags: 积累 | 算法
---

# 题目
![PATB1036.jpg](https://pumpkn.xyz/upload/2021/02/PATB1036-c8d3eaada6b04330a54001e34e1b5660.jpg)
# 代码
```c++
#include<cstdio>
int main(){
    char C ;
    int N = 0 ;
    int columns = 0 ;  //定义正方形的列数
    int rows = 0 ;     //定义正方形的行数
    scanf("%d %c" ,&N,&C) ;
    columns = N ;
    rows = (columns+1)/2 ;
    int count = 0 ;
    for(int i = 0 ; i < rows ; i ++){
        for(int j = 0 ; j < columns ; j++){
            if(count == 0 || count == rows-1){
                printf("%c" ,C) ;
            }else if(j==0 || j == columns-1){
                printf("%c" ,C) ;
            }else{
                printf(" ");
            }
        }
        printf("\n");
        count++ ;
    }
    return 0 ;
}

```