# 链式存储中传入参数的类型


今天本菜鸡开始学离散数学和CMU的213深入操作系统，虽然刚开始学，但收获很大，这几天还看了很多大佬的演讲，他们看世界的角度真的很不一样，高度更高，想得更深，思维更广，羡慕大佬
今天敲基本数据结构的时候，发现一个之前没注意的知识点
<!--more-->

例如链队列的操作
```c++
//定义一个链队列
typedef struct{
	Elemdata data;
	struct qnode *next;
}QNode; *QueueLink;

typedef struct{
	QueueLink rear;
	QueueLink front;
}QLink;
```
由此得到初始化链队列的函数
```c++
int initQLink(QLink *q){
	q->rear = q->front = (QueueLink)malloc(sizeof(Qnode));
	if(q->front == NULL) return 0;
	q->front->next = NULL;
	return 1;
}
```
可以看到在函数初始化时参数是 *q 指针变量
所以对于函数的使用是这样
```c++
QLink q;
initQLink(&q);
```
可以看到使用函数的时候传入的是q的地址
(本菜鸡一直以为函数中的参数是&q)
```c++
*&q = q
```
还有一点需要注意,链队列中,头指针front->next指向头结点(出队的第一个节点) rear指向入队的结点
```c++
q->front->next		//访问头结点
```
今日菜的抠脚

