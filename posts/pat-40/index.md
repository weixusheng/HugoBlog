# PAT-Advanced 45-53



1045-1053
<!--more-->

# 1045-Favorite Color Stripe
一道经典的动态规划,算取最优解,先对相邻元素进行判定,之后扩大搜索区域,开始区间取max优解,并将算得的局部数据,存入数组,以便更大区间取值时用到
```c++
#include<iostream>
using namespace std;

int main(int argc, char const *argv[])
{
    int dp[10001], a[10001], book[201];
    int n, m, x, l, num=0, maxn=0;
    scanf("%d %d",&n, &m);
    for(int i=1; i<=m; i++){
        scanf("%d", &x);
        book[x] = i;
    }
    scanf("%d", &l);
    for(int j=0; j<l; j++){
        scanf("%d", &x);
        if(book[x] >= 1){
            a[num++] = book[x];
        }
    }
    for(int i=0; i<num; i++){
        dp[i] = 1;
        for(int j=0; j<i; j++){
            if(a[i] >= a[j]){
                dp[i] = max(dp[j]+1, dp[i]);
            }
        }
        maxn = max(dp[i], maxn);
    }
    printf("%d", maxn);
    return 0;
}
```

# 1046-Shortest Distance
这道题的精髓在于,将区间距离,保存在了一个数组中,直接数组元素相减即可得到区间距离
```c++
#include<iostream>
#include<vector>
using namespace std;

int main(int argc, char const *argv[])
{
    int num;
    scanf("%d", &num);
    vector<int> data(num);
    int sum = 0;
    for(int i=0; i<num; i++){
        scanf("%d", &data[i]);
        sum += data[i];
    }
    int shit;
    int ans;
    scanf("%d", &shit);
    int s, e;
    for(int i=0; i<shit; i++){
        ans = 0;
        int tsum = sum;
        scanf("%d %d", &s, &e);
        if(s > e){
            for(int j=e; j<s; j++){
                tsum -= data[j-1];
            }
            ans = tsum;
            printf("%d\n", ans);
        }
        else if(e > s){
            for(int j=s; j<e; j++){
                ans += data[j-1];
            }
            printf("%d\n", ans);
        }
    }
    return 0;
}
```

# 1047-Student List for Course
对于 STL 的经典考察
```c++
#include<iostream>
#include<string>
#include<vector>
#include<map>
#include<algorithm>

using namespace std;

bool cmp(string a, string b){
    return a < b;
}

int main(int argc, char const *argv[])
{
    map<int, vector<string> > data;
    int stnum, cnum;
    scanf("%d %d", &stnum, &cnum);
    string id;
    id.resize(30);
    int shit, tmp;
    for(int i=0; i<stnum; i++){
        scanf("%s %d", &id[0], &shit);
        for(int j=0; j<shit; j++){
            scanf("%d", &tmp);
            data[tmp].push_back(id);
        }
    }
    for(int k=1; k<=cnum; k++){
        sort(data[k].begin(), data[k].end(), cmp);
        printf("%d %d\n", k, data[k].size());
        for(int w=0; w<data[k].size(); w++){
            printf("%s\n", data[k][w].c_str());
        }
    }
    return 0;
}
```

# 1048-Find Coins
这道题一开始想用二分查找,但是后来发现,源数据的相互关系就乱套了
看了题解才发现这种相加题,直接用hash就可以解决,很棒的思路
```c++
#include <iostream>
using namespace std;
int a[1001];
int main() {
    int n, m, temp;
    scanf("%d %d", &n, &m);
    for(int i=0; i<n; i++) {
        scanf("%d", &temp);
        a[temp]++;    
    }
    for(int i=0; i<1001; i++) {
        if(a[i]) {
            a[i]--;
            if(m>i && a[m-i]) {
                printf("%d %d", i, m-i);
                return 0;       
            }
            a[i]++;
        }    
    }
    printf("No Solution");
    return 0;
}
/*
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;

bool cmp(int a, int b){
    return a < b; 
}

int main(int argc, char const *argv[])
{
    int num, shit;
    int hs[100000] = {0};
    scanf("%d %d", &num, &shit);
    vector<int> data(num);
    for(int i=1; i<=num; i++){
        scanf("%d", &data[i]);
        hs[data[i]] = i;
    }
    sort(data.begin(), data.end(), cmp);
    int mid;
    int left, right, sum = 0;
    for(int j=1; j<=data.size(); j++){
        left = j+1;
        right = data.size()-1;
        while(left <= right){
            mid = (left + right)/2;
            sum = data[j] + data[mid];
            if(sum == shit){
                if(hs[data[j]] > hs[data[mid]]){
                    break;
                }
                printf("%d %d", data[j], data[mid]);
                return 0;
            }
            else if(sum > shit){
                right = mid-1;
            }
            else{
                left = mid+1;
            }
        }
    }
    printf("No Solution");
    return 0;
}
*/
```

