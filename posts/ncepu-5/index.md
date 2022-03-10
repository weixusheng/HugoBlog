# NCEPU 图



图基本算法
<!--more-->

# 头文件
```c++
#include<iostream>
#include<cstdlib>
#include<stack>
#include<queue>
#include<set>
using namespace std;
#define Elemtype int
#define maxsize 10000
```

# 邻接矩阵结构
```c++
typedef struct {
    int edge[maxsize][maxsize];
    Elemtype vex[maxsize]; 
    int vexnum,arcnum;
}MGraph;
```

# 邻接表结构
```c++
typedef struct arcnode{     //边结点
    int adjvex;   //指向的邻接表中的编号
    struct arcnode *next;
};
typedef struct vnode{   //头结点
    Elemtype val;    //顶点的值
    arcnode *first;
}vnode, adjlist[maxsize];
typedef struct {
    int vexnum, arcnum;
    adjlist vertices;
}ALGraph;
```
# 最短路径
## Dijkstra单源最短
```c++
/*
单源最短路径,一个起始点,到剩余所有点的最短路径
*/
void dijkstra(MGraph g, int p){
    int k, maxs;
    int n = g.vexnum;
    bool visited[n];
    int dis[n];
    for(int i=0; i<n; i++){
        dis[i] = g.edge[p][i];
        visited[i] = false;
    }
    for(int i=0; i<n; i++){ // n次迭代
        maxs = INT_MAX;
        for(int j=0; j<n; j++){ //找到贪心点
            if(!visited[j] && dis[j]<maxs){
                maxs = dis[j];
                k = j;
            }
        }
        visited[k] = true;
        for(int j=0; j<n; j++){ //对贪心点作为中间节点,更新dis[]
            if(!visited[j] && g.edge[k][j] != INT_MAX){
                dis[j] = (dis[k]+g.edge[k][j]) < dis[j] ? (dis[k]+g.edge[k][j]) : dis[j];
            }
        }
    }
}
```

## Floyd多源最短
```c++
/*
多源最短路径
*/
void floyd(MGraph g){
    for(int k=0; k<g.vexnum; k++){
        for(int i=0; i<g.vexnum; i++){
            for(int j=0; j<g.vexnum; j++){
                if(g.edge[i][k] != INT_MAX && g.edge[j][k] != INT_MAX &&
                   g.edge[i][k]+g.edge[k][j] < g.edge[i][j]){
                    g.edge[i][j] = g.edge[i][k]+g.edge[k][j];
                }
            }
        }
    }
}
```

# Prim最小生成树
```c++
typedef struct{
    int weight;
    int vi; //前驱结点
    int vj; //当前节点
}lowcost;
void prim(MGraph g){
    lowcost *lowest = new lowcost[g.vexnum];
    for(int i=0; i<g.vexnum; i++){  //初始化lowest数组
        lowest[i].weight = g.edge[0][i];
        lowest[i].vi = 0;
        lowest[i].vj = i;
    }
    lowest[0].weight = 0;
    int min;
    int k;
    for(int i=0; i<g.vexnum; i++){  // num迭代
        min = INT_MAX;
        for(int j=0; j<g.vexnum; j++){  //找到贪心节点
            if(lowest[j].weight != 0 && lowest[j].weight < min){
                min = lowest[j].weight;
                k = j;
            }
        }
        lowest[k].weight = 0;
        for(int j=0; j<g.vexnum; j++){  //更新路径权值
            if(lowest[j].weight > g.edge[k][j]){
                lowest[j].weight = g.edge[k][j];
                lowest[j].vi = k;
            }
        }
    }
}
```

# DFS-递归
## 邻接矩阵-递归
```c++
void dfs_matrix_traversal(MGraph g){
    int visited[g.vexnum] = {0};
    for(int i=0; i<g.vexnum; i++){
        dfs_matrix(g,i,visited);
    }
}
void dfs_matrix(MGraph g, int v, int visited[]){
    printf("%d", g.vex[v]);
    visited[v] = 1;
    for(int i=0; i<g.vexnum; i++){
        if(g.edge[v][i] != 0 && visited[i] == 0){
            dfs_matrix(g,i,visited);
        }
    }
}
```

## 邻接表-递归
```c++
void dfs_list_traversal(ALGraph g){
    int visited[g.vexnum] = {0};
    for(int i=0; i<g.vexnum; i++){
        if(visited[i] == 0){
            dfs_list(g, i, visited);
        }
    }
}
void dfs_list(ALGraph g, int v, int visited[]){
    printf("%d", g.vertices[v].val);
    visited[v] = 1;
    arcnode *p = g.vertices[v].first;
    while(p){
        int i = p->adjvex;
        if(visited[i] == 0){
            dfs_list(g, i, visited);
        }
        p = p->next;
    }
}
```

# DFS-非递归
## 邻接矩阵-非递归
```c++
void nonrecursive_dfs_matrix(MGraph g, int v, int visited[]){
    stack<int> s;
    printf("%d", g.vex[v]);
    s.push(v);
    while(!s.empty()){
        int t = s.top();
        for(int i=0; i<g.vexnum; i++){
            if(g.edge[t][i] != 0 && visited[i] == 0){
                printf("%d", g.vex[i]);
                visited[i] = 1;
                s.push(i);
                break;
            }
            if(i == g.vexnum){
                s.pop();
            }
        }
    }
}
void nonrevursive_dfs_matrix_traversal(MGraph g){
    int visited[g.vexnum] = {0};
    for(int i=0; i<g.vexnum; i++){
        if(visited[i] == 0){
            nonrecursive_dfs_matrix(g, i, visited);
        }
    }
}
```

## 邻接表-非递归
```c++
void nonrecursive_dfs_list(ALGraph g, int v, int visited[]){
    printf("%d", g.vertices[v].val); //访问初始节点
    visited[v] = 1;
    stack<int> s;
    s.push(v);
    arcnode *p;
    while(!s.empty()){
        p = g.vertices[s.top()].first;
        while(p){
            if(visited[p->adjvex] == 0){
                visited[p->adjvex] = 1;
                s.push(p->adjvex);
                printf("%d", g.vertices[p->adjvex].val);
                p = g.vertices[p->adjvex].first;
            }
            else{
                p = p->next;
            }
            if(p == nullptr){
                s.pop();
            }
        }
    }
}
void nonrecursive_dfs_list_traversal(ALGraph g){
    int visited[g.vexnum] = {0};
    for(int i=0; i<g.vexnum; i++){
        if(visited[i] == 0){
            nonrecursive_dfs_list(g, i, visited);
        }
    }
}
```

# BFS
## 邻接矩阵
```c++
void bfs_matrix(MGraph g, int v, int visited[]){
    queue<int> q;
    q.push(v);
    while(!q.empty()){  //层序遍历
        int t = q.front();
        q.pop();
        visited[v] = 1;
        printf("%d", g.vex[v]);
        for(int i=0; i<g.vexnum; i++){
            if(g.edge[t][i] != 0 && visited[i] == 0){
                q.push(i);
            }
        }
    }
}
```

## 邻接表
```c++
void dfs_list(ALGraph g, int v, int visited[]){
    queue<int> q;
    q.push(v);
    while(!q.empty()){
        int t = q.front();
        q.pop();
        visited[t] = 1;
        printf("%d", g.vertices[t].val);
        arcnode *p = g.vertices[t].first;
        while(p){
            q.push(p->adjvex);
            p = p->next;
        }
    }
}
```
