# 图的DFS/BFS遍历



图的重点!
<!--more-->

## 图的存储结构-这里只写两种常用的
### **邻接矩阵存储**
```C
#define VNUM 20
typedef struct{
    VertexType vexs[VNUM];          //存储顶点信息
    int arcs[VNUM][VUM];            //存储顶点关系
    int vexNum, arcNum;         //存储顶点数,弧数
}MGraph;
```
### **邻接表**
```C
//图的邻接表-边结点的定义
typedef struct ArcNode{
    int adjvex;
    struct adjvex *nextArc;
    [WeightType info;]          //视情况而定
}ArcNode;
//图的邻接表-表头结点的定义
typedef struct VertexNode{
    Elemdata vertex;
    ArcNode *firstArc;
}VertexNode;
//图的邻接表-总体类型
typedef struct ALGraph{
    VertexNode adjlist[MAX];
    int vexNum, arcNum;
}ALGraph;
```

## 图的遍历

### **深度优先遍历**
思路: 非递归使用栈
首先初始化栈和visited数组
之后进栈当前元素,访问当前元素
while(!emptystack)外层循环
访问当前结点的所有边指向的结点
 - 如果是邻接矩阵
```C
i = stackget_top(s);
for(int i=0; i<g.vexnum; i++){
  if(visited[j] && g.arcs[i][j]){
    进栈;访问; break,跳出当前for循环
  }
}
判断是否是因为子结点都无法访问,如果是,pop当前结点,否则下一次while外层循环
```

 - 如果是邻接表
```C
i = stackget_top(s);
p = g.adjlist[i]->firstArc
while(p){
  if(p && visited[p->adjvex] == 0){
    进栈,访问,break;跳出当前循环
  }
  p = p->nextArc;     //寻找下一条能访问的边
}  //子结点访问完成
判断是否所有子结点都无法访问,如果是,pop此结点,否则下一次while外层循环
```

### **广度优先搜索**
思路: 非递归使用队列
首先初始化队列和visited数组
当前结点访问进队列
while(!emptyqueue)外层循环

 - 如果是邻接矩阵
```C
i = dequeue(q);
for(int i=0; i<g.vexnum; i++){
  if(visited[j] && g.arcs[i][j]){
    进队列;访问;
  }
}
```

 - 如果是邻接表
```C
i = dequeue(q);
p = g.adjlist[i]->firstArc;
while(p){
  if该节点未被访问
  访问p->adjvex;
  进队列;
  p = p->nextArc;
}
```

## 代码实现

### **邻接矩阵连通图-深度优先遍历递归**
```C
void DFSR_MGraph(MGraph g, int v, visited[]){
    printf(g.vertex[v]);
    visited[v] = 1;
    for(j = 0; j<g.vexNum; j++){
        if(g.arcs[v][j] == 1 && visited[j] == 0){
            DFSR_MGraph(g, j, visited);
        }
    }
}
```

### **邻接表连通图-深度优先非递归遍历-使用栈**
```C
void DFS_ALGraph(ALGraph g, int v){
    int i;
    SqStack s;
    initStack(s);
    int visited[100];
    for(i = 0; i<g.vexNum; i++){            //初始化visited数组
        visited[i] = 0;
    }
    printf(G.adjlist[v].vertex);            //访问第一个节点
    visited[v] = 1;
    SqStackPush(s, v);
    while(!SqStackEmpty(s)){
        k = SqStackGetTop(s);
        p = g.adjlist[k].firstArc;          //获得栈顶结点的第一条边
        while(p){
            if(p && visited[p->adjvex] == 0){
                printf(g.adjlist[p->adjvex].vertex);
                visited[p->adjvex] = 1;
                SqStackPush(s, p->adjvex);
                break;
            }
            p = p->nextArc;            //下一条边
        }
        if(!p){
            SqStackPop(s);          //没有相连结点可以访问,出栈此时结点
        }
    }
}
```

### **邻接表连通图的广度优先遍历-使用队列**
```C
void BFSRecursion(ALGraph g, int v){
    initQueue(q);
    printf(g.adjlist[v].vertex);
    visited[v] = 1;
    EnQueue(q, v);
    while(!QueueEmpty){         //对队列元素执行广度优先
        v = DeQueue(q);
        p = g.adjlist[v].firstArc;          //得到队头的第一条边
        while(p){
            if(visited[w] == 0){            //此时尚未访问,访问该节点
                printf(g.adjlist[p->adjvex].vertex);
                visited[w] == 1;
                EnQueue(q, w);          //进栈该节点
            }
            p = p->nextArc;         //结点的下一条边
        }
    }
}
```

## 参考
[一篇很好的博客](https://blog.csdn.net/collonn/article/details/17923851)

## 感悟
&emsp; 这一遍下来比之前学的透彻多了,之前只是背代码,现在真正敲出来,才是自己的,图的应用算法本来学完了,但由于篇幅原因,放在明天更了
&emsp; 今天感觉在iPad上看电子版的"算法笔记"很麻烦,还特意去淘宝买了一本,嘿嘿
&emsp; 今年把CCF认证过一下,好好学算法! <br>
&emsp; 冲!

