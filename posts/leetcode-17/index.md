# Leetcode Part-17



20-有效的括号<br>
70-爬楼梯<br>
141-环形链表<br>
14-最长公共前缀
<!--more-->

## 20-有效的括号
题目:
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
```
输入: "([)]"
输出: false
```
解题思路:
暴力栈 + switch

### 暴力求解
没啥技术含量...
```
执行用时 :0 ms, 在所有 C 提交中击败了100.00% 的用户
内存消耗 :5.4 MB, 在所有 C 提交中击败了100.00%的用户
```
```c
bool isValid(char * s){
    int len = strlen(s);
    char* stack_list = (char*)malloc(sizeof(char)*len);
    int top = -1;
    int count = 0;
    for(int i=0; i<len; i++){
        switch(s[i]){
            case '(':
                count++;
                stack_list[++top] = '(';
                break;
            case '[':
                count++;
                stack_list[++top] = '[';
                break;
            case '{':
                count++;
                stack_list[++top] = '{';
                break;
            case ')':
                if(top != -1 && stack_list[top] == '('){
                    count--;
                    top--;
                    break;
                }
                else{
                    return false;
                }
            case ']':
                if(top != -1 && stack_list[top] == '['){
                    count--;
                    top--;
                    break;
                }
                else{
                    return false;
                }
            case '}':
                if(top != -1 && stack_list[top] == '{'){
                    count--;
                    top--;
                    break;
                }
                else{
                    return false;
                }
        }
    }
    if(count!= 0){
        return false;
    }
    return true;
}
```

### 简洁版
大佬的题解,简单优雅,让人羡慕
```c
bool isValid(char * s){
    if (s == NULL || s[0] == '\0') return true;
    char *stack = (char*)malloc(strlen(s)+1); int top =0;
    for (int i = 0; s[i]; ++i) {
        if (s[i] == '(' || s[i] == '[' || s[i] == '{') stack[top++] = s[i];
        else {
            if ((--top) < 0)                      return false;//先减减，让top指向栈顶元素
            if (s[i] == ')' && stack[top] != '(') return false;
            if (s[i] == ']' && stack[top] != '[') return false;
            if (s[i] == '}' && stack[top] != '{') return false;
        }
    }
    return (!top);  //防止'('这种类似情况
}
```

## 70-爬楼梯
题目:
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
解题思路:
这道题我没想出来...看了题解才知道是动态规划
本问题其实常规解法可以分成多个子问题，爬第n阶楼梯的方法数量，等于 2 部分之和
```
爬上 n−1n-1n−1 阶楼梯的方法数量。因为再爬1阶就能到第n阶
爬上 n−2n-2n−2 阶楼梯的方法数量，因为再爬2阶就能到第n阶
```
所以我们得到公式 dp[n]=dp[n−1]+dp[n−2]
同时需要初始化 dp[0]=1 和 dp[1]=1
时间复杂度：O(n)
一道神奇的题,长见识了!
### 递推
**从下到上**进行递推
```c
int climbStairs(int n){
    // f(n) = f(n-1) + f(n-2)
    if ( n < 0 ) return 0;
    if (n <=2) return n;
    int fn1 = 1, fn2 = 2;
    int fn3;
    for (int i = 2 ;i < n; i++){
        fn3 = fn2 + fn1;
        fn1 = fn2;
        fn2 = fn3;
    }
    return fn3;
}
```

### 递归
**从上到下**进行递归
```c
int _climb(int n, int *arr)
{
    if (arr[n] != 0 ) return arr[n];
    arr[n] = _climb(n-1, arr) + _climb(n-2, arr);
    return arr[n];
}

int climbStairs(int n){
    //终止情况
    if ( n <  0 ) return 0;
    if ( n <= 2) return n;
    int *arr = (int*)calloc(n+1, sizeof(int));
    arr[1] = 1;
    arr[2] = 2;
    return _climb(n , arr);

}
```

## 141-环形链表
题目:
给定一个链表，判断链表中是否有环。
为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。
```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```
解题思路:
这道题跟之前撒狗粮的那道题都差不多,那道题是两个链表的相交点,这道题是判断环
快乐双指针就完事了
```c
bool hasCycle(struct ListNode *head) {
    struct ListNode *fast_p=head,*slow_p=head;
    while(fast_p!=0 && fast_p->next!=0){
        slow_p=slow_p->next;
        fast_p=fast_p->next->next;      //注意这里的越界,需要在while中加上 p->next!=0 的额外判断
        if(slow_p==fast_p) return 1;
    }
    return 0;
}
```

## 14-最长公共前缀
题目:
编写一个函数来查找字符串数组中的最长公共前缀。
如果不存在公共前缀，返回空字符串 ""。
```
输入: ["flower","flow","flight"]
输出: "fl"
```
解题思路:
暴力法求解,取第一个字符串为初始结果,然后与第二个进行比较,确定结尾,之后依次遍历确定结尾
```
执行用时 :4 ms, 在所有 C 提交中击败了61.58% 的用户
内存消耗 :5.6 MB, 在所有 C 提交中击败了100.00%的用户
```
```c
char * longestCommonPrefix(char ** strs, int strsSize){
    if(strsSize == 0){
        return "";
    }
    int j;
    char* temp;
    char* res = strs[0];
    for(int i=1; i<strsSize; i++){
        j=0;
        temp = strs[i];
        while(res[j] != '\0' && temp[j] != '\0'){
            if(res[j] == temp[j]){
                j++;
            }
            else{
                break;
            }
        }
        res[j] = '\0';
    }
    return res;
}
```



