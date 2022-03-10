# PAT-Advanced 11-15



1011-1015<br>
甲级题刷的有点不太顺手,慢慢来8
<!--more-->

# 1011-World Cup Betting
一道相对简单的排序题,里边对于字符的输出很巧妙
```c++
#include <cstdio>
using namespace std;

int main() {
    char c[4] = {"WTL"};
    double ans=1.0;
    for(int i=0; i<3; i++) {
        double maxvalue=0.0;
        int maxchar=0;
        for(int j=0; j<3; j++) {
            double temp;
            scanf("%lf", &temp);
            if(maxvalue<=temp) {
                maxvalue=temp;
                maxchar=j;
            }        
        }
        ans*=maxvalue;
        printf("%c ", c[maxchar]);
    }
    printf("%.2f", (ans*0.65-1) *2);
    return 0;
}
```

# 1012-The Best Rank
一道复杂的排序题,用到了 结构体实例中排序标志 的运用
对不同的实例类别的确定
```c++
#include <cstdio>
#include <algorithm>
using namespace std;

struct node {
    int id, best;
    int score[4], rank[4];
}stu[2005];

int exist[1000000], flag=-1;
bool cmp1(node a, node b) {
    return a.score[flag] >b.score[flag];
}
int main() {
    int n, m, id;
    scanf("%d %d", &n, &m);
    for(int i=0; i<n; i++) {
        scanf("%d %d %d %d", &stu[i].id, &stu[i].score[1], &stu[i].score[2],&stu[i].score[3]);
        stu[i].score[0] = (stu[i].score[1] +stu[i].score[2] +stu[i].score[3])/3.0+0.5;
    }
    for(flag=0; flag<=3; flag++) {
        sort(stu, stu+n, cmp1);
        stu[0].rank[flag] =1;
        for(int i=1; i<n; i++) {
            stu[i].rank[flag] =i+1;
            if(stu[i].score[flag] ==stu[i-1].score[flag])
                stu[i].rank[flag] =stu[i-1].rank[flag];        
        }    
    }
    for(int i=0; i<n; i++) {
        exist[stu[i].id] =i+1;
        stu[i].best=0;
        int minn=stu[i].rank[0];
        for(int j=1; j<=3; j++) {
            if(stu[i].rank[j] <minn) {
                minn=stu[i].rank[j];
                stu[i].best=j;            
            }       
        }    
    }
    char c[5] = {'A', 'C', 'M', 'E'};
    for(int i=0; i<m; i++) {
        scanf("%d", &id);
        int temp=exist[id];
        if(temp) {
            int best=stu[temp-1].best;
            printf("%d %c\n", stu[temp-1].rank[best], c[best]);
        } 
        else {
            printf("N/A\n");        
        }    
    }
    return 0;
}
```

# 1013-Battle Over Cities
考察连通图的确定,深度遍历,经典例题
```c++
#include<iostream>
#include<algorithm>

using namespace std;

int v[1010][1010];
bool visit[1010];
int n;

void dfs(int node){
    visit[node] = true;
    for(int i=1; i<=n; i++){
        if(visit[i] == false && v[node][i] == 1){
            dfs(i);
        }
    }
}

int main(int argc, char const *argv[])
{
    int m, k, a, b;
    scanf("%d%d%d",&n ,&m ,&k);
    for(int i=0; i<m; i++){
        scanf("%d%d", &a, &b);
        v[a][b] = v[b][a] = 1;
    }
    for(int i=0; i<k; i++){
        fill(visit, visit+1010, false);
        scanf("%d", &a);
        int cnt = 0;
        visit[a] = true;
        for(int j=1; j<=n; j++){
            if(visit[j] == false){
                dfs(j);
                cnt++;
            }
        }
        printf("%d\n", cnt-1);
    }
    return 0;
}
```

# 1014-Waiting in Line
考察队列应用,以及数据边界的判断
```c++
#include<iostream>
#include<queue>
#include<vector>

using namespace std;

struct node{
    int poptime, endtime;
    queue<int> q;
};

int main(int argc, char const *argv[])
{
    int n, m, k, q, index = 1;
    //n: 窗口的数量 m:队列的最大容量 k:顾客总数 q:需要查询的顾客数
    scanf("%d%d%d%d", &n, &m, &k, &q);
    vector<int> time(k+1), result(k+1);
    for(int i=1; i<=k; i++){
        scanf("%d", &time[i]);
    }
    vector<node> window(n+1);
    vector<bool> sorry(k+1, false);
    for(int i=1; i<=m; i++){
        for(int j=1; j<=n; j++){
            if(index <= k){
                window[j].q.push(time[index]);
                if(window[j].endtime >= 540){
                    sorry[index] = true;
                }
                window[j].endtime += time[index];
                if(i == 1){
                    window[j].poptime = window[j].endtime;
                }
                result[index] = window[j].endtime;
                index++;
            }
        }
    }
    while(index <= k){
        int tmpmin = window[1].poptime, tmpwindow = 1;
        for(int i=2; i<=n; i++){
            if(window[i].poptime < tmpmin){
                tmpmin = window[i].poptime;
                tmpwindow = i;
            }
        }
        window[tmpwindow].q.pop();
        window[tmpwindow].q.push(time[index]);
        window[tmpwindow].poptime += window[tmpwindow].q.front();
        if(window[tmpwindow].endtime >= 540){
            sorry[index] = true;
        }
        window[tmpwindow].endtime += time[index];
        result[index] = window[tmpwindow].endtime;
        index++;
    }
    for(int i=1; i<=q; i++){
        int query, minute;
        scanf("%d", &query);
        minute = result[query];
        if(sorry[query] == true){
            printf("Sorry\n");
        }
        else{
            printf("%02d:%02d\n", (minute+480)/60, (minute+480)%60);
        }
    }
    return 0;
}
```

# 1015-Reversible Primes
考察素数判断,以及进制转换
题解中利用 do-while 来记录转换进制得到的数 运用的十分好,申请数组空间,确定数组长度,之后转为十进制,以后可以直接拿来用
```c++
#include <cstdio>
#include <cmath>
using namespace std;
bool isprime(int n) {
    if(n<=1) 
        return false;
    int sqr=int(sqrt(n*1.0));
    for(int i=2; i<=sqr; i++) {
        if(n%i==0)
            return false;    
        }
        return true;
    }
int main() {
    int n, d, arr[100];
    while(scanf("%d", &n) !=EOF) {
        if(n<0) 
            break;
        scanf("%d", &d);
        if(isprime(n) ==false) {
            printf("No\n");
            continue;        
        }
        int len=0;
        do{
            arr[len++] =n%d;
            n=n/d;
        }while(n!=0);
        for(int i=0; i<len; i++)
            n=n*d+arr[i];
        printf("%s", isprime(n) ?"Yes\n" : "No\n");
    }
    return 0;
}
```
