# C语言-qsort函数


今天刷leetcode遇到一个神奇的函数,但是没看懂大佬怎么用的这个函数,查了一下发现大有用处!在此记录
<!--more--> 

排序方法有很多种：选择排序，冒泡排序，归并排序，快速排序等。 因为快排速度很快，所以系统也在库里实现这个算法，便于我们的使用。这就是qsort函数(全称quicksort)。其声明在stdlib.h文件中，是根据二分法写的，其时间复杂度为n*log(n)
# qsort函数
```
包含头文件为 stdlib.h
```
qsort 函数原型：
**void qsort**(void*base, size_t num, size_t width, int(*compare)(const void*,const void*))
各参数：
```
*base 待排序数组首地址
size_t num 数组中待排序元素数量
size_t width 各元素的占用空间大小
compare 指向函数的指针，用于确定排序的顺序，传入的是地址
```
qsort要求提供一个自己定义的比较函数。比较函数使得qsort通用性更好，有了比较函数qsort可以实现对数组、字符串、结构体等结构进行升序或降序排序。下面以compare函数举例

# compare 函数
compare( (void *) & elem1, (void *) & elem2)
```
1. value <0 	elem1将被排在elem2之前
2. value =0 	位置不变
3. value >0 	elem1将被排在elem2之后
```

# 升序or降序
取决于返回值的**正负**
```
// 由小到大排序
int comp(const void *a, const void *b) {
	return *(int*)a-*(int*)b;
}
// 由大到小排序
int comp(const void *a, const void *b) {
	return *(int*)b-*(int*)a;
}
```
# 实例
## 对int型数组排序
```c
int num[100];
int cmp_int(const void* _a , const void* _b)　　//参数格式固定
{
    /*
    int* a = (int*)_a;    //强制类型转换
    int* b = (int*)_b;
    return *a - *b;
    */
    return *(int*)a-*(int*)b;
}
qsort(num,100,sizeof(num[0]),cmp_int); 
```
## 对字符串进行排序
```c
char word[100][10];
int cmp_string(const void* _a , const void* _b)　　//参数格式固定
{
    char* a = (char*)_a;　　//强制类型转换
    char* b = (char*)_b;
    return strcmp(a,b);
}
qsort(word,100,sizeof(word[0]),cmp_string); 
```

