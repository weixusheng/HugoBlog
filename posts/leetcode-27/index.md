# Leetcode Part-27


最近感觉数学有点搞不动了...练习册也都做的差不多了
所以时隔4个月的本辣鸡又来刷leetcode啦,但不是天天刷,只是作为放松,娱乐刷
吸取了去年算法题的惨痛教训,准备把线性表-二叉树-图的算法都刷一遍,给专业课再上一层保险
最近越来越发现新换的顶配SurfacePro6简直写代码太舒服啦!2k屏幕让我眼睛舒适,i7+16G的性能释放让我快乐
(原来的Pro5 i5是真的拉跨...想从巨硬这里提高生产力,真是要下血本呀)
<!--more-->

# 24.两两交换链表中的节点
## 菜鸡的代码
今天的每日一题正好是链表的,所以直接开写!
这道题是交换链表结点,其实思路相对简单...就是为什么我写的这么啰嗦...
```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(head == nullptr || head->next == nullptr){   //单独判空多余
            return head;
        }
        else{
            ListNode *p, *q;
            p = head;
            q = p->next;
            bool tag = false;
            ListNode *pre;
            ListNode *shit = (ListNode*)malloc(sizeof(ListNode));
            while(q != nullptr){
                if(tag == false){   //头节点特判多余
                    pre = shit;
                    tag = true;
                }
                pre->next = q;
                p->next = q->next;
                q->next = p;
                pre = p;
                if(p->next != nullptr){ //判断条件可以写在while中
                    p = p->next;
                    q = p->next;
                }
                else{
                    break;
                }
            }
            return shit->next;
        }
    }
};
```
## 递归
我竟然没想到用递归...属实有些不应该
```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head == nullptr || head->next == nullptr) {
            return head;
        }
        ListNode* newHead = head->next;
        head->next = swapPairs(newHead->next);
        newHead->next = head;
        return newHead;
    }
};
```
## 递推
同样是一个算法思想,我写了30行,答案写了10行...问题主要集中在边界判断上...感觉还是不太熟
```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* temp = dummyHead;
        while (temp->next != nullptr && temp->next->next != nullptr) {
            ListNode* node1 = temp->next;
            ListNode* node2 = temp->next->next;
            temp->next = node2;
            node1->next = node2->next;
            node2->next = node1;
            temp = node1;
        }
        return dummyHead->next;
    }
};
```

# 最近公共父节点
二叉树寻找两个节点的最近公共父节点,需要用到递归,这种类型题之前见过很多次
代码主要分为三个判定部分
```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        //当前为空或者为p,q,则直接返回
        if(root == NULL || root == p || root == q){
            return root;
        }
        //递归左右子树
        TreeNode *l = lowestCommonAncestor(root->left, p, q);
        TreeNode *r = lowestCommonAncestor(root->right, p, q);
        //如果其中有一个子树不存在p,q则返回另一个分支
        if(l == NULL){
            return r;
        }
        if(r == NULL){
            return l;
        }
        //找到公共父节点
        return root;
    }
};
```

# 将有序数组转换为二叉搜索树
经典的二叉树构造,左右子树递归
值得注意的是,在leetcode编译器中,这道题malloc会报错...需要用new(以后c++就都用new吧,避免不必要的麻烦)
动态创建对象时，只需指定其数据类型，而不必为该对象命名，**new表达式返回指向该新创建对象的指针**，我们可以通过指针来访问此对象
```c++
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        //利用左右子树递归即可完成
        return make_tree(nums, 0, nums.size()-1);
    }
    TreeNode* make_tree(vector<int> &nums, int low, int high){
        if(low > high){
            return NULL;
        }
        int mid = (low+high)/2;
        TreeNode* p = new TreeNode(nums[mid]);
        //TreeNode *p = (TreeNode*)malloc(sizeof(TreeNode));
        p->val = nums[mid];
        p->left = make_tree(nums, low, mid-1);
        p->right = make_tree(nums, mid+1, high);
        return p;
    }
};
```

# 98.判断二叉搜索树(排序树)
1. 方法1-递归判断 
这道题需要注意的是,如果满足二叉搜索树,需要左子树**所有**元素都小于当前节点,且右子树**所有**元素大于当前节点,因此需要设置一个上下界进行判断
2. 方法2-中序遍历
由于中序遍历二叉搜索树,会得到一个递增序列,因此判断这个递增序列就可以知道是否满足

## 判断上下界
非常巧妙的lower和upper
```c++
class Solution {
public:
    bool helper(TreeNode* root, long long lower, long long upper) {
        if (root == nullptr) {
            return true;
        }
        if (root -> val <= lower || root -> val >= upper) {
            return false;
        }
        return helper(root -> left, lower, root -> val) && helper(root -> right, root -> val, upper);
    }
    bool isValidBST(TreeNode* root) {
        return helper(root, LONG_MIN, LONG_MAX);
    }
};
```

## 中序遍历(非递归)
```c++ 
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> stack;
        //inorder为前驱元素
        long long inorder = (long long)INT_MIN - 1;
        //中序遍历
        while (!stack.empty() || root != nullptr) {
            while (root != nullptr) {
                stack.push(root);
                root = root -> left;
            }
            root = stack.top();
            stack.pop();
            // 如果中序遍历得到的节点的值小于等于前一个 inorder，说明不是二叉搜索树
            if (root -> val <= inorder) {
                return false;
            }
            inorder = root -> val;
            root = root -> right;
        }
        return true;
    }
};
```
## 中序遍历(递归)
```c++
class Solution {
public:
    vector<int> v;
    //中序遍历
    void dfs(TreeNode *t){
        if(!t){
            return;
        }
        dfs(t->left);
        v.push_back(t->val);
        dfs(t->right);
    }

    bool isValidBST(TreeNode* root) {
        if(!root){
            return true;
        }
        dfs(root);
        for(int i=0; i<v.size()-1; i++){
            if(v[i+1] <= v[i]){
                return false;
            }
        }
        return true;
    }
};
```

# 二叉树的后序非递归
这个问题困扰了我很久...其实解决这个问题的关键,就是判断好访问结点的时机
对于后序遍历,**只有左右子节点都访问完成,该节点才能进行访问**,这次务必要记住鸭!
```c++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> data;
        if(root == nullptr){
            return data;
        }
        TreeNode *pre = root;
        stack<TreeNode*> s;
        while(root || !s.empty()){
            while(root){
                s.push(root);
                root = root->left;
            }
            root = s.top();
            s.pop();    //总是先出栈
            if(root->right == nullptr || root->right == pre){   
                //该节点满足后序条件,进行访问(进入队列),并且改变前驱结点
                //root赋值为null,要求再次出栈访问
                data.push_back(root->val);
                pre = root;
                root = nullptr;
            }
            else{   //此时右节点没有访问,需要再进入栈中
                s.push(root);
                root = root->right; //转向右节点访问
            }
        }
        return data;
    }
};
```
