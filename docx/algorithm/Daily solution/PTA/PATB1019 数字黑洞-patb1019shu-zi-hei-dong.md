---
title: PATB1019 数字黑洞
date: 2020-12-28 15:11:35.384
updated: 2021-02-20 15:59:31.69
url: https://pumpkn.xyz/archives/patb1019shu-zi-hei-dong
categories: 算法
tags: 积累 | 算法
---

# 题目
![PABT1019.png](https://pumpkn.xyz/upload/2021/02/PABT1019-b208331ac3af426b931a248ce289bf08.png)
# 代码
```c++
#include<cstdio>
#include<algorithm>

using namespace std ;

bool cmp(int a , int b){
    return a > b ;
}

void getNum(int a , int num[] , int reveNum[]){
   int ge , shi , bai , qian ;
    qian = a/1000 ;
    bai = (a-qian*1000)/100 ;
    shi = (a - qian*1000 - bai*100)/10 ;
    ge = (a - qian*1000 - bai*100 - shi*10) ;
    num[0] = qian ;
    num[1] = bai ;
    num[2] = shi ;
    num[3] = ge ;
    reveNum[0] = qian ;
    reveNum[1] = bai ;
    reveNum[2] = shi ;
    reveNum[3] = ge ;
    sort(num ,num+4) ;
    sort(reveNum, reveNum+4 , cmp) ;



}

int main(){
    int a ;
    int num[10] ;
    int reveNum[10] ;
    int dec ;
    int inc;
    int outCome ;
    scanf("%d" ,&a) ;
    getNum(a , num ,reveNum) ;
    dec = reveNum[0]*1000 + reveNum[1]*100 + reveNum[2]*10 + reveNum[3] ;
    inc = num[0]*1000 + num[1]*100 + num[2]*10 + num[3] ;
    if(dec == inc){
        printf("%04d - %04d = %04d" , dec ,inc,dec-inc) ;
        return 0 ;
    }

    do{
        outCome = dec - inc ;
        printf("%04d - %04d = %04d\n" , dec , inc ,outCome) ;
        getNum(outCome , num ,reveNum) ;
        dec = reveNum[0]*1000 + reveNum[1]*100 + reveNum[2]*10 + reveNum[3] ;
        inc = num[0]*1000 + num[1]*100 + num[2]*10 + num[3] ;
    }while(outCome != 6174) ;

    return 0 ;
}
```