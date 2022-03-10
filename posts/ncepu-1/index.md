# NCEPU 基础算法



基础算法整理
<!--more-->

# 线性表与链表
## 线性表
```c++
typedef struct{
    EelemType *elem;
    int length;
    int listsize;
}Sqlist;
```

## 链表结构
```c++
typedef struct ListNode{
    EelemType val;
    //struct ListNode *next;
    ListNode *next;
}LinkNode, *LinkList;
```
## 逆转顺序表
```c++
void Reverse(int A[], int n){
    int i,t;
    for(int i=0; i<n/2; i++){
        t = A[i];
        A[i] = A[n-i-1];
        A[n-i-1] = t;
    }
}
```
## 删除链表中特定值的元素
```c++
void delete_link(LinkList &l,int item){
    LinkList p, q = l;
    ListList tmp;
    p = q->next;
    while(p != NULL){
        if(p->val == item){
            q->next = p->next;
            tmp = p;
            p = p->next;
            free(tmp);
        }
        else{
            q = p;
            p = p->next;
        }
    }
    if(l->val == item){
        q = l;
        l = l->next;
        free(q);
    }
}
```
## 逆转线性表
```c++
void reverse_link(LinkList &l){
    LinkList p,q,r;
    p = l;
    q = NULL;
    while(p != NULL){   //头插法,利用双指针
        r = q;
        q = p;
        p = p->next;
        q->next = r;
    }
    l = q;
}
```
## 复制线性链表(递归)
```c++
LinkList copy(LinkList l){
    LinkList res;
    if(l == NULL){
        return NULL;
    }
    else{
        res = new LinkNode;
        res->val = l->val;
        res->next = copy(l->next);
        return res;
    }
}
```
## 两个递增非空线性链表合并
```c++
LinkList *merge_list(LinkNode a, LinkNode b){
    LinkNode res = new LinkNode();
    LinkNode tmp = res;
    while(a && b){
        if(a->val <= b->val){
            tmp->next = a;
            a = a->next;
        }
        else{
            tmp->next = b;
            b = b->next;
        }
    }
    if(a){
        tmp->next = a;
    }
    else{
        tmp->next = b;
    }
    tmp->next = nullptr;
    return res->next;
}
```

# 二叉树
## 树的结构体
```c++
typedef struct BNode
{
    EelemType val;
    struct BNode *left;
    struct BNode *right;
}BNode, *BTree;
```
## 树的中序遍历
```c++
void inorder(BTree t){
    if(t == NULL){
        return;
    }
    else{
        inorder(t->left);
        printf("%c\n", t->val);
        inorder(t->right);
    }
}
```
## 树的先序遍历
```c++
void preorder(BTree t){
    if(t == NULL){
        return;
    }
    printf("%c", t->val);
    preorder(t->left);
    preorder(t->right);
}
```
## 树的后序遍历
```c++
void postorder(BTree t){
    if(t == NULL){
        return;
    }
    postorder(t->left);
    postorder(t->right);
    printf("%c", t->val);
}
```
## 先序遍历非递归
```c++
void traverse_preorder(BTree t){
    stack<BTree> s;
    BTree n;
    while (t != NULL || !s.empty())
    {
        while(t!= NULL){
            printf("%d", t->val);
            s.push(t);
            t = t->left;
        }
        n = s.top();
        s.pop();
        t = n->right;
    }
}
```
## 二叉树中序遍历
```c++
void traverse_inorder(BTree t){
    stack<BTree> s;
    BTree n;
    while(t!= NULL || !s.empty()){
        while(t!= NULL){
            s.push(t);
            t = t->left;
        }
        n = s.top();
        printf("%d", n->val);
        s.pop();
        t = n->right;
    }
}
```
## 二叉树层序遍历
```c++
void levelorder(BTree t){
    BTree n;
    queue<BTree> q;
    q.push(t);
    while(!q.empty()){
        n = q.front();
        printf("%d",n->val);
        if(n->left){
            q.push(n->left);
        }
        if(n->right){
            q.push(n->right);
        }
    }
}
```
## 建立二叉树
```c++
BTree maketree(){
    char a;
    scanf("%c", &a);
    if(a != ' '){
        BTree t = new BNode;
        t->val = a-'0';
        t->left = maketree();
        t->right = maketree();
        return t;
    }
    else{
        return NULL;
    }
}
```
## 建立二叉树(根据数组获得数据)
### 递归方式
```c++
BTree createtree(int a[], int i, int n){
    BNode *p = new BNode();
    if(i>n){
        return NULL;
    }
    else{
        p->val = a[i];
        p->left = createtree(a, 2*i, n);
        p->right = createtree(a, 2*i+1, n);
        return p;
    }
}
```
### 非递归方式
```c++
BTree createtree_traversal(int a[], int n){
    //在分配内存时,建议使用c的写法,malloc
    BNode *p[n];    //声明一个指针数组
    for(int i=0; i<n; i++){
        if(a[i] != 0){
            p[i] = (BNode*)malloc(sizeof(BNode));
            p[i]->val = a[i];
        }
        else{
            p[i] = NULL;
        }
    }
    for(int i=0; i<=n; i++){
        if(p[i] != NULL){
            p[i]->left = p[2*i];
            p[i]->right = p[2*i+1];
        }
    }
}
```
## 二叉树的深度
```c++
int depth(BTree t){
    int rdepth, ldepth;
    if(t == NULL){
        return 0;
    }
    else{
        ldepth = depth(t->left);
        rdepth = depth(t->right);
    }
    if(ldepth > rdepth){
        return ldepth+1;
    }
    else{
        return rdepth+1;
    }
}
```
## 求结点所在层数
```c++
#define max_stack 50
int layernode(BTree t, int item){
    BTree stack1[max_stack];
    BTree p = t;
    int stack2[max_stack], flag, top = -1;
    while(p != NULL || top != -1){
        while(p != NULL){
            stack1[++top] = p;
            stack2[top] = 0;
            p = p->left;
        }
        p = stack1[top];
        flag = stack2[top--];
        if(flag == 0){
            stack1[++top] = p;
            stack2[top] = 1;
            p = p->right;
        }
        else{
            if(p->val == item){
                return top+2;
            }
            p = NULL;
        }
    }
}
```

