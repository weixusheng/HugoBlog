# Leetcode Part-7



2-两数相加<br>
7-整数反转<br>
用两个栈实现队列<br>
110-平衡二叉树<br>
543-二叉树的直径
<!--more-->

## 2-两数相加
题目:
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
```c
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```
解题思路:我的思路就是暴力求解...
### 暴力求解
```c
struct ListNode* reverse_list(struct ListNode* l){
    struct ListNode* a = l;
    struct ListNode* b = NULL;
    struct ListNode* r = NULL;
    while(a){
        r = a;
        a = a->next;
        r->next = b;
        b = r;
    }
    return r;
}
struct ListNode* addTwoNumbers(struct ListNode* l1, struct ListNode* l2){
    struct ListNode* p = l1;
    struct ListNode* q = l2;
    struct ListNode* nex = NULL; 
    int flag = 0;
    while(1){
        int value = 0;
        int cur = 0;
        if(p == NULL && q == NULL && flag == 0){
            break;
        }
        if(p && q){
            value = p->val + q->val;
            p = p->next;
            q = q->next;
        }
        else if(p == NULL && q != NULL){
            value = q->val;
            q = q->next;
        }
        else if(q == NULL && p != NULL){
            value = p->val;
            p = p->next;
        }
        if(flag == 1){
            ++value;
        }
        if(value >= 10){
            flag = 1;
            cur = value % 10;
        }
        else{
            flag = 0;
            cur = value;
        }
        struct ListNode* s = (struct ListNode*)malloc(sizeof(struct ListNode));
        s->val = cur;
        s->next = nex;
        nex = s;
    }
    struct ListNode* res = reverse_list(nex);
    return res;
}
```

### 递归求解
```c
#define TEN 10
static int g_carry = 0;
struct ListNode *addTwoNumbers(struct ListNode *l1, struct ListNode *l2)
{
    struct ListNode *result = NULL;
    int a = 0;
    int b = 0;

    if ((!l1) && (!l2) && (!g_carry)) {
        return NULL;
    }
    result = (struct ListNode *)malloc(sizeof(struct ListNode));
    l1 = (l1) ? (g_carry += l1->val, l1->next) : (l1);
    l2 = (l2) ? (g_carry += l2->val, l2->next) : (l2);

    result->val = g_carry % TEN;
    g_carry /= TEN;
    result->next = addTwoNumbers(l1, l2);
    return result;
}
```

## 7-整数反转
题目:给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
```
输入: 123
输出: 321
```
解题思路:一开始想到取余,但是没想明白怎么处理未知位数的整数,原来一个while再加上乘10就能解决,不错的思路
```c
int reverse(int x){
    long count=0;
    while(x!=0){
        count=count*10+x%10;
        x=x/10;
    }
    return count>2147483647||count<-2147483648?0:count;
}
```

## 用两个栈实现队列
### 链栈
```c
typedef struct Node
{
    int val;
    struct Node* next;
}Node;
Node* newNode(int Val)
{
    Node* n=(Node*)malloc(sizeof(Node));
    n->val=Val;
    n->next= NULL;
    return n;
}
void push(Node* Head,int Val)
{
    Node* n = newNode(Val);
    n->next = Head->next;
    Head->next=n;
}
int pop(Node* Head)
{
    Node* n=Head->next;
    Head->next=n->next;
    int result=n->val;
    free(n);
    return result;
}
void del(Node* Head)
{
    while(Head!=NULL)
    {
        Node* n=Head;
        Head=Head->next;
        free(n);
    }
}
typedef struct {
    Node* stackIn;  //两个头结点,value = NULL
    Node* stackOut;
} CQueue;
CQueue* cQueueCreate() {
    CQueue* q=(CQueue*)malloc(sizeof(CQueue));
    q->stackIn=newNode(NULL);
    q->stackOut=newNode(NULL);
    return q;
}
void cQueueAppendTail(CQueue* obj, int value) {
    push(obj->stackIn,value);
}
int cQueueDeleteHead(CQueue* obj) {
    if(obj->stackOut->next==NULL && obj->stackIn->next==NULL)
        return -1;
    if(obj->stackOut->next == NULL)
        while(obj->stackIn->next != NULL)
            push(obj->stackOut,pop(obj->stackIn));
    return pop(obj->stackOut);
}
void cQueueFree(CQueue* obj) {
    del(obj->stackIn);
    del(obj->stackOut);
    free(obj);
}
```

