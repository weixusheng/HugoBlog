# 树和哈夫曼树的算法整理



今天应该进入图的,但是感觉还是整理一下树和哈夫曼树吧,知识点挺多的
<!--more-->

## 1.树的表示法

### **双亲表示法(顺序存储)**
```c
typedef struct tnode{
    Elemdata data;
    int parent;         //父节点的位置
}PNode;
typedef struct PNode{
    PNode tree[MAX];            //存放结点的数组
    int n;          //存放树结点的数量
    int r;          //存放根结点的位置
}PTree;
```
理解:只有前驱父节点位置,而没有保存子节点

### **孩子链表表示法(链式存储)**
```c
typedef struct CTNode{          //孩子链表上的结点类型
    struct CTNode *next;            //下一个孩子的位置
    int child;          //第一个孩子结点所在的位置
}CTNode, *ChildPtr;
typedef struct{         //头结点的类型
    Elemdata data;
    ChildPtr link;          //孩子链表的头指针
}CTbox;
typedef struct{         //孩子链表的数据类型
    CTbox nodes[MAX];
    int n,r;            //结点数目和根结点的位置
}ChildList;
```
理解: 虽然前驱后继结点都有保存,但是数据冗余,浪费空间

 - **孩子兄弟链表(链式存储)**
```c
typedef struct CSNode{
    Elemdata data;
    struct CSNode *fch, *nsib;
}CSNode, *CStree;
```
理解: 标准的树存储结构,Perfect

 - **哈夫曼树**
 ```c
typedef struct{
    char data;
    int weight;     //权重
    int parent, lch, rch;
}NodeType;
typedef NodeType HufTree[M+1];
typedef char **HufCode;
 ```
 理解: 值得注意的点是HufCode是动态分配指针数组

## 2.创建树
### **创建孩子链表的方法**
```c
void createPTree(ChildList *t){
    int i, j, k;
    ChildPtr p, s;
    char father, child;
    printf("请输入节点数:");
    scanf("%d", &t->n);
    getchar();
    printf("请依次输入%d个结点的值",t->n);
    for(i = 0; i<t->n; i++){            //初始化结点序列 nodes
        scanf(&t->nodes[i]->data);
        t->nodes[i].link = NULL;
    }
    getchar();
    t->r = 0;
    printf("格式{双亲,孩子}输入%d个分支{从左至右,从上至下}",t->n-1);
    for(i = 1; i < t->n-1; i++){
        scanf(&father, &child);
        getchar();
        for(j=0; j<t->n; j++){
            if(father == t->nodes[j].data) break;           //找到父结点的位置
        }
        if(j > t->n) return;
        for(k=0; k<t->n; k++){
            if(child == t->nodes[k].data) break;            //找到孩子结点的位置
        }
        if(k > t->n) return;
        p = t->nodes[j].link;           //建立链接
        if(p == null){              //当前头结点还没有孩子结点,申请结点,链接
            s = (ChildPtr)malloc(sizeof(CTNode));
            s->child = k;
            s->next = NULL;
            t->nodes[j].link = s;
        }
        else{               //有孩子节点,找到最后一个位置建立兄弟链接
            while(p->next) p = p->next;
            s = (ChildPtr)malloc(sizeof(CTNode));
            s->next = NULL;
            s->child = k;
            p->next = s;
        }
    }
}
```
### **创建孩子兄弟树**
 ```c
 void createCSTree(CStree *t){
     initQueue(q);
     CStree r;
     *t = NULL;
     scanf(fa, ch);
     while(ch != '#'){
         p = (CStree)malloc(sizeof(CSNode));         //初始化结点
         p->data = ch;
         p->fch = p->nsib = NULL;
         EnQueue(Q, p);
         if(fa == '#') *t = p;           //此时为根节点
         else{
             s = LinkStackGetTop(Q);
             while(s->data != fa){           //找到父节点位置
                 DeQueue(Q);
                 s = LinkStackGetTop(Q);
                 if(s->fch == NULL){         //如果没有孩子结点,直接链接为第一个孩子
                     s->fch = p;
                     r = p;          //r存储当前孩子结点位置
                 }
                 else{           //有孩子结点,找到链接孩子节点,并赋值r为当前兄弟结点
                     r->nsib = p;
                     r = p;
                 }
             }
         }
         scanf(fa, ch);
     }
 }
 ```

