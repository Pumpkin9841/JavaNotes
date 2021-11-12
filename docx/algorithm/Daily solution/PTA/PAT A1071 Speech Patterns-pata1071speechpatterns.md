---
title: PAT A1071 Speech Patterns
date: 2021-02-07 20:59:45.841
updated: 2021-02-20 15:53:44.997
url: https://pumpkn.xyz/archives/pata1071speechpatterns
categories: 算法
tags: 算法 | map   
---

# 题目
![PATA1071.jpg](https://pumpkn.xyz/upload/2021/02/PATA1071-17f344986c9243a5b6aa2edf074040ec.jpg)
# 题意
将单词定义为大小写字母、数字的组合。给出一个字符串，问出现最多次数的单词及其出现次数（一切除了大小写字母，数字以外的字符都认为是单词的分隔符）。不区分大小写，且输出结果为小写，若出现字数最多的单词有多个，按字典顺序排序且只输出第一个。
# 题解
主要分两步：</br>
第一步：分割出单词</br>
&ensp;&ensp;遍历输入字符串，判断每个字符是否是有效字符，若不是有效字符，则分割单词，且需要跳过该字符。</br>
第二步：将单词计数（使用map实现）</br>
&ensp;&ensp;map内部使用红黑树实现，即map中所有的键都是按顺序存放的。遍历map，取出第一个值最大的键值对即可。
# 代码
```c++
#include<cstdio>
#include<string>
#include<map>
#include<iostream>
using namespace std ;

bool check(char c){
    if(c >= '0' && c<= '9')
        return true ;
    if(c >= 'A' && c <= 'Z')
        return true ;
    if(c >= 'a' && c <= 'z')
        return true ;
    return false ;
}

int main(){
    string str ;
    map<string , int > mp ;
    getline(cin ,str ) ;

    int i = 0 ;
    while( i < str.length() ){
        string word ;
        while( i < str.length() && check(str[i])){
            //如果是大写字符，转换成小写字符
            if( str[i] >= 'A' && str[i] <= 'Z' ){
                str[i] = str[i] + 32 ;
            }
            word = word + str[i] ;
            i++ ;
        }
        //单词非空
        if(word != ""){
            map< string ,int >::iterator it = mp.find(word) ;
            //如果mp中已经存在该单词，则该单词数量+1
            if( it != mp.end()){
                int num = it->second ;
                mp[word] = num + 1 ;
            }
            //如果不存在，存入该单词，并将数量设置为1
            else{
                mp[word] = 1 ;
            }

        }

        //字符不符合规则，跳过该字符
        while( i < str.length() && !check(str[i]) ){
            i++ ;
        }

    }

    int maxNum = 0 ;
    string res ;
    for( map<string , int >::iterator it = mp.begin() ; it != mp.end() ; it++){
        if(it->second > maxNum){
            res = it->first ;
            maxNum = it->second ;
        }
    }

    cout << res << " " << maxNum ;

    return 0 ;
}

```