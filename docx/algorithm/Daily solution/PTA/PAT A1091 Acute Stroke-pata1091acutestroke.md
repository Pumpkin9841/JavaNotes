---
title: PAT A1091 Acute Stroke
date: 2021-02-22 15:26:45.692
updated: 2021-02-22 15:27:44.005
url: https://pumpkn.xyz/archives/pata1091acutestroke
categories: 算法
tags: 算法 | BFS   
---

# 题目
![PAT A1091 Acute Stroke.png](https://pumpkn.xyz/upload/2021/02/PAT%20A1091%20Acute%20Stroke-9409f5ba1aa34989b7142d67c1073204.png)
# 题意
给出一个三维数组，数组元素的取值为0或1。与某一个元素相邻的元素为其上、下、左、右、前、后这6个方向的邻接元素。另外，若干个相邻的"1"称为一个"块"(不必两两相邻，只要与块中某一个"1"相邻，该"1"就在块中)。而如果某个块中的"1"的个数不低于T个，那么称这个块为"卒中核心区"。现在需要求解所有卒中核心区中的1的个数之和。
# 题解
该题是一个三维的BFS，思路跟二维的BFS是一样的。</br>
基本思路是：枚举三维矩阵中的每一个位置，如果为0，则跳过，如果为1，则使用BFS查询与该位置相邻的前、后、左、右、上、下六个位置(前提是不出边界)，判断他们是否为1。</br>
为了防止重复，可以设置一个bool型数组inq来记录每个位置是否在BFS中已经入过队。</br>
由于题目限制了核心区中1的个数下限，因此只有当块中的1的个数不低于这个下限时，才计入总数。

## 注意点
输入数据是按多个二维矩阵输入的，因此输入时3层for循环中的第一层需要枚举矩阵编号，即z轴，第二、三层才是单个矩阵的输入。
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxH = 1296 ;
const int maxL = 138 ;
const int maxZ = 70 ;

struct Node{
    int x, y , z ;
}node;

int stroke[maxH][maxL][maxZ] ;   //存储stroke core
bool inq[maxH][maxL][maxZ] = {false} ;
//m为行，n为列，l为二维矩阵的个数，t为门阀值
int m ,n , l ,t;
//坐标增量
int X[6] = {0,0,0,0,1,-1} ;
int Y[6] = {0,0,1,-1,0,0} ;
int Z[6] = {1,-1,0,0,0,0} ;


bool judge(int x , int y , int z){
    //越界返回false
    if( x >= m || x < 0 || y >= n || y < 0 || z >= l || z < 0){
        return false ;
    }
    //如果坐标值为0或者已经入过队，返回false
    if( stroke[x][y][z] == 0 || inq[x][y][z] == true ){
        return false ;
    }
    //以上情况都不符合，返回true
    return true ;
}


int BFS(int x , int y , int z){
    int tot = 0 ;
    queue<Node> Q ;
    node.x = x ;
    node.y = y ;
    node.z = z ;
    Q.push(node) ;      //node入队
    inq[x][y][z] = true ;   //标记已入过队
    //队非空
    while( !Q.empty() ){
        Node top = Q.front() ;  //取队头元素
        Q.pop() ;   //队头元素出队
        tot ++ ;    //块中1的个数+1
        for(int i = 0 ; i < 6 ; i++){
            int newX = top.x + X[i] ;
            int newY = top.y + Y[i] ;
            int newZ = top.z + Z[i] ;
            if( judge(newX , newY , newZ) ){
                Node tmp ;
                tmp.x = newX ;
                tmp.y = newY ;
                tmp.z = newZ ;
                Q.push(tmp) ;   //将节点入队
                inq[newX][newY][newZ] = true ;    //标记该节点已入过队
            }
        }
    }

    return tot ;
}

int main(){
    int res = 0 ;
    cin >> m >> n >> l >> t ;
    for(int z = 0 ; z < l ; z++){       //题目给出的输入是以二维矩阵输入的，所以先枚举纵坐标z
        for(int x = 0 ; x < m ; x++){
            for(int y = 0 ; y < n ; y++){
                cin >> stroke[x][y][z] ;
            }
        }
    }


    for(int z = 0 ; z < l ; z++){       //题目给出的输入是以二维矩阵输入的，所以先枚举纵坐标z
        for(int x = 0 ; x < m ; x++){
            for(int y = 0 ; y < n ; y++){
                if( stroke[x][y][z] == 1 && inq[x][y][z] == false ){
                    int tmp = BFS(x,y,z) ;
                    if(tmp >= t){
                        res += tmp ;
                    }
                }
            }

        }
    }

    cout << res ;
    return 0 ;
}

```