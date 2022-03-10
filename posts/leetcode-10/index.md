# Leetcode Part-10


今天忙了一天的论文,晚上开会老师还表扬我辣,拿我的目录做模板!
晚上刷一会快乐leetcode
1441-用栈操作构建数组
<!--more-->

## 1441-用栈操作构建数组
给你一个目标数组 target 和一个整数 n,每次迭代，需要从 list = {1,2,3..., n} 中依序读取一个数字。
请使用下述操作来构建目标数组 target ：
```
Push：从 list 中读取一个新元素， 并将其推入数组中。
Pop：删除数组中的最后一个元素。
如果目标数组构建完成，就停止读取更多元素。
```
实例:
```
输入：target = [2,3,4], n = 4
输出：["Push","Pop","Push","Push","Push"]
```
解题思路: 用数组下标解决...就是走着走着有点懵了,一直超内存,最后才发现是最后那步判断res_len没有-1...行8,以后注意了
```c
char ** buildArray(int* target, int targetSize, int n, int* returnSize){
    int res_len = target[targetSize-1]+(target[targetSize-1]-targetSize);
    char** res = (char**)malloc(sizeof(char*)*res_len);
    for(int j=0; j<res_len; j++){
        res[j] = (char*)malloc(sizeof(char)*5);
    }
    int count = 0;
    int step1 = 0;
    for(int i=0; i<res_len; i++){
        if(target[step1] != i+1){
            res[count] = "Push";
            res[++count] = "Pop";
            ++count;
        }
        else{
            res[count] = "Push";
            ++count;
            step1++;
        }
        if(count > res_len-1){
            break;
        }
    }
    *returnSize = res_len;
    return res;
}
```
