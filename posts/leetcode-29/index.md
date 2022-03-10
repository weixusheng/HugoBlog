# Leetcode Part-29



链表 & 树
<!--more-->

# 19. 删除链表的倒数第N个节点
一开始就想到了快慢指针,想试一试不加头节点,最后发现头节点总是处理不好,后来看提交记录发现几个月前用c做的,没加头节点,但是存储了前驱结点
## cpp 头节点
```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        //链表为了操作,应该直接增加头节点
        ListNode *h = new ListNode();
        h->next = head;
        ListNode *p = h, *q = h;
        while(n--){
            q = q->next;
        }
        while(q->next){
            p = p->next;
            q = q->next;
        }
        p->next = p->next->next;
        ListNode *res = h->next;
        delete(h);  //释放创建的临时头节点,是一个好的习惯
        return res;
    }
};
```

## c 不加头节点
```c++
struct ListNode* removeNthFromEnd(struct ListNode* head, int n){
    struct ListNode*p=head,*q=head,*r;
    int i;
    for(i=0;i<n;i++){
        p=p->next;
    }
    if(p==NULL){    //特判,当前需要删除的是头节点
        head=head->next;
        free(q);
    }
    else{
        while(p){
            r=q;    //保存前驱结点
            p=p->next;
            q=q->next;
        }
        r->next=q->next;
        free(q);
    }
    return head;
}
```

# 199. 二叉树的右视图
```c++
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        //层序遍历,记录最后一个节点的值
        vector<int> res;
        if(root == NULL){
            return res;
        }
        TreeNode *tmp;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int len = q.size();
            for(int i=0; i<len; i++){
                tmp = q.front();
                if(tmp->left){
                    q.push(tmp->left);
                }
                if(tmp->right){
                    q.push(tmp->right);
                }
                q.pop();
                if(i == len-1){
                    res.push_back(tmp->val);
                }
            }
        }
        return res;
    }
};
```

# 113. 路径总和 II
没想到测试数据还能是负值的节点...没想到鸭!
然后把判定改在了叶子节点,就过了! 总耗时7分钟...这么简单的题做的还是有点慢鸭
```c++
class Solution {
public:
    vector<vector<int> > res;
    vector<int> v;
    int shit;
    int ssum;

    void findshit(TreeNode *t, vector<int> v, int shit){
        shit += t->val;
        v.push_back(t->val);
        if(!t->left && !t->right){
            if(shit == ssum){
                res.push_back(v);
                return;
            }
        }
        else{
            if(t->left){
                findshit(t->left, v, shit);
            }
            if(t->right){
                findshit(t->right, v, shit);
            }
        }
        v.pop_back();
        return;
    }

    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        if(!root){
            return res;
        }
        ssum = sum;
        findshit(root, v, 0);
        return res;
    }
};
```

# 103. 二叉树的锯齿形层次遍历
快乐层序遍历,增加一个奇偶层判定
```c++
class Solution {
public:
    vector<vector<int> > res;
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        //快乐层序遍历
        queue<TreeNode*> q;
        TreeNode *tmp;
        if(!root){
            return res;
        }
        int level = 1;
        q.push(root);
        while(!q.empty()){
            vector<int> v;
            int len = q.size();
            for(int i=0; i<len; i++){
                tmp = q.front();
                v.push_back(tmp->val);
                q.pop();
                if(tmp->left){
                    q.push(tmp->left);
                }
                if(tmp->right){
                    q.push(tmp->right);
                }
            }
            if(level %2 == 1){
                res.push_back(v);
            }
            else{
                reverse(v.begin(), v.end());
                res.push_back(v);
            }
            level++;
        }
        return res;
    }
};
```

# 236. 二叉树的最近公共祖先
```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(!root || root == p || root == q){
            return root;
        }
        TreeNode *l = lowestCommonAncestor(root->left, p, q);
        TreeNode *r = lowestCommonAncestor(root->right, p, q);
        if(l == NULL){  //左子树中没有,返回右子树
            return r;
        }
        if(r == NULL){  //右子树中没有,返回左子树
            return l;
        }
        if(r && l){ //左右子树中都存在,返回当前节点
            return root;
        }
        return NULL;
    }
};
```
