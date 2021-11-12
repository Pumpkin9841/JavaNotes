---
title: PAT A1060 Are They Equal
date: 2021-02-08 02:59:33.895
updated: 2021-02-20 15:53:22.477
url: https://pumpkn.xyz/archives/pta1060aretheyequal
categories: 算法
tags: 算法 | string
---

# 题目
![PATA1060.png](https://pumpkn.xyz/upload/2021/02/PATA1060-8c9cecaf442549e6ad4586124663bfb9.png)

# 题意
 给出两个数，问：将他们写成保留N位小数的科学计数法后是否相等。如果相等，则输出YES，并给出转换结果：如果不相等，则输出NO，并分别给出两个数的转换结果。

# 题解
&ensp;&ensp;科学计数法一定是0.abc...*10^e的格式，要比较是否相等我们只需要拿到本体数据adc...和指数e即可。
&ensp;&ensp;由此，可以想到可以分成</br>
&ensp;&ensp;&ensp;&ensp;1.整数部分为0的0.abc...形式 ；</br>
&ensp;&ensp;&ensp;&ensp;2.整数部分不为0的abc.efg...形式 。

&ensp;&ensp;本题中隐含了一个陷阱：输入的数字可能含有前导0，即输入0012.345也是合法的。所以直接判断第一位是否为0来区别以上两种情况是不可取的。

## 步骤
### 1.去除前导0
&ensp;&ensp;若输入的数字前面有0则删除，去除前导0后，如果第一位为"."则表示该数整数部分为0，否则，反之。
### 2.整数部分为0的情况
&ensp;&ensp;删除小数点，若输入的数字为0.0001，为了拿到主体数据部分，我们还需要再去除一次零元素，每去掉一个零，其指数e需要-1，才能保证科学计数法表示的数据正确。
### 3.整数部分不为0的情况
&ensp;&ensp;去除前导0后，从左往右依次寻找小数点的位置，找到后删除小数点，获得的数据为主体数据，每向右一位，指数e需要+1.
# 代码
```c++
#include<iostream>
#include<cstdio>
#include<string>
using namespace std ;

int n ;

string getDigit(string str , int &e){
    //为了应对有前导0的情况，如输入001.12或者000.123，先处理前导0
    while(str[0] == '0' && str.length() > 0){
        str.erase(str.begin()) ;
    }



    int k = 0 ;
    //如果去掉前导0后，该字符串第一位为小数点，则表示该数整数部分为0
    if( str[0] == '.' ){
        str.erase(str.begin()) ;    //删掉小数点
        while(str.length() > 0 && str[0] == '0' ){
            str.erase(str.begin()) ;
            e -- ;
        }
    }else{
        while( k < str.length() && str[k] != '.'){
            k ++ ;
            e ++ ;
        }
          //退出while后，如果k<str.length(),表示找到小数点了
        if(k < str.length() ){
            str.erase(str.begin() + k) ;   //删除小数点
        }
    }

     //如果退出while 且数字的长度为0，表示该数字值为0 ,指数e返回0
    if(str.length() == 0 ){
        e = 0 ;
    }


    int num = 0 ;
    k = 0 ;
    string res ;
    while(num < n ){
        if( k < str.length())
            res += str[k++] ;   //后面还有数字，则加上数字
        else{
            res += '0' ;     //后面没有数字则舔0
        }
        num ++ ;
    }
    return res ;
}


int main(){
    string str1 ;
    string str2 ;
    string s3 ;
    string s4 ;

    cin >> n >> str1 >> str2 ;
    int e1 = 0 ;
    int e2 = 0 ;

    s3 = getDigit(str1 , e1) ;
    s4 = getDigit(str2 , e2) ;

    if( s3 == s4 && e1 == e2 ){
        cout << "YES" << " " << "0." << s3 << "*10^" << e1 ;
    }else{
        cout << "NO" << " " << "0." << s3 << "*10^" << e1 << " " <<"0." << s4 << "*10^" << e2 ;
    }

    return 0 ;
}

```