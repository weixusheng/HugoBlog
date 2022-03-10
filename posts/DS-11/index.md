# 图的最短路径



单源多源最短路径
<!--more-->

## 总体思想
**需要两个辅助数组**
1. dist数组: 记录当前路径长度
2. path数组: 记录当前的路径上的结点

**单源和多源的算法不同点:**
1. 单源path为二维数组(只需要记录目标节点),多源为三维数组(需要记录两个结点)
2. 单源遍历过程中,得到目标节点后,更新其余结点的dist,path,并赋值当前结点dist=0,而多源遍历中,只需要三层for循环,不断更新dist和path即可
3. 单源最外层是while循环,需要flag判断是否还有结点没有进行最短路径计算

每次循环进行新增中间结点判断

## 算法
### 单源最短路径-迪杰斯特拉算法
```C
void dijkstra(MGraph g, int v){
    int dist[MAX];
    int path[MAX][MAX];
    int i, j, k, min, n, flag;
    for(i=0; i<g.vexNum; i++){         //初始化path,都为-1
        for(j=0; j<g.vexNum; j++){
            path[i][j] = -1;
        }
    }
    for(i=0; i<g.vexNum; i++){      //对初始化dist,path第一个结点为v(起始结点)
        dist[i] = g.arcs[v][i];
        if(dist[i] != 0 && dist[i] != 10000){
            path[i][0] = v;
        }
    }
    flag = 1;     //判断循环继续的标志
    while(flag){
        k = 0;
        min = 10000;
        for(j=0; j<g.vexNum; j++){      //获得最小dist的结点
            if(dist[j] !=0 && dist[j] < min){
                k = j;
                min = dist[j];
            }
        }
        for(j = 0; j< g.vexNum; j++){     //以最小dist结点为中间结点更新其他结点的dist和path
            if(j != k && dist[j] != 0 && dist[j] != 10000){
                if(dist[j] > dist[k] + g.arcs[k][j]){
                    dist[j] = dist[k] + g.arcs[k][j];
                    for(m=0; m<g.arcNum; m++){
                        path[j][m] = path[k][m];      //j结点路径与j结点路径相同,进行赋值
                    }
                    for(m=0; m<g.arcNum && path[j][m]!=-1;){
                        m++;      //将j结点路径最后一个位置赋值其自身
                    }
                    path[j][m] = j;     //找到
                }
            }
        }
        dist[k] = 0;      //此时dist最小结点完成最短路径搜索,dist=0
        flag = 0;
        for(j=0 ;j<g.vexnum; j++){      //判断图中是否还有结点可进行最小路径搜索
            if(dist[j] != 0 && dist[j]<10000){
                flag = 1;
            }
        }
    }
}
```

### 多源最短路径-弗洛伊德算法
```c
typedef PATH[MAX];      //定义路径数组

void floyd(MGraph){
    int i, j, m, m, p;
    int d[MAX][MAX];
    PATH path[MAX][MAX];
    for(i=0; i<g.vexNum; i++){      //初始化dist和path
        for(j=0; j<g.vexNum; j++){
            d[i][j] = g.arc[i][j];
            for(k=0; k<g,vexNum; k++){
                path[i][j][k] = -1;     // i, j 结点之间路径的第k个结点
            }
        }
    }
    for(i=0; i<g.vexNum; i++){
        for(j=0; j<g.vexNum; j++){
            if(d[i][j] != 10000 && d[i][j] != 0){
                path[i][j][0] = i;
                path[i][j][1] = j;      //初始化path路径为i, j的直接路径,此时无中间结点
            }
        }
    }
    for(k=0; k<g.vexNum; k++){      //中间节点遍历,更新path和dist
        for(i=0; i<g.vexNum; i++){
            for(j=0; j<g.vexNum; j++){
                if(d[i][k] + d[k][j] < d[i][j]){
                    d[i][j] = d[i][k] + d[k][j];
                    for(m=0; m<g.vexNum && path[i][j][m]!= -1; m++){
                        path[i][j][m] = path[i][k][m];      //将i,k路径赋值给i,j路径(前半段)
                    }
                    for(n=1; n<g.vexNum; m++, n++){
                        path[i][j][m] = path[k][j][n];      //将k,j路径赋值给i,j路径(后半段,从m位置开始)
                    }
                }
            }
        }
    }
}
```

现在图就剩拓扑排序啦!明天算法笔记就到了,基础打好,准备快乐刷算法题!

