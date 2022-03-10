# PAT-Advanced 图的遍历



算法笔记 - BFS-DFS
<!--more-->


# DFS
## 邻接矩阵
```c++
const int maxv = 1000;
const int inf = 1000000000;

int n, G[maxv][maxv];
bool vis[maxv] = false;

void dfs(int u, int depth){
    vis[u] = true;
    for(int v=0; v<n; v++){
        if(vis[v] == false && G[u][v] != inf){
            dfs(v, depth+1);
        }
    }
}

void dfstrave(){
    for(int u=0; u<n; u++){
        if(vis[u] == false){
            dfs(u, 1);
        }
    }
}
```

## 邻接表
```c++
vector<int> adj[maxv];
int n;
bool dfs(int u, int depth){
    vis[u] = true;
    for(int i=0; i<adj[u].size(); i++){
        int v = adj[u][i];
        if(vis[v] == false){
            dfs(v, depth+1);
        }
    }
}

void dfstrave(){
    for(int u = 0; u < n; u++){
        if(vis[u] == false){
            dfs(u, 1);
        }
    }
}
```

# BFS
## 邻接矩阵
```c++
int n, G[maxv][maxv];
bool inq[maxv] = {false};

void bfs(int u){
    queue<int> q;
    q.push(u);
    inq[u] = true;
    while(!q.empty()){
        int u = q.front();
        q.pop();
        for(int v=0; v<n; v++){
            if(inq[v] == false && G[u][v] != inf){
                q.push(v);
                inq[v] = true;
            }
        }
    }
}
void bfstrave(){
    for(int u=0; u<n; u++){
        if(inq[u] == false){
            bfs(q);
        }
    }
}
```

## 邻接表
```c++
vector<int> adj[maxv];
int n;
bool inq[maxv] = {false};

void bfs(int u){
    queue<int> q;
    q.push(u);
    inq[u] = true;
    while(!q.empty()){
        int u = q.front();
        q.pop();
        for(int i=0; i<adj[u].size(); i++){
            int v = adj[u][i];
            if(inq[v] == false){
                q.push(v);
                inq[v] = true;
            }
        }
    }
}

void bfstrave(){
    for(int u=0; u<n; u++){
        if(inq[u] == false){
            bfs(q);
        }
    }
}
```
