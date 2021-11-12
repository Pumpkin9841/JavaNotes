---
title: PTA A1035 Password
date: 2021-06-01 17:33:55.953
updated: 2021-06-01 17:33:55.953
url: https://pumpkn.xyz/archives/ptaa1035password
categories: 算法
tags: 算法 | string | map   
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805454989803520)
![image.png](https://pumpkn.xyz/upload/2021/06/image-fd6e1b8dfb754f3596616e96066854d3.png)
# 题解
将每行的输入以空格分成userName与password两部分，将password赋值给passwordTmp,之后遍历每个密码判断是否需要替换，若执行替换操作后的password == passwordTmp，说明原密码不需要替换，否则替换成功。</br>
将替换了的账户的用户名与密码存入map<string ,string>中，需要注意的是，<font color = 'red'> map的底层通过红黑树实现，即会根据键自动排序 </font>，所以需要再起一个vector<string>数组按输入顺序保存用户名，以便保证输出时的顺序与输入顺序相同。</br>
c++中有```unordered_map```可以不实现自动排序，不过我测试没用。

# 代码
```c++
#include<bits/stdc++.h>
#include <unordered_map>
using namespace std ;

int main(){
    int n ;
    cin >> n ;
    //接收多余的回车
    getchar() ;
    map<string , string > mp ;
    vector<string> names ;
    string str ;
    int count = 0 ;
    for(int i = 0 ; i < n ; i++){
        getline( cin , str ) ;
        string userName = str.substr( 0 , str.find(" ") ) ;
        string password = str.substr( str.find(" ")+1 , str.length() ) ;
        string passwordTmp = password ;
        for( int j = 0 ; j < password.length() ; j++ ){
            if( password[j] == '1' ){
                password[j] = '@' ;
            }
            else if( password[j] == '0' ){
                password[j] = '%' ;
            }
            else if( password[j] == 'l' ){
                password[j] = 'L' ;
            }
            else if( password[j] == 'O' ){
                password[j] = 'o' ;
            }
        }

        if( passwordTmp != password ){
            count ++ ;
            mp[userName] = password ;
            names.push_back(userName) ;
        }
    }

    if( count == 0 && n != 1 ){
        cout << "There are " << n << " accounts and no account is modified" ;
    }
    else if ( count == 0 && n == 1){
        cout << "There is 1 account and no account is modified" ;
    }
    else{
        cout << count << endl;
        for( int i = 0 ; i < names.size() ; i++ ){
            cout << names[i] << " " << mp[ names[i] ] <<endl ;

        }
    }

    return 0 ;
}

```