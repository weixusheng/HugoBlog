# PAT-Advanced 26-30



1026-1030<br>
今天才收到了乙级的证书，突然意识到离甲级考试就剩20天了，加油鸭！
<!--more-->

# 1026-Table Tennis
这道快乐模拟类型的题，我搞了两个小时...模拟类型最好的解决方法就是，在纸上把所有处理情况提前写好，类似于各种情况下的决策树，如果直接开始写，绝对会懵的
后来看到柳神在评论下说，这类复杂模拟不会考了...啊这...
```c++
#include<iostream>
#include<vector>
#include<algorithm>
#include<cmath>
using namespace std;

struct person{
    int arrive, start, time;
    bool vip;
}tmpperson;

struct tablenode{
    int end = 8*3600, num;
    bool vip;
};

bool cmp1(person a, person b){
    return a.arrive < b.arrive;
}

bool cmp2(person a, person b){
    return a.start < b.start;
}

vector<person> player;
vector<tablenode>table;

void alloctable(int personid, int tableid){
    if(player[personid].arrive <= table[tableid].end){
        player[personid].start = table[tableid].end;
    }
    else{
        player[personid].start = player[personid].arrive;
    }
    table[tableid].end = player[personid].start + player[personid].time;
    table[tableid].num++;
}

int findnextvip(int vipid){
    vipid++;
    while(vipid < player.size() && player[vipid].vip == false){
        vipid++;
    }
    return vipid;
}

int main(int argc, char const *argv[])
{
    int n, k, m, viptable;
    scanf("%d", &n);
    for(int i=0; i<n; i++){
        int h,m,s,tmptime, flag;
        scanf("%d:%d:%d %d %d", &h, &m, &s, &tmptime, &flag);
        tmpperson.arrive = h*3600 + m*60 + s;
        tmpperson.start = 21 * 3600;
        if(tmpperson.arrive >= 21*3600){
            continue;
        }
        tmpperson.time = tmptime <= 120 ? tmptime*60 : 7200;
        tmpperson.vip = ((flag == 1) ? true : false);
        player.push_back(tmpperson);
    }
    scanf("%d%d", &k,&m);
    table.resize(k+1);
    for(int i=0; i<m; i++){
        scanf("%d", &viptable);
        table[viptable].vip = true;
    }
    sort(player.begin(), player.end(), cmp1);
    int i=0, vipid = -1;
    vipid = findnextvip(vipid);
    while(i < player.size()){
        int index = -1, minendtime = 999999999;
        for(int j=1; j<=k; j++){    //找到最先结束的table
            if(table[j].end < minendtime){
                minendtime = table[j].end;
                index = j;
            }
        }
        if(table[index].end >= 21*3600){
            break;
        }
        if(player[i].vip == true && i<vipid){
            i++;
            continue;
        }
        if(table[index].vip == true){   // vip table
            if(player[i].vip == true){  //vip table && vip man
                alloctable(i,  index);
                if(vipid == i){
                    vipid = findnextvip(vipid);
                }
                i++;
            }
            else{   // vip table !vip man
                if(vipid < player.size() && player[vipid].arrive <= table[index].end){
                    alloctable(vipid, index);
                    vipid = findnextvip(vipid); //先不跳过,留给非vip的下次循环
                }
                else{
                    alloctable(i, index);
                    i++;
                }
            }
        }
        else{   // !vip table
            if(player[i].vip == false){
                alloctable(i, index);
                i++;
            }
            else{   //vip man
                int vipindex = -1, minvipendtime = 999999999;
                for(int j=1; j<=k; j++){        //寻找符合要求的 vip table
                    if(table[j].vip == true && table[j].end < minvipendtime){
                        minvipendtime = table[j].end;
                        vipindex = j;
                    }
                }
                if(vipindex != -1 && player[i].arrive >= table[vipindex].end){
                    alloctable(i, vipindex);
                    if(vipid == i){
                        vipid = findnextvip(vipid);
                    }
                    i++;
                }
                else{   //没找到 vip table
                    alloctable(i, index);
                    if(vipid == i){
                        vipid = findnextvip(vipid);
                    }
                    i++;
                }
            }
        }
    }
    sort(player.begin(), player.end(), cmp2);
    // 注意最后 double四舍五入的方法，先 round 函数，再 .0f 输出
    for(int i=0; i<player.size() && player[i].start < 21*3600; i++){
        printf("%02d:%02d:%02d ", player[i].arrive/3600, player[i].arrive % 3600 / 60, player[i].arrive%60);
        printf("%02d:%02d:%02d ", player[i].start/3600, player[i].start% 3600 / 60, player[i].start%60);
        printf("%.0f\n", round((player[i].start-player[i].arrive) /60.0));
    }
    for(int i=1; i<=k; i++){
        if(i != 1){
            printf(" ");
        }
        printf("%d", table[i].num);
    }
    return 0;
}
```

