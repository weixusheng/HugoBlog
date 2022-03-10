# 图的算法应用&&最小生成树



简单路径,最短路径,最小生成树
<!--more-->

## 1.求包含图中所有顶点的简单路径问题
思路: 哈密尔顿问题-基于深度优先遍历
创建变量 int n ,访问结点后, n++
创建 path 数组记录顶点(路径)
判断条件: n == g.vexnum 则输出当前path
```C
void Hamilton(MGraph g){
    int i;
    int visited[100];
    int path[100];
    for(i = 0; i<vexNum; i++){          //初始化数组
        visited[i] = 0;
        path[i] = 0;
    }
    int n = 0;
    for(i = 0; i<g.vexNum; i++){        //将所有结点都进行遍历操作
        if(!visited[i]){
            DFS_h(g, i, path, &n);
        }
    }
}
void DFS_h(MGraph g, int i, int path[], int visited[], int *n){
    visited[i] = 1;
    path[*n] = i;
    (*n)++;
    if((*n) == g.vexNum){           //满足条件输出路径
        printf(path);
    }
    fot(j = 0; j<g.vexNum; j++){
        if(g.arcs[i][j] && visited[j]){         //递归执行dfs
            DFS_h(g, j, path, n);
        }
    }
    visited[i] = 0;         //此时所有子结点都访问完成,向上一级继续访问
    (*n)--;         //路径结点数-1
}
```
## 2.判断图中是否存在环
思路:基于深度优先遍历
判断条件: 如果深度遍历过程中结点的子结点都已经被访问,则代表存在环
```C
void DFSloop(MGraph g, int i, int visited[]){
    visited[i] = 1;         //当前结点访问
    for(int di=0, vi=0, j=0; j<g.vexNum; j++){          //判断边是否都被访问
        if(g.arcs[i][j]){
            di++;
            if(visited[j]){
                vi++;
            }
        }
    }
    if(di == vi && di!= 1){         //判断是否存在环
        printf("有回路");
        return;
    }
    for(j=0; j<g.arcNum; j++){          //dfs遍历,递归判断
        if(g.arcs[i][j] && visited[j] == 0){
            DFSloop(g, j, visited);
        }
    }
}
```
## 3.求无向图中顶点 a 到 i 之间的简单路径
思路:基于深度优先遍历
path记录路径上的结点,如果访问到目标节点,输出path,如果当前结点的子节点深度遍历都没有访问到目标节点,则path删除当前结点,回溯上一层节点,进行上次一层节点的下一次循环(下一个子结点)进行递归查找
```C
void DFSSearchPath(MGraph g, int v, int s, char path[], int visited[], int *found){
    visited[v-1] = 1;           //访问该结点
    append(path, g.vertex[v-1]);            //结点进数组path
    for(j = 0; j<g.vexNum && !(*found); j++){
        if(g.arcs[v-1][j] == 1){            //边存在
            if(s == j+1){           //当前结点为为目标节点,则找到路径
                *found = 1;
                append(path, g.vertex[j]);
            }
            else if(visited[j] == 0){           //dfs递归遍历
                DFSSearchPath(g, j+1, s, path, visited, *found);
            }
        }
    }
    if(!(*found)){          //递归子结点都访问完,删除当前结点,回溯进入下一次循环
        delete(path);
    }
}
```
## 4.求无向图中 vi 到 vj 之间的最短路径
思路:这个算法很是巧妙,一开始很懵,直到手动模拟过程,才明白其中的道理,巧妙的点就在于 prior 指针指向的是 front ,这样在输出的时候就是最短路径
1. 算法基于广度优先遍历,这个很好理解,因为最短路径就是最少层去发现的过程
2. 构建了新的队列,带有前驱和后继指针,重写了进栈的函数,prior 指向 front

```C
//求无向图中vi和vj之间的最短路径
int BFSsearchPath(MGraph g, int vi, int vj, SqQueue *q){
    for(int i=0; i<g.vexNum, i++){          //初始化 visted[]
        visited[i] = 0;
    }
    initQueue(q);
    EnQueue_bfs(q, vi);         //第一个节点访问
    visited[vi] = 1;
    while(!QueueEmpty(q)){          //广度优先遍历
        DeQueue_bfs(q, &v);
        for(w = 0; w<g.vexNum; w++){
            if(g.arcs[v][w] && visited[w]){
                visited[w] = 1;
                EnQueue_bfs(q, w);
                if(w == vj){
                    break;
                }
            }
        }
        if(w >= g.vexNum){
            continue;
        }
    }
}
typedef struct bfsnode{     //队列实现
    struct bfsnode *prior;
    struct bfsnode *next;
    Elemdata data;
}bfsnode, *bfslink;
typedef struct {
    bfslink front;
    bfslink rear;
}bfsqueue;
void EnQueue_bfs(bfsqueue *q, Elemdata data){           //prior指向队头front
    p = (bfsqueue)malloc(sizeof(bfsnode));
    p->data = data;
    p->next = NULL;
    p->prior = q->front;
    q->rear->next = p;
    q->rear = p;
}
void Dequeue_bfs(bfsqueue *q, Elemdata *data){
    *data = q->front;
    q->front = q->front->next;
}
```
## 5.最小生成树

