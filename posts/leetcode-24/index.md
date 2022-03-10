# Leetcode Part-24


今天6.1!导员问我学院想让我当 优秀毕业生风采展的代表 可不可以,当然可以鸭嘿嘿
(虽然大学本专业成绩辣鸡的很,但是!找到了自己喜欢的行业和目标,不算太亏吧!)
前天gnome天气插件不好使了,没法刷新数据,困扰了好几天,最后在国外gitlab论坛上看到了(还好俺会点英语)果然国外的技术论坛干货多啊,英语是程序员必须的一项技能
废话不多说,今天的力扣暴力求解开始了哈哈哈哈

1431-拥有最多糖果的孩子<br>
437-路径总和 III<br>
234-回文链表<br>
581-最短无序连续子数组<br>
538-把二叉搜索树转换为累加树
<!--more-->

## 1431-拥有最多糖果的孩子
题目:
给你一个数组 candies 和一个整数 extraCandies ，其中 candies[i] 代表第 i 个孩子拥有的糖果数目。
对每一个孩子，检查是否存在一种方案，将额外的 extraCandies 个糖果分配给孩子们之后，此孩子有 最多 的糖果。注意，允许有多个孩子同时拥有 最多 的糖果数目。
```
输入：candies = [2,3,5,1,3], extraCandies = 3
输出：[true,true,true,false,true] 
```
解题思路:
暴力求解不多说,让俺去看看大佬们的操作(啊,方法跟我一样,没事了)
```c
bool* kidsWithCandies(int* candies, int candiesSize, int extraCandies, int* returnSize){
    /*find max*/
    *returnSize = candiesSize;
    int max = 0;
    bool* res = (bool*)malloc(sizeof(bool)*candiesSize);
    memset(res,false,candiesSize);
    int max_loc = 0;
    for(int i=0; i<candiesSize; i++){
        if(candies[i]>max){
            max = candies[i];
            max_loc = i;
        }
    }
    for(int j=0; j<candiesSize; j++){
        if(candies[j]+extraCandies > max){
            res[j] = true;
        }
        else if(candies[j]+extraCandies == max){
            res[max_loc] = true;
            res[j] = true;
        }
    }
    return res;
}
```

## 437-路径总和 III
题目:
给定一个二叉树，它的每个结点都存放着一个整数值。
找出路径和等于给定数值的路径总数。
```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8
      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1
返回 3。和等于 8 的路径有:
1.  5 -> 3
2.  5 -> 2 -> 1
3.  -3 -> 11
```
解题思路:
标准dfs遍历,但是需要考虑起点的位置
```c
int myPathSum(struct TreeNode *root, int sum)
{
    int cnt = 0;
    if (root == NULL) {
        return 0;
    }
    if (root->val == sum) {
        cnt++;
    }
    cnt += myPathSum(root->left, sum - root->val);
    cnt += myPathSum(root->right, sum - root->val);
    return cnt;
}
int pathSum(struct TreeNode* root, int sum){
    int res = 0;
    if (!root) {
        return res;
    }
    /*这个return很奇妙,需要特别学习一下,要考虑起点的位置*/
    return myPathSum(root, sum) + pathSum(root->left, sum) + pathSum(root->right, sum); 
}
```

## 234-回文链表
题目:
请判断一个链表是否为回文链表,回文就是反转以后和以前一样的就是回文结构
```
输入: 1->2->2->1
输出: true
```
解题思路:
本来我想用栈来解决,但是无法确定奇数和偶数的情况,最后发现没法用呀,只好逆置了
这道题有两种思路:
 - 逆置整个链表,然后和原链表进行比较
 - 首先利用双指针(快慢指针),找到中间点,然后将后半段链表进行逆置,与前半段进行比较

双指针大法好!一定要记住!

### 逆置整个链表
```c
bool isPalindrome(struct ListNode* head){
    if(head==NULL)return 1;
    struct ListNode*ans=head;
    struct ListNode*ami=NULL;
    struct ListNode*transfer=NULL;
    while(ans!=NULL){ //生成新的逆序链表
        ami=(struct ListNode*)malloc(sizeof(struct ListNode));
        ami->val=ans->val;
        ami->next=transfer;
        transfer=ami;
        ans=ans->next;
    }
    while(head!=NULL){  //对比两个链表是不是相同
        if(ami->val!=head->val)
        return false;
        head=head->next;
        ami=ami->next;          
    }
    return true;
}
```

