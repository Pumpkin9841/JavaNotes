---
title: leetcode 1030. 距离顺序排列矩阵单元格
date: 2021-03-15 12:57:57.447
updated: 2021-03-18 13:01:01.502
url: https://pumpkn.xyz/archives/leetcode1030ju-li-shun-xu-pai-lie-ju-zhen-dan-yuan-ge
categories: 算法
tags: 算法 | BFS    | 排序
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/matrix-cells-in-distance-order/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-2480e2e22e1c4251819467e6f9db6507.png)

# 题解
这种题直接排序就行了，我第一反应居然是用BFS思想，写了82行，耗时耗力。提交通过不出所料双低。
![image.png](https://pumpkn.xyz/upload/2021/03/image-04839e38e4094fb88f322ebe02853c69.png)
# 代码（愚蠢的BFS方法)
```c++
class Solution {
public:

    struct Node{
        vector<int> marix ; //坐标
        int distance ;   //距离
    }node;

    vector<Node> res ;

    static bool cmp(Node a , Node b){
        return a.distance < b.distance ;
    }

    bool inq[110][110] = {false} ; //false表示当前没在队列中

    //坐标增量
    int X[4] = { 1 , -1 , 0 , 0 } ;
    int Y[4] = { 0 , 0 , 1 , -1 } ;

    bool judge( int x , int y , int R , int C ){
        //越界判断
         if(x >= R || y >= C || x < 0 || y <  ){
             return false ;
         }

        //该节点已经计算过
        if( inq[x][y] == true ){
            return false ;
        }
        return true ;
    }

    void DFS( int x , int y , int R , int C , int r0 , int c0 ){
        queue<Node> Q ;
        node.marix.push_back(x) ;
        node.marix.push_back(y) ;
        node.distance = abs(x-r0) + abs(y - c0) ;
        res.push_back(node) ;
        Q.push(node) ;
        inq[x][y] = true ;

        while( !Q.empty() ){
            Node top = Q.front() ;
            Q.pop() ;
            for( int i = 0 ; i < 4 ; i++ ){
                int newX = top.marix[0] + X[i] ;
                int newY = top.marix[1] + Y[i] ;
                if( judge(newX , newY , R , C) ){
                    inq[newX][newY] = true ;
                    Node t ;
                    t.marix.push_back(newX) ;
                    t.marix.push_back(newY) ;
                    t.distance = abs(newX - r0) + abs(newY - c0) ;
                    res.push_back(t) ;
                    Q.push(t) ;
                }

            }
        }
        
    }

    vector<vector<int>> allCellsDistOrder(int R, int C, int r0, int c0) {
        for( int x = 0 ; x < R ; x++ ){
            for( int y = 0 ; y < C ; y++ ){
                if( inq[x][y] == false ){
                    DFS(x,y ,R ,C ,r0 ,c0) ;
                }
            }
        }
        sort(res.begin() , res.end() , cmp) ;
        vector< vector<int> > ans ;
        for( int i = 0 ; i < res.size() ; i++ ){
            vector< int > tmp ;
            tmp.push_back(res[i].marix[0]) ;
            tmp.push_back(res[i].marix[1]) ;
            ans.push_back(tmp) ;
        }
        return ans ;
    }
};
```

# 直接排序(官方题解)
```c++
class Solution {
public:
    vector<vector<int>> allCellsDistOrder(int R, int C, int r0, int c0) {
        vector<vector<int>> ret;
        for (int i = 0; i < R; i++) {
            for (int j = 0; j < C; j++) {
                ret.push_back({i, j});
            }
        }
        sort(ret.begin(), ret.end(), [=](vector<int>& a, vector<int>& b) {
            return abs(a[0] - r0) + abs(a[1] - c0) < abs(b[0] - r0) + abs(b[1] - c0);
        });
        return ret;
    }
};
```