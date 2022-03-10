# Leetcode Part-20



394-字符串解码<br>
1021-删除最外层的括号<br>
面试题17.12-BiNode<br>
700-二叉搜索树中的搜索
<!--more-->

## 394-字符串解码
题目:
给定一个经过编码的字符串，返回它解码后的字符串。
编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。
```
s = "3[a]2[bc]", 返回 "aaabcbc".
s = "3[a2[c]]", 返回 "accaccacc".
s = "2[abc]3[cd]ef", 返回 "abcabccdcdcdef".
```
解题思路:这道题我想到了栈和递归,但是就是没法解决内存空间扩充的问题,看了题解学到了几个很好的小知识点
这是一道medium level的题,所以逻辑稍微稍微有点复杂
一开始我还想用hash来解,但是对于平行级别的字符无法解决...
### 递归解法
```c
#define STR_LEN 5000
char * decodeStringCore(char *s, char **e) {
    char *ret = (char*)malloc(sizeof(char) * STR_LEN);
    char *buf, *end = NULL;
    int count = 0, idx = 0;
    while(*s != '\0') {
        if (isalpha(*s)) {
            ret[idx++] = *s;
        } else if (isdigit(*s)) {
            count = 10 * count + *s - '0';
        } else if (*s == '[') {
            buf = decodeStringCore(s + 1, &end);
            while (count) {
                strcpy(ret + idx, buf);
                idx += strlen(buf);
                count--;
            }
            s = end;
        } else if (*s == ']') {
            *e = s;
            ret[idx] = '\0';
            return ret;
        }
        s++;
    }
    ret[idx] = '\0';
    return ret;
}
char * decodeString(char * s){
    char *end = NULL;
    return decodeStringCore(s, &end);
}
```

### 常规栈
```c
char *decodeString(char *s)
{
	if (s == NULL) {
		return NULL;
	}
	char *a = strdup(s);
	while (true) {			
		int len = strlen(a);
		int left = 0, right = len;
		int i = len - 1, num = 0, w = 1;
		for (; i >= 0; i--) {
			if (a[i] == ']') {
				right = i;
			} else if(a[i] == '[') {
				left = i;
			} else if (a[i] >= '0' && a[i] <= '9') {
				do { // 组合数字 
					num += w * (a[i] - '0');
					w *= 10;
					i--; 
				} while (i >= 0 && (a[i] >= '0' && a[i] <= '9'));
				break;
			}		
		}
		if (num == 0) { //没有[]了 
			return a;
		} else {
			int slen = (right - left - 1);
			int count = (i + 1) + (len - right - 1) + num * slen + 1;
			char *t = (char*)calloc(count, sizeof(char));
			if (i + 1 > 0) { // 左 
				memcpy(t, a, i + 1);
			}
			for (int k = 0; k < num; k++) { // 中
				memcpy(t + (i + 1) + k * slen, a + left + 1, slen);
			}
			if (len - right - 1 > 0) {
				memcpy(t + (i + 1) + num * slen, a + right + 1, len - right - 1);
			}
			free(a);
			a = t; 		
		}
	}
}
```
## 知识点总结
### char转int
一个老生常谈的问题(但我忘记了...)
```c
char* s = "12345"
int count = *s - '0';
```
'0'在ASCII表中是第48个，'1'是第49个……以此类推.
int i='0' 所以int a = ch-'0'; 也就是 int a = ch-48; 也就是把权 char 转成了 int。

### char指针的移动
在大佬的题解中,我发现对于对于char值的访问,有两种途径(例如 char* s)
- 第一种: 访问*s,访问下一个字符时 s++;
- 第二种: 访问s[i],访问下一个字符时 i++;
一个是地址相加访问,一个是地址下标访问

### strup函数
```c
char *a = strdup(s);
```
将字符串s复制到一个新的地址空间,并且指针返回到a
说明：strdup不是标准的c函数。**strdup()在内部调用了malloc()为变量分配内存，不需要使用返回的字符串时，需要用free()释放相应的内存空间，否则会造成内存泄漏。**

### strdup和strcpy的对比
- strdup不是标准的c函数，strcpy是标准的c函数，使用时注意场合。
- strdup**可以直接把要复制的内容复制给没有初始化的指针**，因为它会自动分配空间给目的指针，**strcpy的目的指针一定是已经分配内存的指针**。
- strdup用完要free()函数释放内存，否则内存泄露 。
- 使用**strcpy必须事先确定src大小**，可以先strlen判断src的大小，之后为dest申请空间，之后再strcpy就不会有问题了。

