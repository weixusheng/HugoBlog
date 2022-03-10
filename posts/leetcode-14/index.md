# Leetcode Part-14



19-删除链表的倒数第N个节点<br>
3-无重复字符的最长子串<br>
19-删除链表的倒数第N个节点
<!--more-->

## 105-从前序与中序遍历序列构造二叉树
题目:
根据一棵树的前序遍历与中序遍历构造二叉树。
```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```
解题思路:
在中序序列里寻找中间节点(先序遍历第一个节点),然后快乐递归
```c
struct TreeNode* buildTree(int* preorder, int preorderSize, int* inorder, int inorderSize){
if (preorderSize <= 0 || inorderSize <= 0) {
        return NULL;
    }
    struct TreeNode* p = (struct TreeNode *)malloc(sizeof(struct TreeNode));
    p->val = *preorder;
    p->left = NULL;
    p->right = NULL;
    int pos = 0;
    for (pos = 0; pos < inorderSize; pos++) {
        if (inorder[pos] == *preorder){     //*preorder = preorder[0]
            break;
        }  
    }
    // left
    p->left = buildTree(preorder + 1, pos, inorder, pos);       //左子树递归
    // right
    p->right = buildTree(preorder + pos + 1, inorderSize - pos - 1, inorder + pos + 1, inorderSize - pos - 1);      //右子树递归
    return p;
}
```

## 3-无重复字符的最长子串
题目:给定一个字符串，请你找出其中不含有重复字符的最长子串的长度。
```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```
解题思路:暴力求解,每次重构字符串...为什么我只会暴力求解
### 暴力求解
```c
int lengthOfLongestSubstring(char * s){
    int max = 1,count = 0;
    char* temp = (char*)malloc(sizeof(char)* 1024);
    int i = 0,j = 0;
    if(*s!= '\0'){
        count++;
        i = 1;
        temp[0] = s[0];
    }
    else{
        return 0;
    }
    while(s[i] != '\0'){
        for(j=0; j< count; j++){
            if(temp[j] == s[i]){
                count = count-j-1; //剩余的数量
                for(int k=0; k<count; k++){
                    temp[k] = temp[j+1+k];    //重建字符串
                }
                break;
            }
        }
        ++count;
        temp[count-1] = s[i];
        max = max > count? max:count;
        i++;
    }
    return max;
}
```

### 滑动窗口
i:窗口的第一个字符位置
j:窗口之外的第一个字符
k:用于遍历窗口
思路：当遇到窗口中的字符s[k]与窗口外的字符s[j]相等时，计算此时的长度并将窗口的首位置移到k处
如果s[j]字符与窗口内的字符都不相等，再次计算长度即可
```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int max = 0;
        int i = 0;
        int size = s.size();
        for(int j = 0; j < size; j++)
        {
            for(int k = i; k < j; k++)
            {
                if(s[k] == s[j])
                {
                    max = max > (j-i) ? max : j-i;
                    i = k+1;
                }
            }
            max = max > (j-i+1) ? max : j-i+1;
        }
        return max;
    }
};
```

### 滑动窗口结合hash
这是真的大佬,这个题解让我顿悟到了哈希的nb!!!
哈希果然是空间换时间的绝佳秒法
真是"妙蛙种子吃着妙脆角回到了米奇妙妙屋,妙到家了!"
```c
int lengthOfLongestSubstring(char * s){
    int i, j = 0, count = 0, max = 0, index[128] = {0}, start = 0;
    for(i=0;s[i]!='\0';i++)     
    {
        if(index[s[i]]>start)   //index用来储存出现重复字符时
        {                       //子串起始下标应移动到的地方
            count = i-start;
            if(count>max)
            {
                max = count;
            }
            start = index[s[i]];
        }
        index[s[i]] = i+1;
    }
    count = i-start;
    return count>max?count:max;
}
```

## 19-删除链表的倒数第N个节点
题目:
给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
```
给定一个链表: 1->2->3->4->5, 和 n = 2.
当删除了倒数第二个节点后，链表变为 1->2->3->5.
```
解题思路:
暴力求解:逆置链表->删除->逆置
我就不明白,我这暴力求解还能执行速度击败60%,内存击败100%...迷惑

然鹅这道题的精髓是!**快慢指针!!!**双指针总会产生奇迹,这道题也不例外哈哈哈
### 暴力求解
```c
struct ListNode* reverse_list(struct ListNode* head){
    struct ListNode* node = (struct ListNode*)malloc(sizeof(struct ListNode));
    node->next = NULL;
    struct ListNode* p = head;
    struct ListNode* temp = NULL;
    while(p){
        temp = p->next;
        p->next = node->next;
        node->next = p;
        p = temp;
    }
    return node->next;
}

struct ListNode* removeNthFromEnd(struct ListNode* head, int n){
    if(head == NULL){
        return NULL;
    }
    head = reverse_list(head);
    struct ListNode* p = head;
    if(n==1){
        head = head->next;
    }
    else{
        for(int i=0; i<n-2; i++){
            p = p->next;
        }
        struct ListNode* nex = p->next;
        p->next = nex->next;
    }
    return reverse_list(head);
}
```

### 快慢指针
神奇的快慢指针!看完这个题解感觉自己甚至变聪明了一点点!!!
```
struct ListNode* removeNthFromEnd(struct ListNode* head, int n){
    struct ListNode*p=head,*q=head,*r;
    int i;
    for(i=0;i<n;i++) p=p->next;
    if(p==NULL){
        head=head->next;
        free(q);
    }
    else{
        while(p){
            r=q;
            p=p->next;
            q=q->next;
        }
        r->next=q->next;
        free(q);
    }
    return head;
}
```

