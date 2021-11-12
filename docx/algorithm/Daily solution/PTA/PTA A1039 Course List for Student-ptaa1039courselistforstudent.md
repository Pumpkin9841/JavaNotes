---
title: PTA A1039 Course List for Student
date: 2021-07-18 15:56:44.715
updated: 2021-07-18 15:56:44.715
url: https://pumpkn.xyz/archives/ptaa1039courselistforstudent
categories: 算法
tags: 算法 | STL | vector
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805447855292416)
![image.png](https://pumpkn.xyz/upload/2021/07/image-4cde97e14ff6492c868c01d7b20e397b.png)
# 题意
有N个学生，K门课。现在给出选择每门课的学生姓名，并在之后给出N个学生的姓名，要求按顺序给出每个学生的选课情况。
# 题解
读取每门课的所有选课学生，然后将该课程编号加入所有选择该门课的学生中去。这就可以在最后输出学生的选课情况。但是本题有一个问题：给出的学生姓名无法作为学生stu结构体数组的下标。</br>
本题可以采用hash进行求解。即将char数组类型的姓名转化为int类型。

```c++
//hash函数，将字符串的姓名转化为数字
int getId( char name[] ){
    int id = 0 ;
    //题中给出的姓名都有3位字母和1位数字组成，只需将前三位的字母hash成数字
    for( int i = 0 ; i < 3 ; i++ ){
        id = id*26 + ( name[i] - 'A' ) ;
    }
    id = id*10 + name[3] - '0' ;
    return id ;
}
```

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

struct Student{
    vector<int> selectCourses ;
}stu[26*26*26*10+1];

int getId( char name[] ){
    int id = 0 ;
    for( int i = 0 ; i < 3 ; i++ ){
        id = id*26 + ( name[i] - 'A' ) ;
    }
    id = id*10 + name[3] - '0' ;
    return id ;
}



int main(){
    int n , k ;
    cin >> n >> k ;
    for( int i = 0 ; i < k ; i++ ){
        int courseNum , num ;
        cin >> courseNum >> num ;
        for( int j = 0 ; j < num ; j++ ){
            char name[5] ;
            cin >> name ;
            int id = getId(name) ;
            stu[id].selectCourses.push_back(courseNum) ;
        }
    }

    for( int i = 0 ; i < n ; i++ ){
        char name[5] ;
        cin >> name ;
        int id = getId(name) ;
        sort( stu[id].selectCourses.begin() , stu[id].selectCourses.end() ) ;
        cout << name << " " ;
        if( stu[id].selectCourses.size() == 0 ){
            cout << stu[id].selectCourses.size()  ;
        }
        else{
            cout << stu[id].selectCourses.size() << " " ;
        }

        for( int j = 0 ; j < stu[id].selectCourses.size() ; j++ ){
            cout << stu[id].selectCourses[j] ;
            if( j < stu[id].selectCourses.size()-1 ){
                cout << " " ;
            }
        }
        cout << endl ;
    }


    return 0 ;
}

```

## 不使用结构体方法
```c++
#include<cstdio>
#include<algorithm>
#include<vector>
#include<cstring>
using namespace std ;

const int maxStudent = 40010 ;
const int M = 26*26*26*10 ;

/**
*  Hash函数
*  将字符串name转换成int型id
*/
int getId(char name[]){
    int id = 0 ;
    for(int i = 0 ; i < 3 ; i++){
        id = id*26 + (name[i] - 'A') ;
    }
    id = id*10 + name[3] - '0' ;
    return id ;
}

int main(){
    int N,K;
    char name[5] ;
    scanf("%d %d" ,&N , &K) ;
    vector<int> selectCource[M] ;   //每个学生的选课信息
    int studentList[maxStudent] ;
    for(int i = 0 ; i < K ; i++){
        int courseI , studentNum ;
        scanf("%d %d" , &courseI ,&studentNum) ;
        for(int j = 0 ; j < studentNum ; j++){
            scanf("%s" , name) ;
            int id = getId(name) ;
            selectCource[id].push_back(courseI) ;
        }
    }

    for(int i = 0 ; i < N ; i++){
        scanf("%s" , name) ;
        int id = getId(name) ;
        sort(selectCource[id].begin() , selectCource[id].end()) ;
        int total = selectCource[id].size() ;
        printf("%s %d" , name , total) ;
        for(int y = 0 ; y < total ; y++){
            printf(" %d" ,selectCource[id][y]) ;
        }
        if(i < N -1){
            printf("\n") ;
        }
    }

    return 0 ;
}

```