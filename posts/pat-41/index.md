# PAT典型题 Part-1


最近几天就总结一下解题套路吧...毕竟没几天就要考试了,绝不能做一只轻言放弃鸭!(甲级太顶了...我好菜)
<!--more-->

做到现在,感觉甲级题也是有套路的...第一题和第二题白给,第三题树或者排序,第四题图或者动态规划或者一些我还没接触的东西(菜是问题的本源)

# 树的BFS/DFS
## A1053
带权重的树:
提供一个节点上带权重的树,并且要求输出权重和(根节点到叶节点完整路径)符合要求的路径
并且输出要求按照路径的非递增顺序输出
首先对于此类问题的求解,一定会用到递归,那么就要根据题意来写出一个合理的 递归边界 和 递归式
1. 当路径和大于所给值时,break return
2. 当路径和等于所给值时,判断当前是否为节点是否为叶子节点
3. 当路径和小于所给值时,对其子节点进行递归操作

对于如何存储路径,也有两种方式
 - 使用数组,利用变量 nodenum 来记录当前数组中的节点个数
 - STL-vector 利用push_back 和pop_back 以及size()实现

## CODE
```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

int target;
struct node{
    int weight;
    vector<int> child;
};

vector<node> data;
vector<int> path;

void dfs(int index, int sum){
    path.push_back(index);  //子节点加入路径
    if(sum > target){   //大于所给条件,舍弃
        path.pop_back();
        return;
    }
    if(sum == target){
        if(data[index].child.size() !=0) {  //判断是否为叶子节点
            path.pop_back();
            return;
        }
        for(int i=0; i<path.size(); i++){   //打印路径
            printf("%d%c", data[path[i]].weight, i != path.size() - 1 ? ' ' : '\n');
        }
        path.pop_back();
        return;
    }
    for(int j=0; j<data[index].child.size(); j++){  // 对子节点递归
        int node = data[index].child[j];
        dfs(node, sum + data[node].weight);
    }
    path.pop_back();    //回溯,删除路径中一开始加入的节点
    return;
}

bool cmp(int a, int b){
    return data[a].weight > data[b].weight;
}
int main(int argc, char const *argv[])
{
    int n, m, node, k;
    scanf("%d %d %d", &n, &m, &target);
    data.resize(n);
    for(int i=0; i<n; i++)
        scanf("%d", &data[i].weight);
    for(int i=0; i<m; i++) {
        scanf("%d %d", &node, &k);
        data[node].child.resize(k);
        for(int j=0; j<k; j++)
            scanf("%d", &data[node].child[j]);
        sort(data[node].child.begin(), data[node].child.end(), cmp);    //对加入的子节点序列进行排列
    }
    dfs(0, data[0].weight);
    return 0;
}
```

# 图的遍历DFS/BFS
无论是dfs还是bfs,都要有一个trave循环,处理非连通情况
## DFS-A1034
考察图的深度遍历,并且有**边权**附加条件(应该用**邻接矩阵**保存数据)
```c++
#include<iostream>
#include<string>
#include<map>
using namespace std;

const int maxn = 2010;
const int INF = 999999999;  //初始极大值条件

map<int, string> intTostring;
map<string, int> stringToint;
map<string, int> gang;

int G[maxn][maxn] = {0};    //初始化邻接矩阵
int weight[maxn] = {0};     //初始化权重数组(额外条件)

int n, k, numperson = 0;
bool vis[maxn] = {false};   //判定访问数组

void dfs(int nowvisit, int &head, int &num, int &totalvalue){
    num++;
    vis[nowvisit] = true;
    if(weight[nowvisit] > weight[head]){
        head = nowvisit;
    }
    for(int i=0; i<num; i++){
        if(G[nowvisit][i] > 0){
            totalvalue += G[nowvisit][i];   //权重加和
            G[nowvisit][i] = G[i][nowvisit] = 0;    //将当前计算过的边除去
        }
        if(vis[i] == false){
            dfs(i, head, num, totalvalue); //递归访问
        }
    }
}

void dfstrave(){
    for(int i=0; i<numperson; i++){
        if(vis[i] == false){    //没有被访问
            int head = i;
            int num = 0;
            int totalvalue = 0;
            dfs(i, head, num, totalvalue);  //递归
            if(num > 2 && totalvalue > k){  //符合条件,放进 res数组
                gang[intTostring[head]] = num;
            }
        }
    }
}

int change(string str){     //字符串名字 转 数字编号,方便数据处理
    if(stringToint.find(str) != stringToint.end()){
        return stringToint[str];
    }
    else{
        stringToint[str] = numperson;
        intTostring[numperson] = str;
        return numperson++;
    }
}

int main(){
    int w;
    string str1, str2;
    cin >> n >> k;
    for(int i=0; i<n; i++){
        cin >> str1 >> str2 >> w;
        int id1 = change(str1);
        int id2 = change(str2);
        weight[id1] += w;   //权重初始化
        weight[id2] += w;
        G[id1][id2] += w;   //邻接矩阵数据初始化
        G[id2][id1] += w;
    }
    dfstrave();
    cout << gang.size() << endl;
    for(auto it = gang.begin(); it != gang.end(); it++){
        cout << it->first << it->second << endl; //遍历打印数据
    }
    return 0;
}
```

