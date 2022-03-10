# 线索二叉树



二叉树的线索化
<!--more-->

首先在线索二叉树中有一个头结点(为了不浪费树中的空指针)
 - 初始化时左指针指向根结点,右指针指向自己
 - 线索化完成后,右指针指向最后一个结点
 - 某序遍历下的第一个结点和最后一个结点指向头结点
 - rtag,ltag = 1时表示指向后继结点

## **1. 二叉树的线索化**
### 不带头结点的中序线索化

```c
void InThreading(BiThrTree t, BiThrTree *pre){      //基于递归中序遍历
  if(p){
    InThreading(t->lchild, pre);
    if(!p -> lchild){
      p->ltag = 1;
      p->lchild = *pre;
    }
    else{
      p -> ltag = 0;
    }
    if(!(*pre) -> rchild){
      (*pre) -> rtag = 1;
      (*pre) -> rchild = p;
    }
    else{
      (*pre) -> rtag = 0;
    }
    InThreading(t->rchild, pre);
  }
}
```
### 带头结点的中序线索化
 - 首先申请一个头结点
 - 初始化头结点,随后调用 InThreading 函数


 ```c
 int InorderThr(BiThrTree *head, BiThrTree t){
   if(!(*head = (BiThrTree)malloc(sizeof(BiThrNodeType))){
     return 0;
   }
   (*head) -> rchild = *head;
   (*head) -> ltag = 0;
   (*head) -> rtag = 1;
   BiThrTree pre;
   if(!t) (*head) -> lchild = *head;      //如果树 t 为空,则左指针也回指*head
   else{
     (*head) -> lchild = t;
     InThreading(t, &pre);
     pre -> rchild = *head;     //最后一个结点指回*head
     pre -> rtag = 1;     //改变最后一个结点的tag
     (*head) -> rchild = pre;     //头指针的 rchild 指向最后一个结点
   }
 }
 ```

 ## **2. 在中序线索二叉树中寻找*序的前驱或后继**
 ### 中序线索树查找任意结点的中序前驱结点
 ```c
 BiThrTree InPreNode(BiThrTree p){
    BiThrTree pre;
    pre = p->lchild;
    if(p->ltag != 1){
        while(pre->rtag == 0){          //如果结点有左子树,则前驱结点应该是左子树的最右端结点
            pre = pre->rchild;
        }
    }
    return pre;
}
 ```
### 中序线索二叉树查找任意结点的中序后继结点
 ```c
 BiThrTree InPostNode(BiThrTree p){
    BiThrTree post = p->rchild;
    if(p->rtag != 1){
        while(post->ltag == 0){         //如果结点有右子树,则后驱结点应该是右子树的最左端结点
            post = post->lchild;
        }
    }
    return post;
}
 ```

 ### 中序线索树查找任意结点先序前驱结点

 - p为非叶子结点:
    1. 如果结点p->rtag = 0,则先序遍历的后继结点即p->rchild

    2. 如果p->rtag = 1,先序后继结点为p->lchild
 - p为叶子节点:
    1. 如果p->lchild = *head,则p->lchild为后继结点,因为先序遍历和中序遍历最后一个结点相同,后继也相同

    2. 如果p->lchild != *head,若p->lchild有右子树,直接p = p->lchild->lchild,若p->lchild有右线索,进入循环 pre = pre->lchild


 ```c
 BiThrNode IPostPreNode(BiThrTree p){
    BiThrNode pre;
    if(p->rtag == 0) pre = pre->rchild;
    else{
        pre = p;
        while(pre->ltag == 1 && pre->lchild != head) pre = pre->lchild;
        pre = pre->lchild
    }
    return pre;
}
 ```

 ### 中序线索树查找任意结点先序后继结点

 - p为非叶子结点:
    1. 如果结点p->ltag = 0,则先序遍历的后继结点即p->lchild

    2. 如果p->ltag = 1,先序后继结点为p->rchild
 - p为叶子节点:
    1. 如果p->rchild = *head,则p->rchild为后继结点,因为先序遍历和中序遍历最后一个结点相同,后继也相同

    2. 如果p->rchild != *head,若p->rchild有右子树,直接p = p->rchild->rchild,若p->rchild有右线索,进入循环 pre = pre->rchild


 ```c
 BiThrNode IPrePostNode(BiThrTree p){
    BiThrNode post;
    if(p->ltag == 0;) post = p->lchild;
    else{
        post = p;
        while(post->rtag == 1 && post->rchild != head) post = post->rchild;
        post = post->rchild;
    }
    return post;
}
 ```

