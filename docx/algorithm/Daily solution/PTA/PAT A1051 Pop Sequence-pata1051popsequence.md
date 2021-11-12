---
title: PAT A1051 Pop Sequence
date: 2021-02-12 15:06:11.629
updated: 2021-02-20 15:52:42.976
url: https://pumpkn.xyz/archives/pata1051popsequence
categories: 算法
tags: 算法 | stack
---

# 题目
![PAT A1051.jpg](https://pumpkn.xyz/upload/2021/02/PAT%20A1051-66ad05cb84db4a3f84fe685694cc0719.jpg)
# 题意
给出1个入栈序列和K个出栈序列，判断出栈序列是否合法。
# 题解
按题目要求进行模拟，在入栈过程中，如果入栈元素等于当前出栈元素，将元素出栈，并且将指向出栈序列的指针向前移动一位。
## 注意点
在出栈和读栈顶元素前，需判断栈是否为空；
每个出栈序列输入前都要清空栈，否则，如果上个出栈序列的结果没被清空，那么会影响下个出栈序列的过程。
# 代码
```c++
#include<cstdio>
#include<stack>
#include<iostream>
using namespace std ;

stack<int > st ;
int order[1010] ;   //接收题目指定的出栈序列


int main(){
   int m , n, k ;
   cin >> m >> n >> k ;
   while( k-- ){
        //清空栈
        while( !st.empty() ){
            st.pop() ;
        }

        //读入出栈序列
        for( int i = 1 ; i <= n ; i++){
            scanf("%d" ,&order[i]) ;
        }

        int z = 1 ;
        bool flag = true ;
        for(int i = 1 ; i <= n ; i++ ){
            st.push(i) ;
            if(st.size() > m ){
                flag = false ;
                break ;
            }

            while( !st.empty() && st.top() == order[z] ){
                st.pop() ;
                z++ ;
            }
        }

        if( st.empty() && flag == true ){
            cout << "YES" << endl ;
        }else{
            cout << "NO" << endl ;
        }

   }

}

```