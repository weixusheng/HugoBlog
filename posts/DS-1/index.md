# 算法题 - typedef与struct


最近刷leedcode以及看数据结构的代码，发现一直弄不太清typedef的使用情况，百度之后才明白这其中的原理
<!--more-->

1. typedef的最简单使用
```
typedef long byte_4;
```
给已知数据类型long起个新名字，叫byte_4。
2. typedef与结构结合使用
```
typedef struct tagMyStruct{ 
	int iNum; 
	long lLength; 
} MyStruct;
```
#### 这语句实际上完成两个操作：
1) 定义一个新的结构类型
```
struct tagMyStruct{ 
	int iNum; 
	long lLength; 
};
```
- 分析：tagMyStruct称为“tag”，即“标签”，实际上是一个临时名字，struct 关键字和tagMyStruct一起，构成了这个结构类型，不论是否有typedef，这个结构都存在。
- 我们可以用struct tagMyStruct varName来定义变量，但要注意，使用tagMyStruct varName来定义变量是不对的，因为struct 和tagMyStruct合在一起才能表示一个结构类型。

2) typedef为这个新的结构起了一个名字，叫MyStruct。
```
typedef struct tagMyStruct MyStruct;
```
- 因此，MyStruct实际上相当于struct tagMyStruct，我们可以使用MyStruct varName来定义变量。

3) 规范做法：
```
struct tagNode{ 
	char *pItem; 
	struct tagNode *pNext; 
}; 
typedef struct tagNode *pNode;
```

