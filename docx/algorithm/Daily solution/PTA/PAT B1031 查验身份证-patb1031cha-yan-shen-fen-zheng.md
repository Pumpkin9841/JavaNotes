---
title: PAT B1031 查验身份证
date: 2021-01-20 13:29:15.046
updated: 2021-02-24 13:29:38.002
url: https://pumpkn.xyz/archives/patb1031cha-yan-shen-fen-zheng
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805290334011392)
![image.png](https://pumpkn.xyz/upload/2021/02/image-10e3699b03a6467c9fd1991be0c75fa1.png)
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

char z[11] = {'1' , '0' , 'X' , '9' , '8' , '7' , '6' ,'5' , '4' , '3' , '2'} ;
int quan[17] = {7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2};


int main(){
    int n ;
    bool flag = true ;  //标记是否全对
    cin >> n ;
    getchar() ; //接收输入n后的回车
    string strId[n] ;
    for(int i = 0 ; i < n ; i++){
        getline(cin , strId[i]) ;
    }

    for(int i = 0 ; i < n ; i++){
        int quanSum = 0 ;
        for(int j = 0 ; j < 17 ; j++){
            int tmp = strId[i][j] - '0' ;
            quanSum += tmp*quan[j] ;
        }
        int index = quanSum%11 ;
        char m = z[index] ;
        //如果校验码不等
        if( m != strId[i][17]){
            cout << strId[i] << endl ;
            flag = false ;
        }
    }

    if(flag == true ){
        cout << "All passed" ;
    }

    return 0 ;
}

```