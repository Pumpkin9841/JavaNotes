---
title: leetcode 690. 员工的重要性
date: 2021-05-01 23:49:29.734
updated: 2021-05-01 23:49:29.734
url: https://pumpkn.xyz/archives/leetcode690yuan-gong-de-zhong-yao-xing
categories: 算法
tags: 算法 | 递归
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/employee-importance/submissions/)
![image.png](https://pumpkn.xyz/upload/2021/05/image-019ab5570153473da768dc3db805ef69.png)

# 题解
题目求的指定员工及其所有下属的重要性之和(不止是直系下属)，采用递归的方法，设置递归出口为某个员工的直系下属数组为空。


# 代码
```c++
/*
// Definition for Employee.
class Employee {
public:
    int id;
    int importance;
    vector<int> subordinates;
};
*/

class Solution {
public:
    vector<int> subor ;
    int res = 0 ;
    void get( vector<int> subors , vector<Employee*> employees ){
        
        for( int i = 0 ; i < subors.size() ; i++ ){
            for( int j = 0 ; j < employees.size() ; j++ ){
                if( subors[i] == employees[j]->id ){
                    res += employees[j]->importance ;
                    if( employees[j]->subordinates.size() != 0 ){
                        get( employees[j]->subordinates , employees ) ;
                    }
                }
            }
        }

    }


    int getImportance(vector<Employee*> employees, int id) {

        for( int i = 0 ; i < employees.size() ; i++ ){
            if( employees[i]->id == id ){
                subor = employees[i]->subordinates ;
                res += employees[i]->importance ;
            }
        }

        get( subor , employees ) ;
        return res ;

    }
};
```