---
title: 广度优先搜索BFS
date: 2021-02-21 23:59:24.751
updated: 2021-07-24 19:05:43.852
url: https://pumpkn.xyz/archives/guang-du-you-xian-sou-suo-bfs
categories: 算法
tags: 算法 | BFS   
---

广度优先算法一般由**队列**实现，且总是按照层次的顺序进行遍历，基本写法为（可做模板）:
```c++
void BFS(int s){
    queue<int> q ;
    q.push(s) ;
    while( !q.empty() ){
        取出队首元素top ;
        访问队首元素top ;
	将队首元素出队 ;
	将top的下一层节点中未曾入队的节点全部入队，并设置为已入队 ;
    }
}
```



# 题目
给出一个m*n的矩阵，矩阵中的元素为0或1.称位置(x,y)与其上下左右四个位置是相邻的。如果矩阵中有若干个1个相邻的（不必亮亮相邻），那么称这些1构成了一个“块”。求给定的矩阵中“块”的个数。
## 输入数据
```c++
6 7 // m为6，n为7
0 1 1 1 0 0 1
0 0 1 0 0 0 0
0 0 0 0 1 0 0
0 0 0 1 1 1 0
1 1 1 0 1 0 0
1 1 1 1 0 0 0
```
## 输出
```c++
4
```

# 题解
对这个问题,求解的基本思想是:枚举每一个位置的元素，如果为0，则跳过;如果为1，则使用BFS查询与该位置相邻的4个位置(前提是不出界)，判断它们是否为1 (如果某个相邻的位置为1, 则同样去查询与该位置相邻的4个位置，直到整个"1"块访问完毕)。而为了防止走回头路,一般可以设置一个bool型数组inq (即in queue的简写)来记录每个位置是否在BFS中已入过队。</br>
***一个小技巧是：***对当前位置(x,y)来说，由于与其相邻的四个位置分别为(x,y+1)、(x,y-1)、(x-1,y)、(x+1,y),那么不妨设置下面<font color="red">两个增量数组</font>，来表示四个方向(竖着看即为(0,1)、(0,-1)、(1,0)、(-1,0))。
```c++
int X[4] = {0,0,1,-1} ;
int Y[4] = {1,-1,0,0} ;
```
这样就可以使用for循环来枚举4个方向，以确定与当前坐标(nowX ,nowY)相邻的4个位置。
```c++
for(int i = 0 ; i < 4 ; i++){
	newX = nowX + X[i] ;
	newY = nowY + Y[i] ;
}
```
# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 100 ;

struct Node{
    int x , y ;
}node;

int m , n ;  //矩阵的行和列
int marix[maxn][maxn] ;   //矩阵
bool inq[maxn][maxn] = {false} ;  //false表示该坐标没在队列中，否则，在队列中
//坐标增量
int X[4] = {0,0,1,-1} ;
int Y[4] = {1,-1,0,0} ;

bool judge(int x , int y){
    //越界返回false
    if(x >= m || x < 0 || y >= n || y < 0){
        return false ;
    }

    //如果坐标值为0或者已在队列中，返回false
    if( marix[x][y] == 0 || inq[x][y] == true ){
        return false ;
    }
    //两种情况都不是，返回true
    return true ;
}

void BFS(int x , int y){
    queue<Node> Q ;
    node.x = x ;
    node.y = y ;
    //入队
    Q.push(node) ;
    inq[x][y] = true ;
    //队列非空
    while( !Q.empty() ){
        //取队头元素
        Node top = Q.front() ;
        //队头元素出队
        Q.pop() ;
        for(int i = 0 ; i < 4 ; i++ ){
            int newX = top.x + X[i] ;
            int newY = top.y + Y[i] ;

            if( judge(newX , newY)){
                inq[newX][newY] = true ;
                Node t ;
                t.x = newX ;
                t.y = newY ;
                Q.push(t) ;
            }
        }

    }
}

int main(){
    cin >> m >> n ;
    for(int i = 0 ; i < m ; i ++){
        for(int j = 0 ; j < n ;j++){
            cin >> marix[i][j] ;
        }
    }

    //存放块数
    int res = 0;

    for(int x = 0 ; x<m ; x++){
        for(int y = 0 ; y < n ; y++){
            //如果坐标元素为1且没有入队
            if( marix[x][y] == 1 && inq[x][y] == false ){
                res ++ ;        //块数加1
                BFS(x , y) ;    //将相邻的为1的元素入队
            }
        }
    }

    cout << res ;

    return 0 ;
}

```

# 题目二

给定一个n*m大小的迷宫，其中“*”代0表不可通过，而“.”代表可以通过，S表示起点，T表示终点。移动过程中，如果当前位置(x,y)下标从0开始，且每次只能前往上下左右四个位置的"."，求从起点S到终点T的最小步数。
```c++
//样例
. . . . .
. * . * .
. * S * .
. * * * .
. . . T *

//S的坐标为(2,2) ,T的坐标为(4,3)
```

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

struct Node{
    int x , y ;
    int step ;
}start,d,node;

int m , n ;
char marix[100][100] ;
bool inq[100][100] = {false} ;

int X[4] = {0,0,1,-1} ;
int Y[4] = {1,-1,0,0} ;

bool judge( int x, int y ){
    if( x >= m || x < 0 || y >= n || y < 0 ){
        return false ;
    }
    if( marix[x][y] == '*' || inq[x][y] == true ){
        return false ;
    }

    return true ;
}

int BFS(){
    queue<Node> q ;
    q.push(start) ;
    inq[start.x][start.y] = true ;

    while( !q.empty() ){
        node = q.front() ;
        q.pop() ;

        if( node.x == d.x && node.y == d.y ){
            return node.step ;
        }

        for( int i = 0 ; i < 4 ; i++ ){
            int newX = node.x + X[i] ;
            int newY = node.y + Y[i] ;

            if( judge(newX , newY) ){
                Node t ;
                t.x = newX ;
                t.y = newY ;
                t.step = node.step + 1 ;
                inq[newX][newY] = true ;
                q.push(t) ;
            }
        }
    }
}


int main(){
    cin >> m >> n ;
    cin >> start.x >> start.y ;
    cin >> d.x >> d.y ;
    for( int i = 0 ; i < m ; i++ ){
        for( int j = 0 ; j < n ; j++ ){
            cin >> marix[i][j] ;
        }

    }

    cout <<"------------------------" <<endl ;

    for( int i = 0 ; i < m ; i++ ){
        for( int j = 0 ; j < n ; j++ ){
            cout << marix[i][j] <<" " ;
        }
        cout <<endl ;

    }

    int ans = BFS() ;
    cout <<ans ;

    return 0 ;
}

```

强调一下：当使用STL的queue时，元素入队的push操作只是制造了该元素的一个**副本**入队，因此在入队对元素的修改不会影响队列中的副本，而对队列中副本的修改也不会改变原元素。<font color ="red">这就是说，对需要对队列中的元素进行修改而不仅仅是访问时，队列中存放的元素最好不要是元素本身，而是它们的编号（如果是数组则是下标）</font>