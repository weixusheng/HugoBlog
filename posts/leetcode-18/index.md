# Leetcode Part-18


这两天整论文,写读书笔记,刷题有点慢,刷到100题去看"算法笔记"
287-寻找重复数<br>
27-二叉树的镜像<br>
897-递增顺序查找树
<!--more-->

## 287-寻找重复数
题目:
给定一个包含 n + 1 个整数的数组 nums，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。
```
输入: [1,3,4,2,2]
输出: 2
```
解题思路:
用c自带的快排排序,再查,暴力求解
### 暴力求解
```c
int comp(const void *a, const void *b) {
	return *(int*)a-*(int*)b;
}
int findDuplicate(int* nums, int numsSize){
    qsort(nums, numsSize, sizeof(int), comp);
    int pre = nums[0];
    for(int i=1; i<numsSize; i++){
        if(pre != nums[i]){
            pre = nums[i];
        }
        else{
            break;
        }
    }
    return pre;
}
```

### 快慢指针
这道题用双指针寻找环我是没想到的...算是一种思路的开拓吧(虽然下次也不知道怎么用)
```c
int findDuplicate(int* nums, int numsSize){
    int fast, slow;
    slow = 0;
    fast = 0;
    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    }while(slow != fast);
    fast = 0;
    do {
        slow = nums[slow];
        fast = nums[fast];
    }while(slow != fast);
    return slow;
}
```

## 27-二叉树的镜像
题目:
请完成一个函数，输入一个二叉树，该函数输出它的镜像。
```
     4                    4     
   /   \                /   \
  2     7              7     2
 / \   / \            / \   / \
1   3 6   9          9   6 3   1
```
解题思路:
快乐递归,简简单单
```c
struct TreeNode* mirrorTree(struct TreeNode* root){
    if(root != NULL){
        struct TreeNode* temp = NULL;
        temp = root->left;
        root->left = root->right;
        root->right = temp;
    }
    else{
        return;
    }
    mirrorTree(root->left);
    mirrorTree(root->right);
    return root;
}
```

## 897-递增顺序查找树
题目:
给你一个树，请你 按中序遍历 重新排列树，使树中最左边的结点现在是树的根，并且每个结点没有左子结点，只有一个右子结点。
```
       5         1
      / \          \
    3    6           2 
   / \    \    =>     \
  2   4    8            3
 /        / \             \
1        7   9              4
```
解题思路:
本题考察中序遍历的实现,是一道好题!核心思路是:保存上级节点pre,进行连接
>收获: 递归很占用时间,非递归性能是很好的,以后多写非递归
### 递归构造新树
```c
void iBST(struct TreeNode* root, struct TreeNode** newRoot, struct TreeNode** head)
{
	struct TreeNode *leaf = NULL;
	if (root == NULL) {
		return;
	}
	iBST(root->left, newRoot, head);
	leaf = (struct TreeNode*)calloc(sizeof(struct TreeNode), 1);
	if (leaf == NULL) {
		return;
	}
	leaf->val = root->val;
	if (*newRoot == NULL) {
		*head = leaf;
	} else {
		(*newRoot)->right = leaf;
	}
	*newRoot = leaf;
	iBST(root->right, newRoot, head);
	return;
}
struct TreeNode* increasingBST(struct TreeNode* root){
	struct TreeNode* head = NULL;
	struct TreeNode* newRoot = NULL;
	iBST(root, &newRoot, &head);
	return head;
}
```

### 递归修改原树
```c
struct TreeNode* newRoot;
void Find(struct TreeNode* root)
{
    if (root == NULL) {
        return;
    }
    Find(root->left);
    newRoot->right = root;
    newRoot->left = NULL;
    newRoot = newRoot->right;
    Find(root->right);
    return;
}
struct TreeNode* increasingBST(struct TreeNode* root){
    struct TreeNode* ans = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    ans->val = 0;
    ans->left = NULL;
    ans->right = NULL;      //newroot 保存上级结点
    newRoot = ans;
    Find(root);
    return ans->right;
}
```

### 非递归修改原树
```c
#define max 1000
struct TreeNode* increasingBST(struct TreeNode* root){
    struct TreeNode *stack[max],*t,*pre=NULL,*new_root=NULL;
    int top=-1;
    t=root;
    while(t||top>=0){
        while(t)
        {
            stack[++top]=t;
            t=t->left;            
        }
        if(top>=0){
            t=stack[top--];
            //确定新的根结点
            if(!new_root)
                new_root=t;
            else{
                //前一点在序列中的位置已经确定 并且已经出栈了 所以可以修改left 和right 指针  而不会影响后续遍历
                pre->right=t;
                pre->left=NULL;
            }
            pre=t;          //保存上级结点
            t=t->right;
        }
    }
    //修改最后一个节点的left指针
    pre->left=NULL;
    return new_root;    
}
```


