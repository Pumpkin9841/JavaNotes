---
title: PAT B1013 数素数
date: 2020-12-29 15:14:50.503
updated: 2021-02-20 15:59:19.299
url: https://pumpkn.xyz/archives/patb1013shu-su-shu
categories: 算法
tags: 积累 | 算法
---

# 题目
![PATB1013.png](https://pumpkn.xyz/upload/2021/02/PATB1013-d5689c8eafda48e2952e4cae96bde1b4.png)
# 代码
```c++
#include<cstdio>
#include<algorithm>
#include<math.h>

using namespace std ;

int prime[1000001] ;
int PNum = 0 ;

bool isprime(int i ){
    if(i <= 1)
        return false ;
    else{
        for(int j = 2 ; j <= (int)sqrt(1.0 * i) ; j++){
            if(i%j == 0){
                return false ;
            }
        }
        return true ;

    }

}

void find_prime(int n){
    for(int i = 0 ; i <1000001 ; i++ ){
        if(isprime(i) == true){
            prime[PNum++] = i ;
            if(PNum > n){
                break ;
            }
        }
    }
}

int main(){
    int N , M ;
    int count = 0 ;
    scanf("%d %d" ,&N ,&M) ;
    find_prime(M) ;
    for(int a = N-1 ; a<M ; a++){
        printf("%d" , prime[a]) ;
        if(a <M-1){
            if(count < 9){
                printf(" ") ;

            }
        }
        count ++ ;
        if(count == 10){
            printf("\n") ;
            count = 0 ;
        }
    }

    return 0 ;
}


```