### Prim算法
思路: 使用 lowcost 辅助数组记录结点到生成树的距离
1. 从未包含在生成树的结点(weight == 0)的边中选取权值最小
2. 赋值新进入结点的lowcost.weigh = 0
3. 从新进入生成树的 i 结点开始更新lowcost数组(比较g.arc[i][j]和lowcost[j])
外部循环g.vexnum次,即对每个节点都进行该操作
```C
#define maxcost 10000
typedef struct{
    int vi;
    int weight;
}Lowcost;
void prim(MGraph g){
    int mincost, i, j, k;
    Lowcost *lowcost = (Lowcost*)malloc(sizeof(Lowcost) * g.vexNum);            //申请 lowcost 数组空间
    for(i=0; i<g.vexNum; i++){          //初始化 lowcost 数组
        lowcost[i].vi = 0;
        lowcost[i].weight = g.arcs[0][i];
    }
    lowcost[0].weight = 0;          //第一个节点进入生成树中,weight=0
    for(i=0; i<g.vexNum; i++){          //找到当前离树权值最小的结点
        mincost = maxcost;
        for(j=0; j<g.vexNum; j++){
            if(lowcost[j].weight < mincost && lowcost[j].weight != 0){
                mincost = lowcost[j].weight;
                k = j;
            }
        }
        lowcost[k].weight = 0;          //权值最小结点进生成树,weight=0
        for(j=0; j<g.vexNum; g++){          //更新其余未进入生成树结点的权值
            if(g.ars[k][j] < lowcost[j].weight){
                lowcost[j] = g.ars[k][j];
                lowcost[j].vi = k;
            }
        }
    }
}
```

### Kruskal算法
思路:这个算法也很有意思,一开始看到五个循环我人都傻...
其实蛮简单的,应该是作者希望读者理解的更快一点,才这么写的
算法中有两个小知识点
1. int转char 因为字符是ASCII值表示的,'0'是一个字符,由48表示
 所以c[i][0] = i+48;
2. stract函数
例如 char d[20]="Golden";  char s[20]="View";
strcat(d,s); printf("%s",d);
输出 d 为 'GoldenView'   返回指向d的指针。


 - 第一个for循环: 将结点的下标转换为字符串 i+48,加入c数组中
 - 第二个for循环: 将所有边进行初始化,加入x数组
 - 第三个for循环: 将边的权值从小到大排序
 - 第四个for循环: 依次找到最小边,判断是否边的两个节点都在生成树中,如果不在生成树中,则将两个结点合并到第一个节点,并删除第二个结点
 - 第五个for循环: 输出生成树的边
```C
typedef char afc[MAX];
void kruskal(MGraph g){
    int i, j, k, t., p, q, x[MAX][4];
    zfc c[MAX];
    for(i=0; i<g.vexNum; i++){
        c[i][0] = i+48;         //初始化,下标转字符串
        c[i][1] = 0;            //字符串结束标志
    }
    for(i=0; i<g,vexNum; i++){          //初始化所有边,放进 x[max][4] 数组
        for(j=i; j<g.vexNum; j++){
            if(g.arcs[i][j] != 0 && g.arcs[i][j] != maxcost){
                x[k][0] = i;
                x[k][1] = j;
                x[k][2] = g.arcs[i][j];
                x[k][3] = 0
                k++;
            }
        }
    }
    for(i=0; i<g.arcNum; i++){          //对所有边进行排列,权值从小到大
        for(j=0; j<g.arcNum-1-i; j++){
            if(x[j][2] > x[j+1][2]){
                for(p=0; p<4; p++){
                    t = x[j][p];
                    x[j][p] = x[j+1][p];
                    x[j+1][p] = t;
                }
            }
        }
    }
    for(i=0, k=0; i<g.arcNum; i++){         //对所有边进行从小到大判断
        p = find(c, g.vexNum, x[i][0]+48);
        q = find(c, g.vexNum, x[i][1]+48);          //判断边的两个结点是否成环
        if(p != q){
            stract(c[p], c[q]);
            c[q][0] = 0;
            k++;
            x[i][3] = 1;
        }
        if(k == g.vexNum-1){            //当有 vexnum-1 条边时,最小生成树构造完成
            break;
        }
    }
    printf("%7c%7c%6s", "顶点", "顶点", "权值");
    for(i=0; i<g.arcNum; i++){          //输出边
        if(x[i][3] == 1){
            printf("%7c%7c%6d", g.vertex[x[i][0]], g.vertex[x[i][1]], x[i][2])
        }
    }
}
int find(afc *s, int n, char c){
    for(int i=0; i<n; i++){
        for(int j=0; j<strlen(s[i]); j++){
            if(s[i][j] == c){
                return i;
            }
        }
    }
    return -1;
}
```

