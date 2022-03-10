# 二叉树应用算法整理


今天线索二叉树还是没弄完,快给我脑壳绕晕了,开题报告和论文翻译昨晚肝到今天中午,最后可算完成了。
上一篇总结了非递归遍历,这篇总结各种应用算法...本来7号就应该写完的,结果拖了这么久...(惭愧)
<!--more-->

##  1. 读边创建二叉树:
 - 读取参数为（fa, ch, lrtag）分别为father, child, 左右子树标志
 - 根据读取的参数,创建一个结点
 ```c
 BiTree p = (BiTree)malloc(sizeof(BiTNode));      //申请空间
 p -> data = ch;      //后来的结点通过data寻找father
 p ->lchild = p->rchild = null;
 Enqueue(p,s);      //进栈
 ```
  - 判断此时节点是否是根节点
  ```c
  if(p - >fa == '') *t = p;
  ```
   - 在栈中寻找父节点,并链接
   ```c
   else{
     s = gethead(s);
     while(s->data != fa){
       Dequeue(s);
       s = gethead(s);
     }
   }
   ```
 - 递归传入新的节点


 ## 2. 先序和中序确定一棵二叉树
  - 思想:根据先序遍历序列中第一个字符在中序中的位置,分两种情况递归函数
  - 参数:先序遍历的序列,中序遍历的序列,两种遍历的首字符位置,默认为0
  - 判断是否为空树
  ```c
  if(n == 0) *t = null;
  ```

  -  判断先序遍历序列第一个字符是否在中序遍历序列中存在,否则为空树
  ```c
  k = search(ino, pre[ps]);
  if(k == -1) *t = null;
  ```

  - 分两种情况递归
  ```c
  if(!(*t = (BiTree)malloc(sizeof(BiTNode))))  exit 0;
  if(k == is){      //没有左子树
    (*t) -> lchild = null;
  }
  else{
    CrtBT(&(*t)->lchild, pre, ino, ps+1, is, k-is);
  }
  if(k == is+n-1){      //没有右子树
    (*t) ->rchild = null;
  }
  else{
    CrtBT(&(*t)->rchild, ps, ino, ps+1+(k-is), k+1, n-(k-is)-1);
  }
  ```


## 3. 求二叉树深度
 - 思想:后序递归遍历,在父节点进行判断左右子树的深度
 - 代码实现

```c
 int depth (BiTree t){
   if(t == null) return 0;
   else{
     depth_left = depth(t->lchild);
     depth_right = depth(t->rchild);
     if(depth_left > depth_right) return depth_left;
     else return depth_right;
   }
 }
```

## 4. 求二叉树节点数量
```c
void count(BiTree t, int *x){
  if(t == null) return;     //空树直接返回
  else{
    count(t->rchild, x);      //递归左子树,传入x参数
    if(t->lchild == null && t->rchild == null) x++;     //当前为叶子节点
    count(t-<rchild, x);      //递归右子树
  }
}
```

## 5. 二叉树任意结点的父节点
 - 思想:二叉树基于路径分析的非递归遍历--后序遍历
 - 使用栈,直接goleft,到最左结点,进栈
  1. 第一次访问结点,标记数组初始化:c[p] = 'L'
  2. 第二次遇到时c[p] = 'R'
  3. 第三次遇到代表该结点的子结点都访问完成
  如果此时p为需要求父节点的结点,那么此时栈中的元素就是它的所有父结点

```c
 void search_priorder(BiTree t, Elemdata x){     //基于路径分析法实现-后序遍历
     SqStack s;
     initStack(&s);
     char lrtag[MAX] = "";
     BiTree t;
     t = priGoleft(t, &s, lrtag);
     while(t && t->data != x){
         lrtag[s.top] = 'R';
         if(t->rchild) t = priGoleft(t->rchild, &s, lrtag);
         else{
             while(!SqStackEmpty(s) && lrtag[s.top] == 'R'){
                 gettop(s, &t);
                 if(t->data == x){
                     printstack(s);
                     return;
                 }
             }
             if(!SqStackEmpty(s)) gettop(s, &t);
             else t = NULL;
         }
     }
 }
 BiTree priGoleft(BiTree t, SqStack *s, char c[]){
     if(!t){
         return NULL;
     }
     else{
         while(t){
             push(s, t);
             c[s->top] = 'L';
             if(t == NULL){
                 break;
             }
             t = t->lchild;
         }
     }
     return t;
 }
```

## 6. 二叉树叶子节点的路径
```c
void AllBitreePath(BiTree t, stack *s){
    while(t){
        SqStackPush(s, t->data);
        if(!t-> lchild && !t->rchild){          //叶子节点此时输出栈
            SqStackTraverse(*s);
        }
        else{
            AllBitreePath(t->lchild, *s);       //非叶子结点进栈
            AllBitreePath(t->rchild, *s);
        }
        SqStackPop(s);          //此时已经输出栈,出栈当前叶子结点
    }
}
```

目前书本上的应用是这几种,如果以后遇到新的,还会加上!

这两天看了<Inside Bill's Brain: Decoding Bill Gates>,本以为是在讲bill的成功历程,但主题是bill现在搞的慈善,帮助非洲贫穷国家改善卫生,消除一些很低级的疾病,相比其他传记,感觉可能讲这个更加人文一点(虽然也并不是很明白人文这个词的含义),讲到bill的成功秘诀就是天生的聪慧和不断的思考(恰巧我这两点都没有),果然伟大的人,总是如此谦虚而又有些高傲...

