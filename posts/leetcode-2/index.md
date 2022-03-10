# Leetcode Part-2



11-栈中保存最小值
<!--more-->

## 11-栈中保存最小值
题目:设计一个支持push,pop,top操作，并能在常数时间内检索到最小元素的栈。
```
    push(x) —— 将元素 x 推入栈中。
    pop() —— 删除栈顶的元素。
    top() —— 获取栈顶元素。
    getMin() —— 检索栈中的最小元素。
```
此题求解有两个思路,分别是双栈和单栈
1. 双栈中一个栈记录栈中元素,另一个栈记录最小元素(似乎有种似曾相识的感觉)
2. 单栈即在栈结构中保存最小元素的值

我只想到了第二种方法,然后还没写明白...第一种压根没想到,算法还是要奇思广义啊
### 方法一
```c
typedef struct tagNode{ //定义链栈节点
    int val;
    struct tagNode *next;
}LinkList, *Node_ptr;

typedef struct { //定义链栈
    LinkList* stk;
    int min_val;
} MinStack;

MinStack* minStackCreate() {
    MinStack *p = (MinStack*)malloc(sizeof(MinStack));
    p->stk = (LinkList*)malloc(sizeof(LinkList));
    p->stk->next = NULL;
    return p;
}

void minStackPush(MinStack* obj, int x) {
    if(!obj->stk->next || (obj->stk->next && x < obj->min_val))
        obj->min_val = x;
    Node_ptr p = (Node_ptr)malloc(sizeof(LinkList));
    p->val = x;
    p->next = obj->stk->next;
    obj->stk->next = p;
}

void findMinVal(MinStack *obj)　　//遍历链表找最小值节点
{
    Node_ptr p = obj->stk->next;
    if(p){
        obj->min_val = p->val;
        while(p){
            if(p->val < obj->min_val)
                obj->min_val = p->val;
            p = p->next;
        }
    }
}

void minStackPop(MinStack* obj) {
    if(obj->stk->next){
        Node_ptr p = obj->stk->next;
        obj->stk->next = p->next;
        if(p->val == obj->min_val){   //若要pop的节点为最小值节点，需要找到新的最小值节点
            findMinVal(obj);
        }
        free(p);
    }
}

int minStackTop(MinStack* obj) {
    if(obj->stk->next){
        return obj->stk->next->val;
    }
    return;
}

int minStackGetMin(MinStack* obj) {
    if(obj->stk->next){
        return obj->min_val;
    }
    return;
}

void minStackFree(MinStack* obj) {
    free(obj);
}
```

### 方法二
使用了静态栈
```c
#define STACK_MAX 1000

typedef struct {
    int top;
    int data[STACK_MAX];
} MyStack;

typedef struct {
    MyStack stackAll;
    MyStack stackMin;
} MinStack;

MinStack *minStackCreate() {
    MinStack *stack = malloc(sizeof(MinStack));
    stack->stackAll.top = -1;
    stack->stackMin.top = -1;

    return stack;
}

void minStackPush(MinStack *obj, int x) {
    //只判断stackAll即可，stackMin的数据小于等于stackAll
    if (obj->stackAll.top == (STACK_MAX - 1))
        return;

    //stackAll保存所有数据，都要入栈
    obj->stackAll.data[++obj->stackAll.top] = x;

    //stackMin保存最小数据，入栈前需要判断栈顶值
    if (-1 == obj->stackMin.top || x <= obj->stackMin.data[obj->stackMin.top]) {
        obj->stackMin.data[++obj->stackMin.top] = x;
    }
}

void minStackPop(MinStack *obj) {
    if (obj->stackAll.top == -1)
        return;

    if (obj->stackAll.data[obj->stackAll.top] == obj->stackMin.data[obj->stackMin.top]) {
        obj->stackMin.top--;
    }
    obj->stackAll.top--;
}

int minStackTop(MinStack *obj) {
    return obj->stackAll.data[obj->stackAll.top];
}

int minStackGetMin(MinStack *obj) {
    return obj->stackMin.data[obj->stackMin.top];
}

void minStackFree(MinStack *obj) {
    free(obj);
}
```