### 静态栈实现
```c
#define MAX_SIZE 1000
typedef struct {
    int top;
    int arr[MAX_SIZE];
} Stack;
Stack *createStack()
{
    Stack *stack = (Stack *)malloc(sizeof(Stack));
    if (stack == NULL) {
        return NULL;
    }
    stack->top = -1;
    memset(stack->arr, 0x0, sizeof(int) * MAX_SIZE);
    return stack;
}
void push(Stack *stack, int val)
{
    if ((stack == NULL) || (stack->top >= MAX_SIZE)) {
        return;
    }
    stack->arr[++stack->top] = val;
    return;
}
int pop(Stack *stack)
{
    if ((stack == NULL) || (stack->top == -1)) {
        return INT_MAX;
    }
    return stack->arr[stack->top--];
}
int isEmpty(Stack *stack)
{
    if (stack == NULL) {
        return 1; // 1表示栈为空
    }

    return stack->top == -1 ? 1 : 0;
}
typedef struct {
    Stack *dataStack;
    Stack *helpStack;
} CQueue;
CQueue* cQueueCreate() {
    CQueue *queue = (CQueue *)malloc(sizeof(CQueue));
    if (queue == NULL) {
        return NULL;
    }
    queue->dataStack = createStack();
    queue->helpStack = createStack();
    return queue;
}
void cQueueAppendTail(CQueue* obj, int value) {
    if (obj == NULL) {
        return;
    }
    push(obj->dataStack, value);
    return;
}
int cQueueDeleteHead(CQueue* obj) {
    if (obj == NULL) {
        return -1;
    }
    int tmp;
    if (isEmpty(obj->helpStack) == 1) {
        while (isEmpty(obj->dataStack) != 1) {
            tmp = pop(obj->dataStack);
            push(obj->helpStack, tmp);
        }
    }
    return isEmpty(obj->helpStack) == 1 ? -1 : pop(obj->helpStack);
}
void cQueueFree(CQueue* obj) {
    if (obj == NULL) {
        return;
    }
    if (obj->dataStack != NULL) {
        free(obj->dataStack);
    }
    if (obj->helpStack != NULL) {
        free(obj->helpStack);
    }
    free(obj);
    return;
}
```

## 110-平衡二叉树
题目: 判断一个数是否为平衡二叉树
解题思路: 首先递归求每个节点的深度,然后递归将左右子数深度进行比较
```c
int Depth(struct TreeNode* root){
    if(!root) return 0;
    int lren=Depth(root->left);
    int rlen=Depth(root->right);
    return lren>rlen?(lren+1):(rlen+1);
}

bool isBalanced(struct TreeNode* root){
    if(root==NULL)return true;
    if(abs(Depth(root->left)-Depth(root->right))>1) //绝对值大于1,非平衡
        return false;
    return (isBalanced(root->left)&&isBalanced(root->right));
}
```

## 543-二叉树的直径
题目: 给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。
解题思路: 深度优先遍历,在节点遍历过程中,进行比较max以及left+right的值
```c
int diameterOfBinaryTree(struct TreeNode* root){
    int max = 0;
    if(root == NULL){
        return 0;
    }
    depth(root, &max);      //max取地址,递归遍历过程一直比较
    return max;
}
int depth(struct TreeNode* root, int *max){
    if(root == NULL){
        return 0;
    }
    else{
        int depthleft = depth(root->left,max);
        int depthright = depth(root->right,max);
        int sum = depthleft + depthright;
        if(sum > *max){
            *max = sum;
        }
        return depthleft > depthright ? depthleft+1 : depthright+1;
    }
}
```


