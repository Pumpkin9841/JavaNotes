---
title: PAT B1014 福尔摩斯的约会
date: 2021-02-16 22:15:36.027
updated: 2021-05-31 13:48:19.524
url: https://pumpkn.xyz/archives/patb1014fu-er-mo-si-de-yue-hui
categories: 算法
tags: 算法 | string
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805260223102976/problems/994805308755394560)
![image.png](https://pumpkn.xyz/upload/2021/03/image-07d40d26fe82445e97c6d8a30ea8fb6a.png)

# 题解
数字与字母的ascii码</br>
a-z：97-122 </br>

A-Z：65-90  </br>

0-9：48-57  </br>

# 代码
```c++

#include <iostream>
#include <cstdio>
#include <string>
 
using namespace std;
 
int main()
{
    string s1, s2, s3, s4;
 
    cin >> s1 >> s2 >> s3 >> s4;
 
    int len1 = s1.length() < s2.length() ? s1.length() : s2.length();    //s1 s2 两者中的最小长度
 
    int mark = 1;     //判断是否已经找到'星期几'
 
    char weekday, hour;
 
    int minute;
 
    int len2 = s3.length() < s4.length() ? s3.length() : s4.length();    //s3 s4 两者中的最小长度
 
    for(int i = 0; i < len1; i++)
    {
        if(s1[i] == s2[i] && mark && s1[i] >= 'A' && s1[i] <= 'G')     //星期几
        {
            weekday = s1[i];
            mark = 0;
            switch(weekday)         //或者是if-else
            {
            case 'A' :
                cout << "MON ";
                break;
            case 'B' :
                cout << "TUE ";
                break;
            case 'C' :
                cout << "WED ";
                break;
            case 'D' :
                cout << "THU ";
                break;
            case 'E' :
                cout << "FRI ";
                break;
            case 'F' :
                cout << "SAT ";
                break;
            case 'G' :
                cout << "SUN ";
                break;
            }
            continue;
        }
        if(s1[i] == s2[i] && mark == 0 && ((s1[i]>='A' && s1[i]<='N') || (s1[i] >= '0' && s1[i] <= '9')))  //注这里不能使用isupper()函数  因为范围是'A' ~ 'N'
        {
            hour = s1[i];
            break;
        }
    }
 
    for(int i = 0; i < len2; i++)
    {
        if(s3[i] == s4[i] && isalpha(s3[i]))     //必须是字母
        {
            minute = i;
            break;
        }
    }
 
    if(hour >= '0' && hour <= '9'){
        cout << '0' << hour << ':';
    }else{
    cout << hour - 'A' + 10 << ':';
    }
 
    printf("%02d", minute);
 
    return 0;
}

```