# 单链表排序


今天被单链表排序那题卡了大半天,我一直想写冒泡...但是最后给我绕懵了,发现单链表的冒泡也太难写了(哭)
最后我还写了构造单链表和打印单链表的一套函数用来debug...
失败的收获:排序过程中其实并不用去交换节点,交换节点的value完全可以啊
过几天开始系统学查找排序算法
<!--more--> 

# 常见排序算法
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/C-12/3.png)

# 快速排序
## 快排思想
 - 对于一个数组A，选定一个基准元素
 - 遍历剩余元素，并与基准元素进行比较
 - 按比较结果的大小将剩余元素分成两组，一组全部比基准元素大，记为B，另外一组全部基准元素小，记为S
 - 将基准元素的位置挪到比它小的那组元素的最后
 - 分别对B和S重复以上4个步骤，直到所有的元素都已经排序成功
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/C-12/1.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/C-12/5.png)

## 单链表快速排序
但是对于单链表我们没有前驱指针，怎么能使得后面的那个指针往前移动呢？所以这种快排思路行不通滴。
这时需要两个指针p和q，这两个指针**均往next方向移动**，**移动的过程中保持p之前的key都小于选定的key，p和q之间的key都大于选定的key**，那么当q走到末尾的时候便完成了一次支点的寻找。
### 示例图
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/C-12/2.png)

### 代码实现
```c
//指针交换辅助函数
void swap(int *a, int *b)
{
    int t=*a;
    *a=*b;
    *b=t;
}

struct ListNode *partion(struct ListNode *left,struct ListNode *right)
{
    if(left == right || left->next == right)    //如果只有一个元素或者两个元素，则直接返回第一个指针
        return left;
    int pivot = left->val;    //选择头节点作为基准元素
    struct ListNode *p1 = left ,*p2 = left->next; 
    while(p2 != right)
    {   
    //从left开始向后进行一次遍历，大于pivot值时，p1向前走一步，交换p1与p2的值
        if(p2->val < pivot)
        {
            p1 = p1->next;
            swap(&p1->val, &p2->val);
        }
        p2 = p2->next;
    }
    swap(&p1->val, &left->val);
    return p1;
    free(p2); //释放p2指针的内存

}
    
void quick_sort(struct ListNode *left,struct ListNode *right)
{
    if(left == right||left ->next == right)    
        return;
    struct ListNode *mid = partion(left, right);
    quick_sort(left, mid);
    quick_sort(mid->next, right);
}
   
struct ListNode* sortList(struct ListNode* head) 
{
    if(head==NULL||head->next==NULL)    
        return head;
    quick_sort(head, NULL);
    return head;
}
```

# 归并排序
## 归并思想
归并排序基于分治的思想，但是不同的是，分完了以后后面还需要**从底层开始向上合并**，主要思想如下：
 - 遍历链表L，找到链表中间节点将链表分成两部分L1，L2
 - 分别对L1、L2重复进行遍历并分组，直到每个链表近含有一个元素为止
 - 然后调用合并两个有序链表的函数将链表两两合并，直到全部完成为止
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/C-12/4.png)

## 单链表归并算法实现
```c
struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2)
{
    struct ListNode *head = (struct ListNode*)malloc(sizeof(struct ListNode));
    struct ListNode *cur = head;
    
    while(l1 && l2)
    {
        if(l1->val > l2->val)
        {
            cur->next = l2;
            l2 = l2->next;
        }
        else
        {
            cur->next = l1;
            l1 = l1->next;
        }
        cur = cur->next;
    }
    
    cur->next = l1 ? l1:l2;
    return head->next;
}

struct ListNode* sortList(struct ListNode* head)
{
	if(!head || !head->next)
		return head;
	struct ListNode *slow = head, *fast = head, *pre = head;
	while(fast && fast->next)
	{
		pre = slow;
		slow = slow->next;
		fast = fast->next->next;
	}
	pre->next = NULL;
	return mergeTwoLists(sortList(head), sortList(slow));//slow为原链表的中间节点
}
```
