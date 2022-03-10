# Leetcode Part-3



232-用栈实现队列<br>
662-二叉树最大宽度<br>
21-合并两个有序链表<br>
1-两数之和
<!--more-->

## 232-用栈实现队列
使用栈实现队列的下列操作：
```
    push(x) -- 将一个元素放入队列的尾部。
    pop() -- 从队列首部移除元素。
    peek() -- 返回队列首部的元素。
    empty() -- 返回队列是否为空。
```
一开始我是想到用两个栈的，但竟然傻到...要用链栈去实现...我鲨我自己
这道题用静态栈很棒，因为两个栈元素来回移动，不用malloc啊

```c
#define MAXSIZE 100
//创建栈
struct Stack{
    int Data[MAXSIZE];
    int TOP;
};
typedef struct Stack MyStack;
//队列定义为双栈
typedef struct {
    MyStack S1; //S1为主栈
    MyStack S2; //S2为用来反转的栈
} MyQueue;

/** Initialize your data structure here. */

MyQueue* myQueueCreate() {
    MyQueue* TempQueue=(MyQueue*)malloc(sizeof(MyQueue));
    TempQueue->S1.TOP=-1;
    TempQueue->S2.TOP=-1;
    return TempQueue;
}

/** Push element x to the back of queue. */
void myQueuePush(MyQueue* obj, int x) {
    
    if(obj->S1.TOP<MAXSIZE){ //栈是否满
        while(obj->S1.TOP!=-1){
            obj->S2.Data[++(obj->S2.TOP)]=obj->S1.Data[(obj->S1.TOP)--]; //把S1栈中元素压入S2实现反转
        }
        obj->S1.Data[++(obj->S1.TOP)]=x; //把push的元素压入S1栈（此时S1为空栈，因为它的元素已经全部给S2啦）
        while(obj->S2.TOP!=-1){
            obj->S1.Data[++(obj->S1.TOP)]=obj->S2.Data[(obj->S2.TOP)--]; //再把S2栈中的元素全部反转压入S1
        }
    } 
}

/** Removes the element from in front of queue and returns that element. */
int myQueuePop(MyQueue* obj) {
    if(obj->S1.TOP!=-1)
        return obj->S1.Data[(obj->S1.TOP)--];
    return NULL;   
}

/** Get the front element. */
int myQueuePeek(MyQueue* obj) {
    if(obj->S1.TOP!=-1)
        return obj->S1.Data[obj->S1.TOP];
    return NULL; 
}

/** Returns whether the queue is empty. */
bool myQueueEmpty(MyQueue* obj) {
    if(obj->S1.TOP==-1)
        return true;
    return false;
}

void myQueueFree(MyQueue* obj) {
    free(obj);
}
```

## 662-二叉树最大宽度
1. 问题描述：
   给定一个二叉树，编写一个函数来获取这个树的最大宽度。树的宽度是所有层中的最大宽度。这个二叉树与满二叉树（full binary tree）结构相同，但一些节点为空。
   每一层的宽度被定义为两个端点（该层最左和最右的非空节点，两端点间的null节点也计入长度）之间的长度。
2. 解题思路：
   本题有两种思路，分别是层次遍历和深度遍历
   由于层次遍历还要构造一个队列...算了算了
   这里提供一个好的方法，递归深度遍历，用一个数组存储每层第一个元素的position，这样每次访问元素时，就和当前层的第一个元素进行position-a[index]+1运算，得到宽度

```c
int max = 1;
void func(struct TreeNode *node, int index, double pos, int *a) {
    
    if (node == NULL) {
        return;
    }
    if (a[index] == 0) {    //如果当前值为0，则为第一次访问该层
        a[index] = pos;
    } else {
        max = max > (pos - a[index] + 1) ? max : (pos - a[index] + 1);
    }
    func(node->left, index + 1, 2 * pos, a);
    func(node->right, index + 1, 2 * pos + 1, a);
}

int widthOfBinaryTree(struct TreeNode* root){
    int index = 0;
    int a[1000000] = {0};    //将静态数组的值都初始化为0
    if (root == NULL) {
        return 0;
    }
    max = 1;
    func(root, 1, 1, a);
    return max;
}
```

## 21-合并两个有序链表
1. 常规算法

```c
struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2){
    if(!l1){
        return l2;
    }
    if(!l2){
        return l1;
    }
    struct ListNode *head = (struct ListNode*)malloc(sizeof(struct ListNode)),*p = head; //head node
    while(l1 && l2){
        if(l1->val > l2->val){
            p->next = l2;
            l2 = l2->next;
        }
        else{
            p->next = l1;
            l1 = l1->next;
        }
        p = p->next;
    }
    if(l1){
        p->next = l1;
    }
    else if(l2){
        p->next = l2;
    }
    return head->next;
}
```
1. 递归算法

```c
struct ListNode * mergeTwoLists(struct ListNode * l1, struct ListNode * l2) {
    if (!l1)
        return l2;
    if (!l2)
        return l1;
    if (l1 - > val < l2 - > val) {
        l1 - > next = mergeTwoLists(l1 - > next, l2);
        return l1;
    } else {
        l2 - > next = mergeTwoLists(l1, l2 - > next);
        return l2;
    }
}
```

## 1-两数之和
题目：给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

打扰了，我只会暴力算法...
```c
int* twoSum(int* nums, int numsSize, int target, int* returnSize){
    int *res = (int*)malloc(sizeof(int)*2);
    *returnSize = 0;
    for(int i=0; i<numsSize; i++){
        for(int j=i+1; j<numsSize; j++){
            if(nums[i]+nums[j] == target){
                res[0] = i;
                res[1] = j;
                *returnSize = 2;
                return res;
            }
        }
    }
    return res;
}
```
