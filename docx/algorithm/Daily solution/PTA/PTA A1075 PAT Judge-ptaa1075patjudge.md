---
title: PTA A1075 PAT Judge
date: 2021-06-04 20:22:58.036
updated: 2021-06-04 20:22:58.036
url: https://pumpkn.xyz/archives/ptaa1075patjudge
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805393241260032)
![image.png](https://pumpkn.xyz/upload/2021/06/image-0f31eac349bd4046ad3c496d756be2da.png)
# 题意
有N位考生，其准考证号为00001~N。共有K道题，编号为1~K，且每道题的分值给出。然后给出M次提交记录，每个提交记录显示了该次提交所属考生的准考证号、交题的题号及所得的分值，其中分值要么是-1（表示未通过编译），要么是0到该题满分区间的一个整数。现在要求对所有考生按下面的规则排序：</br>
1. 先按K道题所得总}从高到低 排序。
2. 若总分相同，则按完美解决（即获得题目满分）的题目数量从高到低排序。
3. 若完美解决的题目数量也相同，则按准考证号从小到大排序。
输出规则：</br>
1. 输出每位考生的排名、准考证号、总分、K道题的各自得分，若总分相等，则排名相同。
2. 如果某位考生全场都没有提交记录，或是没有能通过编译的提交，则该考生的信息不输出。
3. 对需要输出的考生，如果某道题没有能通过编译的提交，则将该题记为0分；如果某道题没有提交记录，则输出"-"

# 注意
需要注意几个点：
1. 排名中，总分相等的排名相同，但是之后的排名应当算上这些相同排名的考生数，例如样例中的排名是12225而不是12223.
2. 样例中有三个总分为42分的考生，其中由于00005和00007的完美解题数均为1，而00001的完美解题数为0，因此00001被排在他们的最后面；这种情况下，准考证号00005比00007小，因此00005排在前面。
3. 00005对第2题的提交记录是-1，即编译无法通过，但是在分数显示上应该为0分。





# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

struct Student{
    int id = -1 ;
    int total = 0 ;
    int grade[6]  ;
    int num = 0 ;    //完美接题数
}stu[10010];

bool cmp( Student a , Student b){
    if( a.total != b.total ){
        return a.total > b.total ;
    }
    else if( a.num != b.num ){
        return a.num > b.num ;
    }
    else{
        return a.id < b.id ;
    }
}


int main(){
    int n , k , m ;
    cin >> n >> k >> m;
    int problems[k+1] ;

    for(int i = 1 ; i <= k ; i++){
        cin >> problems[i] ;
    }

    //初始化各题分数
    for( int i = 0 ; i < 10010 ; i++ ){
        for( int j = 1 ; j <= k ; j++ ){
            stu[i].grade[j] = -2 ;
        }
    }


    for( int i = 0 ; i < m ; i++ ){
        int tmpId , pro , score ;
        cin >> tmpId >> pro >> score ;
        stu[tmpId].id = tmpId ;

        if( score == -1 ){
            stu[tmpId].grade[pro] = 0 ;
        }

        if( score > stu[tmpId].grade[pro] ){
            stu[tmpId].grade[pro] = score ;
        }

        if( score == problems[pro] ){
            stu[tmpId].num ++ ;
        }
    }


    for( int i = 0 ; i < 10010 ; i++ ){
        for( int j = 1 ; j <= k ; j++ ){
            if( stu[i].grade[j] != -2 ){
                stu[i].total += stu[i].grade[j] ;
            }
        }
    }

    sort(stu , stu+10010 , cmp) ;


    int rank = 0 ;
    if(stu[0].total > 0){
        if( stu[0].id != -1 ){
            cout << 1 << " ";
            rank = 1 ;
            cout << setw(5) << setfill('0') << stu[0].id << " " << stu[0].total << " " ;
            for( int j = 1 ; j <= k ; j++ ){
                if( stu[0].grade[j] == -2 ){
                    cout << "-" ;
                }
                else{
                    cout << stu[0].grade[j]  ;
                }

                if( j < k ){
                    cout << " " ;
                }
            }
            cout << endl ;
        }
    }


    for( int i = 1 ; i < n ;i++ ){
        if(stu[i].total > 0){
            if( stu[i].id != -1 ){
                if( stu[i].total == stu[i-1].total ){
                    cout << rank <<" " ;
                }
                else{
                    cout << i+1 << " " ;
                    rank = i+1 ;
                }

                cout << setw(5) << setfill('0') << stu[i].id << " " << stu[i].total << " " ;
                for( int j = 1 ; j <= k ; j++ ){
                    if( stu[i].grade[j] == -2 ){
                        cout << "-" ;
                    }
                    else{
                        cout << stu[i].grade[j]  ;
                    }

                    if( j < k ){
                        cout << " " ;
                    }
                }
                cout << endl ;
            }
        }
    }



    return 0 ;
}

```