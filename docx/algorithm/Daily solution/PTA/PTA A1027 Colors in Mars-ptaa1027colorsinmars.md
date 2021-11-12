---
title: PTA A1027 Colors in Mars
date: 2021-05-30 00:09:25.849
updated: 2021-05-30 00:09:25.849
url: https://pumpkn.xyz/archives/ptaa1027colorsinmars
categories: 算法
tags: 算法 | 进制转换
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805470349344768)
![image.png](https://pumpkn.xyz/upload/2021/05/image-e04fe62be8d04297a69fcdc4c1aaecbe.png)

# 题解
由于题目的数据范围为[0，168]，因此给定的整数x在十三进制下一定可以表示为x=a*131+b*130（因为168< 132），于是只要想办法求出a跟b即可。</br>
事实上，对上面的等式两边同时整除13，可以得到x/13=al；对上面的等式两边同时对13取模，可以得到x%13=b，这样就得到了a与b，接下来只需要输出ab即可。但是题目要求十三进制下的10，11，12分别用A，B、C代替，因此不妨开一个char型数组RGB[13]来表示这种关系。
# 注意点
本题也可以采用正常的进制转换写法，然后输出转换的结果（同时要根据位数来确实是否需要输出多余的0）
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const char RGB[13] = {'0' , '1' , '2' , '3' , '4' , '5' , '6' , '7' , '8' , '9' , 'A' , 'B' , 'C' } ;

int main(){

    int a , b ,c ;
    cin >> a >> b >> c ;
    cout << "#" ;
    printf("%c%c",RGB[a/13] , RGB[a%13]) ;
    printf("%c%c",RGB[b/13] , RGB[b%13]) ;
    printf("%c%c",RGB[c/13] , RGB[c%13]) ;

}

```