## 1021-删除最外层的括号
```
题目:
输入："(()())(())"
输出："()()()"
解释：
输入字符串为 "(()())(())"，原语化分解得到 "(()())" + "(())"，
删除每个部分中的最外层括号后得到 "()()" + "()" = "()()()"。
```
解题思路:
不想一次一次移动,但最后也没想出来时间复杂度O(n)的方法
看了一眼题解...这记数法,大佬真强啊,我咋就没想到呢
```c
char * removeOuterParentheses(char * S){
    int cnt = 0,k = 0;
    for(int i = 0;i < strlen(S);i++){
        if(S[i] == '('){
            if(cnt != 0) S[k++] = S[i];
            cnt ++;
        }
        if(S[i] == ')'){
            cnt--;
            if(cnt != 0) S[k++] = S[i];
        }
    }
    S[k] = '\0';
    return S;
}
```
### register int
还有一种方法,新建一个空间,但是用到了**register int**性能提升了很多
register int中的register 表示使用cpu内部寄存器（寄存器是中央处理器内的组成部分。寄存器是有限存贮容量的高速存贮部件）的变量
而平时的int是把变量放在内存中，存到寄存器中可以加快变量的读写速度
```c
char * removeOuterParentheses(char * S){
    int count = 0;
    char *out = (char*)malloc(strlen(S) + 1);
    register int j = 0, len = strlen(S);
    for (int i = 0; i < len; i++){
        if (S[i] == '('){
            if(count++ != 0){
                out[j++] = S[i];
            }
        } else {
            if (--count != 0){
                out[j++] = S[i];
            }
        }
    }
    out[j] = 0;
    return out;
}
```

## 面试题17.12-BiNode
题目:
二叉树数据结构TreeNode可用来表示单向链表（其中left置空，right为下一个链表节点）。实现一个方法，把二叉搜索树转换为单向链表，要求值的顺序保持不变，转换操作应是原址的，也就是在原始的二叉搜索树上直接修改。
返回转换后的单向链表的头节点。
```
输入： [4,2,5,1,3,null,6,0]
输出： [0,null,1,null,2,null,3,null,4,null,5,null,6]
解题思路:
快乐递归,但是一开始我没有把函数分开,但是一提交就报错,但是报错数据测试却能通过
所以以后涉及全局变量的函数,应该分开写
```
### 中序遍历-由左向右
```c
struct TreeNode* pre = NULL;
int flag = 0;
struct TreeNode* res = NULL;

void dfs(struct TreeNode* root) {
    if(root == NULL) {
        return;
    }
    dfs(root->left);
    root->left = NULL;
    if(flag == 0) {
        pre = root;
        flag = 1;
        res = pre;
    } else {
        pre->right = root;
        pre = pre->right;
    }
    dfs(root->right);
    return;
}
struct TreeNode* convertBiNode(struct TreeNode* root){
    pre = NULL;
    res = NULL;
    flag = 0;
    if(root == NULL) {
        return NULL;
    }
    dfs(root);
    return res;
}
```

### 中序遍历-由右向左
这个又右向左是我没想到的!感觉很棒,不用判断头节点了!
```c
struct TreeNode *pre;
void inorder(struct TreeNode *root,struct TreeNode **r){
    if(!root) return ;
    inorder(root->right,r);
    root->right=pre;
    if(pre) pre->left=0;
    pre=root; *r=root;
    inorder(root->left,r); 
}
struct TreeNode* convertBiNode(struct TreeNode* root){
    pre=0;
    inorder(root,&root);
    return root;
```

## 700-二叉搜索树中的搜索
题目:
给定二叉搜索树（BST）的根节点和一个值。 你需要在BST中找到节点值等于给定值的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 NULL。
给定二叉搜索树:

        4
       / \                     2
      2   7   和值:2 ->返回    /  \
     / \                     1   3
    1   3
解题思路:
我就想到了一个递归
### 传统递归
```c
struct TreeNode* searchBST(struct TreeNode* root, int val)
{
    if (root == NULL || val == root->val) return root;
    return val < root->val ? searchBST(root->left, val) : searchBST(root->right, val);
}
```

### 非递归
```c
struct TreeNode* searchBST(struct TreeNode* root, int val)
{
    if (root == NULL || val == root->val) return root;
    while(root != NULL && root->val != val){
        root = root->val > val ? root->left:root->right;
    }
    return root;
}
```

