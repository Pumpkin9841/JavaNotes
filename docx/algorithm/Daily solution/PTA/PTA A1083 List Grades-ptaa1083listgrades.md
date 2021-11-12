---
title: PTA A1083 List Grades
date: 2021-06-03 23:59:41.982
updated: 2021-06-04 00:00:12.268
url: https://pumpkn.xyz/archives/ptaa1083listgrades
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805383929905152)
![image.png](https://pumpkn.xyz/upload/2021/06/image-7d319a9376424bcc81d2fab93cf52a7c.png)
# 题意
按学生成绩降序输出指定成绩区间的学生的姓名和学号，该区间没有学生则输出NONE
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

int N ;
struct Student{
    char name[15] ;
    char id[15] ;
    int grade ;
};

bool cmp( Student a , Student b ){
    return a.grade > b.grade ;
}


int main(){
    cin >> N ;
    Student stu[N+10] ;
    for( int i = 0 ; i < N ; i++ ){
        cin >> stu[i].name >> stu[i].id >> stu[i].grade ;
    }

    int small , big ;
    cin >> small >> big ;

    sort( stu , stu+N , cmp ) ;

    int count = 0 ;

    for( int i = 0 ; i < N ; i++ ){
        if( stu[i].grade <= big && stu[i].grade >= small ){
            cout << stu[i].name << " " << stu[i].id <<endl ;
            count ++ ;
        }
    }

    if( count == 0 ){
        cout << "NONE" << endl ;
    }

    return 0 ;
}

```