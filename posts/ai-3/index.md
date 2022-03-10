# AI-A* Algorithm


Berkeley CS-188
<!--more-->

# A算法
A*(A-Star)算法是一种静态路网中求解最短路径最有效的直接搜索方法
A算法公式表示为：f(n)=g(n)+h(n)其中
 - f(n) 是从初始点经由节点n到目标点的估价函
 - g(n) 是在状态空间中从初始节点到n节点的实际代价
 - h(n) 是从n到目标节点**最佳路径的估计代价**

启发信息给得越多（估价函数值越大），则A算法需要搜索处理的状态数就越少，效率就越高。但并不是估计值越大越好，有时估价函数值太大会使A算法不一定搜索到最优解。(局部最优解)

# A*算法
A* 算法公式：f*(n) = g* (n) + h* (n)
 - g* (n)是从初始结点到n结点的最短路径代价
 - h* (n)是从n结点到目的结点的**最佳路径代价**

当我们要求估价函数f(n)中的**h(n)都小于等于h*(n)**即： h(n) <= h*(n)时 A搜索算法就成为A*搜索算法，所得路径为最优路径。

# [大佬的讲解](https://blog.csdn.net/hitwhylz/article/details/23089415)

# 实例
问题分析：从节点A开始搜索至#节点，该节点对应的h* (n)即为该城市到目标城市的直线距离（即最佳路径代价），从A点到#节点走过的实际距离（即最短路径代价）则为g* (n)。
>每次搜索节点查找最小的g* (n) + h* (n)

![](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/AI/3/1.png)

