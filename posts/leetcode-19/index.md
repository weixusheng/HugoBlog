# Leetcode Part-19


今天降了一天的论文重复率,才降了10%多...枯了,感觉语文水平有了很大的提高,明天继续改
晚上来刷会快乐leetcode吧
974-和可被 K 整除的子数组
<!--more-->

## 974-和可被 K 整除的子数组
题目:
给定一个整数数组 A，返回其中元素之和可被 K 整除的（连续、非空）子数组的数目。
```
输入：A = [4,5,0,-2,-3,1], K = 5
输出：7
解释：
有 7 个子数组满足其元素之和可被 K = 5 整除：
[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]
```
解题思路:
连续子数组和，要用前缀和
前缀和：前n项和
sum[i] % K == sum[j] % K，说明 i 和 j 之间的子数组和是可以被K整除的
负数求余，是负数，要将其转化为正数，方法就是 (sum % K + K) % K
map数组，下标为余数，即 (sum % K + K) % K，值为count，记录前缀和对K取余的个数
```c
int subarraysDivByK(int* A, int ASize, int K){
    int hash[K];
    memset(hash,0,sizeof(hash));
    for(int i=1;i<ASize;i++)//前缀和
    {
        A[i]=A[i-1]+A[i]; 
    } 
    for(int i=0;i<ASize;i++)//余数为键的哈希表
    {
       hash[(A[i]%K+K)%K]++;//消除负数
    }
    int sum=0;
    for(int i=0;i<K;i++)//统计相同余数满足要求的数目
    {
        sum+=hash[i]*(hash[i]-1)/2;
    }
    return sum+hash[0];//加上单个满足余数为零的数目
}
```
因为昨天是睡觉前做的题,所以有点囫囵吞枣,今天又细品了品这道题,感觉还挺有东西的,首先需要明白的是,若两个前缀和的余数**相同**,那么这两个前缀之间的元素的余数就=0了,所以构建一个hash保存前缀和的余数,然后n = x*(x-1)/2 计算相同前缀和的组合次数,最后再加上单个余数为0的情况