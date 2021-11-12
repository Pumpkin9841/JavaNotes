---
title: PTA A1028 List Sorting
date: 2021-06-04 15:37:14.502
updated: 2021-06-04 15:37:14.502
url: https://pumpkn.xyz/archives/ptaa1028listsorting
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接]()
![image.png](https://pumpkn.xyz/upload/2021/06/image-36571293549743a39ee2484d719b85a8.png)
# 题意
给出N个考生的准考证号、姓名、分数，并输入参数C，要求按C的不同取值进行排序：</br>
C=1，则按准考证号从小到大排序。</br>
C=2，则按姓名字典序从小到大排序；若姓名相同，则按准考证号从小到大排序。</br>
C=3，则按分数从小到大排序；若分数相同，则按准考证号从小到大排序。</br>
# 题解
题目简单，根据题目走就行。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

struct Student{
    int id ;
    char name[10] ;
    int grade ;
}stu[100010];

string column[4] = { " " , "id" , "name" , "grade" } ;
int C = 0 ;

bool cmp(Student a , Student b){
    if( column[C] == "id" ){
        return a.id < b.id ;
    }

    if( column[C] == "name" ){
        if( strcmp( a.name , b.name ) == 0 ){
            return a.id < b.id ;
        }
        else{
            return strcmp( a.name , b.name ) < 0 ;
        }
    }
    if( column[C] == "grade" ){
        if( a.grade != b.grade ){
            return a.grade < b.grade ;
        }
        else{
            return a.id < b.id ;
        }
    }

}


int main(){
    int n ;
    cin >> n >> C ;
    for( int i = 0 ; i < n ; i++ ){
        cin >> stu[i].id >> stu[i].name >> stu[i].grade ;
    }

    sort( stu , stu+n , cmp ) ;
    for( int i = 0 ; i < n ; i++ ){
        cout << setw(6) << setfill('0') << stu[i].id << " " << stu[i].name << " "<< stu[i].grade <<endl ;
    }


    return 0 ;
}

```
