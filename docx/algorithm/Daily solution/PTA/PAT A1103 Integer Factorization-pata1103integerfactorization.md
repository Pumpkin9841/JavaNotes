---
title: PAT A1103 Integer Factorization
date: 2021-02-21 18:13:28.949
updated: 2021-02-21 18:14:56.301
url: https://pumpkn.xyz/archives/pata1103integerfactorization
categories: 算法
tags: 算法 | DFS 
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805364711604224)
![PAT A1103 Integer Factorization.jpg](https://pumpkn.xyz/upload/2021/02/PAT%20A1103%20Integer%20Factorization-21b3914e406e4736a6e9fc690353c590.jpg)
# 题意
给定正整数N、K、P，将N表示成K个正整数(可以相同,递减排列)的P次方的和, 即N=n1^P+…nk^P。如果有多种方案,那么选择底数和n1+…+nK最大的方案;如果还有多种方案,那么选择底数序列的字典序最大的方案。
# 题解
### 步骤1
由于p不小于2,并且在单次运行中是固定的,因此不妨开一个vector<int> fac, 在输入p之后就预处理出所有不超过N的n^p,为了方便下标与元素有直接的对应,这里应把0也存进去,于是对N: 10、 p=2来说,fac[0]=0. fac[1] = 1、 fac[2]=4. fac[3]=9。</br>
### 步骤2
接下来便是DFS函数。DFS用于从fac中选择若干个数(可以重复选),使得它们的和等于N。于是需要针对fac中的毎个数,根据选与不选这个数来进入两个分支,因此DFS的参数中必须有：</br>
&ensp;&ensp;&ensp;&ensp;1.当前处理到的是fac的几号位,不妨记为index;</br>
&ensp;&ensp;&ensp;&ensp;2.当前已经选择了几个数,不妨记为nowK。 由于目的是选出的数之和为N, 因此参数中也需要记录当前选择出的数之和powSum，而为了保证有多个方案时底数之和最小,还需要在参数中记录当前选择出的数的底数之和facSum。于是需要的参数就齐全了。</br>
此外,还需要开一个vector<int>res,用来存放最优的底数序列，而用一个 vector<int> tmp 来存放当前选中的底数组成的临时序列</br>
### 步骤3
考虑递归本身。<font color="red">注意：为了让结果能保证字典序大的序列优先被选中,让index从大到小递减来遍历</font>,这样就总是能先选中fac中较大的数了。</br>
显然,如果当前需要对fac[index]进行选择，那么就会有“选”与“不选”两种选择。</br>
&ensp;&ensp;&ensp;&ensp;如果不选，就可以把问题转化为对 fac[index-1]进行选择，此时nowK、sum、 facSum均不变, 因此往 DFS(index-1,nowK,facSum, powSum)这条分支前进；</br>
&ensp;&ensp;&ensp;&ensp;而如果选,由于每个数字可以重复选择，因此下一步还应当对 fac[index]进行选择，但由于当前选了 fac[index]，需要把底数 index 加入当前序列tmp中，同时让nowK加1、powSum加上fac[index]、 facSum加上index，即往 DFS(index , nowK+1 , facSum + index , powSum + fac[index])这条分支前进。显然，DFS必须在 index 不小于1时执行，因为题目求的是正整数的幂次之和。</br>
### 步骤4
那么，递归到什么时候停止呢？首先，如果到了某个时候powSum=N并且nowK= k成立，那么说明找到了一个满足条件的序列(就是tmp，注意保存的是底数)，此时为了处理多方案的情况況，需要判断底数之和 facSum是否比一个全局记录的最大底数之和 maxFacSum更大，若是，则更新 maxFacSum，并把tmp赋给res。除此之外，当sum>N或者nowK>K时，不可能会产生答案，可以直接返回。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

//将n写成k个正整数的p次幂的和,因子最大和maxFacSum
int n , k , p , maxFacSum = 0 ;
//fac存储不大于n的x^p , tmp临时存放方案，res存放结果
vector<int > fac ,tmp ,res ;

void init(){
   int i = 0 ;
   int t = 0 ;
   //i^p不大于n时，不断将i^p加入到fac中
   while( t <= n){
        fac.push_back(t) ;
        t = pow(++i , p) ;
   }
}

//index当前处理的数，nowK已经选择的数字个数 ,facSum所有因子和，powSum每个因子的p次方的和
void DFS(int index , int nowK , int facSum , int powSum){
    if( powSum == n && nowK == k){
        if(facSum > maxFacSum){
            maxFacSum = facSum ;    //更新最大因子和
            res = tmp ;         //将因子和最大的一组赋值给res
        }
        return ;
    }

    if(powSum > n || nowK > k){
        return ;
    }

    if(index - 1 >= 0){
        //选择当前数的分支
        tmp.push_back(index) ;
        DFS(index , nowK+1 , facSum + index , powSum + fac[index]) ;  //可以重复选择一个数，所以index不需要-1
        tmp.pop_back() ;
        //不选当前数的分支
        DFS(index-1 ,nowK , facSum ,powSum) ;

    }
}

int main(){
    cin >> n >> k >> p ;
    init() ;
    DFS(fac.size()-1 , 0 ,0,0) ;

    if(res.size() > 0){
        cout << n << " = " ;
        for(int i = 0 ; i < res.size() ; i ++){
           //不是最后一个因子
           if( i < res.size()-1 ){
             cout << res[i] << "^" << p <<" + " ;
           }else{
             cout << res[i] << "^" << p  ;
           }
        }
    }else{
        cout <<"Impossible" ;
    }


    return 0 ;
}

```