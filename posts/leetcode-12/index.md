# Leetcode Part-12



203-移除链表元素<br>
160-相交链表<br>
1019-链表中的下一个更大节点<br>
malloc与calloc的区别
<!--more-->

## 203-移除链表元素
题目:删除链表中等于给定值 val 的所有节点。
解题思路:简简单单,常规操作,但是头结点要进行额外判断
```c
struct ListNode* removeElements(struct ListNode* head, int val){
    if(head == NULL){
        return NULL;
    }
    struct ListNode* p = head;
    struct ListNode* temp = NULL;
    struct ListNode* pre = NULL;
    while(p != NULL){
        if(p->val == val){
            temp = p;
            if(pre == NULL){
                head = head->next;
                p = head;
            }
            else{
                pre->next = p->next;
                p = p->next;
                free(temp);
            }
        }
        else{
            pre = p;
            p = p->next;
        }
    }
    return head;
}
```

## 160-相交链表
题目:
编写一个程序，找到两个单链表相交的起始节点。
解题思路:
一开始写了一个逆置链表,然后也能找到,但是解题说改变了链表结构...没通过
后来也没想出来,看到题解恍然大悟(还被撒了一脸狗粮...躲过了520,没躲过leetcode的花样题解...md)
定义两个指针p,q,分别从l1,l2出发,p到达l1链表末尾后,继续从**l2头节点**处继续遍历,q同理,从l1处继续遍历,直到p,q相等找到相交节点(或p,q = NULL)
```c
/*struct ListNode* reverse_list(struct ListNode* p){
    struct ListNode* res = (struct ListNode*)malloc(sizeof(struct ListNode));
    struct ListNode* temp = NULL;
    res->next = NULL;
    while(p->next != NULL){
        temp = p->next;
        p->next = res->next;
        res->next = p;
        p = temp;
        p = p->next;
    }
    return res->next;
}
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    struct ListNode* l1 = reverse_list(headA);
    struct ListNode* l2 = reverse_list(headB);
    struct ListNode* res = NULL;
    while(l1!= NULL && l2!= NULL){
        if(l1->val == l2->val){
            l1 = l1->next;
            l2 = l2->next;
        }
        else{
            res = l1;
            break;
        }
    }
    if(res!= NULL){
        return res;
    }
    else{
        return NULL;
    }
}*/
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    struct ListNode*p=headA;
    struct ListNode*q=headB;
    while(p!=q){
        p=p!=NULL?p->next:headB;
        q=q!=NULL?q->next:headA;
    }
    return p;
}
```

## 1019-链表中的下一个更大节点
题目:示例输入
```
输入：[1,7,5,1,9,2,5,1]
输出：[7,9,9,9,0,5,0,0]
```
解题思路:
这题快给我搞崩溃了...初始化数组为链表的值,然后从右向左进行判断,入栈出栈比较操作(静态栈)
看了题解之后感觉实现起来并不是很难,就是...好绕啊,尤其那个栈给我绕进去了
```c
int* nextLargerNodes(struct ListNode* head, int* returnSize){
    if (head == NULL) return NULL;
    int *ret_arr = (int *)malloc(10240*sizeof(int));
    struct ListNode* p1 = head;
    int i = 0,top = -1;
    while (p1){ //初始化返回数组,赋值,之后进行比较
        ret_arr[i++] = p1->val;
        p1 = p1->next;
    }
    int *stack = (int*)calloc(i, sizeof(int));//申请栈空间
    *returnSize = i--;     //返回num值
    while (i > -1) {
        if (top == -1) { //栈为空
            stack[++top] = ret_arr[i];
            ret_arr[i--] = 0;
        }
        else{
            while (top > -1 && stack[top] <= ret_arr[i]){
                --top;  //找到最近更大的值,并且一直出栈操作
            } 
            if (top == -1) { //没有找到更大值,栈为空,赋值为0
                stack[++top] = ret_arr[i];
                ret_arr[i--] = 0;
            }
            else{  //找到更大值,进栈,并且更改return_array的值
                stack[top+1] = ret_arr[i];
                ret_arr[i--] = stack[top++];
            }
        }
    }
    return ret_arr;
}
```

## malloc与calloc的区别
malloc函数：malloc(size_t size)函数有一个参数，即要分配的内存空间的大小。
calloc函数：calloc(size_t numElements,size_t sizeOfElement)有两个参数，分别为元素的数目和每个元素的大小，这两个参数的乘积就是要分配的内存空间的大小。


