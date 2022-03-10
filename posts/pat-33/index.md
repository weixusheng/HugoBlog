# PAT-Advanced 16-20



1016-1020
<!--more-->

# 1016-Phone Bills
一道比较巧妙而复杂的快乐排序题
开始的时候看到题被吓到了,以为很复杂,但是其实就是 筛选合理数据 + 排序 就能解决的问题
对于这种分秒格式的数据,应该化为秒数表示,这样区间值计算和比较
并且这种带有开头结尾的合理区间判断,这道题的排序判断相邻元素的方法就很好
```c++
#include <iostream>
#include <map>
#include <vector>
#include <algorithm>
using namespace std;

struct node {
    string name;
    int status, month, time, day, hour, minute;
};
bool cmp(node a, node b) {
    return a.name!=b.name?a.name<b.name : a.time<b.time;
}
double billFromZero(node call, int*rate) {
    double total=rate[call.hour] *call.minute+rate[24] *60*call.day;
    for (int i=0; i<call.hour; i++)
        total+=rate[i] *60;
    return total/100.0;
}
int main() {
    int rate[25] = {0}, n;
    for (int i=0; i<24; i++) {
        scanf("%d", &rate[i]);
        rate[24] += rate[i];    
    }
    scanf("%d", &n);
    vector<node> data(n);
    for (inti=0; i<n; i++) {
        cin>>data[i].name;
        scanf("%d:%d:%d:%d", &data[i].month, &data[i].day, &data[i].hour,&data[i].minute);
        string temp;
        cin>>temp;
        data[i].status = (temp=="on-line") ?1 : 0;data[i].time=data[i].day*24*60+data[i].hour*60+data[i].minute;
    }
    sort(data.begin(), data.end(), cmp);
    map<string, vector<node> > custom;
    for (int i=1; i<n; i++) {
        if (data[i].name==data[i-1].name&&data[i-1].status==1&&data[i].status==0) {
            custom[data[i-1].name].push_back(data[i-1]);
            custom[data[i].name].push_back(data[i]);
        }    
    }
    for (auto it : custom) {
        vector<node> temp=it.second;
        cout<<it.first;
        printf(" %02d\n", temp[0].month);
        double total=0.0;
        for (int i=1; i<temp.size(); i+=2) {
            double t=billFromZero(temp[i], rate) -billFromZero(temp[i-1],rate);
            printf("%02d:%02d:%02d %02d:%02d:%02d %d $%.2f\n", temp[i-1].day,temp[i-1].hour, temp[i-1].minute, temp[i].day, temp[i].hour,temp[i].minute, temp[i].time-temp[i-1].time, t);
            total+=t;        
        }
        printf("Total amount: $%.2f\n", total);    
    }
    return 0;
}
```

# 1017-Queueing at Bank
又是这种银行窗口排队题
一开始想像之前一样用队列,发现只要找到队列中最早结束的队列就可,因为顾客可以随意挑选队列,所以只需要记录endtime即可
对于这种时间的,依旧是需要转为秒数进行判断,格式化时间比较是别想了
并且需要注意的情况是,顾客可能需要等待队列,或者队列等待顾客两种情况
题目只要求计算等待时间,所以只需要在特定的情况下进行 result += time 计算
```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct node {
    int come, time;
} tempcustomer;

bool cmp1(node a, node b) {
    return a.come<b.come;
}
int main() {
    int n, k;
    scanf("%d%d", &n, &k);
    vector<node> custom;
    for(int i=0; i<n; i++) {
        int hh, mm, ss, time;
        scanf("%d:%d:%d %d", &hh, &mm, &ss, &time);
        int cometime = hh*3600+mm*60+ss;
        if(cometime>61200) 
            continue;
        tempcustomer = {cometime, time*60};
        custom.push_back(tempcustomer);
    }
    sort(custom.begin(), custom.end(), cmp1);
    vector<int> window(k, 28800);
    double result=0.0;
    for(int i=0; i<custom.size(); i++) {
        int tempindex=0, minfinish=window[0];
        for(int j=1; j<k; j++) {
            if(minfinish > window[j]) {
                minfinish = window[j];
                tempindex = j;            
            }        
        }
        if(window[tempindex] <= custom[i].come) {
            window[tempindex] = custom[i].come+custom[i].time;
        } 
        else {
            result += (window[tempindex] -custom[i].come);
            window[tempindex] += custom[i].time;        
        }    
    }
    if(custom.size() ==0)
        printf("0.0");
    else 
        printf("%.1f", result/60.0/custom.size());
    return 0;
}
```

