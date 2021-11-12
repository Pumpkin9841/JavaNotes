---
title: PTA A1067 Sort with Swap(0, i)
date: 2021-07-04 15:54:58.982
updated: 2021-07-04 15:54:58.982
url: https://pumpkn.xyz/archives/ptaa1067sortwithswap0i
categories: 算法
tags: 积累 | 算法 | 贪心算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805403651522560)
![image.png](https://pumpkn.xyz/upload/2021/07/image-0958e24a13114ea3a825f8b6fb796663.png)
# 题解
由于必须使用数字0跟其他数交换，可以想到的策略是：如果数字0当前在i号位，则找到数字i当前所处的位置，然后把0与i进行交换。但是发现这个策略并不能完全解决问题，因为一旦交换过程中数字0回到了0号位，按上面的算法就不能将其与一个确定的数交换.发现通过该策略，一旦一个非0的数字回到了本位，在后面的步骤中就不应当再让0去与他交换，也就是说非0的数字回到本位就不再变动。于是得出结论，如果在交换过程中0回到了0号位，只需要让0与一个不在本位的非0数字交换即可。


# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 100010 ;
int pos[maxn] ;   //记录每个数的当前位置

int main(){
    int n ;
    cin >> n ;
    int left = n-1 ; //除0以外不在本位上的数的个数
    for( int i = 0 ; i < n ; i++ ){
        int tmp ;
        cin >> tmp ;
        pos[tmp] = i ;

        if( tmp == i && tmp !=0 ){     //如果除0以外的任何数在本位上
            left -- ;
        }
    }

    int ans = 0 ;
    int k = 1 ; //存放不在本位上的最小的数
    while( left > 0 ){  //只要还有不在本位上的数
        //如果0在本位上
        if( pos[0] == 0 ){
            while( k < n ){
                //将0与任何一个不在本位上的数字交换
                if( pos[k] != k  ){
                    swap( pos[0] , pos[k] ) ;
                    ans ++ ;
                    break ;
                }
                k ++ ;
            }
           
        }

        //只要0不在本位上
        while( pos[0] != 0 ){
            swap( pos[0] , pos[pos[0]] ) ;
            ans ++ ;
            left -- ;
        }
    }

    cout <<ans ;

    return 0 ;
}

```