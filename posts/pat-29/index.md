# PAT-Advanced 并查集



算法笔记 - 并查集
<!--more-->

所谓并查集,就是判断两个元素是否在同一集合中,在kruskal中有所应用,实现起来还蛮简单的
本质上利用数组

# 初始化
```c++
for(int i=1; i<=N; i++){
    father[i] = i;
}
```

# 查找
## 递推
```c++
int findfather(int x){
    while( != father[x]){
        x = father[x];
    }
    return x;
}
```
## 递归
```c++
int findfather(int x){
    if(x == father[x]){
        return x;
    }
    else return findfather(father[x]);
}
```

# 合并
```c++
void union(int a, int b){
    int faa = findfather(a);    //寻找父节点
    int fab = findfather(b);
    if(faa != fab){
        father[faa] = fab;  //链接父节点
    }
}
```

# 路径压缩
## 递推
```c++
int findfather(int x){
    int a = x;
    while(x != father[x]){
        x = father[x];
    }
    while(a != father[a]){
        int z = a;
        a = father[a];
        father[z] = x;
    }
    return x;
}
```
## 递归
```c++
int findfather(int v){
    if(v == father[v]){
        return v;   //找到根节点
    }
    else{
        int f = findfather(father[v]);
        father[v] = f;  //赋值
        return f;   //返回根节点
    }
}
```
