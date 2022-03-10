# Leetcode Part-16



114-二叉树展开为链表<br>
856-括号的分数
<!--more-->

## 114-二叉树展开为链表
题目:
给定一个二叉树，原地将它展开为一个单链表。
```
    1      
   / \
  2   5
 / \   \
3   4   6
```
将其展开为：
```
1
 \
  2
   \
    3
     \....
```
解题思路:这道题的题解让我耳目一新,感觉二叉树操作也没那么难啊...(但是就是想不到)
所以不管怎么动,核心思想还是树的遍历
### 递归
```c
void flatten(struct TreeNode* root)
{
	if (root == NULL) {
		return;
	}
	flatten(root->left);
	flatten(root->right);
	if (root->left != NULL) {
		struct TreeNode* lr = root->left;
		while (lr->right != NULL) {
			lr = lr->right;
		}
		lr->right = root->right;
		root->right = root->left;
		root->left = NULL;
	}
}
```

### 迭代
```c
void flatten(struct TreeNode* root){
    while (root) {
        if (root->left) {
            struct TreeNode* lr = root->left;
            while (lr->right) { // 找左子树的最右节点
                lr = lr->right;
            }
            lr->right = root->right;
            root->right = root->left; 
            root->left = NULL;
        }
        root = root->right;
    }
}
```

## 856-括号的分数
题目:
给定一个平衡括号字符串 S，按下述规则计算该字符串的分数：
```
() 得 1 分。
AB 得 A + B 分，其中 A 和 B 是平衡括号字符串。
(A) 得 2 * A 分，其中 A 是平衡括号字符串。
```
示例
```
输入： "(()(()))"
输出： 6
```
解题思路:分治递归是个神奇的东西...return就很巧妙
### 分治递归
```c
int CalScore(char *S, int len)
{
    int i;
    int j = 1;
    if (len == 2) {
        return 1;
    }
    for (i = 1; i < len; i++) {
        if (S[i] == '(') {
            j++;
        } else {
            j--;
        }
        if (j == 0) {
            break;
        }
    }
    if (i == len - 1) {
        return CalScore(S + 1, len - 2) * 2;
    } else {
        return CalScore(S, i + 1) + CalScore(S + i + 1, len - i - 1);
    }
}
```

### 从内到外递归
```c 
int GetScore(char **S)
{
    int score = 0;
    while (**S != '\0') {
        if (**S == '(') {
            *S += 1;
            score += GetScore(S);
        } else {
            *S += 1;
            score *= 2;
            return (score == 0) ? 1 : score;
        }
    }
    return score;
}
int scoreOfParentheses(char *S)
{
    return GetScore(&S);
}
```


