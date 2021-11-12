---
title: PTA A1041 Be Unique 
date: 2021-06-07 14:17:02.209
updated: 2021-06-07 14:17:02.209
url: https://pumpkn.xyz/archives/ptaa1041beunique
categories: 算法
tags: 算法 | map   
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805444361437184)
![image.png](https://pumpkn.xyz/upload/2021/06/image-e540aa9e99b943e8abd91059584c1b7c.png)

# 题意
给出N个数字，问按照读入的顺序，哪个数字是第一个在所有数字中只出现一次（Unique）
的数字。如果所有N个数字都出现了超过一次，则输出“None".

# 题解
注意到所有数字都满足number< 10"，因此只需要用一个int型数组HashTable[10001]来记录这些数字出现的次数，例如HashTable[2333]=520表示数字2333出现了520次。显然，可以在读入数字x时直接让Hash Tablx]加1来表示x的出现次数增加1。这样当读入完成后，每个数字的出现次数也统计完毕，只需要按照读入数字的顺序来判断哪个是第一个只在序列中出现了一次即可。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int n ;
    cin >> n ;
    vector<int> bets ;
    int record[10010] ;
    for( int i = 0 ; i < 10010 ; i++ ){
        record[i] = 0 ;
    }
    vector<int> order ;
    for( int i = 0 ; i < n ; i++ ){
        int tmp ;
        cin >> tmp ;
        bets.push_back(tmp) ;
        record[tmp] ++ ;
        order.push_back(tmp) ;
    }

   // cout << record[0] << endl ;
    vector<int> res ;
    for( int i = 0 ; i < 10010 ; i++ ){
        if( record[i] == 1 ){
            res.push_back(i) ;
        }
    }


    if( res.size() == 0 ){
        cout <<"None" ;
    }
    else{
        bool flag = false ;
        for( int i = 0 ; i < order.size() ; i++ ){
            for( int j = 0 ; j < res.size() ; j++ ){
                if( order[i] == res[j] ){
                    cout << res[j] ;
                    flag = true ;
                    break ;
                }
            }
            if( flag ){
                break ;
            }
        }
    }

    return 0 ;
}

```