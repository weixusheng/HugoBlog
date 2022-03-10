# Leetcode Part-21



198-打家劫舍<br>
121-买卖股票的最佳时机<br>
76-最小覆盖子串
<!--more-->

今天论文写了(复制粘贴)了5k,字数到1.5w了,嘿嘿,明天降重,论文越写越有感觉了
## 198-打家劫舍
题目:
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。
```
输入: [1,2,3,1]
输出: 4
解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
偷窃到的最高金额 = 1 + 3 = 4 。
```
解题思路:
偷窃第 k 间房屋，那么就不能偷窃第 k−1 间房屋，偷窃总金额为前 k−2 间房屋的最高总金额与第 kkk 间房屋的金额之和。
不偷窃第 k 间房屋，偷窃总金额为前 k−1 间房屋的最高总金额。
在两个选项中选择偷窃总金额较大的选项，该选项对应的偷窃总金额即为前 k 间房屋能偷窃到的最高总金额。
用 dp[i]表示前 i 间房屋能偷窃到的最高总金额，那么就有如下的状态转移方程：
dp[i]=max⁡(dp[i−2]+nums[i],dp[i−1])
最终的答案即为 dp[n−1]，其中 n 是数组的长度。
我太菜了...动态规划没想到,跟上台阶那道题很像,这方面做的题太少了,没啥感觉
简单一点说,这道题的动态规划就是,由于不能相邻取值,只能当前值和i-2节点,或者当前节点不选择,只选择上一个节点,而每个节点都是如此判断,所以可以进行for-max取值
```c
int rob(int* nums, int numsSize)
{
    if(!numsSize){
        return 0;
    }
    int dp[numsSize][2];
    dp[0][0] = 0;
    dp[0][1] = nums[0];
    for(int i=1;i<numsSize;i++)
    {
        dp[i][1] = nums[i] + dp[i-1][0];
        dp[i][0] = dp[i-1][0]>dp[i-1][1]?dp[i-1][0]:dp[i-1][1];
    }
    return dp[numsSize-1][0]>dp[numsSize-1][1]?dp[numsSize-1][0]:dp[numsSize-1][1];
}
```

## 121-买卖股票的最佳时机
题目:
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。
```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
```
解题思路:
滑动窗口解法
```c
int maxProfit(int* a, int n)
{
	if (a == NULL || n ==0) {
		return 0; // 参数有误 
	}
	int min = a[0]; // 暴力破解 
	int profit = 0; // 默认是0 
	for (int i = 1; i < n; i++) {
		if (min > a[i]) { // 最低价 
			min = a[i];
		}
		int d1 = profit; // 之前卖 
		int d2 = a[i] - min; // 当天卖 
		profit = d1 > d2 ? d1 : d2;
	}	
	return profit;
}
```

## 76-最小覆盖子串
题目:
给你一个字符串 S、一个字符串 T，请在字符串 S 里面找出：包含 T 所有字符的最小子串。
```
输入: S = "ADOBECODEBANC", T = "ABC"
输出: "BANC"
```
解题思路: 滑动窗口!
在滑动窗口类型的问题中都会有两个指针。一个用于「延伸」现有窗口的 rrr 指针，和一个用于「收缩」窗口的 lll 指针。在任意时刻，只有一个指针运动，而另一个保持静止。我们在 sss 上滑动窗口，通过移动 rrr 指针不断扩张窗口。当窗口包含 ttt 全部所需的字符后，如果能收缩，我们就收缩窗口直到得到最小窗口。
如何判断当前的窗口包含所有 ttt 所需的字符呢？我们可以用一个哈希表表示 ttt 中所有的字符以及它们的个数，用一个哈希表动态维护窗口中所有的字符以及它们的个数，如果这个动态表中包含 ttt 的哈希表中的所有字符，并且对应的个数都不小于 ttt 的哈希表中各个字符的个数，那么当前的窗口是「可行」的。
**此题推荐观看**[官方求解视频](https://leetcode-cn.com/problems/minimum-window-substring/solution/zui-xiao-fu-gai-zi-chuan-by-leetcode-solution/)**,良心的25min解体思路**
```c
#define LEN  125
char* minWindow(char* s, char* t)
{
    int hash[LEN] = {0};
    int start=0, end=0, tLen=strlen(t), sLen=strlen(s);
    int minStart=0, minLen=INT_MAX;
    for(int i = 0; t[i]; i++) hash[t[i]]++;	//统计 t 串字母出现的次数
    while(end < sLen)
    {
    	//若 s 串有 t 串的字母，则对应的哈希表减一，tlen 也减一
        if(hash[s[end]]-- > 0){
            tLen--;
        }      //不管啥,减就完事啦,统计是否t中元素都被减掉了
        end++; //窗口前指针继续滑动
        while(tLen == 0) //t完整啦!
        {
            if(end-start < minLen) //更新最小串数的值
                minStart = start, minLen = end-start;
            hash[s[start]]++;//开始缩小窗口
            //若 s[start] 在 t 串中，则 tlen 要++,说明缩小之后，当前窗口不存在有效的子串了
            if(hash[s[start]] > 0){
                tLen++; 
            } 
            start++; //接着移动
        }
    }
    if(minLen != INT_MAX)//若找得到
    {
    	// 写法一：
        // char* t = (char*)malloc(sizeof(char)*(minLen+1));
        // *t = '\0';
        // strncat(t, s+minStart, minLen);
        // return t;
        // 写法二：emmm~ 这个写法会小一丢丢的空间复杂度
        s[minStart+minLen] = '\0';
        printf("%s",s);
        return s+minStart; //很好的返回形式,blog之前说过这种方法
    }
    return "";//那就找不到啦
} 
```