# 1049-Counting Ones
我直接用了string来做,果不其然,两个测试点超时了,但是真要是考试我能压轴题做成这样,我就太知足了
## 超时-string
```c++
#include<iostream>
#include<string>
using namespace std;

int main(int argc, char const *argv[])
{
    int shit;
    scanf("%d", &shit);
    string data;
    int cnt = 0;
    for(int i=1; i<=shit; i++){
        data += to_string(i);
    }
    for(int j=0; j<data.size(); j++){
        if(data[j] == '1'){
            cnt++;
        }
    }
    printf("%d", cnt);
    return 0;
}
```

# 1050-String Subtraction
非常快乐的string操作
```c++
#include<iostream>
#include<string>
#include<vector>
using namespace std;

int main(int argc, char const *argv[])
{
    string a, b;
    getline(cin, a);
    getline(cin, b);
    int len = b.size();
    int shit[100000];
    for(int i=0; i<len; i++){
        shit[b[i]] = 1;
    }
    for(int j=0; j<a.size(); j++){
        if(shit[a[j]] != 1){
            printf("%c", a[j]);
        }
    }
    return 0;
}
```

# 1051-Pop Sequence
栈的模拟操作,经典类型题
没想到还能这样直接模拟出来,来判断序列是否合理
```c++
#include<iostream>
#include<stack>
#include<vector>
using namespace std;

int main(int argc, char const *argv[])
{
    int m, n, k;
    // m: maximum stack n:sequence length k:check num
    scanf("%d %d %d", &m, &n, &k);
    for(int i=0; i<k; i++){
        vector<int> data(n);
        stack<int> s;
        for(int j=0; j<n; j++){
            scanf("%d", &data[j]);
        }
        int cur = 0;
        for(int k=1; k<=n; k++){
            s.push(k);
            if(s.size() > m){
                break;
            }
            while(!s.empty() && s.top() == data[cur]){
                s.pop();
                cur++;
            }
        }
        if(cur == n){
            printf("YES\n");
        }
        else{
            printf("NO\n");
        }
    }
    return 0;
}
```

# 1052-Linked List Sorting
快乐的模拟链表类型题
```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

struct node{
    int ad;
    int key;
    int next;
}origin[1000000];

bool cmp(node a, node b){
    return a.key < b.key;
}

int main(int argc, char const *argv[])
{
    int n, ad, cnt = 0;
    scanf("%d %d", &n, &ad);
    int a, b, c;
    for(int i=0; i<n; i++){
        scanf("%d %d %d", &a, &b, &c);
        origin[a].ad = a;
        origin[a].key = b;
        origin[a].next = c;
    }
    vector<node> data;
    while(ad != -1){
        data.push_back(origin[ad]);
        ad = origin[ad].next;
    }
    sort(data.begin(), data.end(), cmp);
    cnt = data.size();
    if(cnt == 0){
        printf("0 -1\n");
        return 0;
    }
    else{
        printf("%d %05d\n", cnt, data[0].ad);
            for(int k=0; k<cnt; k++){
            if(k == 0){
                printf("%05d %d ", data[k].ad, data[k].key);
            }
            else{
                printf("%05d\n%05d %d ", data[k].ad, data[k].ad, data[k].key);
            }
            if(k == data.size()-1){
                printf("-1");
            }
        }
    }
    return 0;
}
```

# 1053-Path of Equal Weight
经典 dfs + 二级条件 的遍历
最后琢磨琢磨自己终于写出来了...下次遇到树遍历不再怕啦!
```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

int target;
struct node{
    int weight;
    vector<int> child;
};

vector<node> data;
vector<int> path;

void dfs(int index, int sum){
    path.push_back(index);
    if(sum > target){
        path.pop_back();
        return;
    }
    if(sum == target){
        if(data[index].child.size() !=0) {
            path.pop_back();
            return;
        }
        for(int i=0; i<path.size(); i++){
            /*
            if(i != 0){
                printf(" ");
            }
            printf("%d", data[path[i]].weight);
            if(i == path.size()-1){
                printf("\n");
            }
            */
            printf("%d%c", data[path[i]].weight, i != path.size() - 1 ? ' ' : '\n');
        }
        path.pop_back();
        return;
    }
    for(int j=0; j<data[index].child.size(); j++){
        int node = data[index].child[j];
        dfs(node, sum + data[node].weight);
    }
    path.pop_back();
    return;
}

bool cmp(int a, int b){
    return data[a].weight > data[b].weight;
}
int main(int argc, char const *argv[])
{
    int n, m, node, k;
    scanf("%d %d %d", &n, &m, &target);
    data.resize(n);
    for(int i=0; i<n; i++)
        scanf("%d", &data[i].weight);
    for(int i=0; i<m; i++) {
        scanf("%d %d", &node, &k);
        data[node].child.resize(k);
        for(int j=0; j<k; j++)
            scanf("%d", &data[node].child[j]);
        sort(data[node].child.begin(), data[node].child.end(), cmp);
    }
    //path.push_back(0);
    dfs(0, data[0].weight);
    return 0;
}
```
