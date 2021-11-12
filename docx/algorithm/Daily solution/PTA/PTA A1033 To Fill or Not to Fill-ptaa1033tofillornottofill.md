---
title: PTA A1033 To Fill or Not to Fill
date: 2021-07-02 17:11:22.123
updated: 2021-07-02 17:11:22.123
url: https://pumpkn.xyz/archives/ptaa1033tofillornottofill
categories: 算法
tags: 积累 | 算法 | 贪心算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805458722734080)
![image.png](https://pumpkn.xyz/upload/2021/07/image-4d433429a7bc487fbc5c7d6338e5c1ca.png)

# 题意
已知起点与终点的距离D，油箱的最大容量C，单位汽油能行驶的距离Davg,加油站数量N，以及每个加油站与起点的距离和油价。假设汽车初始时刻邮箱为空，第一个加油站与起点距离为0，求到达终点的最小花费。若不能到达终点，输出能行驶的最大距离。

# 题解
**步骤1**：把终点看作为油价为0，与起点距离为D的加油站，将所有加油站按距离升序排序，若第一个加油站的距离不为0，则无法行驶(初始油量为0)。输出"The maximum travel distance = 0.00"；如果第一个加油站距离为0，则进入步骤2。</br>
**步骤2**：假设当前加油站编号为now，接下来将从满油状态下能到达的所有加油站中选出下一个前往的加油站，策略如下：</br>

1. 寻找距离当前加油站最近，且油价低于当前油价的加油站（记为K），加恰好能行驶到加油站K的油量，然后前往加油站k(<font color = "red">即优先前往更低油价的加油站</font>)
 
2. 如果找不到油价比当前加油站低的，则寻找能到达的加油站中油价最低的(记为K)，在当前加油站中加满油，然后前往加油站k。 

3. 如果满油状态下都找不到能到达的加油站，则最远能到达的距离为当前加油站与起点的距离加上满油能行驶的距离，算法结束。



# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

struct Gas{
    double price ;
    double distence ;
}gas[510];

//将加油站按距离升序排序
bool cmp(Gas a , Gas b){
    return a.distence < b.distence ;
}


int main(){
    // c 油箱最大容量， d 距离目的地距离， davg每个单位油能行驶的距离，n 加油站数量
    int  n ;
    double c , d , davg  ;
    cin >> c >> d >> davg >> n;

    for( int i = 0 ; i < n ; i++ ){
        cin >> gas[i].price >> gas[i].distence ;
    }

    //将终点视为油价为0，距离为d的加油站
    gas[n].price = 0.0 ;
    gas[n].distence = d ;

    sort( gas , gas+n+1 , cmp ) ;

    //若第一个加油站不在起始地，无法行使
    if( gas[0].distence != 0 ){
        cout << "The maximum travel distance = 0.00" << endl ;
        return 0 ;
    }

    int now = 0 ; //当前所处的加油站编号
    //ans为总花费，nowTank为邮箱中的油量，MAX为满油行驶距离
    double ans = 0 , nowTank = 0 , MAX = c * davg ;
    while( now < n ){
        int k = -1 ;//油价最低的加油站编号
        double minPrice = 10000000 ;
        for( int i = now+1 ; i <= n && gas[i].distence - gas[now].distence <= MAX ; i++ ){
            if( gas[i].price < minPrice ){
                minPrice = gas[i].price ;
                k = i ;

                //如果找到第一个低于当前油价的加油站
                if( minPrice < gas[now].price ){
                    break ;
                }
            }
        }
        //如果满油状态下行驶不到下一个加油站
        if( k == -1 ){
            break ;
        }
        // need为行驶到下一个加油站需要的油量
        double need = (gas[k].distence - gas[now].distence )/davg ;
        //如果加油站k的油价低于当前油价
        if( minPrice < gas[now].price ){
            //只买足够到达加油站k的油量
            //当前油量不足
            if(nowTank < need){
                ans += ( need - nowTank ) * gas[now].price ;
                nowTank = 0 ;  //到达加油站k后，油箱中没有油
            }
            else{
                nowTank -= need ;
            }
        }
        else{
            ans += ( c - nowTank ) * gas[now].price ;
            nowTank = c - need ;
        }
        now = k ;

    }

    if( now == n  ){
        printf("%0.2f\n",ans) ;
    }
    else{
        printf("The maximum travel distance = %.2f\n",gas[now].distence+MAX) ;
    }


    return 0 ;
}

```