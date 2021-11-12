---
title: PTA A1012 The Best Rank
date: 2021-06-02 18:08:55.985
updated: 2021-06-02 18:10:03.404
url: https://pumpkn.xyz/archives/ptaa1012thebestrank
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805502658068480)
![image.png](https://pumpkn.xyz/upload/2021/06/image-371ae55ab0c6497ba28538240490286e.png)
# 题意
现已知n个考生的3门课分数C.M，E，而平均分数A可以由这3个分数得到。现在分别按这4个分数对n个考生从高到低排序，这样对每个考生来说，就会有4个排名且每个分数都会有一个排名。接下来会有m个查询，每个查询输入一个考生的ID，输出该考生4个排名中最高的那个排名及对应是A、C、M、E中的哪一个。如果对不同课程有相同排名的情况，则按优先级A>C>M>E输出；如果查询的考生ID不存在，则输出N/A.
# 题解
**步骤1**：考虑到优先级为A>C>M>E，不妨在设置数组时就按这个顺序分配序号为03的元素，即0对应A、1对应C，2对应M及3对应E.</br>
以结构体类型Student存放6位整数的ID和4个分数（grade[0]~ grade[3]分别代表A，C，M，E）。</br>
由于ID是6位的整数，因此不妨设置Rank[1000000][4]数组，其中Rank[id][0]-Rank[id][3]表示编号为ID的考生的4个分数各自在所有考生中的排名。</br>
**步骤2**：读入考生的ID和3个分数，同时计算平均分A.</br>
按顺序枚举A，C，M，E，对每个分数，将所有考生排序，并在Rank数组中记录排名。</br>
在查询时，对读入的查询ID，先看其是否存在（可通过Rank[id]的初值做判定）。如果存在，选出Rank[id][0]-Rank[id][3]中数字最小（即排名最高）的那个即可。

# 注意
1. 要注意优先级顺序是A>C>M>E，所以为了方便枚举，在设置数组时尽量把A放在C、M、E前面。</br>
2. 排名时，相同分数算作排名相同，所以91，90，88，88，84的排名应该算作1、2、3、3、5。在具体实现时，切记不要算作1、2、3、3、4，否则中间3个测试点至少会错一个。</br>
3. ID是整数，不会是字符型。</br>
4. 本题没有明示平均分是否需要取整以及取整方式，根据题目描述中的例子可以看出是四舍五入。但本题采用向下取整的方式也能通过，或者采用更简洁的方式-不取平均，直接存储三门课的总分。</br>


# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;


struct Student{
    int id ;
    int grade[4] ;
}student[2010];

int now ;

bool cmp(Student a , Student b){
    return a.grade[now] > b.grade[now] ;
}

int Rank[1000000][4] = {0} ;
char course[4] = {'A' , 'C' , 'M' , 'E'} ;

int main(){
    int n , m ;
    cin >> n >> m ;

    for( int i = 0 ; i < n ; i++ ){
        cin >> student[i].id  >>  student[i].grade[1] >> student[i].grade[2] >> student[i].grade[3] ;
        student[i].grade[0] = student[i].grade[1] + student[i].grade[2] + student[i].grade[3] ;
    }

    for( now = 0 ; now < 4 ; now++ ){
        sort( student , student+n , cmp ) ;
        //第一名
        Rank[student[0].id][now] = 1 ;

        for( int i = 1 ; i < n ; i++ ){
            //与上一个同学分数相同，排名并列
            if(  student[i].grade[now] == student[i-1].grade[now] ){
                Rank[student[i].id][now] = Rank[ student[i-1].id ][now] ;
            }
            else{
                Rank[student[i].id][now] = i + 1;
            }
        }

    }

    for( int i = 0 ; i < m ; i++ ){
        int query ;
        cin >> query ;
        if( Rank[query][0] == 0 ){
            cout << "N/A" <<endl ;
            continue ;
        }

        int j = 0  ;
        for( int k = 0 ; k < 4 ; k++ ){
            if( Rank[query][k] < Rank[query][j]  ){
                j = k ;
            }
        }

        cout << Rank[query][j] << " " << course[j] <<endl ;
    }

    return 0 ;
}

```