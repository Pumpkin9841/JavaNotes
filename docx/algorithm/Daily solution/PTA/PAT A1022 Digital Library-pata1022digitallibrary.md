---
title: PAT A1022 Digital Library
date: 2021-02-11 12:20:32.161
updated: 2021-02-20 15:52:52.769
url: https://pumpkn.xyz/archives/pata1022digitallibrary
categories: 算法
tags: 算法 | string | map   
---

# 题目
![image.png](https://pumpkn.xyz/upload/2021/02/image-52b23cd05ae1470686e338cab1b8cbb8.png)
# 题意

# 题解

## 关键字的输入
由于每本书都可能有多个关键字(以空格分隔),因此需要将单词分隔开来。一个比较好的办法就是使用cin来读入单词，然后用getchar接收再这个关键词后面的字符：如果是换行符，则说明关键词的输入结束；如果是空格，则继续读入关键词。这样做的原因是，<font color ="red">cin读入字符串是以空格或换行为截至标志的。</font>
## 关键字输入的代码
```c++
while( cin >> key ){	//每次读入单个关键字
	mpKey[key].insert(id) ;	  //把id添加到key对应的集合
	c = getchar() ;      //接收关键字后面的字符
	if( c == '\n'){     //如果是换行，关键字接收结束
	    break ;
	}	
}
```

# 代码

## 方法一
```c++
#include<cstdio>
#include<string>
#include<iostream>
#include<map>
#include<vector>
#include<sstream>
#include<algorithm>
using namespace std ;

struct Book{
    int id ;
    string title ;  //书名
    string author ;     //作者
    vector<string > keywords ;  //关键字，以空格分割，不超过五个
    string publisher ;      //出版社
    int year ;      //出版年份
}book[10010] ;

//以id升序排序
bool cmp(Book a , Book b){
    return a.id < b.id ;
}

int main(){
    int N ; //总数
    scanf("%d%*c" , &N) ;
    for(int i = 0 ; i < N ; i++){
        scanf("%d%*c" , &book[i].id) ;
        getline(cin , book[i].title) ;
        getline(cin , book[i].author) ;

        //处理关键字
        string str ;
        getline(cin , str) ;
        while(str.find(" ") != string::npos){
            int pos = str.find(" ") ;
            string tmp  ;
            tmp = str.substr(0,pos) ;
            str = str.substr(pos+1 , str.length()) ;
            book[i].keywords.push_back(tmp) ;
            //收尾工作，若后面再无空格将str输出
            if(str.find(" ") == string::npos){
                book[i].keywords.push_back(str) ;
            }
        }

        getline(cin , book[i].publisher) ;
        cin >> book[i].year ;
    }

  int M ;
  scanf("%d%*c" , &M) ;
  for(int i = 0 ; i < M ; i++){
    string query ;
    getline(cin , query) ;
    string queryNo ;
    string question ;
    queryNo = query.substr(0 , query.find(":")) ;
    question = query.substr(query.find(":")+2 , query.length()) ;


    //按id升序排序
    sort(book , book+N , cmp ) ;

    cout << query << endl ;
    //查询书名
    if( queryNo == "1"){
        int flag = 0 ;   //flag为1表示找到，为0输出Not Found ;
        for(int j = 0 ; j < N ; j++){
            if(book[j].title == question){
                cout << book[j].id << endl ;
                flag = 1 ;
            }
        }
        if( flag == 0 ){
            cout << "Not Found" <<endl ;
        }
    }
    //查询作者
    if( queryNo == "2" ){
        int flag = 0 ;   //flag为1表示找到，为0输出Not Found ;
        for(int j = 0 ; j < N ; j++){
            if(book[j].author == question){
                cout << book[j].id << endl ;
                flag = 1 ;
            }
        }
        if( flag == 0 ){
            cout << "Not Found" <<endl ;
        }
    }
    //查询关键字
    if( queryNo == "3" ){
        int flag = 0 ;   //flag为1表示找到，为0输出Not Found ;
        for(int j = 0 ; j < N ; j++){
            for(int p = 0 ; p < book[j].keywords.size() ; p++){
                if( question == book[j].keywords[p] ){
                    cout << book[j].id  << endl ;
                    flag = 1 ;
                    break ;
                }
            }
        }
         if( flag == 0 ){
            cout << "Not Found" <<endl ;
        }

    }
    //查询出版社
    if( queryNo == "4" ){
        int flag = 0 ;   //flag为1表示找到，为0输出Not Found ;
        for(int j = 0 ; j < N ; j++){
            if(book[j].publisher == question){
                cout << book[j].id << endl ;
                flag = 1 ;
            }
        }
        if( flag == 0 ){
            cout << "Not Found" <<endl ;
        }
    }
    //查询出版日期
    if( queryNo == "5" ){
        int flag = 0 ;   //flag为1表示找到，为0输出Not Found ;
        stringstream ss ;
        ss << question ;
        int number ;
        ss >> number ;
        for(int j = 0 ; j < N ; j++){
            if(book[j].year == number){
                cout << book[j].id << endl ;
                flag = 1 ;
            }
        }
        if( flag == 0 ){
            cout << "Not Found" <<endl ;
        }
    }



  }


    return 0 ;
}

```

## 方法二