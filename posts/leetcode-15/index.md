# Leetcode Part-15



783-二叉搜索树节点最小距离<br>
169-多数元素<br>
283-移动零<br>
448-找到所有数组中消失的数字
<!--more-->

## 783-二叉搜索树节点最小距离
题目:
给定一个二叉搜索树的根节点 root，返回树中任意两节点的差的最小值。
解题思路:
**二叉搜索树,中序遍历得到递增队列**,最小值存在于相邻节点的差
跑的时候不知道为啥,本地测试跑示例没问题,提交测试用的同样的示例,但是结果不对,迷惑...
```c
#define MAX 20000

void inorderTrace(struct TreeNode* root, int* arr, int* count)
{
    if (root == NULL) {
        return;
    }
    inorderTrace(root->left, arr, count);
    arr[(*count)++] = root->val;
    inorderTrace(root->right, arr, count);
    return;
}

int minDiffInBST(struct TreeNode* root)
{
    int arr[MAX] = {0};
    int count = 0;
    int value;
    inorderTrace(root, arr, &count); // 中序遍历后的结果，最小差值必定在相邻元素间
    int min = arr[count - 1] - arr[0];
    for (int i = 0; i < count - 1; i++) {
        value = arr[i + 1] - arr[i];
        min = min<value? min:value; 
    }
    return min;
}
```

## 169-多数元素
题目:
给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。
解题思路:
这道题我想用hash来解决,甚至感觉自己还挺机智的,没想到系统给我传这种东西...
```
[-2147483648,0,0]
```
哇,我真的吐了...行吧
### 错误的hash
```c
int majorityElement(int* nums, int numsSize){
    int* a = (int*)malloc(sizeof(int)*102400);
    memset(a, 0, 102400);
    if(numsSize == 1){
        return nums[0];
    }
    for(int i=0; i<numsSize; i++){
        int loc = nums[i];
        ++a[loc+1];
        if(a[loc+1] > numsSize/2){
            return loc;
        }
    }
    return 0;
}
```

### 摩尔投票法
摩尔投票法，通过一个计数变量s，来观察哪一个数最后不是0即为majority;相同加，不相同减，变为0后换下一个。
```c
int majorityElement(int* nums, int numsSize){
    int key = nums[0];
    int count = 0;
    for (size_t i = 0; i < numsSize; i++)
    {
        if(nums[i] == key)
            count++;
        else
            count--;
        
        if(count <= 0)
        {
            key = nums[i+1];
        }
    }
    return key;
}
```

### 排序取中
如果将数组 nums 中的所有元素按照单调递增或单调递减的顺序排序，那么下标为n/2的元素（下标从 0 开始）一定是众数。
```c
int cmpfunc (const void * a, const void * b)
{
   return ( *(int*)a - *(int*)b );
}
int majorityElement(int *nums, int numsSize) {
    qsort(nums, numsSize, sizeof(nums[0]), cmpfunc);
    return nums[numsSize / 2];
}
```

## 283-移动零
题目:
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。
```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```
解题思路:
暴力算法惨遭滑铁卢哈哈哈,空气中弥漫着尴尬的气息
```
执行用时 :32 ms, 在所有 C 提交中击败了15.59% 的用户
内存消耗 :7.2 MB, 在所有 C 提交中击败了100.00%的用户
```
### 暴力算法
```c
void moveZeroes(int* nums, int numsSize){
    for(int i=0; i<numsSize; i++){
        if(nums[i] == 0){
            for(int j=i; j<numsSize-1; j++){
                nums[j] = nums[j+1];
            }
            nums[numsSize-1] = 0;
            numsSize--;
            if(nums[i] == 0){   //判定下一个元素如果还是0,则从当前重新计算
                --i;
            }
        }
    }
}
```

### 一次for循环
一层for循环，直接进行非零数值移动，后尾补零
大佬大佬,学到了!是我把移动搞复杂了
```c
//solution-1
void moveZeroes(int* nums, int numsSize){
    int m = 0;
    for (int i = 0; i < numsSize; i++) {
        if (nums[i] == 0) {
            m++;
        } else if (m > 0) {
            nums[i - m] = nums[i];
            nums[i] = 0;
        }
    }
}
//solution-2
void moveZeroes(int *nums, int numsSize) {
    int zeroCount = 0;
    for (int i = 0; i < numsSize; i++){
        if (nums[i] != 0){
            nums[zeroCount++] = nums[i];
        }
    }
    while (zeroCount < numsSize){
        nums[zeroCount++] = 0;
    } 
}
```

### 快慢指针
双指针i，j，i为快指针，j为慢指针。i遇到零继续移动，遇到非零元素时，则与j所指的零交换位置。
双指针创造奇迹...膜拜大佬
```c
void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}
/* moveZeroes: 将非零元素移动到零的左边 */
void moveZeroes(int* nums, int numsSize)
{
    int i, j;   /* i为快指针，指向非零元素；j为慢指针，指向零 */
    for (int i = 0, j = 0; i < numsSize; ++i)
        if (nums[i] != 0)   /* 遇到非零元素，与零交换。交换完毕，j后移一位 */
            swap(&nums[i], &nums[j++]);
}
```

## 448-找到所有数组中消失的数字
题目:
给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。
找到所有在 [1, n] 范围之间没有出现在数组中的数字。
```
输入:
[4,3,2,7,8,2,3,1]
输出:
[5,6]
```
解题思路:
暴力双指针,由于未知缺少元素的数量,所以只能先申请一个大的空间
```
执行用时 :148 ms, 在所有 C 提交中击败了40% 的用户
内存消耗 :18.9 MB, 在所有 C 提交中击败了100.00%的用户
```

### 暴力求解
```c
int cmp_int(const void *a, const void *b) {
	return *(int*)a-*(int*)b;
}
int* findDisappearedNumbers(int* nums, int numsSize, int* returnSize){
    qsort(nums,numsSize,sizeof(nums[0]),cmp_int); 
    int* res = (int*)malloc(sizeof(int)*numsSize);
    memset(res, 0, sizeof(int) * (numsSize));
    int j = 0,i = 0,k = 0;
    for(i=0; i< numsSize; i++){
        if(nums[i] == j+1){
            j++; 
        }
        else{
            if(nums[i]>j+1){
                for(int h=j+1; h<nums[i]; h++){
                    res[k++] = h;
                }
                j = nums[i];
            }
        }
    }
    while(j<numsSize){
        res[k++] = j+1;
        j++;
    }
    /*今天发现,并需要重建一个空间,之前memset初始化为0就可以了
    int* res2 = (int*)malloc(sizeof(int)*(k));
    for(int w=0; w< k; w++){
        res2[w] = res[w];
    }*/
    *returnSize = k;
    return res1;
}
```

### hash
快乐哈希,果然哈希还是神奇
```c
int* findDisappearedNumbers(int* nums, int numsSize, int* returnSize){
    int *hash = (int *)malloc(sizeof(int) * (numsSize + 1));
    memset(hash, 0, sizeof(int) * (numsSize + 1));
    for (int i = 0; i < numsSize; i ++) {
        hash[nums[i]]++;
    }
    int cnt = 0;
    for (int i = 1; i < numsSize + 1; i++) {
        if (hash[i] == 0) {
            hash[cnt++] = i;
        }
    }
    *returnSize = cnt;
    return hash;
}
```






