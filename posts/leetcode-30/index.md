# Leetcode Part-30



链表
<!--more-->

# 142. 环形链表 II
查找环形链表中的第一个入环元素
首先对于这种环形链表,判定成环有两种方法
1. 利用set或者hash,来判定遍历链表过程中是否存在相同的元素,如果存在相同元素,就代表成环
2. 利用快慢指针,如果快慢指针相遇则代表成环

这道题我一开始想用快慢指针,但是快慢指针相遇的点是在环中,而不是题中要求的环首元素,后来看了题解,发现如果使用快慢指针的话,还需要一个数学推导,学到啦

## 使用set
这里需要注意的是,set 有count()方法,即计算元素出现
```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        //使用set
        unordered_set<ListNode*> s;
        while(head != NULL){
            if(s.count(head)){
                return head;
            }
            s.insert(head);
            head = head->next;
        }
        return NULL;
    }
};
```

## 双指针
[官方题解](https://leetcode-cn.com/problems/linked-list-cycle-ii/solution/huan-xing-lian-biao-ii-by-leetcode-solution/)
```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head, *fast = head;
        while (fast != nullptr) {
            slow = slow->next;  //慢指针
            if (fast->next == nullptr) {
                return nullptr;
            }
            fast = fast->next->next;    //快指针
            if (fast == slow) { //快慢指针相遇时刻,一个新的指针从head出发
                ListNode *ptr = head;
                while (ptr != slow) {   //新的指针与慢指针相遇,此时为环首
                    ptr = ptr->next;
                    slow = slow->next;
                }
                return ptr;
            }
        }
        return nullptr;
    }
};
```

# 23. 合并K个升序链表
难点在于,k个链表组合成一个链表
这道题方法很多,这里只写上暴力方法,即与每个链表进行挨个合并

## 暴力组合
```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode *a, ListNode *b) {
        if ((!a) || (!b)) return a ? a : b;
        ListNode head, *tail = &head, *aPtr = a, *bPtr = b;
        while (aPtr && bPtr) {
            if (aPtr->val < bPtr->val) {
                tail->next = aPtr; aPtr = aPtr->next;
            } else {
                tail->next = bPtr; bPtr = bPtr->next;
            }
            tail = tail->next;
        }
        tail->next = (aPtr ? aPtr : bPtr);
        return head.next;
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode *ans = nullptr;
        for (size_t i = 0; i < lists.size(); ++i) {
            ans = mergeTwoLists(ans, lists[i]); //依次合并
        }
        return ans;
    }
};
```

## 分治合并
这个分治思想真的很重要鸭!!!很灵性的递归分治
与归并排序有异曲同工之妙
```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode *a, ListNode *b) {
        if ((!a) || (!b)) return a ? a : b;
        ListNode head, *tail = &head, *aPtr = a, *bPtr = b;
        while (aPtr && bPtr) {
            if (aPtr->val < bPtr->val) {
                tail->next = aPtr; aPtr = aPtr->next;
            } else {
                tail->next = bPtr; bPtr = bPtr->next;
            }
            tail = tail->next;
        }
        tail->next = (aPtr ? aPtr : bPtr);
        return head.next;
    }

    ListNode* merge(vector <ListNode*> &lists, int l, int r) {
        if (l == r) return lists[l];
        if (l > r) return nullptr;
        int mid = (l + r) >> 1;
        //左右递归,分治归并
        return mergeTwoLists(merge(lists, l, mid), merge(lists, mid + 1, r));   
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        return merge(lists, 0, lists.size() - 1);
    }
};
```

# 61. 旋转链表
很开心!快慢指针无敌!
## 快慢指针
```c++
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(!head || !head->next){
            return head;
        }
        //暴力
        int num = 0;
        ListNode *p = head;
        while(p){
            ++num;
            p = p->next;
        }
        int shit = k % num;
        ListNode *res = new ListNode();
        ListNode *m = head;
        ListNode *n = head;
        if(shit == 0){
            return head;
        }
        while(shit--){
            m = m->next;
        }
        while(m->next){
            m = m->next;
            n = n->next;
        }
        ListNode *rr = n->next;
        n->next = NULL;
        m->next = head;
        return rr;
    }
};
```
## 题解
只是切法跟我不太一样哈,题解没用到双指针
```c++
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(!head || !head->next || k==0) return head;
        ListNode *pos = head;
        int size = 1;
        while(pos && pos->next)
        {
            pos = pos -> next;
            size ++;
        }
        int move = k % size;
        if(move == 0) return head;
        ListNode *cut = head;
        for(int i=0; i<size-move-1; ++i) cut = cut -> next;
        ListNode *result = cut -> next;
        cut -> next = nullptr;
        pos -> next = head;
        return result;
    }
};
```

# 92. 反转链表 II
链表的部分反转,菜鸡只会暴力...所以用的栈来做逆转,但是后来发现链表会在头节点处出现问题
所以以后在操作链表时
**一定要加上头节点!!!**这是链表算法的精髓
## 栈逆置
```c++
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if(!head || m == n){
            return head;
        }
        stack<ListNode*> s;
        ListNode *first = new ListNode();
        first->next = head;
        ListNode *a, *b, *p = first;
        for(int i=0; i<=n; ++i){
            if(i == m-1){
                a = p;
            }
            if(i == n){
                b = p->next;
            }
            if(i >= m){
                s.push(p);
            }
            p = p->next;
        }
        ListNode *tmp;
        while(!s.empty()){
            tmp = s.top();
            s.pop();
            a->next = tmp;
            a = a->next;
        }
        a->next = b;
        return first->next;
    }
};
```
## 递归
在题解里发现了一个[大佬的操作](https://leetcode-cn.com/problems/reverse-linked-list-ii/solution/bu-bu-chai-jie-ru-he-di-gui-di-fan-zhuan-lian-biao/)
感觉这个对于递归的解释不能更赞了!
(但是这个方法的话,内存占用甚至比我还多,只能说这个题用递归的方法会很精妙)
```c++
class Solution {
public:
    ListNode *successor = NULL; // 后驱节点
    // 反转以 head 为起点的 n 个节点，返回新的头结点
    ListNode *reverseN(ListNode *head, int n) {
        if (n == 1) { 
        // 记录第 n + 1 个节点
            successor = head->next;
            return head;
        }
        // 以 head.next 为起点，需要反转前 n - 1 个节点
        ListNode *last = reverseN(head->next, n - 1);
        head->next->next = head;
        // 让反转之后的 head 节点和后面的节点连起来
        head->next = successor;
        return last;
    }    

    ListNode *reverseBetween(ListNode *head, int m, int n) {
        if (m == 1) {
            return reverseN(head, n);
        }
        // 前进到反转的起点触发 base case
        head->next = reverseBetween(head->next, m - 1, n - 1);
        return head;
    }
};
```
