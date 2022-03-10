# Leetcode Part-11



83-删除排序链表中的重复元素<br>
257-二叉树的所有路径
<!--more-->

## 83-删除排序链表中的重复元素
题目: 
给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。
示例:
```
输入: 1->1->2
输出: 1->2
```
解题思路:我以为50%通过率是啥题呢,做题的时候一直想着是不是有啥坑啊...多虑了,打扰了
```c
struct ListNode* deleteDuplicates(struct ListNode* head){
    if(head == NULL){
        return NULL;
    }
    struct ListNode* p = NULL;
    p = head;
    while(p->next != NULL){
        if(p->next->val == p->val){
            struct ListNode* q = p->next;
            p->next = q->next;
            free(q);
        }
        else{
            p = p->next;
        }
    }
    return head;
}
```

## 257-二叉树的所有路径
题目:
给定一个二叉树，返回所有从根节点到叶子节点的路径。
```
输出: ["1->2->5", "1->3"]
解释: 所有根节点到叶子节点的路径为: 1->2->5, 1->3
```
解题思路:
静态数组+递归遍历,记录当前经过元素的数量(num),然后在数组中在下标(num)处开始添加新的元素
知道遇到叶子节点,打印当前数组
```c
#define NUM 100

void dfs(char **path, char *temp, struct TreeNode *root, int cnt, int *size)
{
    if (root == NULL) {
        return;
    }
    temp[cnt++] = root->val;        //递归到当前元素的数量cnt
    if (root->left == NULL && root->right == NULL) {
        int len = 0;
        for (int i = 0; i < cnt - 1; i++) {
            len += sprintf(&path[*size][len], "%d->", temp[i]);           
        }
        sprintf(&path[*size][len], "%d", temp[cnt - 1]);   
        *size += 1;
        return;
    }
    dfs(path, temp, root->left, cnt, size);
    dfs(path, temp, root->right, cnt, size);
    return;
}

char ** binaryTreePaths(struct TreeNode* root, int* returnSize){
    char **path = (char **)malloc(NUM * sizeof(char *));
    for (int i = 0; i < NUM; i++) {
        path[i] = (char *)malloc(NUM * sizeof(char));
    }
    char temp[NUM]; //用来保存临时变量
    int cnt = 0;
    int size = 0;
    dfs(path, temp, root, cnt, &size);
    *returnSize = size;
    return path;
}
```

### sprintf函数
srpintf()函数的功能很强大：效率比一些字符串操作函数要高；而且更具灵活性；**可以将想要的结果输出到指定的字符串中**
头文件：stdio.h
函数功能：**格式化字符串，将格式化的数据写入字符串中**。
函数原型：int sprintf(char *buffer, const char *format, [argument]...)
参数：
1. buffer：是char类型的指针，指向写入的字符串指针；
2. format：格式化字符串，即在程序中想要的格式；
3. argument：可选参数，可以为任意类型的数据；
函数返回值：buffer指向的**字符串的长度**；

