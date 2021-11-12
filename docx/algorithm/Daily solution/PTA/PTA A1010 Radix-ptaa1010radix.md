---
title: PTA A1010 Radix
date: 2021-07-10 16:53:45.263
updated: 2021-07-10 16:53:45.263
url: https://pumpkn.xyz/archives/ptaa1010radix
categories: 算法
tags: 学习 | 积累 | 算法 | 二分
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805507225665536)
![image.png](https://pumpkn.xyz/upload/2021/07/image-1bf0c79f889f4723846e70df393e8316.png)

# 题意
输入4个数N1 ， N2 ， tag ， radix。其中tag == 1表示N1为radix进制数，tag==2表示N2为radix进制数。范围：N1和N2均不超过10个数位，且每个数位均为0~9或a~z，其中0~9表示数字为0~9,a~z表示数字10~35.</br>
求N1和N2中未知进制的那个数是否存在，并满足某个进制时和另一个数在十进制下相等的条件。若存在，则输出满足条件的最小进制：否则，输出Impossible。
# 题解
考虑对一个确定的数字串来说，他的进制越大，则该数字串转换为10进制的结果也就越大（例如对一个数字串101，如果他是二进制，那么转换为十进制为5；如果它是十六进制，那么转换为十进制为272），因此可以使用二分法。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

typedef long long LL ;
LL Map[256] ;
LL inf = (1LL << 63) - 1 ;  //long long 的最大值2^63-1

void init(){
    for( char c = '0' ; c <= '9' ; c++ ){
        Map[c] = c - '0' ;
    }
    for( char c = 'a' ; c <= 'z' ; c++ ){
        Map[c] = c - 'a' + 10 ;
    }
}

//将a转换为10进制
LL convertNum10( char a[] , LL radix , LL t ){
    LL ans = 0 ;
    int len = strlen(a) ;
    for( int i = 0 ; i < len ; i++ ){
        ans = ans * radix + Map[ a[i] ] ;
        if( ans < 0 || ans > t ){
            return -1 ;   //溢出或超过N1的十进制
        }
    }
    return ans ;
}

int cmp( char N2[] , LL radix , LL t ){
    int len = strlen(N2) ;
    LL num = convertNum10(N2 , radix , t) ;
    if( num < 0 ){
        return 1 ;
    }
    if( t > num ){
        return -1 ;
    }
    else if( t == num ){
        return 0 ;
    }
    else
        return 1 ;
}

//二分求解N2的进制
int binartSearch( char N2[] , LL left  , LL right , LL t ){
    LL mid ;
    while( left <= right ){
        mid = (left + right) / 2 ;
        int flag = cmp( N2 , mid , t ) ; //判断N2转换为十进制后与t比较
        if( flag == 0 ){
            return mid ;
        }
        else if( flag == -1 ){
            left = mid + 1 ;
        }
        else {
            right = mid - 1 ;
        }
    }
    return -1 ;
}

int findLargestDigit(char N2[]){ //求最大数位
    int ans = -1 , len = strlen(N2) ;
    for( int i = 0 ; i < len ; i++ ){
        if( Map[ N2[i] ] > ans ){
            ans = Map[ N2[i] ] ;
        }
    }
    return ans + 1 ;
}

char N1[20] , N2[20] , temp[20] ;
int tag , radix ;


int main(){
   init() ;
   scanf("%s %s %d %d" , N1 ,N2 , &tag , &radix) ;
   if( tag == 2 ){ //交换N1，N2
        strcpy(temp , N1) ;
        strcpy(N1 , N2) ;
        strcpy(N2 , temp) ;
   }

   LL t = convertNum10(N1 , radix , inf) ;  //将N1从radix进制转换为10进制
   LL low = findLargestDigit(N2) ; //找到N2中数位最大的位加1 ， 当成二分下界
   LL high = max(low ,t) +1 ;
   LL ans = binartSearch(N2 , low , high , t) ;
   if( ans == -1 ){
        cout << "Impossible" ;
   }
   else
        cout << ans ;


    return 0 ;
}

```