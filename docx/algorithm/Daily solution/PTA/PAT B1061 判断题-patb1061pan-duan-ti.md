---
title: PAT B1061 判断题
date: 2021-01-25 22:01:38.971
updated: 2021-02-25 22:01:52.206
url: https://pumpkn.xyz/archives/patb1061pan-duan-ti
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805268817231872)
![image.png](https://pumpkn.xyz/upload/2021/02/image-7032810dcf8d4a3e8fd10f2b52ec53dd.png)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int main(){
    int n , m ;
    cin >> n >> m ;
    int grade[m] ;  //每道题的分数
    int ans[m] ;    //正确答案
    int sol[n][m] ; //每个学生的答案

    for(int i = 0 ; i < m ; i++){
        cin >> grade[i] ;
    }

    for(int i = 0 ; i < m ; i++){
        cin >> ans[i] ;
    }

    for(int i = 0 ; i < n ; i++){
        int res = 0 ;
        for(int j = 0 ; j < m ; j++){
            cin >> sol[i][j] ;
            if( sol[i][j] == ans[j] ){
                res += grade[j] ;
            }
        }
        cout << res << endl  ;
    }



    return 0 ;
}

```