## 交换二叉树所有节点的左右子树的位置
### c语言
```c++ 
#define max 50
void exchangebt(BTree t){
    BTree queue[max];
    BTree tmp, p = t;
    int front, rear;
    if(t != NULL){
        queue[0] = t;
        front = -1;
        rear = 0;
    }
    while(front < rear){
        p = queue[++front];
        tmp = p->left;
        p->left = p->right;
        p->right = tmp;
    }
    if(p->left != NULL){
        queue[++rear] = p->left;
    }
    if(p->right != NULL){
        queue[++rear] = p->right;
    }
}
```
### c++版本
```c++
void swaptree(BTree t){
    queue<BTree> q;
    BTree tmp, p;
    q.push(t);
    while(!q.empty()){
        p = q.front();
        tmp = p->left;
        p->left = p->right;
        p->right = tmp;
        q.pop();
        if(p->left){
            q.push(p->left);
        }
        if(p->right){
            q.push(p->right);
        }
    }
}
```
## 删除二叉树中以某个节点为根节点的子树
```c++
void delete_node(BTree t, int itm){
    //层序遍历首先找到节点- 版本1
    queue<BTree> q;
    BTree p, tmp;
    q.push(t);
    while(!q.empty()){
        p = q.front();
        if(p->val == itm){
            des(p);
        }
        q.pop();
        if(p->left){
            q.push(p->left);     
        }
        if(p->right){
            q.push(p->right);
        }
    }
    //递归先序遍历- 版本2
    if(t != NULL){
        if(t->val = itm){
            des(t);
        }
        delete_node(t->left, itm);
        delete_node(t->right, itm);
    }
    //非递归先序遍历- 版本3
    stack<BTree> s;
    BTree a;
    while(!s.empty() || p != NULL){
        while(p != NULL){   //注意非递归此处为判定 p
            if(a->val == itm){
                des(a);
            }
            p = p->left;
        }
        a = s.top();
        s.pop();
        p = a->right;
    }
}
void des(BTree t){
    if(t != NULL){
        des(t->left);
        des(t->right);
        free(t);
    }
}
```

# 查找
## 顺序查找-递归
```c++
int recur_search(int a[], int n, int key, int i){
    if(i >= n){
        return -1;
    }
    if(a[i] == key){
        return i;
    }
    else{
        return recur_search(a, n, key, i+1);
    }
}
/*函数调用
int pos = recur_search(a, n, key, 0);
*/
```

## 折半查找
```c++
int binsearch(int a[], int n, int key){
    int low = 0, high = n-1, mid;
    while(low <= high){ //循环边界
        mid = (low + high)/2;
        if(key == a[mid]){
            return mid;
        }
        if(key > a[mid]){
            low = mid+1;
        }
        else{
            high = mid-1;
        }
    }
    return -1;
}
```

