# PAT-Advanced 31-35



1031-1035<br>
一直在做题,但是没更新blog,这两天都补上
<!--more-->

# 1031 Hello World for U
这道题是打印图形题,由于这个图形较为特殊(U型),因此利用一个数组,先将图形字符存在二维数组中,最后直接打印即可
但是这就需要注意打印边界
```c++
#include<iostream>
#include<string>
using namespace std;

int main(int argc, char const *argv[])
{
    char data[30][30];
    string c;
    cin >> c;
    fill(data[0],data[0]+30*30,' ');
    int n = c.size() + 2;
    int n1 = n/3;
    int n2 = n/3 + n%3;
    int index = 0;
    for(int i=0; i<n1; i++){
        data[i][0] = c[index++];
    }
    for(int i=1; i<=n2-2; i++){
        data[n1-1][i] = c[index++];
    }
    for(int i=n1-1; i>=0; i--){
        data[i][n2-1] = c[index++];
    }
    for(int i=0; i<n1; i++){
        for(int j=0; j<n2; j++){
            printf("%c", data[i][j]);
        }
        printf("\n");
    }
    return 0;
}
```

# 1032-Sharing
这道题就是利用结构体来模拟链表,题解中for循环写的很棒,利用 .next 作为for循环的递条件
```c++
#include <cstdio>
using namespace std;
struct NODE {
    char key;
    int next;
    bool flag;
}node[100000];

int main() {
    int s1, s2, n, a, b;
    scanf("%d%d%d", &s1, &s2, &n);
    char data;
    for(int i=0; i<n; i++) {
        scanf("%d %c %d", &a, &data, &b);
        node[a] = {data, b, false};
    }
    for(int i=s1; i!=-1; i=node[i].next){
        node[i].flag=true;
    }
    for(int i=s2; i!=-1; i=node[i].next) {
        if(node[i].flag==true) {
            printf("%05d", i);
            return 0;        
        }    
    }
    printf("-1");
    return 0;
}
```

# 1033-To Fill or Not to Fill
这道题...贪心算法的经典例题吧,做完这道题确实对贪心有了更深刻的理解
好题好题
```c++
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;

const int inf = 99999999;  
struct station{
    double pris;
    double dis;
};

bool cmp(station a, station b){
    return a.dis < b.dis;
}

int main(int argc, char const *argv[])
{
    double cmax, d, avg;
    int n;
    scanf("%lf%lf%lf%d", &cmax, &d, &avg, &n);
    vector<station> sta(n+1);
    sta[0] = {0.0, d};
    for(int i=1; i<=n; i++){
        scanf("%lf%lf", &sta[i].pris, &sta[i].dis);
    }
    sort(sta.begin(), sta.end(), cmp);
    double nowdis = 0.0, maxdis = 0.0, nowpris = 0.0, totalpris = 0.0, leftdis = 0.0;
    //特判不存加原点加油站
    if(sta[0].dis != 0){
        printf("The maximum travel distance = 0.00");
        return 0;
    }
    else{
        nowpris = sta[0].pris;
    }
    while(nowdis < d){
        maxdis = nowdis + cmax*avg; //最大行驶路程
        double minpris = inf;
        double minprisdis = 0;
        int flag = 0;
        //找到第一个符合条件的车站
        for(int i=1; i<=n && sta[i].dis <= maxdis; i++){
            if(sta[i].dis <= nowdis){
                continue;
            }
            if(sta[i].pris < nowpris){
                totalpris += (nowpris * (sta[i].dis - nowdis - leftdis)/avg);
                leftdis = 0.0;
                nowpris = sta[i].pris;
                nowdis = sta[i].dis;
                flag = 1;
                break;
            }
            if(sta[i].pris < minpris){
                minpris = sta[i].pris;
                minprisdis = sta[i].dis;
            }
        }
        if(flag == 0 && minpris != inf){
            totalpris += (nowpris * (cmax - leftdis/avg));
            leftdis = (cmax*avg)-(minprisdis - nowdis);
            nowdis = minprisdis;
            nowpris = minpris;
        }
        if(flag == 0 && minpris == inf){
            nowdis += cmax*avg;
            printf("The maximum travel distance = %.2f", nowdis);
            return 0;
        }
    }
    printf("%.2f", totalpris);
    return 0;
}
```

# 1034-Head of a Gang
连通图判定 + 附加条件(权重)判定
首先一个快乐dfs寻找连通图,其次在连通图内的循环中,寻找max值
```c++
#include<iostream>
#include<map>
using namespace std;

map<string,int> stoim;
map<int,string> itosm;
int idnum = 1;
int sifunc(string a){
    if(stoim[a] == 0){
        stoim[a] = idnum;
        itosm[idnum] = a;
        return idnum++;
    }
    else{
        return stoim[a];
    }
}
int k;
int g[2010][2010];
int weight[2010];
bool visited[2010];
map<string, int> ans;

void dfs(int v, int &head, int &totalnum, int &totalweight){
    visited[v] = true;
    totalnum++;
    if(weight[v] > weight[head]){
        head = v;
    }
    for(int i=1; i<idnum; i++){
        if(g[v][i] > 0){
            totalweight += g[v][i];
            g[v][i] = g[i][v] = 0;
            if(visited[i] == false){
                dfs(i, head, totalnum, totalweight);
            }
        }
    }
}

void dfstrave(){
    for(int i=1; i<idnum; i++){
        if(visited[i] == false){
            int head = i, totalnum = 0, totalweight = 0;
            dfs(i, head, totalnum, totalweight);
            if(totalnum > 2 && totalweight > k){
                ans[itosm[head]] = totalnum;
            }
        }
    }
}

int main(int argc, char const *argv[])
{
    int n, w;
    cin >> n >> k;
    string s1, s2;
    for(int i=0; i<n; i++){
        cin >> s1 >> s2 >> w;
        int id1 = sifunc(s1);
        int id2 = sifunc(s2);
        g[id1][id2] += w;
        g[id2][id1] += w;
        weight[id1] += w;
        weight[id2] += w;
    }
    dfstrave();
    cout<< ans.size() << endl;
    for(auto it=ans.begin(); it!=ans.end(); it++)
        cout<< it->first <<" "<< it->second << endl;
    return 0;
}
```

# 1035-Password
这道题对于字符串的简单应用,相对快乐一些
```c++
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    scanf("%d", &n);
    vector<string>v;
    for(int i=0; i<n; i++) {
        string name, s;
        cin>>name>>s;
        int len=s.length(), flag=0;
        for(int j=0; j<len; j++) {
            switch(s[j]) {
                case'1' : s[j] ='@'; flag=1; break;
                case'0' : s[j] ='%'; flag=1; break;
                case'l' : s[j] ='L'; flag=1; break;
                case'O' : s[j] ='o'; flag=1; break;
            }        
        }
        if(flag) {
            string temp = name+" "+s;
            v.push_back(temp);
        }    
    }
    int cnt=v.size();
    if(cnt!=0) {
        printf("%d\n", cnt);
        for(int i=0; i<cnt; i++)
            cout<<v[i] <<endl;    
    } 
    else if(n==1) {
        printf("There is 1 account and no account is modified");
    } 
    else {
        printf("There are %d accounts and no account is modified", n);
    }
    return 0;
}
```
