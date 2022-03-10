# Leetcode Part-8



328-奇偶链表
<!--more-->

## 328-奇偶链表
题目:给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。
示例
```
输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL
```
解题思路:我还是只想到了暴力法...先建立一个奇数链表,再搞一个偶数链表,然后相连,但是一开始总是超时,后来把 b->next = NULL;就好了,感觉有些离谱,不过以后**注意数据封口和销毁**就好啦
### 暴力法
```c
struct ListNode* oddEvenList(struct ListNode* head){
    if (head == NULL || head->next == NULL){
        return head;
    }
    int count = 0;
    struct ListNode* single_head = (struct ListNode*)malloc(sizeof(struct ListNode)),*a = single_head;
    struct ListNode* nosingke_head = (struct ListNode*)malloc(sizeof(struct ListNode)),*b = nosingke_head;
    while(head){
        if(++count%2){      //single
            a = head = a->next = head;
        }
        else{
            b = head = b->next = head;
            
        }
        head = head->next;
    }
    a->next = nosingke_head->next;
    b->next = NULL;
    a = single_head->next;
    free(single_head);
    free(nosingke_head);
    return single_head->next;
}
```

### 指针跳跃(带头节点)
```c
struct ListNode* oddEvenList(struct ListNode* head){
    if (head == NULL || head->next == NULL) return head;
    struct ListNode *l1 = (struct ListNode *)malloc(sizeof(struct ListNode)), *rear1 = l1;
    struct ListNode *l2 = (struct ListNode *)malloc(sizeof(struct ListNode)), *rear2 = l2;
    while (head && head->next) {
        rear1 = rear1->next = head;
        rear2 = rear2->next = head->next;
        head = head->next->next;
    }
    rear1 = head? rear1->next = head: rear1;
    rear1->next = l2->next;
    rear2->next = NULL;
    rear1 = l1->next;
    free(l1);
    free(l2);
    return rear1;
}
```

### 指针跳跃(不带头节点)
```c
struct ListNode* oddEvenList(struct ListNode* head){
    if(head == NULL){
        return NULL;
    }
    struct ListNode* oddHeah = head; // 保存奇数u偶数链表头部
    struct ListNode* oddNode = oddHeah;
    struct ListNode* evenHeah = head -> next;
    struct ListNode* evenNode = evenHeah;
    while(evenNode != NULL && evenNode -> next != NULL){
        oddNode -> next = oddNode -> next -> next;
        evenNode -> next = evenNode -> next -> next;
        oddNode = oddNode -> next;
        evenNode = evenNode -> next;
    }
    oddNode -> next = evenHeah; // 拼接链表， 循环结束时 oddNode 指向奇数链表最后一个元素
    return oddHeah;
}
```


