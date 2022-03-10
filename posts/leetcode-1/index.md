# Leetcode Part-1



今天是blog记录leetcode刷题的第一天!努力做到每天打卡!加油!!!<br>
206-单链表反转<br>
225-队列实现栈<br>
100-判断两棵相同的树<br>
111-求树叶子节点所处在的最小深度
<!--more-->

# 206-单链表反转
输入 1->2->3->4->5->null
输出 5->4->3->2->1->null
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* reverseList(struct ListNode* head){
    struct ListNode* nextTmp = NULL;
    struct ListNode* cur = head;
    struct ListNode* prv = NULL;
    while(NULL != cur){
        nextTmp = cur->next;
        cur->next = prv;
        prv = cur;
        cur = nextTmp;
    }
    return prv;
}
```

# 225-队列实现栈
使用队列实现栈的下列操作：
```
    push(x) -- 元素 x 入栈
    pop() -- 移除栈顶元素
    top() -- 获取栈顶元素
    empty() -- 返回栈是否为空
```
思路:使用两个队列来实现栈
```
队列A、B
入栈：入队列A
出栈：把队列A的前n-1个元素倒到队列B，把第n个元素去掉。此时数据在B中，下次操作，则对B操作。
栈顶：把队列A的前n-1个元素倒到队列B，把第n个元素作为栈顶。 
```
**stdlib头文件**
即standard library标准库函数头文件
包含了C、C++语言的最常用的系统函数
常用的函数如malloc()、calloc()、realloc()、free()、system()、atoi()、atol()、rand()、srand()、exit()
```c
#include <stdlib.h>
#define LEN 20
typedef struct queue{   //定义queue结构
    int *data;
    int head;
    int rear;
    int size;
}Queue;

typedef struct {    //定义栈结构-包含两个队列指针
    Queue *q1, *q2;
} MyStack;

Queue *initQueue(int k){       //队列初始化
    Queue *obj=(Queue *)malloc(sizeof(Queue));
    obj->data=(int *)malloc(k*sizeof(int));
    obj->head=-1;
    obj->rear=-1;
    obj->size=k;
    return obj;
}
void enQueue(Queue *obj,int e){
    //入队的时候，此题不必考虑队列满状态情形
    //队列为空的情形
    if(obj->head==-1){
      obj->head=0;
    }
    //队列一般情形
    obj->rear=(obj->rear+1)%obj->size;
    obj->data[obj->rear]=e;
}
int deQueue(Queue *obj){
    //出队的时候，此题不必考虑队列为空的情形
    int a=obj->data[obj->head];
    //特殊情形，队列中只有一个元素
    if(obj->head==obj->rear){
        obj->rear=-1;
        obj->head=-1;
        return a;
    }
    //队列一般情形
    obj->head=(obj->head+1)%obj->size;
    return a;
}
int isEmpty(Queue *obj){
    return obj->head==-1;
}

/** Initialize your data structure here. */
MyStack* myStackCreate() {
    MyStack *obj=(MyStack *)malloc(sizeof(MyStack));
    obj->q1=initQueue(LEN);
    obj->q2=initQueue(LEN);
    return obj;
}

/** Push element x onto stack. */
void myStackPush(MyStack* obj, int x) {
  //只要我找到一个队列为非空，我就向里面添加元素，如果两个都是空的，那随便哪一个都可以
  if(isEmpty(obj->q1)){
    enQueue(obj->q2,x);
  }else{
    enQueue(obj->q1,x);
  }
}

/** Removes the element on top of the stack and returns that element. */
//栈弹出的时候，有且只有一个队列为非空
int myStackPop(MyStack* obj) {
    //q2为非空的时候，q2出列直到q2中只有一个元素
    if(isEmpty(obj->q1)){
        while(obj->q2->head != obj->q2->rear){
            //q2出列的元素，入列q1
            enQueue(obj->q1,deQueue(obj->q2));
        }
        return  deQueue(obj->q2);
    }
    //反之q1非空
    while(obj->q1->head != obj->q1->rear){
        enQueue(obj->q2,deQueue(obj->q1));
    }
    return  deQueue(obj->q1);
}