## BFS-A1076
这道题是讨论weibo的转发,首先给一个用户的树状关系,之后每个用户的转发只能最多三层
因此用到广度优先遍历,即层序遍历,这就需要在结点结构中,加入node属性进行判定
```c++
#include<cstdio>
#include<cstring>
#include<vector>
#include<queue>

using namespace std;

const int maxv = 1010;

struct node{
    int id;
    int layer;
};

vector<node> adj[maxv];
bool inq[maxv] = {false};   //全局访问判定数组

int bfs(int s, int L){
    int numforward = 0;
    queue<node> q;
    node start;
    start.id = s;
    start.layer = 0;
    q.push(start);
    inq[start.id] = true;
    while(!q.empty()){
        node topnode = q.front();
        q.pop();
        int u = topnode.id;
        for(int i=0; i<adj[u].size(); i++){
            node next = adj[u][i];
            next.layer = topnode.layer + 1;
            if(inq[next.id] == false && next.layer <= L){   //遍历队头元素
                q.push(next);
                inq[next.id] = true;
                numforward++;
            }
        }
    }
    return numforward;
}

int main(){
    node user;
    int n, L, numfollow, idfollow;
    scanf("%d%d", &n, &L);
    for(int i=1; i<=n; i++){
        user.id = i;
        scanf("%d", &numfollow);
        for(int j=0; j<numfollow; j++){
            scanf("%d", &idfollow);
            adj[idfollow].push_back(user);  //初始化数据
        }
    }
    int numquery, s;
    scanf("%d", &numquery);
    for(int i=0; i<numquery; i++){
        memset(inq, false, sizeof(inq));    //初始化数组
        scanf("%d", &s);
        int numforward = bfs(s, L);
        printf("%d\n", numforward);
    }
    return 0;
}
```

# Dijkstra-A1030
寻找最短的出行路径,并且附加条件为路程消耗最小(权重相加最小)
```c++
#include <cstdio>
#include <algorithm>
#include <vector>
using namespace std;

int n, m, s, d;
int e[510][510];    //邻接矩阵
int dis[510];   //离开始点的距离
int cost[510][510]; //路程消耗
vector<int> pre[510];   //路径记录(重点)
bool visit[510];    //是否访问过 判定

const int inf=99999999;
vector<int> path, temppath;
int mincost=inf;

void dfs(int v) {
    temppath.push_back(v);
    if(v==s) {  //到达目的地,进行路径消耗计算
        int tempcost=0;
        for(int i=temppath.size() -1; i>0; i--) {
            int id=temppath[i], nextid=temppath[i-1];
            tempcost += cost[id][nextid];
        }
        if(tempcost<mincost) {
            mincost = tempcost;
            path = temppath;        
        }
        temppath.pop_back();    //回溯
        return ;    
    }
    for(int i=0; i<pre[v].size(); i++)
        dfs(pre[v][i]); //没有达到目的地,进行递归
    temppath.pop_back();    //回溯
}
int main() {
    fill(e[0], e[0] +510*510, inf); //fill初始化
    fill(dis, dis+510, inf);
    scanf("%d%d%d%d", &n, &m, &s, &d);
    for(int i=0; i<m; i++) {
        int a, b;
        scanf("%d%d", &a, &b);
        scanf("%d", &e[a][b]);
        e[b][a] = e[a][b];
        scanf("%d", &cost[a][b]);
        cost[b][a] =cost[a][b];    
    }
    pre[s].push_back(s);    //防止坏数据(可去)
    // Dijkstra begin
    dis[s] =0;
    for(int i=0; i<n; i++) {
        int u=-1, minn = inf;
        for(int j=0; j<n; j++) {
            if(visit[j] ==false && dis[j] < minn) { //找到最短边
                u=j;
                minn=dis[j];
            }        
        }
        if(u == -1) //没有找到最短边
            break;
        visit[u] =true; 
        for(int v=0; v<n; v++) {    //从当前边找到的最短边顶点出发
            if(visit[v] == false && e[u][v] !=inf) {    //更新距离,并且记录前驱点
                if(dis[v] >dis[u] +e[u][v]) {
                    dis[v] =dis[u] +e[u][v];
                    pre[v].clear();
                    pre[v].push_back(u);                
                } 
                else if(dis[v] ==dis[u] +e[u][v]) {
                    pre[v].push_back(u);                
                }            
            }        
        }    
    }
    // Dijkstra end
    dfs(d); //dfs打印路径
    for(int i = path.size() -1; i>=0; i--)
        printf("%d ", path[i]);
    printf("%d %d", dis[d], mincost);
    return 0;
}
```