### **创建哈夫曼树**
```c
//已知n个字符,生成一棵哈夫曼树
void huff_tree(int w[], int n, HufTree ht){
    int i, s1, s2;
    fot(i = 1; i<2*n; i++){         //首先初始化,一共2n-1个结点
        if(i>=1; && i<=n){
            ht[i].weight = w[i-1];          //将所给字符,赋值到初始结点中
        }
        else{
            ht[i].weight = 0;
        }
        ht[i].parent = 0;           //初始化结点
        ht[i].lch = 0;
        ht[i],rch = 0;
    }
    for(i = n+1; i<2*n, i++){
        select(ht, n, &s1, &s2);            //从结点中挑选两个最小权值结点
        ht[i].weight = ht[s1].weight + ht[s2].weight;           //结点合并
        ht[i].lch = s1;         //合并结点建立父子链接
        ht[i].rch = s2;
        ht[s1].parent = i;
        ht[s2].parent = i;
    }
}
void select(HufTree ht, int n, int *s1, int *s2){           //从结点中挑选最小的两个结点
    int i, min;
    for(min = 100, i = 1; i<2*n; i++){          //最小结点1
        if(ht[i].parent == 0 && ht[i].weight != 0 && ht[i].weight <= min){
            min = ht[i].weight;
            *s1 = i;
        }
    }
    for(min = 100, i = 1; i<2*n; i++){          //最小结点2
        if(ht[i],parent == 0 && ht[i].weight != 0 && i != s1 && ht[i].weight <=min){
            min = ht[i].weight;
            *s2 = i
        }
    }
}
```
## 3.树的一般操作
### **树的结点插入**
 ```c
 int insertTree(CStree *t, char father, char child){         //父结点的值为 father,需要插入的孩子结点值为 child
    CStree p = NULL, q, s;
    findTree(t, father, &p);            //找父节点,其值为father
    if(p){
        s = (CStree)malloc(sizeof(CSNode));         //初始化结点
        strcpy(s->data, child);
        s->fch = s->nsib = NULL;
        if(!p->fch){            //如果没有孩子结点
            p->fch = s;
        }
        else{           //有孩子节点
            q = p->fch;
            while(q->nsib){         //找到最后一个兄弟节点
                q = q->nsib;
            }
            q->nsib = s;
        }
        return 1;
    }
    else{
        return 0;
    }
}
 ```

### **树的结点删除**
 ```c
 int deleteTree(CStree *t, char fa, char ch){
    CStree pfa = NULL, pch = NULL;
    if(strcmp(fa, "#") == 0){           //如果此时删除的结点为根结点
        postdelete(*t);         //递归删除树(根结点为 t )
        *t = NULL;
        return;
    }
    else{
        findTree(*t, fa, &pfa);         //得到父结点,赋值指针 pfa
        findTree(*t, ch, &pch);         //得到孩子结点,赋值指针 pch
        if(pfa == NULL || pch == NULL){
            return;         //指针不存在,数据有误
        }
        else{
            if(pfa->fch != pch){            //不是直接孩子,需要查找 father's child's sibling
                pfa = pfa-> fch;            
                while(pfa){
                    if(pfa->nsib == pch) break;
                    pfa = pfa->nsib;            //这里得到 pfa 为 pch 的前驱结点
                }
            }
            delete(pch, pfa);           //在删除 pch 的子树时需要 pch 的前驱,用来重新链接其他子树
        }          
    }
}
void delete(CStree p, CStree f){            //删除单个结点,重新连接后序子树
    if(f->fch == p){
        f->fch = p->nsib;
        p->nsib = NULL;
        postdelete(p);
    }
    if(f->nsib == p){
        f->nsib = f->nsib;
        p->nisb = NULL;
        postdelete(p);
    }
}
void postdelete(CStree t){          //递归删除整个树
    if(t){
        postdelete(t->fch);
        postdelete(t->nsib);
        free(p);
    }
}
 ```

### **树的深度**
  ```c
  int depthTree(BiThrTree t){
    if(t == NULL){
        return 0;
    }
    else{
        d1 = depthTree(t->fch);           //递归求深度
        d2 = depthTree(t->nsib);
        return d1+1 > d2 ? d1+1:d2;
    }
}
  ```

### **树的叶子结点路径**
```c
void AllTreePath(CStree t, SqStack *s){
    while(t){
        SqStackPush(s, t->data);
        if(!t->fch){            //没有子结点,输出栈
            SqStackTraverse(s);
        }
        else{           //一直递归直到遇到叶子结点输出栈
            AllTreePath(t->fch, *s);
        }
        SqStackPop(s);          //叶子结点出栈
        t = t->nsib;            //上一层开始寻找兄弟结点
    }
}
```

## 4.哈夫曼树的操作
### **求叶子结点的哈夫曼编码**
 ```c
void hufcode(HufCode *hcd, HufTree ht, int n){
    char *cd;
    int i, start, c, f;
    *hcd = (HufCode)malloc((n+1)*sizeof(*char));
    cd = (char *)malloc(n*sizeof(char));            //申请存放编码的数组
    for(i = 1; i<=n; i++){
        cd[n-1] = '\0';
        start = n-1;
        c = i;
        f = ht[c].parent;              //从下向上获得编码
        while(f){
            if(ht[f].lch == c){
                cd[--start] = '0';
            }
            else{           //根据左右子树状态确定编码0或1
                cd[--start] = '1';
            }
            c = f;
            f = ht[f].parent;
        }
        (*hcd)[i] = (char *)malloc((n-start)*sizeof(char));
        strcpy((*hcd)[i], &cd[start]);      //使用char数组指针,一直到'\0'结束
    }
}
 ```