# 1027-Colors in Mars
快乐的进制转换,巧妙使用char数组
```c++
#include <cstdio>
using namespace std;
int main() {
    char c[14] = {"0123456789ABC"};
    printf("#");
    for(int i=0; i<3; i++) {
        int num;
        scanf("%d", &num);
        printf("%c%c", c[num/13], c[num%13]);
    }
    return 0;
}
```

# 1028-List Sorting
一道稍微快乐的常规排序题,不过需要注意的是,题中要求 非递减 那么就需要在 cmp 中用 <= 符号
```c++
#include<iostream>
#include<algorithm>
#include<string>
using namespace std;

const int maxn = 100001;
struct Node{
    int no, score;
    string name;
}node[maxn];
int c;

bool cmp(Node a, Node b){
    if(c == 1){
        return a.no < b.no;
    }
    else if(c == 2){
        if(a.name == b.name){
            return a.no < b.no;
        }
        else{
            return a.name <= b.name;
        }
    }
    else {
        if(a.score == b.score){
            return a.no < b.no;
        }
        else{
            return a.score <= b.score;
        }
    }
}

int main(int argc, char const *argv[])
{
    int n;
    scanf("%d%d", &n, &c);
    for(int i=0; i<n; i++){
        //scanf("%d %s %d", &node[i].no, &node[i].name, &node[i].score);
        cin >> node[i].no >> node[i].name >> node[i].score;
    }
    sort(node, node+n, cmp);
    for(int i=0; i<n; i++){
        printf("%06d %s %d\n", node[i].no, node[i].name.c_str(), node[i].score);
    }
    return 0;
}
```

# 1029-Median
对于这种取中间数的题,最好的方法就是 cnt 计数寻找
并且题中两组有序数据分开给,就需要类似合并排序的比较方法
```c++
#include<iostream>
using namespace std;

int k[200005];

int main(int argc, char const *argv[])
{
    int n,m, tmp, count = 0;
    cin >> n;
    for(int i=1; i<=n; i++){
        scanf("%d", &k[i]);
    }
    k[n+1] = 0x7fffffff; // int 's max value = 0x7fffffff
    cin >> m;
    int midpos = (n+m+1)/2, i=1;
    for(int j=1; j<=m; j++){
        scanf("%d", &tmp);
        while(k[i] < tmp){
            count++;
            if(count == midpos){
                cout << k[i];
            }
            i++;
        }
        count++;
        if(count == midpos){
            cout << tmp;
        }
    }
    while(i <= n){
        count++;
        if(count == midpos){
            cout << k[i];
        }
        i++;
    }
    return 0;
}
```
# 1030-Travel Plan
标准 dijkstra + dfs 模板,和1018题一样的思路
dijkstra寻找最短路径构建pre数组,dfs循迹判断权重
```c++
#include<cstdio>
#include<algorithm>
#include<vector>
using namespace std;

int n,m,s,d;
int e[510][510], dis[510], cost[510][510];
vector<int> pre[510];
bool visit[510];
const int inf = 999999999;
vector<int> path, tmppath;
int mincost = inf;

void dfs(int v){
    tmppath.push_back(v);
    if(v == s){
        int tmpcost = 0;
        for(int i=tmppath.size(); i>0; i--){
            int id = tmppath[i];
            int nextid = tmppath[i-1];
            tmpcost += cost[id][nextid];
        }
        if(tmpcost < mincost){
            mincost = tmpcost;
            path = tmppath;
        }
        tmppath.pop_back();
        return;
    }
    for(int i=0; i<pre[v].size(); i++){
        dfs(pre[v][i]);
    }
    tmppath.pop_back();
}

int main(){
    fill(e[0], e[0]+510*510, inf);
    fill(dis, dis+510, inf);
    scanf("%d%d%d%d", &n, &m, &s, &d);
    for(int i=0; i<m; i++){
        int a, b;
        scanf("%d%d", &a, &b);
        scanf("%d", &e[a][b]);
        e[b][a] = e[a][b];
        scanf("%d", &cost[a][b]);
        cost[b][a] = cost[a][b];
    }
    pre[s].push_back(s);
    dis[0] = 0;
    for(int i=0; i<n; i++){
        int u = -1, minn = inf;
        for(int j=0; j<n; j++){
            if(visit[j] == false && dis[j] < minn){
                u = j;
                minn = dis[j];
            }
        }
        if(u == -1){
            break;
        }
        visit[u] = true;
        for(int v=0; v<n; v++){
            if(visit[v] == false && e[u][v] != inf){
                if(dis[v] > dis[u] + e[u][v]){
                    dis[v] = dis[u] + e[u][v];
                    pre[v].clear();
                    pre[v].push_back(u);
                }
                else if(dis[v] == dis[u] + e[u][v]){
                    pre[v].push_back(u);
                }
            }
        }
    }
    dfs(d);
    for(int i=path.size()-1; i>0; i--){
        printf("%d", path[i]);
    }
    printf(" %d %d", dis[d], mincost);
    return 0;
}
```