/** Get the top element. */
//取栈顶元素，有且只有一个队列为非空，我直接取非空队列的尾部即可
int myStackTop(MyStack* obj) {
    if(isEmpty(obj->q1)){
        return obj->q2->data[obj->q2->rear];
    }
    return obj->q1->data[obj->q1->rear];
}

/** Returns whether the stack is empty. */
//当且仅当两个队列都是空的情形
bool myStackEmpty(MyStack* obj) {
    if(obj->q1->head==-1 && obj->q2->head==-1){
        return true;
    }
    return false;
}

void myStackFree(MyStack* obj) {    //释放两个队列以及栈
    free(obj->q1->data);
    obj->q1->data=NULL;
    free(obj->q1);
    obj->q1=NULL;
    free(obj->q2->data);
    obj->q2->data=NULL;
    free(obj->q2);
    obj->q2=NULL;
    free(obj);
    obj=NULL;
}
```

# 100-判断两棵相同的树
这道题太简单啦!
```c
bool isSameTree(struct TreeNode* p, struct TreeNode* q){
    if (NULL == p && NULL == q)
    {
        return true;
    }

    if (NULL == p || NULL == q)
    {
        return false;
    }

    if (p->val != q->val)
    {
        return false;
    }

    return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
}
```

# 111-求树叶子节点所处在的最小深度
**这道题在队列的操作上有些难度,需要自己写队列的实现**

想了好久,没想起来队列操作咋写了...
看到题解我傻了...用c是真的爽鸭,用个队列要写60多行代码
```c
typedef struct QueueNode {
    struct TreeNode *data;
    struct QueueNode *next;
} QueueNode_t;

typedef struct LinkQueue {
    int count;
    QueueNode_t *front;
    QueueNode_t *rear;
} LinkQueue_t;

int initQueue(LinkQueue_t *queue) {
    queue->front = queue->rear = malloc(sizeof(QueueNode_t));
    if (NULL == queue->front)
        return -1;
    queue->front->next = NULL;
    queue->count = 0;
    return 0;
}

int enqueue(LinkQueue_t *queue, struct TreeNode *data) {
    QueueNode_t *newNode = malloc(sizeof(QueueNode_t));
    if (NULL == newNode) {
        return -1;
    }
    newNode->data = data;
    newNode->next = NULL;
    queue->rear->next = newNode;
    queue->rear = newNode;
    queue->count++;
    return 0;
}
//出栈的特别之处,使用了双指针 ** 
int dequeue(LinkQueue_t *queue, struct TreeNode **data) {
    if (queue->front == queue->rear) {
        return -1;
    }

    QueueNode_t *denode = queue->front->next;
    *data = denode->data;
    queue->front->next = denode->next;

    if (denode == queue->rear) {
        queue->rear = queue->front;
    }

    free(denode);
    queue->count--;
    return 0;
}

void destroyQueue(LinkQueue_t *queue) {
    /*q->front 指向头node, queue->rear指向下一个节点，这里当临时指针用*/
    while (queue->front) {
        queue->rear = queue->front->next;
        free(queue->front);
        queue->front = queue->rear;
    }
}

int emptyQueue(LinkQueue_t *queue) {
    return queue->front == queue->rear ? 1 : 0;
}

int minDepth(struct TreeNode *root) {
    if (NULL == root)
        return 0;

    int max = 0, i = 0, cnt = 0;
    struct TreeNode *data = NULL;
    LinkQueue_t queue;
    initQueue(&queue);

    enqueue(&queue, root);
    while (!emptyQueue(&queue)) {
        cnt = queue.count;  //cnt保存当前层的节点数量
        max++;
        for (i; i < cnt; i++) {
            dequeue(&queue, &data);
            if (NULL == data->left && NULL == data->right) {
                return max;
            }
            if (data->left) {
                enqueue(&queue, data->left);
            }
            if (data->right) {
                enqueue(&queue, data->right);
            }
        }
    }
    destroyQueue(&queue);   //良好的习惯,注销掉辅助队列

    return max;
}

```
