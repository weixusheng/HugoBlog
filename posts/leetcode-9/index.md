# Leetcode Part-9



993-二叉树的堂兄弟节点
<!--more-->

## 993-二叉树的堂兄弟节点
题目:
在二叉树中，根节点位于深度 0 处，每个深度为 k 的节点的子节点位于深度 k+1 处。
如果二叉树的两个节点深度相同，但父节点不同，则它们是一对堂兄弟节点。
我们给出了具有唯一值的二叉树的根节点 root，以及树中两个不同节点的值 x 和 y。
解题思路:
从这道题里我学到了好多!感觉现在只要想到思路了,代码就都能写出来,不像一开始写代码磕磕巴巴的,一开始就想到层序遍历,然后暴力求解,所以思路也没啥解释的,一直没看题解的我感觉很开心(虽然只是一道简单题)
### 暴力求解
```c
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
};

typedef struct List_node {
    struct TreeNode* data;
    struct TreeNode* father;
    struct List_node* next;
}listnode;

typedef struct{
    listnode* head;
    int count;
    listnode* next;
}listhead;

bool isCousins(struct TreeNode* root, int x, int y){
    listhead* head = (listhead*)malloc(sizeof(listhead));
    head->count = 1;
    listnode* firstnode = (listnode*)malloc(sizeof(listnode));
    firstnode->data = root;
    head->next = firstnode;
    int num = 0;
    listnode* flag1 = NULL;
    listnode* flag2 = NULL;
    while(head->count != 0){
        num = head->count;
        listnode* visinode = head->next;
        for(int i=0; i<num; i++){
            if(visinode->data->val == x){
                flag1 = visinode;
            }
            if(visinode->data->val == y){
                flag2 = visinode;
            }
            if(visinode->data->left){
                head->count++;
                listnode* newnode = (listnode*)malloc(sizeof(listnode));
                newnode->data = visinode->data->left;
                newnode->father = visinode;
                newnode->next = head->next;
                head->next = newnode;
            }
            if(visinode->data->right){
                head->count++;
                listnode* newnode = (listnode*)malloc(sizeof(listnode));
                newnode->data = visinode->data->right;
                newnode->father = visinode;
                newnode->next = head->next;
                head->next = newnode;
            }
            visinode = visinode->next;
            head->count--;
        }
        if(flag1 && flag2 && flag1->father != flag2->father){
            return true;
        }
        else{
            flag1 = NULL;
            flag2 = NULL;
        }
    }
    return false;
}
```

### 递归
题解中大佬的递归算法,大佬大佬...学到了
```c
struct TreeNode* parent(struct TreeNode* root, int x,int *hx){
    if(root==NULL) return NULL;
    /*判断左右节点的值是否为x,y如果为真,则当前节点为父节点,返回*/
    if((root->left&&root->left->val==x)||(root->right&&root->right->val==x))
        return root;    //找到结点 x,y
    else{
        (*hx)++;    //hx为层数
        int t=*hx;  //保存当前递归开始的层数,等待回溯
        struct TreeNode *p=parent(root->left,x,hx); //递归左子树,计算层数
        if(p) return p;
        else{
            *hx=t;  //递归右子树,赋值之前保存的层数,回溯
            return parent(root->right,x,hx);    //计算层数
        } 
    }
}
bool isCousins(struct TreeNode* root, int x, int y){
    if(root==NULL) return 0;
    if(root->val==x||root->val==y) return 0;
    struct TreeNode *p,*q;
    int *hx,*hy;    //两个int变量
    hx=(int*)malloc(sizeof(int));
    hy=(int*)malloc(sizeof(int));
    *hx=0,*hy=0;
    p=parent(root,x,hx);
    q=parent(root,y,hy);
    if(p!=q&&*hx==*hy) return 1;
    return 0;
}
```

看着周围的同学都陆续被录取了,心里有些不是滋味,但还是清楚的明白,是自己没下工夫
羡慕别人,痛恨自己,后悔过去,继续前行
今天早睡,明天继续快乐刷题
