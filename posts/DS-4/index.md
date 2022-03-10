# 二叉树的非递归遍历


今天算是更加深刻地学习了一下二叉树的非递归遍历,学习期间们摸鱼去看了两集<伟大的卫国战争>不禁感叹纳粹德国的勇猛和苏联人的顽强,最开始闪电战的顺利进行以为可以四个月攻占苏联,就像德国将军所说,这样的战争局势放在任何一个别的国家都是注定的结局,敌人只会溃不成军,但是苏联军队却是如此的顽强。这么一看二战中苏联和中国的情况很是相似，不过中国似乎表现的更有些...说不上来的迟钝吧
又扯了这么多，好想写一篇最近感受的专题哈哈哈，回到主题
之前学习树的非递归总是有些迷糊，先序和中序还好，后序压根在脑袋中像一锅粥一样。
<!--more-->

非递归的方法，我自己总结了算是四种方法
1. 常规压栈
2. 标记上一个结点
3. 任务点分析方法
4. 路径规划法
## 1. 常规压栈
这种方法适合于前序和中序，结构很简单。
```c
先序遍历举例
void tree_preorder(BiTree t){
    BiTNode p = t;
    initStack(S);
    while(!SqStackEmpty(S) || p){
        if(p){
            push(S,p);
            visit(p);   //前进到最左侧节点，先序中途一直访问
            p = p->lchild;
        }
        else{
            pop(S,p);   //当左侧无节点时，转向右节点
            p = p->rchild;
        }
    }
}
```
但是此方法对于后序遍历不适用，因为没办法去做合理的回溯操作
## 2. 标记上一个节点
这个方法适用于后序遍历,也算是一个简单方法,不需要数组额外空间或者改变结点的构造,只需要创建一个结点元素
后序遍历中最大的问题就是解决回溯,这个结点的存在就可以知道上级结点是否应该执行回溯操作
```c
void tree_postorder(BiTree t){
    BiTNode p = t;
    BiTNode lastnode = NULL;
    initStack(S);
    while(!SqStackEmpty(S) || p){
        while(p){ 		//直接到达最左侧结点
            push(S,p);
            p = p->lchild;
        }
        pop(S,p);		//先出栈然后判断是否需要回溯
        if(p->rchild == NULL || p->rchild == lastnode){		//右子树完成访问,不需要回溯
            visit(p);
            lastnode = p;
        }
        else{			//右子树需要访问,此时结点重新进栈,等待回溯
            p = p->rchild;
            while(p){
                push(S,p);
                p = p->lchild;
            }
        }
    }
}
```
## 3. 任务点分析
这个方法是在教材上看到的，感觉领会方法之后似乎对二叉树有了一点新的认识，很巧妙
对于在遍历过程中的每个结点来说，都有两个任务点，第一个就是遍历子节点，第二个是访问（自己），那么就可以将结点新增一个变量task
```c
typedef struct{
    BiTree ptr;
    int task;   //task = 1表示遍历，task = 0表示访问
}Elemdata;
```
适当的，在栈的定义中Elemtype也要改为Elemdata
这样在遍历过程中，只需要改变task的值，来确定结点的状态
```c
//以后序遍历来举例
void task_postorder(BiTree t){
    initStack(S);		//创建栈
    Elemdata e;			//初始化结点
    e.ptr = t;
    e.task = 1;
    BiTree p; 			//初始化操作指针
    if(t) push(S,e); 	//进栈
    while(!SqStackEmpty(S)){
        push(S,e);
        if(e.task == 0){		//task = 0访问
            visit(e.ptr);
        }
        else{
            p = e.ptr;		//后序遍历先进入右结点
            e.ptr = p->rchild;
            e.task = 1;		//初始化右结点进栈元素
            if(e.ptr){
                push(S,e);
            }
            e.ptr = p->lchild;
            e.task = 1;		//初始化左结点进栈严肃
            if(e.ptr){
                push(S,e);
            }
            e.ptr = p;
            e.task = 0;		//最后改变当前元素的task,下次循环访问操作
            push(S,e);
        }
    }
}
```
## 4.路径分析方法
这种方法需要额外构造一个数组,来存放结点的访问状态,此时的状态不再是上一种方法的任务点,结点有null,L,R三种路径,null代表结点没有访问,R代表右子树访问完成,L代表左子树访问完成
```c
BiTree Goleft(BiTree t, SqStack *s, char c[]){  //左子树一直进栈
    if(!t) return NULL;
    while(t){
        push(s,t);
        c[s->top] = 'L';		//第一次访问改变为"L"
        if(t->lchild == NULL) break;
        t = t->lchild;
    }
    return t;                   //返回最左侧结点
}
void NR_postorder(BiTree t){
    initStack(S);
    char lrtag[MAX];            //创建标记数组
    BiTree p;
    t = Goleft(t,S,lrtag);      //左子树进栈,p为最左侧结点
    while(t){
        lrtag[S.top] = 'R';     //第二次访问,改变为"R",紧接着访问"rchild"
        if(t->rchild){
            t = Goleft(t->rchild,S,lrtag);
        }
        else{
            while(!SqStackEmpty(S) && lrtag[S.top] == 'R'){
                pop(S,p);
                visit(p);
            }
        }
        if(!SqStackEmpty(S)) gettop(S,t);
        else{
            t = NULL;
        }
    }
}
```
以上为总结的四种方法,下一期总结线索二叉树,嘿嘿,快乐数据结构

