# C-打印层序遍历构造的二叉树


方便leetcode本地调试
<!--more--> 

## 通过层序遍历数组构造二叉树
```c
/*树的节点*/
struct node {
    int val;
    struct node *left, *right;
};

/*构造二叉树*/
void insert(struct node *p, int *a,int t, int num){
    //int length = sizeof(a)/sizeof(int);
    //printf("this is the length:%d",length);
    if(t>=num)return;
    p->val = a[t];
    if((2 * (t+1)) <= num){
        if(a[2 * (t+1) -1]!=0){
            struct node *l;
            l = (struct node*)malloc(sizeof(struct node));
            l->left = NULL;
            l->right = NULL;
            p->left = l;
            insert(p->left, a, 2 * (t+1) -1,num);
            printf("建立节点");
        }
        if((2 * (t+1) +1) <= num){
            if(a[2 * (t+1)]!=0){
                struct node *r;
                r = (struct node*)malloc(sizeof(struct node));
                r->left = NULL;
                r->right = NULL;
                p->right = r;
                insert(p->right, a, 2 * (t+1),num);
                printf("建立节点");
            }
        }
    }
}
     
struct node* createFromArray(int* a, int num){
    struct node *p;
    p = (struct node*)malloc(sizeof(struct node));
    p->left = NULL;
    p->right = NULL;
    insert(p,a,0,num);
    return p;
}
```

## 打印二叉树
```c
int vec_left[100] = {0};
// 显示二叉树的函数，只要调用Display(root, 0)即可
void Display(struct node* root, int ident)
{
    if(ident > 0)
    {
        for(int i = 0; i < ident - 1; ++i)
        {
            printf(vec_left[i] ? "│   " : "    ");
        }
        printf(vec_left[ident-1] ? "├── " : "└── ");
    }

    if(! root)
    {
        printf("gg啦\n");
        return;
    }
    printf("%d\n", root->val);
    if(!root->left && !root->right)
    {
        return;
    }
    vec_left[ident] = 1;
    Display(root->left, ident + 1);
    vec_left[ident] = 0;
    Display(root->right, ident + 1);
}
```

## main函数
```c
int main(){
    int temp[] = {1,2,3,4,8,9,10,11,14,24,13};
    int length = sizeof(temp)/sizeof(temp[0]);
    struct node* root = createFromArray(temp,length);
    Display(root,0);
    return 1;
}
```