## 折半查找递归
```c++
int recur_binsearch(int a[], int low, int high, int key){
    int mid;
    if(low > high){
        return -1;
    }
    else{
        mid = (low + high)/2;
        if(a[mid] == key){
            return mid;
        }
        if(a[mid] > key){
            return recur_binsearch(a, low, mid-1, key);
        }
        else{
            return recur_binsearch(a, mid+1, high, key); 
        }
    }
}
```
## 按值递增线性表中折半查找-插入(非递归)
```c++
void bin_insert(int a[], int n, int key){
    int low = 0, high = n-1, mid;
    while(low <= high){
        mid = (low + high)/2;
        if(key > a[mid]){
            low = mid+1;
        }
        else{
            high = mid-1;
        }
    }
    for(int i=n; i>low; --i){
        a[i] = a[i-1];
    }
    a[low] = key;
    n++;
}
```
## 递增线性表中折半查找值不小于key的最小元素
```c++
int bin_search(int a[], int n, int key){
    int low = 0, high = n-1, mid;
    while(low <= high){
        mid = (low+high)/2;
        if(a[mid] == key){
            return mid;
        }
        if(key > a[mid]){
            low = mid+1;
        }
        else{
            high = mid-1;
        }
    }
    if(low <= n-1){
        return low;
    }
    else{
        return -1;
    }
}
```

# 图
## 邻接矩阵
```c++
#define maxvertexNum 100
typedef char vertexType;
typedef int edgeType;
typedef struct{
    vertexType vex[maxvertexNum];
    edgeType edge[maxvertexNum][maxvertexNum];
    int vexnum, arcnum;
}MGraph;
```
## 邻接表
```c++
typedef struct arcnode{
    int adjvex;
    struct arcnode *next;
}arcnode;

typedef struct vnode{
    vertexType data;
    arcnode *first;
}vnode, adjlist[maxvertexNum];

typedef struct{
    adjlist vertices;
    int vexnum, arcnum;
}ALGraph;
```
## 深度优先遍历 dfs 邻接矩阵
```c++
bool visited[maxvertexNum];
queue<int> q;
void dfsTraverse(MGraph g){
    for(int i=0; i<g.vexnum; i++){
        visited[i] = false;
    }
    for(int i=0; i<g.vexnum; i++){
        if(!visited[i]){    //防止非连通图
            dfs(g, i);
        }
    }
}

void dfs(MGraph g, int v){
    printf("%d", g.vex[v]);
    visited[v] = true;
    for(int i=0; i<g.vexnum; i++){
        if(visited[i] != false && g.edge[v][i] != 0){
            dfs(g, i);
        }
    }
}
```
## 广度优先搜索 邻接表
```c++
void bfsTraverse(ALGraph g, int v){
    for(int i=0; i<g.vexnum; i++){
        visited[i] = false;
    }
    for(int i=0; i<g.vexnum; i++){
        if(visited[i] == false){
            bfs(g, i);
        }
    }
}

void bfs(ALGraph g, int v){
    printf("%d", g.vertices[v].data);
    queue<int> q;
    arcnode *tmp;
    q.push(v);
    int p,a;
    while (!q.empty()){
        p = q.front();
        q.pop();
        tmp = g.vertices[p].first;
        while(tmp){
            a = tmp->adjvex;
            if(visited[a] == false){
                printf("%d", g.vertices[a].data);
                visited[a] = true;
                q.push(a);
            }
            tmp = tmp->next;
        }
    }
}
```
## 邻接表的深度遍历 dfs
```c++
void dfsTraverse(ALGraph g, int v){
    for(int i=0; i<g.vexnum; i++){
        visited[i] = false;
    }
    for(int i=0; i<g.vexnum; i++){
        if(visited[i] == false){
            dfs(g, i);
        }
    }
}

void dfs(ALGraph g, int v){
    printf("%d", g.vertices[v].data);
    stack<int> s;
    arcnode *p;
    int tmp;
    s.push(v);
    while(!s.empty()){
        tmp = s.top();
        p = g.vertices[tmp].first;
        while(p){
            if(visited[p->adjvex] == false && p){
                q.push(p->adjvex);
                visited[p->adjvex] = true;
                printf("%d", g.vertices[p->adjvex].data);
                break;
            }
            p = p->next;
        }
        if(!p){
            s.pop();
        }
    }
}
```

