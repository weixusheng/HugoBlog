# Leetcode Part-26



面试题29-顺时针打印矩阵<br>
96-不同的二叉搜索树
<!--more-->

## 面试题29-顺时针打印矩阵
题目:
输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。
```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```
解题思路:
暴力求解,一圈一圈往里遍历

```c
int* spiralOrder(int** matrix, int matrixSize, int* matrixColSize, int* returnSize){
    if (matrix == NULL || matrixSize == 0) {
        *returnSize = 0;
        return NULL;
    }
    *returnSize = matrixSize * matrixColSize[0];
    int *res = calloc(*returnSize, sizeof(int));
    int i = 0;
    int urow, rcol, drow, lcol, r, c;
    urow = -1;
    lcol = -1;
    drow = matrixSize;
    rcol = matrixColSize[0];
    while (i < *returnSize) {
        //right
        r = urow + 1;
        for (c = lcol + 1; i < *returnSize && c < rcol; c++) {
            res[i] = matrix[r][c];
            i++;
        }
        urow++;
        //down
        c = rcol - 1;
        for (r = urow + 1; i < *returnSize && r < drow; r++) {
            res[i] = matrix[r][c];
            i++;
        }
        rcol--;
        //left
        r = drow - 1;
        for (c = rcol - 1; i < *returnSize && c > lcol; c--) {
            res[i] = matrix[r][c];
            i++;
        }
        drow--;
        //up
        c = lcol + 1;
        for (r = drow - 1; i < *returnSize && r > urow; r--) {
            res[i] = matrix[r][c];
            i++;
        }
        lcol++;
    }
    return res;
}
```

## 96-不同的二叉搜索树
题目:
给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？
```
输入: 3
输出: 5
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```
解题思路:
这道题把我按地上摩擦了,神奇的动态规划...真的神奇[官方思路](https://leetcode-cn.com/problems/unique-binary-search-trees/solution/bu-tong-de-er-cha-sou-suo-shu-by-leetcode/)
```c
int numTrees(int n){
    int i = 0;
    int j = 2;
    int* dpTable = malloc((n+1) * sizeof(int));
    if(!dpTable)
    {
        return 0;
    }
    memset(dpTable,0,(n+1)*sizeof(int));
    if(n == 1)
    {
        return 1;
    }
    dpTable[0] = 1;
    dpTable[1] = 1;
    for(int i = 2; i <= n; i++)
    {
        for(int j = 0; j < i; j++)
        {
            dpTable[i] += dpTable[j] * dpTable[i-j-1];
        }
    }
    int res = dpTable[n];
    free(dpTable);
    return res;
}
```
