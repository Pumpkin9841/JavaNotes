---
title: PAT A1053 Path of Equal Weight
date: 2021-02-26 19:31:53.721
updated: 2021-03-07 19:34:34.69
url: https://pumpkn.xyz/archives/pata1053pathofequalweight
categories: 算法
tags: 算法 | 树
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805424153280512)
![image.png](https://pumpkn.xyz/upload/2021/03/image-5e1f9265fae14cc4b8bcdcd7b7a4a111.png)

# 题意
给定一棵树和每个结点的权值，求所有从根结点到叶子结点的路径，使得每条路径上的结点的权值之和等于给定的常数s。如果有多条这样的路径，则按路径非递增的顺序输出。</br>
其中路径的大小是指，如果两条路径分别为a-a2-...a-a，与b-b2-b-，且有al-bi，a2-b2..a1-bi1成立，但a>b，那么称第一条路径比第二条路径大。
# 样例解释
样例所给的树即题目描述中的树，从根到叶子的带权路径和为24的路径有4条，经过的结点标号分别为（括号中为点权）：</br>
①00（10）-04（5）-06（2）-09（7）</br>
② 00（10）-02（4）-05（10）</br>
③ 00（10）-03（3）-13（3）-17（6）-19（2）</br>
④ 00（10）-03（3）-13（3）-17（6）-18（2）</br>
# 题解
##步骤1：
这是一棵普通性质的树，因此令结构体node存放结点的数据域和指针域，其中指针域使用vector存放所有孩子结点的编号。又考虑到最后的输出需要按权值从大到小排序，因此不妨在读入时就事先对每个结点的子结点vector进行排序（即对vector中的结点按权值从大到小排序），这样在遍历时就会优先遍历到权值大的子结点。
## 步骤2：
令int型数组path[MAXV存放递归过程中产生的路径上的结点编号。接下来进行DFS，参数有三个：当前访问的结点标号index、当前路径path上的结点个数numNode（也是递归层数，因为每深入一层，path上就会多一个结点）以及当前路径上的权值和sum。递归过程的伪代码如下：
①若sum>s，直接return.
②若sum-s，说明到当前访问结点index为止，输入中需要达到的s已经得到，这时如果结点index为叶子结点，则输出path数组中的所有数据；否则returm
③若sum<S，说明要求还未满足。此时枚举当前访问结点index的所有子结点，对每一个子结点child，先将其存入pathInumNode]，然后在此基础上往下一层递归，下一层的递归参数为child，numNode + 1，sum + node[child].weight.
# 注意
在递归的过程中保存路径有很多方法，这里介绍两种：</br>
**第一种**：使用path[]数组表示路径，其中path[j]表示路径上第i个结点的编号（i从0开始），然后使用初值为0的变量numNode作为下标，在递归过程中每向下递归一层，numNode加1。这样numNode就可以随时跟踪path]数组当前的结点个数，便于随时将新的结点加入路径或者将旧的结点覆盖。</br>
**第二种**：使用STL的vector，vector中有push back0函数和pop-back0函数，其作用分别是将给定元素添加入vector的末尾和将vector的末尾元素删除。这样，当枚举当前访问结点的子结点的过程中，就可以先使用push backO）方法将子结点加入路径中，然后往下一层递归。最后在下一层递归回溯上来之后将前面加入的子结点pop back）即可。</br>
题目要求是从根结点到叶结点的路径，所以在递归过程中出现sum-s时必须判断当前访问结点是否是叶结点（即是否有子结点）。只有当前访问结点不是叶结点时才能输出路径，如果不是叶结点，则必须返回。

# 代码
```c++
#include<bits/stdc++.h>
using namespace std ;

const int maxn = 110 ;
//共n个节点，m个非叶节点,需要求得的路劲权重s
int n , m , s ;
int weight[maxn] ; //存放各节点权重
int path[maxn] ;

struct Node{
    int data ;      //节点值
    int w ;         //节点权重
    vector<int > child ;    // 孩子节点
}node[maxn];

bool cmp(int a , int b){
    return node[a].w > node[b].w ;
}

//先序遍历
void preOrder(int root , int nodeSum , int sum ){
    if( sum > s ){
        return ;
    }
    if( sum == s ){
        //如果当前节点不是叶节点
        if( !node[root].child.empty() ){
            return ;
        }

        for( int i = 0 ; i < nodeSum ; i++ ){
            cout << node[path[i]].w ;
            if( i < nodeSum - 1 ){
                cout << " " ;
            }else{
                cout << "\n" ;
            }
        }
        return ;
    }



    for(int i = 0 ; i < node[root].child.size() ; i++){
        int child = node[root].child[i] ;
        path[nodeSum] = child ;
        preOrder(child , nodeSum+1 , sum+node[child].w ) ;
    }

}


int main(){
    cin >> n >> m >> s ;
    //输入各节点对应权重
    for( int i = 0 ; i < n ; i++ ){
        cin >> node[i].w ;
    }
    for(int i = 0 ; i < m ; i++ ){
        int tmp , num ;  //非叶节点下标与其孩子节点个数
        cin >> tmp >> num ;
        for(int j = 0 ; j < num ; j++){
            int tmpChild ;
            cin >> tmpChild ;
            node[tmp].child.push_back(tmpChild) ;
        }

        sort(node[tmp].child.begin() , node[tmp].child.end() , cmp) ;
    }



    preOrder(0 , 1 ,node[0].w) ;

 //   cout << paths[0][1] << " " ;

    return 0 ;
}

```