### 双指针逆置半个链表
对奇数和偶数的情况要进行考虑
```c
void cut(struct ListNode* head,struct ListNode* cutNode);
struct ListNode* reverse(struct ListNode* head);
bool IsEqual(struct ListNode* l1,struct ListNode* l2);

bool isPalindrome(struct ListNode* head){
    if(head==NULL || head->next==NULL) return true;
    struct ListNode* slow=head;
    struct ListNode* fast=head;
    while(fast!=NULL && fast->next!=NULL)
    {
        fast=fast->next->next;   
        //慢指针走一步，快指针走两步，最后把单链表分为以慢指针分成两部分
        slow=slow->next;
    }
    cut(head,slow);
    if(fast!=NULL) slow=slow->next;    
    /*如果是奇数个结点，那么cut后将slow后移一位再reverse，就将中间的结点扔掉了，但其实不这么做也是可以的，因为在IsEqual函数中while的判断条件里是&&，即使不把中间的点扔掉，它也不会加入判断，结果是一样的*/
    return IsEqual(head,reverse(slow));
}
void cut(struct ListNode* head,struct ListNode* cutNode){ //在slow结点前砍断链表
    while(head->next!=cutNode)
        head=head->next;
    head->next=NULL;
}
struct ListNode* reverse(struct ListNode* head) 
{
    struct ListNode* newHead=NULL;
    while(head!=NULL)
    {
        struct ListNode* nextNode=head->next;
        head->next=newHead;
        newHead=head;
        head=nextNode;
    }
    return newHead;
}
bool IsEqual(struct ListNode* l1,struct ListNode* l2){
    while(l1!=NULL && l2 !=NULL)
    {
        if(l1->val != l2->val) return false;
        l1=l1->next;
        l2=l2->next;
    }
    return true;
}
```

## 581-最短无序连续子数组
题目:
给定一个整数数组，你需要寻找一个连续的子数组，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。
```
输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
```
解题思路:
这道简单题...我尝试用滑动窗口,没做出来,用单调栈,没做出来...
看了题解发现,是方法没找对,枯了

### 先排序后检查
1. 先进行排序，让数组升序；
2. 从前向后，寻找第一个与之前序列不同的元素所在位置begin
3. 从后向前遍历 , ,end
4. 相减返回begin-end+1。如果已经有序就返回0

```c
int compare(const void *a,const void *b){
    return *(int*)a-*(int*)b;
}
int findUnsortedSubarray(int* nums, int numsSize){
    int begin=0;int end=0;
    int *numscpy=(int*)malloc(numsSize*sizeof(int));
    memcpy(numscpy,nums,sizeof(int)*numsSize);
    qsort(numscpy,numsSize,sizeof(int),compare);
    for(int i=0;i<numsSize;i++){
        if(numscpy[i]!=nums[i]){
            begin=i;
            break;
        }
    }
    for(int i=numsSize-1;i>=0;i--){
        if(numscpy[i]!=nums[i]){
            end=i;
            break;
        }
    }
    if(begin==end){
        return 0;
    }
    return end-begin+1;
}
```

### 在逆序内寻找最值
1. 先找到发生逆序的起始元素，这里需要注意的是逆序是指a1,a2,如果a2小于a1，那么a1的下标的就是逆序的起始元素
2. 在找到逆序的头尾之后，再寻找在逆序中的最大最小值。
3. 利用子序列的最值再从两头开始寻找真正的下标。
这里发生了两次寻找下标，需要注意相同元素的判断。

```c
int findUnsortedSubarray(int* nums, int numsSize){
    int posi=0,posj=numsSize-1,i,j,min,max;//游标和定位
        while(posj>posi&&nums[posi]<=nums[posi+1])posi++;//定位下标
        while(posj>posi&&nums[posj]>=nums[posj-1])posj--;
    if(posi==posj)
        return 0;
    min=nums[posi],max=nums[posj];
    for(i=posi;i<=posj;i++)//找到子数组中最大最小值
        {
        if(nums[i]<min)
            min=nums[i];
        if(nums[i]>max)
            max=nums[i];
    }
    posi=0,posj=numsSize-1;
    while(nums[posi]<=min)posi++;
    while(nums[posj]>=max)posj--;
    return posj-posi+1;
}
```

## 538-把二叉搜索树转换为累加树
题目:
给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。
```
输入: 原始二叉搜索树:
              5
            /   \
           2     13
输出: 转换为累加树:
             18
            /   \
          20     13
```
解题思路:中序遍历的改造
### 先右后左的中序遍历
```c
void in_order(struct TreeNode* root);
int g_sum;

struct TreeNode* convertBST(struct TreeNode* root)
{
    g_sum = 0;
    if (root == NULL) {
        return NULL;
    }
    in_order(root);
    return root;
}
void in_order(struct TreeNode* root)
{
    if (root == NULL) {
        return;
    }
    in_order(root->right);
    root->val += g_sum;
    g_sum = root->val;
    in_order(root->left);
}
```

### 中序遍历结果存入数组
```c
void  cBST(struct TreeNode*root, struct TreeNode* arr[],int *count){
    if(root!=NULL){
        cBST(root->left,arr,count);
        arr[*count]=root;(*count)++;
        cBST(root->right,arr,count);
    }
}
struct TreeNode* convertBST(struct TreeNode* root){
    struct TreeNode* arr[20000];
    int count=0;
    cBST(root,arr,&count);
    for(int i=count-2;i>=0;i--){
        arr[i]->val=arr[i]->val+arr[i+1]->val;
    }
    return root;
}
```
