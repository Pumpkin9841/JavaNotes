---
title: 深度优先搜索DFS
date: 2021-02-21 13:40:43.372
updated: 2021-07-24 18:08:48.354
url: https://pumpkn.xyz/archives/shen-du-you-xian-sou-suo-dfs
categories: 算法
tags: 算法 | DFS 
---

深度优先搜索是一种枚举所有完整路径以遍历所有情况的搜索方法，记住使用递归来实现DFS的本质其实还是栈——使用递归时，系统会调用<font color = "red">系统栈 </font>来存放递归中的每一层状态。</br>
# 通过例子学习理解DFS

## 题目一
&ensp;&ensp; 有n件物品,每件物品的重量为w[i],价值为c[i],现在需要选出若干件物品放入一个容量为V的背包中,使得在选入背包的物品重量和不超过容量V的前提下,让背包中物品的价值之和最大,求最大价值,(1<n<20)</br>

## 题解
&ensp;&ensp; 显然，每件物品都有装入背包与不装入背包两种选择，因此DFS函数的参数中必须记录当前处理的物品编号index。而题目中涉及了物品的重量和价值，因此也需要参数来记录在处理当前物品之前，已选物品的总重量sumW和总价值sumC。于是DFS函数看起来应该是这样子的:
```c++
void DFS(int index , int sumW , int sumC ){
	...
}
```
于是，如果选择不放入idenx号物品，那么sumW与sumC就将不变，接下来处理index+1号物品，即前往
```c++
DFS(index+1 , sumW , sumC)
```
这条分支；</br>
如果选择放入index号物品，那么sumW需要加上w[index] , sumC需要加上c[index],即前往
```c++
DFS(index+1 , sumW+w[index] , sumC+c[index] )
```
这条分支。</br>
&ensp;&ensp; 一旦index增长到n，则说明已经把n件物品处理完毕，此时记录的sumW和sumC就是所选物品的总重量和总价值。
## 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 30 ;
int n = 0, maxValue = 0 , v = 0 ; //n 物品总数， maxValue当前背包中物品价值总和， v 容量
int w[maxn] , c[maxn] ;  //w为每件物品的重量，c为每件物品的价值

void DFS(int index , int sumW , int sumC){
    //若当前物品是最后一件
    if( index == n){
        if( sumW <= v && sumC > maxValue ){
            maxValue = sumC ;
        }
        return ;
    }

    //每件物品都有两种选择——装与不装
    DFS(index+1 , sumW , sumC ) ;
    DFS(index+1 , sumW + w[index] , sumC+c[index]) ;

}


int main(){

    //输入物品总数，背包容量
    cin >> n >> v ;
    for(int i = 0 ; i < n ; i++){
        cin >> w[i] ;            //输入每件物品的重量
    }

    for(int i = 0 ; i < n ; i++){
        cin >> c[i] ;            //输入每件物品的价值
    }
    DFS(0,0,0) ;
    cout << maxValue <<endl ;

    return 0 ;
}

```
## 输入数据
```c++
5 8 	 //5件物品，背包容量为8
3 5 1 2 2   //重量分别为
4 5 2 1 3  //价值分别为
```
## 输出结果
```c++
10
```

## 算法优化
上面的代码复杂度为O(2^n),这看起来不是很优秀。再上述代码中，总是把n件物品处理之后才去更新最大价值，但是忽略了背包容量的限制。也就是说，可以把对sumW的判断放到分支前，只有sunW<=v时才能进入分支。
```c++
void DFS(int index , int sumW , int sumC){
	if(index == n){
		return ;   //已经完成n件物品的处理
	}
	
	DFS(index+1 , sumW , sumC) ;   //不选该物品
	//只有加入第index件物品后未超过背包容量v，才加入
	if(sumW +w[index] <= v){
        if(sumC + c[index] > maxValue ){
            maxValue = sumC+c[index] ;  //更新最大价值
        }
        DFS(index+1 , sumW+w[index] , sumC+c[index]) ;   //选第index件物品
	}
	
}
```
这种通过题目条件限制节省DFS计算量的方法称作<font color="red">剪枝</font></br>

## 题目二
&ensp;&ensp;给定N个整数(可能为负数)，从中选择K个数，使得这K个数之和恰好等于一个给定的整数X ；如果有多种方案，选择他们中元素平方和最大的一个。数据保证这样的方案唯一。
</br>

## 思路
当试图进入“选index号数”这条分支时，就把A[index]加入temp中；而当这条分支结束时，就把他从temp中除去，使他不会影响“不选index号数”这条分支。接着，如果在某个时候发现当前已经选择了K个数，且这K个数之和恰好为sum时，就去判断平方和是否比已有的maxPow还要大：如果确实更大，就把temp赋给ans。


## 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 30 ;

//从序列A中N个数选择k个数使其和为x，最大平方和为maxSumSqu
int n , k , x , maxSumSqu = -1 , A[maxn] ;
//tmp 存放临时方案，ans存放平方和最大的方案
vector<int > tmp , ans ;

//当前处理index号整数，当前选择整数个数为nowK
//当前已选整数和为sum，当前已选整数平方和为sumSqu
void DFS(int index , int nowK , int sum , int sumSqu){
    if( nowK == k && sum == x ){
        if(sumSqu > maxSumSqu){
            maxSumSqu = sumSqu ;
            ans = tmp ;
        }
        return ;
    }

    //已经处理完N个数，或者选择的数超过k个数，或者所选整数之和大于x
    if(index == n || nowK > k || sum > x){
        return ;
    }

    //选当前数
    tmp.push_back(A[index]) ;
    DFS(index+1 , nowK+1 , sum + A[index] , sumSqu+A[index]*A[index] ) ;
    tmp.pop_back() ;
    //不选当前数
    DFS(index+1 , nowK , sum ,sumSqu) ;
}

int main(){
    cin >> n >> k >> x ;
    for(int i = 0 ; i < n ; i++){
        cin >> A[i] ;
    }

    DFS(0,0,0,0) ;

    for(int i = 0 ; i < ans.size() ; i++){
        cout << ans[i] << "  " ;
    }

    return 0 ;
}

```

## 输入样例
```c++
4 2 6	//从4个整数中选择2个数，使和为6
2 3 3 4	  //整数分别为
```
## 输出结果
```c++
2  4
```
&ensp;&ensp;上面这个问题中的每个数都只能选择一次,现在稍微修改题目:**假设N个整数中的每一个都可以被选择多次**,那么选择K个数,使得K个数之和恰好为X,例如有三个整数1、 4, 7, 需要从中选择5个数,使得这5个数之和为17,显然,只需要选择3个1和2个7, 即可得到17。</br>
&ensp;&ensp;这个问题只需要对上面的代码进行少量的修改即可。由于每个整数都可以被选择多次, 因此当选择了index号数时,不应当直接进入index+1号数的处理。显然,应当能够继续选择index号数,直到某个时刻决定不再选择index号数,就会通过"不选index号数"这条分支进入index+1号数的处理。因此只需要把”选index号数"这条分支的代码修改为
```c++
DFS(index, nowK+ 1, sum+A[index], sumSqu+A[index] * A[index])
```
即可。