---
title: PTA A1098 Insertion or Heap Sort
date: 2021-08-02 00:20:33.587
updated: 2021-08-02 00:20:33.587
url: https://pumpkn.xyz/archives/ptaa1098insertionorheapsort
categories: 算法
tags: 学习 | 算法 | 堆
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805368847187968)
![image.png](https://pumpkn.xyz/upload/2021/08/image-5541e048c4b546eb806b739ad5a83093.png)
# 题意
第一行输入序列元素个数，第二行输入原始序列，第三行给出将原始序列经过插入排序或堆排序的某一趟序列，判断是插入排序还是堆排序，并给出下一趟排序的结果。


# 代码
```C++
#include<bits/stdc++.h>
using namespace std ;

vector< vector<int> > insertSeq ;
vector< vector<int> > heapSeq ;

//插入排序
void insertionSort( vector<int> &seq ){
    int n = seq.size() ;
    for( int i = 2 ; i < n ; i++){
        int temp = seq[i] ;
        int j = i ;
        while( j > 1 && temp < seq[j-1] ){
            seq[j] = seq[j-1] ;
            j-- ;
        }
        seq[j] = temp ;
        insertSeq.push_back(seq) ;  //记录每一趟插入排序结果
    }
}

//向下调整
void downAdjust( vector<int> &seq , int low , int high ){
    int i = low ;  //i指向欲调整结点
    int j = i * 2 ;  //j指向其左孩子
    //存在孩子节点
    while( j <= high ){
        if( j+1 <= high && seq[j] < seq[j+1] ){  //右孩子存在且左孩子权值小于右孩子
            j = j + 1 ;  //j指向右孩子
        }

        if( seq[i] < seq[j] ){  //父节点小于孩子节点
            swap( seq[i] , seq[j] ) ;
            i = j ;
            j = i * 2 ;
        }
        else{
            break ;
        }
    }
}

//建堆
void createHeap( vector<int> &seq ){
    int n = seq.size() - 1 ;
    for( int i = n / 2 ; i >= 1 ; i--  ){
        downAdjust( seq , i , n ) ;
    }
}


//堆排序
void heapSort( vector<int> &seq ){
    createHeap( seq );
    int n = seq.size() - 1 ;
    for( int i = n ; i > 1 ;i-- ){
        swap( seq[i] , seq[1] ) ;
        downAdjust( seq , 1 , i-1 ) ;
        heapSeq.push_back(seq) ;  //记录每一趟堆排序结果
    }
}


int main(){
    vector<int> seq ;
    seq.push_back(0) ;
    int n ;
    cin >> n ;
    for( int i = 1 ; i <= n ; i++ ){
        int temp ;
        cin >> temp ;
        seq.push_back(temp) ;
    }

    vector<int> query ;
    query.push_back(0) ;
    for( int i = 0 ; i < n ; i++ ){
        int temp ;
        cin >> temp ;
        query.push_back(temp) ;
    }

    vector<int> seq1 = seq ;
    insertionSort(seq1) ;



    vector<int> seq2 = seq ;
    heapSort(seq2) ;

    for( int i = 0 ; i < insertSeq.size() ; i++ ){
        if( insertSeq[i] == query ){
            cout << "Insertion Sort" << endl ;
            for( int j = 1 ; j < insertSeq[i+1].size() ; j++ ){
                cout << insertSeq[i+1][j] ;
                if( j < insertSeq[i+1].size() - 1 ){
                    cout << " " ;
                }
            }
            return 0 ;
        }
    }

    for( int i = 0 ; i < heapSeq.size() ; i++ ){
        if( heapSeq[i] == query ){
            cout << "Heap Sort" << endl ;
            for( int j = 1 ; j < heapSeq[i+1].size() ; j++ ){
                cout << heapSeq[i+1][j] ;
                if( j < heapSeq[i+1].size() - 1 ){
                    cout << " " ;
                }
            }
            return 0 ;
        }
    }
}

```