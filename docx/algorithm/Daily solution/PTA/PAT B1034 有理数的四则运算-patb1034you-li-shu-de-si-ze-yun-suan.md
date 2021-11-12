---
title: PAT B1034 有理数的四则运算
date: 2021-01-07 15:44:38.112
updated: 2021-02-20 15:57:25.002
url: https://pumpkn.xyz/archives/patb1034you-li-shu-de-si-ze-yun-suan
categories: 算法
tags: 积累 | 算法
---

# 题目
![PATB1034.jpg](https://pumpkn.xyz/upload/2021/02/PATB1034-ce45bf1e51b04c0a86c12e7806d71217.jpg)
# 代码

```c++
#include<cstdio>
#include<algorithm>
#include<math.h>
using namespace std ;

typedef long long ll ;

struct Frac{
    ll up ;
    ll down ;
}a,b,c;

//计算最大公约数
int gcd(ll a , ll b){
    if(b == 0){
        return a ;
    }else{
        return gcd(b , a%b) ;
    }
}


//化简
Frac Reduce(Frac a){
    //只能分子带负号
    if(a.down < 0){
        a.down = -a.down ;
        a.up = -a.up ;
    }

    if(a.up == 0){
        a.down = 1 ;
    }else{   //如果分子不为0 , 进行约分
        int d = gcd(abs(a.up) , abs(a.down)) ;
        a.up = a.up / d ;
        a.down = a.down / d ;
    }

    return a ;
}

//打印数据
void showResult(Frac a){
    if(a.up < 0){
        printf("(") ;
    }
    if(a.down == 1){
        printf("%lld" , a.up) ;
    }else if(abs(a.up) > a.down){
        printf("%lld %lld/%lld" , a.up/a.down , abs(a.up) % a.down , a.down) ;
    }else{
        printf("%lld/%lld" , a.up , a.down ) ;
    }

    if(a.up < 0){
        printf(")") ;
    }
}

//加法
Frac add(Frac f1 , Frac f2){
    Frac res ;
    res.up = f1.up*f2.down + f2.up*f1.down ;
    res.down = f1.down*f2.down ;
    return res ;
}

//减法
Frac sub(Frac f1 ,Frac f2){
    Frac res ;
    res.up = f1.up*f2.down - f2.up*f1.down ;
    res.down = f1.down*f2.down ;
    return res ;
}

//乘法
Frac cheng(Frac f1 , Frac f2){
    Frac res ;
    res.up = f1.up*f2.up ;
    res.down = f1.down*f2.down ;
    return res ;
}
//除法
Frac divd(Frac f1 , Frac  f2){
    Frac res ;
    res.up = f1.up*f2.down ;
    res.down = f1.down*f2.up ;
    return res ;

}


int main(){
    scanf("%lld/%lld %lld/%lld" , &a.up , &a.down ,&b.up, &b.down ) ;
    a = Reduce(a) ;         //化简a
    b = Reduce(b) ;         //化简b
    //加法
    Frac addResult = add(a,b) ;
    addResult = Reduce(addResult) ;
    showResult(a) ;
    printf(" + ") ;
    showResult(b) ;
    printf(" = ") ;
    showResult(addResult) ;
    printf("\n") ;
    //减法
    Frac subResult = sub(a,b) ;
    subResult = Reduce(subResult) ;
    showResult(a) ;
    printf(" - ") ;
    showResult(b) ;
    printf(" = ") ;
    showResult(subResult) ;
    printf("\n") ;
    //乘法
    Frac chengResult = cheng(a,b) ;
    chengResult = Reduce(chengResult) ;
    showResult(a) ;
    printf(" * ") ;
    showResult(b) ;
    printf(" = ") ;
    showResult(chengResult) ;
    //除法
    printf("\n") ;
    Frac divdResult = divd(a,b) ;
    divdResult = Reduce(divdResult) ;
    showResult(a) ;
    printf(" / ") ;
    showResult(b) ;
    printf(" = ") ;
    if(b.up == 0){
        printf("Inf") ;
    }else{
        showResult(divdResult) ;
    }

    return 0 ;
}

```