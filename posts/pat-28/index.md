# PAT-Advanced 树专题



算法笔记 - 树专题
<!--more-->

# 遍历
最基础的知识点,还是敲一遍把
## 先序遍历
```c++
void preorder(node* root){
    if(root == NULL){
        return;
    }
    printf("%d\n", root->data);
    preorder(root->lchild);
    preorder(root->rchild);
}
```

## 中序遍历
```c++
void inorder(node* root){
    if(root == NULL){
        return;
    }
    inorder(root->lchild);
    printf("%d\n", root->data);
    inorder(root->rchild);
}
```

## 后序遍历
```c++
void inorder(node* root){
    if(root == NULL){
        return;
    }
    inorder(root->lchild);
    inorder(root->rchild);
    printf("%d\n", root->data);
}
```

## 层序遍历
```c++
void layerorder(node* root){
    queue<node*> q;
    q.push(root);
    while(!q.empty()){
        node* mow = q.front();
        q.pop();
        printf("%d", now->data);
        if(now->lchild != NULL){
            q.push(now->lchild);
        }
        if(now->rchild != NULL){
            q.push(now->rchild);
        }
    }
}
```

## 层序遍历记录层数
```c++
struct node{
    int data;
    int layer;
    node* lchild;
    node* rchild;
}

void layerorder(node* root){
    queue<node*> q;
    root->layer = 1;
    q.push(root);
    while(!q.empty()){
        node *now = q.front();
        q.pop();
        printf("%d", now->data);
        if(now->lchild != NULL){
            now->lchild->layer++;
            q.push(now->lchild);
        }
        if(now->rchild != NULL){
            now->rchild->layer++;
            q.push(now->rchild);
        }
    }
}
```

## 先序序列和中序序列 构建二叉树
```c++
node* create(int prel, int prer, int inl, int inr){
    if(prel > prer){
        return NULL;
    }
    node* root = new node;
    root->data = pre[prel];
    int k;
    for(k=inl; k<= inr; k++){
        if(in[k] == pre[prel]){ //在中序序列中,找到前序遍历中的根节点
            break;
        }
    }
    int numleft = k-inl;
    root->lchild = create(prel+1, prel+numleft, inl, k-1);
    root->rchild = create(prel+numleft+1, prer, k+1, inr);
    return root;
}
```

## 二叉查找树
### 查找
```c++
void search(node* root, int x){
    if(root == NULL){
        printf("search failed\n");
        return;
    }
    if(x == root->data){
        printf("%d\n", root->data);
    }
    else if(x < root->data){
        search(root->lchild, x);
    }
    else{
        search(root->rchild, x);
    }
}
```

### 插入
```c++
void insert(node* &root, int x){
    if(root == NULL){
        root = newNode(x);
        return;
    }
    if(x == rot->data){
        return;
    }
    else if(x < root->data){
        insert(root->lchild, x);
    }
    else{
        insert(root->rchild, x);
    }
}
```

### 建立
```c++
node* create(int data[], int n){
    node* root = NULL;
    for(int i=0; i<n; i++){
        insert(root, data[i]);
    }
    return root;
}
```

### 查找最大值
```c++
node* findmax(node* root){
    while(root->rchild != NULL){
        root = root->rchild;
    }
    return root;
}
```

### 删除结点
如果当前节点root不存在左右结点,说明是叶子结点,直接删除
如果当前root存在左孩子,那么在左子树中寻找结点前驱pre(最右结点),然后让pre的数据覆盖root
如果当前root存在右子树,那么在右子树中寻找结点后继next(最左结点),然后让next的数据覆盖root
```c++
void delete(node* &root, int x){
    if(root == NULL){
        return;
    }
    if(root -> data == x){
        if(root->lchild == NULL && root->rchild == NULL){
            root = NULL;
        }
        else if(root->lchild != NULL){
            node* pre = findmax(root->lchild);
            root->data = pre->data;
            deletenode(root->lchild, pre->data);  
        }
        else{
            node* next = findmax(root->rchild);
            root->data = next->data;
            deletenode(root->lchild, next->data);
        }
    }
    else if(root->data > x){
        deletenode(root->lchild, x);
    }
    else{
        deletenode(root->rchild, x);
    }
}
```

# 平衡二叉树
```c++
struct node{
    int v, height;
    node *lchild, *rchild;
};

struct* newnode(int v){
    node* node = new node;
    node->v = v;
    node->height = 1;
    node->lchild = node->rchild = NULL;
    return node;
}
//获取当前高度
int getheight(node* root){
    if(root == NULL){
        return 0;
    }
    return root->height;
}
//计算平衡因子
int getbalancefactor(node* root){
    return getheight(root->lchild)-getheight(root->rchild);
}

//更新root的height
void updateheight(node* root){
    root->height = max(getheight(root->lchild), getheight(root->rchild))+1;
}

//查找
void search(node* root, int x){
    if(root == NULL){
        printf("search failed\n");
        return;
    }
    if(x == root->data){
        printf("%d\n", root->data);
    }
    else if(x < root->data){
        search(root->lchild, x);
    }
    else{
        search(root->rchild, x);
    }
}
```

# 平衡二叉树插入
## 左旋
```c++
void l(node* &root){
    node* tmp = root->rchild;
    root->rchild = tmp->lchild;
    tmp->lchild = root;
    updateheight(root);
    updateheight(tmp);
    root = tmp;
}
```

## 右旋
```c++
void r(node* &root){
    node* tmp = root->lchild;
    root->lchild = tmp->rchild;
    tmp->rchild = root;
    updateheight(root);
    updateheight(tmp);
    root->tmp;
}
```

## 插入操作
```c++
void insert(node* &root, int v){
    if(root == NULL){
        root = newnode(v);
        return;
    }
    if(v < root->v){
        insert(root->lchild, v);
        updateheight(root);
        if(getbalancefactor(root->lchild) == 1){
            r(root);    //LL旋转
        }
        else if(getbalancefactor(root->lchild) == -1){
            l(root->lchild);
            r(root);        //LR旋转
        }
    }
    else{
        insert(root->rchild, v);
        updateheight(root);
        if(getbalancefactor(root) == -2){
            if(getbalancefactor(root->rchild) == -1){
                l(root);    //RR旋转
            }
            else if(getbalancefactor(root->rchild) == 1){
                r(root->rchild);
                l(root);    //RL旋转
            }
        }
    }
}
```