# 1018-Public Bike Management
dijkstra模板 计算一级条件 加上 dfs 计算二级条件
并且 dijkstra中建立 pre链接数组 在 dfs 中为寻找递归提供方向
但是这个代码量...给我要看哭了,难道甲级不光拼的是算法能力,还拼谁敲的快嘛
这道题的 dfs 用的真是精妙!甚至让我体会到了dfs所蕴含的无限魔力
总之是一道很好的经典题
**经典 dfs 对于路径记录以及路径最值的比较**
```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

const int inf=99999999;
int cmax, n, sp, m;
int minNeed=inf, minBack=inf;
int e[510][510], dis[510], weight[510];
bool visit[510];
vector<int> pre[510];
vector<int> path, temppath;

/* DFS NB! */
void dfs(int v) {
    temppath.push_back(v);  //首先进入
    if(v==0) {  //递归边界
        int need=0, back=0;
        for(int i=temppath.size() -1; i>=0; i--) {
            int id=temppath[i];
            if(weight[id] >0) {
                back+=weight[id];
            } 
            else {
                if(back > (0-weight[id])) {
                    back+=weight[id];
                } 
                else {
                    need += ((0-weight[id]) -back);
                    back=0;                
                }            
            }        
        }
        if(need < minNeed) {
            minNeed=need;
            minBack=back;
            path=temppath;   //赋值新的path      
        } 
        else if(need==minNeed && back<minBack) {
            minBack=back;
            path=temppath;  //赋值新的path
        }
        temppath.pop_back();    //当前临时路径数组退回上级(初始状态)
        return;    
    }
    for(int i=0; i<pre[v].size(); i++)  //快乐递归
        dfs(pre[v][i]);
    temppath.pop_back();    //每次递归结束,返回初始状态
}
int main() {
    fill(e[0], e[0] +510*510, inf);
    fill(dis, dis+510, inf);
    scanf("%d%d%d%d", &cmax, &n, &sp, &m);
    for(int i=1; i<=n; i++) {
        scanf("%d", &weight[i]);
        weight[i] =weight[i] -cmax/2;    
    }
    for(int i=0; i<m; i++) {
        int a, b;
        scanf("%d%d", &a, &b);
        scanf("%d", &e[a][b]);
        e[b][a] = e[a][b];
    }
    dis[0] =0;
    for(int i=0; i<=n; i++) {
        int u=-1, minn=inf;
        for(int j=0; j<=n; j++) {
            if(visit[j] ==false && dis[j] <minn) {
                u=j;
                minn=dis[j];            
            }        
        }
        if(u==-1) 
            break;
        visit[u] = true;
        for(int v=0; v<=n; v++) {
            if(visit[v] ==false && e[u][v] !=inf) {
                if(dis[v] >dis[u] +e[u][v]) {
                    dis[v] =dis[u] +e[u][v];
                    pre[v].clear(); // pre刷新
                    pre[v].push_back(u);    //路径加入
                }
                else if(dis[v] ==dis[u] +e[u][v]) {
                    pre[v].push_back(u);    //相同距离路径增加
                }            
            }        
        }    
    }
    dfs(sp);
    printf("%d 0", minNeed);
    for(int i=path.size() -2; i>=0; i--)
        printf("->%d", path[i]);
    printf(" %d", minBack);
    return 0;
}
```

# 1019-General Palindromic Number
又是进制转换的题,申请数组空间,之后 a%b a/b 就完事啦
```c++
#include <cstdio>
using namespace std;
int main() {
    int a, b;
    scanf("%d %d", &a, &b);
    int arr[40], index=0;
    while(a!=0) {
        arr[index++] =a%b;
        a=a/b;
    }
    int flag=0;
    for(int i=0; i<index/2; i++) {
        if(arr[i] !=arr[index-i-1]) {
            printf("No\n");
            flag=1;
            break;
        }
    }
    if(!flag) 
        printf("Yes\n");
    for(int i=index-1; i>=0; i--) {
        printf("%d", arr[i]);
        if(i!=0) 
            printf(" ");
    }
    if(index==0)
        printf("0");
    return 0;
}
```

# 1020-Tree Traversals
一道经典题
触及到了我的知识盲区...代码不会写了
后序-中序 建树
并且这里还有个很好的方法,就是在遍历过程中,直接命名层序遍历下标
虽然我不知道里边什么原理,但确实都是树的下标...
```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

struct node {
    int index, value;
};
bool cmp(node a, node b) {
    return a.index<b.index;
}
vector<int>post, in;
vector<node>ans;
void pre(int root, int start, int end, int index) {
    if (start>end){
        return;
    }
    int i=start;
    while (i<end && in[i] != post[root]){
        i++;    //找到中间结点(后序遍历最后一个结点是根结点)
    }
    ans.push_back({index, post[root]});
    pre(root-1-end+i, start, i-1, 2*index+1);   //前段递归
    pre(root-1, i+1, end, 2*index+2);   //后段递归
}
int main() {
    int n;
    scanf("%d", &n);
    post.resize(n);
    in.resize(n);     //很好的方法来建立全局数组
    for (int i=0; i<n; i++) 
        scanf("%d", &post[i]);
    for (int i=0; i<n; i++)
        scanf("%d", &in[i]);
    pre(n-1, 0, n-1, 0);
    sort(ans.begin(), ans.end(), cmp);
    for (int i=0; i<ans.size(); i++) {
        if (i!=0) 
            cout << " ";
        cout << ans[i].value;    
    }
    return 0;
}
```
