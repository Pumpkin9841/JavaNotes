---
title: leetcode 1710. 卡车上的最大单元数
date: 2021-05-25 23:30:43.099
updated: 2021-05-25 23:30:43.099
url: https://pumpkn.xyz/archives/leetcode1710ka-che-shang-de-zui-da-dan-yuan-shu
categories: 算法
tags: 算法 | 排序
---

# 题解
## [原题链接](https://leetcode-cn.com/problems/maximum-units-on-a-truck/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-b57338b0adc34596b9281fd33f46e9b5.png)

# 题解
使用贪心算法，优先取容量大的箱子即可。

# 代码
```c++
class Solution {
public:

    struct Box{
        int num ; 
        int weight ;
    }boxes[1010];

    static bool cmp( Box a , Box b ){
        if( a.weight != b.weight ){
            return a.weight > b.weight ;
        }
        else{
            return a.num > b.num ;
        }
    }

    int maximumUnits(vector<vector<int>>& boxTypes, int truckSize) {
        for( int i = 0 ; i < boxTypes.size() ; i++ ){
            boxes[i].num = boxTypes[i][0] ;
            boxes[i].weight = boxTypes[i][1] ;
        }

        sort( boxes , boxes+boxTypes.size() , cmp ) ;

        int res = 0 ;
        int number = 0 ;
        int index = 0 ;
        while( ( number < truckSize ) && ( index < 1010 ) ){
            if(  number + boxes[index].num <= truckSize  ){
                res += boxes[index].num*boxes[index].weight ;
               // cout << index << endl ;
                number += boxes[index].num ; 
                index ++ ;
            }
            else{
                int left = truckSize - number ;
                res += left * boxes[index].weight ;
                number = truckSize ;
                index ++ ;
            }
        }

        //cout << boxes[2].num << " " << boxes[2].weight ; 
        return res ;
    }
};
```