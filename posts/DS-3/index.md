# 算法题 - 小知识点2


今天在学习算法题的基础数据结构操作的时候,发现教材里在构造单向链表时有头结点，而在构造链栈时采用不带头结点的单向链表
在此记录一下两种的差别
<!--more-->

首先对于数据结构的构造
- 单链表-带头结点
```c
typedef struct  snode{
	Elemtype data;
	struct snode *next;
}LNode, *LinkList;
```
初始化操作
```c
int initLinkList(LinkList *l){
	*l = (LinkList)malloc(sizeof(LNode));
	if(*l == NULL){
		printf("申请空间失败");
		return 0；
	}
	(*l) ->next = NULL;  //头结点初始化next指针为空
	return 1;
}
```
- 链栈-不带头结点
```c
typedef struct snode{
	Elemtype data;
	struct node *next;	
}SNode, *LinkStack;
```
初始化操作
```c
int initLinkStack(LinkStack *s){
	*s = (LinkStack)malloc(sizeof(SNode));
	if(*s == NULL){
		printf("申请空间失败");
		return 0;
	}
	*s = NULL;  //不带头结点时直接初始化链栈指针为空
	return 1;
}
```

