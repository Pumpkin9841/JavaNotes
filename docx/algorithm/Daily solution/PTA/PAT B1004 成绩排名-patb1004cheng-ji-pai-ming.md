---
title: PAT B1004 成绩排名
date: 2021-02-15 21:27:01.992
updated: 2021-03-09 21:27:56.173
url: https://pumpkn.xyz/archives/patb1004cheng-ji-pai-ming
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805321640296448)

![image.png](https://pumpkn.xyz/upload/2021/03/image-e7a8112af70f482f8134613de5af9740.png)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 100010 ;

struct Student{
    string name ;
    string ID ;
    int grade ;
}stu[maxn];

bool cmp( Student a , Student b ){
    return a.grade > b.grade ;
}

int main(){
    int n ;
    cin >> n ;
    getchar() ;
    for(int i = 0 ; i < n ; i++){
        string str ;
        getline(cin , str) ;

        int tmp1 = str.find(" ") ;
        string name = str.substr(0 , tmp1) ;
        stu[i].name = name ;
        str = str.substr( tmp1+1 , str.length() ) ;
        int tmp2 = str.find(" ") ;
        string id = str.substr( 0 , tmp2 ) ;
        stu[i].ID = id ;
        string grade = str.substr( tmp2+1 , str.length() ) ;
        stringstream ss ;
        ss << grade ;
        int tmp ;
        ss >> tmp  ;
        stu[i].grade = tmp ;
    }

    sort( stu , stu+n , cmp ) ;

    cout << stu[0].name << " " << stu[0].ID << "\n" ;

    cout << stu[n-1].name << " " << stu[n-1].ID ;


    return 0 ;
}


```