```c++
#include<stdio.h>
#include<stdio.h>
#include<stdlib.h>
#include<string>
int city_num=0;//城市数目
int g[100][100];//存储城市间实际距离的邻接矩阵

int closeNum=0;//close表大小
int track[100];//行驶路径
int tra_num=0;//途径城市的数目
typedef struct node//代表城市的结构体
  { int data; //城市名
    int num;//估价函数值
     struct node *next; //存储下一个结点的地址
     int fa;//上一个城市
  }LinkList;

typedef struct distance
  { 
      char city;
      int dist;
  }dis;//存储实际距离和数字与城市名的对应关系

dis h[100];
LinkList *head,*p,*q;

void INITLIST(LinkList *L){
 // L=(LinkList*)malloc(sizeof(LinkList));
  //判断是否申请成功
  if(!L){
     printf("无内存空间可分配");exit(0);}
  L->next=NULL;
} //INITLIST      
LinkList close[100];//close表

void Init()//初始化，主要是将文件中的内容读入
{
    FILE *stream=fopen("text.txt","r");
    int initNum=0;//城市数目
    if(stream==NULL)
    printf("The file fscanf.out was not opened\n");
    else
    {
        while(fscanf(stream,"%d%c",&h[initNum].dist,&h[initNum].city) !=EOF){//h数组存储各城市到达目标城市的直线距离
            initNum++;
        }
        city_num=initNum;
     fclose(stream);
    }
    FILE *stream2=fopen("map.txt","r");
    //int initNum=0;
    if(stream2==NULL)
    printf("The file fscanf.out was not opened\n");
    else
    {
        for(int i=0;i<initNum;i++){
            for(int j=0;j<initNum;j++){
                if(fscanf(stream,"%d",&g[i][j]) !=EOF)
                    //initNum++;
                        continue;
            }
        }
     fclose(stream);
    }
}

int Initend()//仅仅是为了找到目标城市而已
{
    for(int i=0;i<city_num;i++){
        if(h[i].dist==0)
            return i;
    }
    return -1;
}

int find(char c){
    for(int i=0;i<city_num;i++){
        if(h[i].city==c)
            return i;
    }
    return -1;
}

void addOpen(LinkList *p,int a,int dis,int f)//将走过的节点加入到open表
{
    LinkList *q;
    //p =(LinkList *)malloc(sizeof(LinkList));
    //q =(LinkList *)malloc(sizeof(LinkList));
    q=head;
    p->data=a;
    p->fa=f;
    p->num=dis+h[a].dist;
    while(1){
        if(q->next==NULL||q->next->num>p->num){
            p->next=q->next;
            q->next=p;
            return;
        }
        q=q->next;
    }
}

void deleteOpen(int n)//将重新找到更短的路径从open表替换
{
    LinkList *r;
    r=head;
    while(r->next!=NULL){
        if(r->next->data==n){
            r->next=r->next->next;
            return;
        }
        else
        {
            r=r->next;
        }
    }
    return;
}

int findClose(int a)//查找某城市是否在close表中
{
    for(int i=0;i<city_num;i++){
        if(close[i].data==a)
            return i;
    }
    return -1;
}

void deleteclose(int n)//在需要时从close表中转移出来
{
    for(int i=0;i<closeNum;i++){
        if(close[i].data==n){
            close[i].data=close[closeNum-1].data;
            close[i].num=close[closeNum-1].num;
            closeNum--;
            return;
        }

    }
    return;
}

int findOpen(int n,int dis)//查找某节点是否在open表中
{
    LinkList *r;
    r=head;
    int i=0;
    while(r->next!=NULL){
        if(r->next->data==n){
            if(r->next->num>dis+h[n].dist)
                return 1;
            else
                return 0;
        }   
        else
        {
            r=r->next;
            i++;
        }
    }
    return -1;
}

void Track()//形成track表
{
    int a,b;
    track[tra_num++]=head->next->data;
    b=head->next->fa;
    track[tra_num++]=b;
    while(1){
        for(int i=0;i<closeNum;i++){
            if(close[i].data==b){
                if(close[i].fa==-1)
                    return;
                b=close[i].fa;
                track[tra_num++]=b;
                break;
            }
        }

    }
}

int main(){
    Init();
    char name;
    int end;
    printf("输入初始城市:");
    scanf("%c",&name);
    end=Initend();//找到目标城市
    head =(LinkList *)malloc(sizeof(LinkList));
    INITLIST(head);//初始化open表
    p =(LinkList *)malloc(sizeof(LinkList));//...
    p->next=head->next;
    head->next=p;
    p->num=h[find(name)].dist;
    p->data=find(name);
    p->fa=-1;//...起点城市入open表
    LinkList *t;
    while(head->next->data!=end){
        t=head->next;
        //addTrack(t);
        head->next=t->next;//open表头入close表
        close[closeNum].data=t->data;
        close[closeNum].num=t->num;
        close[closeNum++].fa=t->fa;
        //printf("%d\n",t->data);
        for(int i=0;i<city_num;i++){
            if(g[t->data][i]!=0){//如果找到与此城市相连的城市&&不在close表中
                p =(LinkList *)malloc(sizeof(LinkList));
                if(findClose(i)==-1&&findOpen(i,t->num-h[t->data].dist+g[t->data][i])==-1){
                    addOpen(p,i,t->num-h[t->data].dist+g[t->data][i],t->data);//添加到open表中（城市序号，到此城市要走的路）

                }
                if(findOpen(i,t->num-h[t->data].dist+g[t->data][i])==1){
                    deleteOpen(i);
                    addOpen(p,i,t->num-h[t->data].dist+g[t->data][i],t->data);
                }
                if(findClose(i)!=-1){
                    if(close[findClose(i)].num>t->num-h[t->data].dist+g[t->data][i]+h[t->data].dist){
                        deleteclose(i);
                        addOpen(p,i,t->num-h[t->data].dist+g[t->data][i],t->data);
                    }
                }

            }
    //close[closeNum].data=t->data;
    //close[closeNum].next=NULL;
    //close[closeNum].num=t->num;
    //closeNum++;
        }
    }
    Track();
    printf("最优路径为:");
    for(int i=tra_num-1;i>=0;i--){
        printf("%c  ",h[track[i]].city);
    }
    printf("\n");
    return 0;
}
```
