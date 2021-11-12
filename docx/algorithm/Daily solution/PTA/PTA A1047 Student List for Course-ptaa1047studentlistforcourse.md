---
title: PTA A1047 Student List for Course
date: 2021-07-18 16:36:15.684
updated: 2021-07-18 16:36:15.684
url: https://pumpkn.xyz/archives/ptaa1047studentlistforcourse
categories: 算法
tags: 算法 | STL | vector
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805433955368960)
![image.png](https://pumpkn.xyz/upload/2021/07/image-851cdb4f3bc2464dbceecd22b81214dd.png)

# 题意
给出选课人数和选课数目，然后再给出每个人的选课情况，针对每门课程输出选课人数以及选该课的学生姓名(学生姓名按字母表升序输出)

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

struct Course{
    vector<string> courses ;
}cou[2510];

int main(){
    int n , k ;
    cin >> n >>k ;
    for( int i = 0 ; i < n ; i++ ){
        char name[5] ;
        cin >> name ;
        string nameStr(name) ;
        int courseNum ;
        cin >> courseNum ;
        for( int j = 0 ; j < courseNum ; j++){
            int temp ;
            cin >> temp ;
            cou[temp].courses.push_back(nameStr) ;
        }
    }


    for( int i = 1 ; i <= k ; i++ ){
        int total = cou[i].courses.size() ;
        sort( cou[i].courses.begin() , cou[i].courses.end() ) ;
        cout << i << " " << total << endl ;
        for( int j = 0 ; j < total ; j++  ){
            cout  << cou[i].courses[j] << endl ;
        }

    }

    return 0 ;
}

```