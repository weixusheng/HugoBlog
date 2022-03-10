# Leetcode Part-6



136-只出现一次的数字
<!--more-->

## 136-只出现一次的数字
题目:
给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
解题思路: 使用数组建立哈希表,线性时间复杂度->初始化哈希表,赋值哈希表,查找哈希表
```c
int singleNumber(int* nums, int numsSize){
    if(numsSize == 1)
    return nums[0];

    int i = 0;
    int max = nums[0];
    int min = nums[0];

    for(i = 0; i<numsSize; i++) //找到最大最小的元素,作为哈希表的开头和结尾
    {
        max = max > nums[i]? max:nums[i];
        min = min < nums[i]? min:nums[i];
    }
    max = max-min+1;
    int a[max];     //建立哈希表
    memset(a,0,sizeof(int)*(max));  //初始化哈希表为0
    for(i = 0; i<numsSize; i++)
    {
        a[nums[i]-min] += 1;    //将nums中元素的值作为a中的下标,并且将其+1
    }
    for(i = 0; i < numsSize; i++)
    {
        if(a[nums[i]-min] == 1) //寻找只为1的元素,该元素为出现一次的元素
        {
            return nums[i];
        }
    }
    return 0;
}
```
