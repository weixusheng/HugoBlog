# Leetcode Part-4


今天母亲节,做了锅包肉和蜜汁鸡排嘿嘿(没想到锅包肉做的还蛮好吃的),晚上家里的仙人球还开花了!今天还第一次通过了leetcode,好开心嗷,做题好像有感觉了!
明天更操作系统,不拖了不拖了...

1047-删除字符串中的所有相邻重复项<br>
617-合并二叉树<br>
226-翻转二叉树<br>
101-对称二叉树<br>
236-二叉树的最近公共祖先<br>
1-两数之和(哈希散列法)
<!--more-->


## 1047-删除字符串中的所有相邻重复项
1. 题目:
给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。
在 S 上反复执行重复项删除操作，直到无法继续删除。
在完成所有重复项删除操作后返回最终的字符串。
2. 解题思路:
我竟然上来先写了一个栈(以后不要这样了...)下次碰到字符串要注意了,字符串本身加一个int top就可以实现栈

```c
char * removeDuplicates(char * s){
    int sl = strlen(s);
    char *stack = (char *)malloc(sizeof(char) * sl); // 申请一个栈 返回也用它
    int top = -1; // 栈顶
    for(int i = 0; i < sl; i++) {
        if(0 > top) {
            // 空的栈 直接加
            stack[++top] = s[i];
        } else {
            if(stack[top] == s[i]) {
                // 栈顶元素与当前元素相同则出栈
                top--;
            } else {
                // 不同则入栈
                stack[++top] = s[i];
            }
        }
    }
    // 封口
    stack[++top] = 0;
    return stack;
}
```

## 617-合并二叉树
1. 题目:
给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。
你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。
2. 解题思路:
基于二叉树的递归遍历,这道题是我自己写出来的!没看题解,感觉现在做题有手感了!!!

```c
struct TreeNode* mergeTrees(struct TreeNode* t1, struct TreeNode* t2){
    if(t1 == NULL){
        return t2;
    }
    if(t2 == NULL){
        return t1;
    }
    if(t1 && t2){
        t1->val = t1->val+t2->val;
    }
    t1->left = mergeTrees(t1->left, t2->left);
    t1->right = mergeTrees(t1->right, t2->right);
    return t1;
}
```

## 226-翻转二叉树
1. 题目:
翻转一棵二叉树
2. 解题思路:
快乐递归二叉树

```c
struct TreeNode* invertTree(struct TreeNode* root){
    if(root == NULL){
        return NULL;
    }
    struct TreeNode* node = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    node = root->right;
    root->right = root->left;
    root->left = node;
    invertTree(root->left);
    invertTree(root->right);
    return root;
}
```

## 101-对称二叉树
1. 题目:
给定一个二叉树，检查它是否是镜像对称的
2. 解题思路:
左右子树的分别遍历,但是遍历路径是镜像路径

```c
//我一开始写错的代码,错在于没有进行镜像遍历
/*bool isSymmetric(struct TreeNode* root){
    struct TreeNode* root_left = root->left;
    struct TreeNode* root_right = root->right;
    if(root_left == NULL && root_right == NULL){
        return true;
    }
    if(root_left->val == root_right->val){
        return true;
    }
    else{
        return false;
    }
    isSymmetric(root->left);
    isSymmetric(root_right);
}*/
static bool Symmetric(struct TreeNode* leftNode, struct TreeNode* rightNode)
{
    if (NULL == leftNode && NULL == rightNode)
    {
        return true;
    }

    if (NULL == leftNode || NULL == rightNode)
    {
        return false;
    }

    return (leftNode->val == rightNode->val) &&           \
            Symmetric(leftNode->right, rightNode->left) && \
            Symmetric(leftNode->left, rightNode->right);
}

bool isSymmetric(struct TreeNode* root){
    return Symmetric(root, root);
}
```

## 236-二叉树的最近公共祖先
1. 题目:寻找二叉树两个节点的最近公共祖先
2. 解题思路:这个题我一开始没想出来...看了题解才想明白,使用dfs即可,豁然开朗,**dfs在判定时是从下向上的顺序进行的**所以即可得到最近的公共结点
### 官方解法
[解法地址](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/solution/er-cha-shu-de-zui-jin-gong-gong-zu-xian-by-leetc-2/)

```c++
/*由于判定条件有些技巧,所以还是看官方解释吧*/
class Solution {
public:
    TreeNode* ans;
    bool dfs(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == nullptr) return false;
        bool lson = dfs(root->left, p, q);
        bool rson = dfs(root->right, p, q);
        if ((lson && rson) || ((root->val == p->val || root->val == q->val) && (lson || rson))) {
            ans = root;
        } 
        return lson || rson || (root->val == p->val || root->val == q->val);
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        dfs(root, p, q);
        return ans;
    }
};
```

### DFS递归判定
[解答地址](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/solution/c-jing-dian-di-gui-si-lu-fei-chang-hao-li-jie-shi-/)
```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == NULL)
            return NULL;
        if(root == p || root == q) 
            return root;  
        TreeNode* left =  lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if(left == NULL)
            return right;
        if(right == NULL)
            return left;      
        if(left && right) // p和q在两侧
            return root;
        return NULL; // 必须有返回值
    }
};
```

## 1-两数之和(哈希散列法)
之前用暴力求解,今天带着好奇来看看哈希表怎么做,果然c的哈希还要自己写,很麻烦,但是看到这个解法很新颖,直接把数组的值和数组下标进行哈希表的key和value
[解体地址](https://leetcode-cn.com/problems/two-sum/solution/cyu-yan-ji-yu-shu-zu-de-san-lie-15xing-dai-ma-8ms-/)

```c
#define MAX_SIZE 2048
int *twoSum(int *nums, int numsSize, int target, int *returnSize)
{
    int i, hash[MAX_SIZE], *res = (int *)malloc(sizeof(int) * 2);
    memset(hash, -1, sizeof(hash));
    for (i = 0; i < numsSize; i++)
    {
        if (hash[(target - nums[i] + MAX_SIZE) % MAX_SIZE] != -1)
        {
            res[0] = hash[(target - nums[i] + MAX_SIZE) % MAX_SIZE];
            res[1] = i;
            *returnSize = 2;
            return res;
        }
        hash[(nums[i] + MAX_SIZE) % MAX_SIZE] = i;  //将数组中的值放入哈希数组中,因为一开始都是-1,所以都在执行该行代码,直到遇到和为目标值的另一个数,才执行if中的结果语句
        //防止负数下标越界，循环散列
    }
    free(hash);
    *returnSize = 0;
    return res;
}
```




