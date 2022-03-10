# Leetcode Part-13



739-每日温度<br>
53-最大子序和<br>
104-二叉树的最大深度<br>
malloc的使用时机 & 指针数组
<!--more-->

## 739-每日温度
题目: 
根据每日 气温 列表，请重新生成一个列表，对应位置的输出是需要再等待多久温度才会升高超过该日的天数。如果之后都不会升高，请在该位置用 0 来代替。
```
例如，给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。
```
解题思路:
本来想用今天刚学到的那个算法,单向栈来解,然后光调试程序就花了一晚上,最后跑出来结果发现...方法不适用(哭)
因为单向栈在比较过程中pop了,然而这道题不能pop!要算路径上节点的数量
看了题解之后突然发现个妙招,核心思想也是单向栈,不过解决pop丢失数量的方法是,在节点本身加上index,保存节点此时对应的数量
### 我的错误代码
```c
/* 错误代码,虽然错了,但是我一晚上的努力,好歹跑起来了!贴上!
int* dailyTemperatures(int* T, int TSize, int* returnSize){
    int i = TSize-1;
    *returnSize = TSize;
    int count;
    //int* res_array = (int*)malloc(sizeof(int)*1024); //init return_list
    int* stack = (int*)malloc(sizeof(int)*i);
    int top = -1;
    while (i > -1) {
        if (top == -1) { //栈为空
            stack[++top] = T[i];
            T[i--] = 0;
        }
        else{
            count = 0;
            while (top > -1 && stack[top] <= T[i]){
                --top;  //找到最近更大的值,并且一直出栈操作
                ++count;
            } 
            if (top == -1) { //没有找到更大值,栈为空,赋值为0
                stack[++top] = T[i];
                T[i--] = 0;
            }else{  //找到更大值,进栈,并且更改return_array的值
                stack[++top] = T[i];
                T[i--] = ++count;
            }
        }
    }
    return T;
}
*/
```

### 大佬的正确代码
```c
struct Temp
{
    int index;
    int temp;  
};

typedef struct Temp Temp;
int* dailyTemperatures(int* T, int TSize, int* returnSize){
    int *ans = (int *)malloc(sizeof(T[0])*TSize);
    Temp *stack = (Temp *)malloc(TSize*sizeof(Temp));
    int i,top = -1;
    int n = 0;
    *returnSize = TSize;
    for(i=0;i<TSize;i++)    ans[i] = 0;
    i=0;
    while(i< TSize)
    {   
        if(top == -1 || T[i] <= stack[top].temp)
        {
            top++;
            stack[top].index = i;
            stack[top].temp = T[i];
            i++;
        }
        else if(top >= 0 && T[i] > stack[top].temp)
        {   
            top++;
            for(;top >= 0 && T[i] > stack[top-1].temp ; )
            {
                ans[stack[top-1].index] = i - stack[top-1].index ;
                top --;
                if(top == 0)    break;
            }
            stack[top].index = i;
            stack[top].temp = T[i];
            //n = 0;
            i++;
        }
    }
    return ans;
}
```

## 53-最大子序和
题目:
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
```
输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```
解题思想:
简简单单,动态规划.
```c
int maxSubArray(int* nums, int numsSize){
    int max = nums[0];
    int temp = 0;
    for(int i=0; i< numsSize; i++){
        if(temp<0){
            temp = nums[i];
        }
        else{
            temp += nums[i];
        }
        max = max < temp ? temp : max;
    }
    return max;
}
```

## 104-二叉树的最大深度
题目:
给定一个二叉树，找出其最大深度。
二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
解题思路:
### 快乐递归
```c
int maxDepth(struct TreeNode* root){
    if(root == NULL){
        return 0;
    }
    int left = maxDepth(root->left);
    int right = maxDepth(root->right);
    return (left > right? left:right)+1;
}
```

### 层序遍历
### 大佬的方法
```c
struct TreeNode *g_queue[MAX_NODE_NUM];
int TreeDepthBfs(struct TreeNode* root)
{
    int head = 0;
    int tail = 0;
    struct TreeNode *node = root;
    int deep = 0;
    int curtail;

    if (node == NULL) {
        return 0;
    }
    g_queue[tail++] = root;
    while (head < tail && tail < MAX_NODE_NUM) {
        deep++;
        curtail = tail;
        // 逐层遍历
        for (int i = head; i < curtail; i++) {
            node = g_queue[i];
            if (node->left != NULL && tail < MAX_NODE_NUM) {
                g_queue[tail++] = node->left;
            }
            if (node->right != NULL && tail < MAX_NODE_NUM) {
                g_queue[tail++] = node->right;
            }
        }
        head = curtail;
    }
    return deep;
}
```

### 我的方法
我是保存了每层的元素数量,然后动头指针,这样感觉更清晰一点
```c
int maxDepth(struct TreeNode* root){
    if(root == NULL){
        return 0;
    }
    struct TreeNode* a[10240];
    int front = 0, rear = 0, num = 1, deep = 0;
    int temp;
    struct TreeNode* node = NULL;
    a[rear++] = root;
    //printf("%d\n",rear);
    while(rear > front){
        temp = num;
        //printf("当前层有%d个元素\n",temp);
        num = 0;
        for(int i=0; i<temp; i++){
            node = a[front++];
            if(node!=NULL && node->left!= NULL){
                num++;
                a[rear++] = node->left; 
            }
            if(node!=NULL && node->right!= NULL){
                num++;
                a[rear++] = node->right;
            }
            //printf("front:%d\n",front);
            //printf("rear:%d\n",rear);
        }
        ++deep;
        //printf("第%d层\n",deep);
    }
    return deep;
}
```

## malloc的使用时机 & 指针数组
上一题层序遍历的时候,一开始想整一个指针数组,就malloc了一个,但是一直报错,查了才知道其实对于指针数组的初始化,直接声明就好
```c
struct TreeNode* [MAX];
```
### 指向数组的指针和存放指针的数组
指向数组的指针：
```c
char (*array)[5];含义是一个指向存放5个字符的数组的指针
```
存放指针的数组：
```c
char *array[5];含义是一个数组中存放了5个指向字符型数据的指针
```
一个是字符的数组  一个是字符型的数据

### malloc()

如果临时需要一块内存，这块内存用来存储n个int的变量。就需要使用malloc为pMax分配一块内存。可以这样做：
```
int* pMax;
pMax = malloc(sizeof(int) * n);
```
这里malloc返回一个指向这块内存的首地址并将其赋给了int型指针变量pMax.
这时pMax已经可以使用了。我们需要对它进行初始化。这个可以使用memset函数
```c
memset(pMax, 0 , sizeof(int) * n);
```
现在就可以像数组一样操作这块int型的内存了。
```c
pMax[0] = iMax;
pMax[1] = iMax + 1;
pMax[2] = pMax[0];
```
**malloc的返回值是一个指针，指向一段可用内存的起始地址**

### malloc()和calloc()
当我们每次使用完malloc（）函数的时候都必须将指针赋予一个空指针
相对于malloc()函数，**calloc()函数就不需要赋予NULL，这是因为在每次调用完calloc()函数的时候系统会自动将原先的指针赋予一个空指针，即归于“0”**。
calloc()函数的原型是void *calloc（count，sizeof（类型名称））；比如：p=（char*）calloc（4，sizeof（char））；我们为p分配了指向char型指针的“4”个空间。
