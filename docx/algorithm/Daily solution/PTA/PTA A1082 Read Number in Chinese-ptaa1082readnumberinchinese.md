---
title: PTA A1082 Read Number in Chinese
date: 2021-06-01 21:03:54.298
updated: 2021-06-01 21:05:05.544
url: https://pumpkn.xyz/archives/ptaa1082readnumberinchinese
categories: 算法
tags: 算法 | string
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805385053978624)
![image.png](https://pumpkn.xyz/upload/2021/06/image-36147279b1654e96a84897a1eb9ab17b.png)
# 题意
按中文发音规则输出一个绝对值在9位以内的整数。需要明确如下规则：</br>
1. 如果在数字的某节（例如个节、万节、亿节）中，某个非零位（该节的千位除外）的高位是零，那么需要在该非零位的发音前额外发音一个零。例如8080的发音为"ba qian ling ba shi"，8008的发音为"ba qian ling ba"，10808的发音为"yi Wan ling ba Bai ling ba".
2. 每节的末尾要视情况输出方或者亿（个节除外）。


# 题解
步骤1：整体思路是将数字按字符串方式处理，并设置下标let和right来处理数字的每一个节（个节、万节、亿节）的输出，即令left指向当前需要输出的位，而right指向与left同节的个位。</br>
步骤2：在需要输出的某个节中，需要解决的问题是如何处理额外发音的零。事实上，可以直接采用规则0中的表述来设计如下的算法：</br>
设置bool型变量flag表示当前是否存在累积的零。当输出let指向的位之前，先判断该位是否为0：如果为0，则令flag为true，表示存在累积的零；如果非0，则根据flag的值来判断是否需要输出额外的零。在这之后，就可以输出该位本身以及该位对应的位号（十、百、干）。而当整一小节处理完毕后，再输出万或者亿。


# 注意
1. 边界数据0的输出应该为"ling"，编者采用的处理方法是在步骤2判断当前位是否为0时增加判断当前位是否为首位，只有当前位不是首位且为0时才令flag为true（详见代码）。</br>
2. 让left和right指向一个节的首尾可以采用如下方法：初始先令left =0，即用left指向首位；而令right = len-1，即用right指向末位，其中len表示字符串长度。之后不断让right减4，并控制left +4不超过right，就可以得到第一个节。由于left会在输出过程中自增至下一节的首位，因此只需要在当前节处理完毕后令right加4，即可让right指向下一节的末位。</br>
3. 如果万节所有位都为0，那么就要注意不能输出多余的万。例如，800000008的输出应该是"ba Yi ling ba"而不是"ba Yi Wan ling ba".



# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

string nums[10] = { "ling" ,  "yi" , "er" , "san" , "si" , "wu" , "liu" , "qi" , "ba" , "jiu" } ;
string money[5] = { "Shi" , "Bai" , "Qian" , "Wan" , "Yi" } ;

int main(){
    string str ;
    getline( cin , str) ;
    if( str[0] == '-' ){
        str = str.substr( 1 , str.length() ) ;
        cout << "Fu" <<" " ;
    }

    int left = 0 ;
    int right = str.length() - 1 ;

    //将right每次左移4位，直到right与left在同一节
    while( left + 4  <= right ){
        right = right - 4 ;
    }

    while( left < str.length() - 1 ){
        bool flag = false ; //flag为false时表示前面没有积累0
        bool isPrint = false ;//isPrint为false表示当前节没有输出

        //依次处理每一节的每一位
        while( left <= right ){
            if( left > 0 && str[left] == '0' ){
                flag = true ;
            }
            else{
                if( flag == true ){     //存在累积的零
                    cout << " ling" ;
                    flag = false ;
                }

                if( left > 0 ){
                    cout << " " ;
                }

                cout << nums[ str[left] - '0' ] ;
                isPrint = true ;  //该节至少有一位输出
                if( left != right ){
                    cout <<  " " << money[ right - left - 1 ] ;
                }

            }
            left ++ ;
        }
        if( isPrint == true && right != str.length() - 1){
            cout << " " << money[ (str.length() - 1 - right)/4 +2 ] ;
        }
        right += 4 ;
    }


    return 0 ;
}

```