---
title: PAT B1044 火星数字
date: 2021-01-13 18:59:51.697
updated: 2021-02-20 15:46:11.415
url: https://pumpkn.xyz/archives/patb1044huo-xing-shu-zi
categories: 算法
tags: 积累 | 算法
---

# 题目
![PATB1044.png](http://pumpkn.xyz:8090/upload/2021/02/PATB1044-6020aedfc99e4ea08b70c499925ea1bd.png)
# 代码
```c++
#include<cstdio>
#include<string>
#include<iostream>
#include<map>
#include<vector>
#include<sstream>
#include<cstdlib>
using namespace std ;



int main(){
    int n  ;
    scanf("%d%*c" , &n) ;

    string fire[13] = {"tret", "jan" , "feb" , "mar" , "apr" ,"may"
                , "jun" , "jly" ,"aug" , "sep" , "oct" , "nov" , "dec"} ;

    string fireAdd[13] = {"tret" , "tam" , "hel" , "maa" , "huh" ,
                "tou" , "kes" , "hei" , "elo" ,"syy" , "lok" , "mer" , "jou"} ;


    map<string , int> fireToMan ;

    map<string , int > fireAddToMan ;

    for(int i = 0 ; i < 13 ; i++){
        fireToMan[fire[i]] = i ;
        fireAddToMan[fireAdd[i]] = i ;
    }

    vector<string> str ;

    while( n-- ){

        string tmp ;
        getline(cin , tmp) ;
        str.push_back(tmp) ;

    }

    for(int i = 0 ; i < str.size() ; i++){
        //如果是地球文
        if(str[i][0] >= '0' && str[i][0] <= '9'){
            //将字符串转换成数字
            int num ;
            stringstream ss ;
            ss << str[i] ;
            ss >> num ;
            if(num < 13){
                cout << fire[num] << endl ;
            }
            //13的倍数不应该输出个位火星文，如13输出tam ，而不是tam tret
            else if(num % 13 == 0){
                cout << fireAdd[num/13] <<endl ;
            }
            else{
                int shiwei = num / 13 ;     //获取十位数字
                int gewei = num % 13 ;      //获取个位数字
                cout << fireAdd[shiwei] << " " << fire[gewei] << endl ;
            }
        }
        //如果是火星文
        else{
            //如果存在进位
            if(str[i].find(" ") != string::npos){
                int pos = str[i].find(" ") ;
                int live = str[i].length() - pos ;
                string jin = str[i].substr(0 , pos) ;
                string ge = str[i].substr(pos+1,live) ;
                int res = fireAddToMan[jin]*13 + fireToMan[ge];
                cout << res << endl ;
            }
            //不存在进位
            else{
                if( str[i] == "tret" ){
                    cout << 0 << endl ;
                    continue ;
                }

                 if( fireAddToMan[str[i]] != NULL){
                    cout << fireAddToMan[str[i]] * 13  << endl ;
                }

                if( fireToMan[str[i]] != NULL){
                    cout << fireToMan[str[i]] << endl ;
                }

            }
        }

    }


    return 0 ;
}

```