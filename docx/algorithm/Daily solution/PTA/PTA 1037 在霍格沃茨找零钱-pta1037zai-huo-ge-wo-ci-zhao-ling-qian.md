---
title: PTA 1037 在霍格沃茨找零钱
date: 2021-05-29 16:44:08.259
updated: 2021-05-29 17:06:49.637
url: https://pumpkn.xyz/archives/pta1037zai-huo-ge-wo-ci-zhao-ling-qian
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805284923359232)
![image.png](https://pumpkn.xyz/upload/2021/05/image-a0757cde2fb449a2b8af8a1077b54996.png)

# 题解
## 思路一
将实际付款A与应付款P全转化为纳特比较，存在两种情况</br>
1. 实际付款A >= 应付款P</br>
	- 这种情况只需要将实际付款-应付款即可，注意减法过程中的借位。
2. 实际付款A < 应付款P
	- 用应付款P-实际付款A，同样需要注意借位操作
	- 最后的加隆值应取相反数

## 思路二
根据题意可知，1个Galleon可以兑换17 x 29个Knut，1个Sickle可以兑换29个Knut，因此直接把货币全部转换成Knut来计算。于是第二个减去第一个即可得到要找的钱，假设为K。由于此时单位是Knut，因此若要转换为原来的格式，就有K/（17 x 29）个Galleon，K%（17x 29）/29个 Sickle，K%29个 Knut。
### 注意
获得Knut为单位的找零的钱K后要将它取绝对值，不能直接把负数直接代入后面的运算。

# 代码
## 思路一
```c++
#include<bits/stdc++.h>
using namespace std ;

void getInt( string str , vector<int>& money ){
    int postion = str.find(".") ;
    while( postion != string::npos ){
        string tmpNum = str.substr(0,postion) ;
        stringstream ss ;
        ss << tmpNum ;
        int intMoney  ;
        ss >> intMoney ;
        money.push_back(intMoney) ;
        str = str.substr(postion+1 , str.length()) ;
        postion = str.find(".") ;
    }
    stringstream s ;
    s << str ;
    int tmp1 ;
    s >> tmp1 ;
    money.push_back(tmp1) ;

}


int main(){
    //P为应付，A为实际付款
    string P , A ;
    cin >> P >> A ;

    vector<int> needPay ;
    vector<int> factPay ;
    vector<int> res ;

    getInt( P , needPay ) ;
    getInt( A , factPay ) ;

    int sumP = needPay[0]*17*29 + needPay[1]*29 + needPay[2] ;
    int sumA = factPay[0]*17*29 + factPay[1]*29 + factPay[2] ;

    //实际付款大于应付款，应找零钱
    if( sumA >= sumP ){
        //计算纳特
        if( factPay[2] >= needPay[2] ){
            res.push_back( factPay[2] - needPay[2] ) ;
        }
        else{
            factPay[1] -- ;
            factPay[2] += 29 ;
            res.push_back( factPay[2] - needPay[2] ) ;
        }

        //计算银西
        if( factPay[1] >= needPay[1] ){
            res.push_back( factPay[1] - needPay[1] ) ;
        }
        else{
            factPay[0] -- ;
            factPay[1] += 17 ;
            res.push_back( factPay[1] - needPay[1] ) ;
        }

        res.push_back( factPay[0] - needPay[0] ) ;

    }
    //实际付款小于应付款，输出负数
    else{
         //计算纳特
        if( factPay[2] <= needPay[2] ){
            res.push_back( needPay[2] - factPay[2] ) ;
        }
        else{
            needPay[1] -- ;
            needPay[2] += 29 ;
            res.push_back( needPay[2] - factPay[2] ) ;
        }

        //计算银西
        if( factPay[1] <= needPay[1] ){
            res.push_back( needPay[1] - factPay[1] ) ;
        }
        else{
            needPay[0] -- ;
            needPay[1] += 17 ;
            res.push_back( needPay[1] - factPay[1] ) ;
        }

        res.push_back( -(needPay[0] - factPay[0]) ) ;
    }



    for( int i = res.size()-1 ; i >= 0 ; i--  ){
        cout << res[i]  ;
        if( i != 0 ){
            cout << "." ;
        }
    }

    return 0 ;
}


```

## 思路二
```c++
#include<bits/stdc++.h>
using namespace std ;

const int Galleon = 17 * 29 ;
const int Sickle = 29 ;

int main(){

    //应付款
    int a1 , a2 , a3 ;
    //实际付款
    int b1 , b2 , b3 ;

    scanf("%d.%d.%d %d.%d.%d" , &a1,&a2,&a3,&b1,&b2,&b3) ;
    int need = a1*Galleon + a2*Sickle + a3 ;
    int fact = b1*Galleon + b2*Sickle + b3 ;
    int change = fact - need ;

    if( change < 0 ){
        //输出负号
        cout << "-" ;
        //change取绝对值
        change = -change ;
    }

    printf("%d.%d.%d" , change /Galleon , change%Galleon/Sickle , change%Sickle) ;

}

```