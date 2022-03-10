# PAT-Basic 91~95



B1091-B1095<br>
上一次PAT考试真题: 一个半小时做完前四道题,两个测试点没过,第五题AC<br>
成绩: 96<br>
错的两个测试点,一个是getchar(),另一个是输出格式不对...我人傻了<br>
PAT所有的真题都刷完撒花了,这一个月确实从一个低级菜鸡变成了一个高级菜鸡,不仅熟悉了c++的基本语法,并且感觉coding能力也变强了不少,确实刷题才是变强的唯一途径<br>
希望25号的考试能取得一个好成绩!下一步备战甲级!<br>
Beijing 冲冲冲!
<!--more-->

# 1091 N-自守数
简单AC
```c++
aor(int i=0; i<num; i++){
        cin >> tmp;
        bool hasshit = false;
        for(int j=1; j<10; j++){
            if(isshit(tmp, j)){
                printf("%d %d\n", j, tmp*tmp*j);
                hasshit = true;
                break;
            }
        }
        if(!hasshit){
            printf("No\n");
        }
    }
    system("pause");
    return 0;
}
```

# 1092 最好吃的月饼
一道快乐计数题
```c++
#include<cstdlib>
#include<vector>
#include<iostream>
#include<algorithm>

using namespace std;

struct bing{
    int id;
    int cnt;
};

bool cmp(bing a, bing b){
    if(a.cnt == b.cnt){
        return a.id < b.id;
    }
    else{
        return a.cnt > b.cnt;
    }
}

int main(int argc, char const *argv[])
{
    int m, c, tmp;
    cin >> m >> c;
    vector<bing> data(m);
    /*
    for(int i=0; i<m; i++){
        data[i].id = i;
        data[i].cnt = 0;
    }
    */
    for(int k=0; k<c; k++){
        for(int j=0; j<m; j++){
            cin >> tmp;
            data[j].id = j;
            data[j].cnt += tmp;
        }
    }
    sort(data.begin(), data.end(), cmp);
    int max = data[0].cnt;
    printf("%d\n", max);
    for(int i=0; i<m; i++){
        if(data[i].cnt == max){
            if(i != 0){
                printf(" ");
            }
            printf("%d", data[i].id+1);
        }
        else{
            break;
        }
    }
    system("pause");
    return 0;
}
```

# 1093 字符串A+B
这个getchar()就真的梦幻,之前不加出问题,现在加了还出问题...
这回知道了,以后像这种连续输入行的中间不需要加,如果后面接"%c",就需要加上
```c++
#include<cstdlib>
#include<set>
#include<iostream>
#include<string>

using namespace std;

int main(int argc, char const *argv[])
{
    string a, b;
    getline(cin, a);
    //getchar();
    getline(cin, b);
    string c = a+b;
    set<char> data;
    for(int i=0; i<c.size(); i++){
        if(data.find(c[i]) == data.end()){
            printf("%c", c[i]);
            data.insert(c[i]);
        }
    }
    system("pause");
    return 0;
}
```

# 1094 谷歌的招聘
这道题输出格式没有弄明白,需要输出这个部分字符串,我最后输出成int了,消去了0
```c++
#include<cstdlib>
#include<iostream>
#include<string>
#include<cmath>

using namespace std;

bool isshit(int a){
    if (a == 0 || a == 1) return false;
    //int s = (int)sqrt(double(a));
    for(int i=2; i*i<=a; i++){
        if(a%i == 0){
            return false;
        }
    }
    return true;
}

int main(int argc, char const *argv[])
{
    int num, k;
    string s;
    cin >> num >> k;
    getchar();
    getline(cin, s);
    int i=0;
    string tmp;
    int shit;
    bool has = false;
    while(i+k <= num){
        tmp = s.substr(i,k);
        shit = stoi(tmp);
        if(isshit(shit)){
            //printf("%d",shit);
            cout << tmp;
            //has = true;
            //break;
            return 0;
        }
        i++;
    }
    //if(!has){
        printf("404\n");
    //}
    //system("pause");
    return 0;
}
```

# 1095 解码PAT准考证
这道题我写的有一点点麻烦,不过还好,至少写的过程中思路清晰一点
## 大佬的解法
```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <algorithm>
using namespace std;
struct node {
    string t;
    int value;
};
bool cmp(const node &a, const node &b) {
    return a.value != b.value ? a.value > b.value : a.t < b.t;
}
int main() {
    int n, k, num;
    string s;
    cin >> n >> k;
    vector<node> v(n);
    for (int i = 0; i < n; i++)
        cin >> v[i].t >> v[i].value;
    for (int i = 1; i <= k; i++) {
        cin >> num >> s;
        printf("Case %d: %d %s\n", i, num, s.c_str());
        vector<node> ans;
        int cnt = 0, sum = 0;
        if (num == 1) {
            for (int j = 0; j < n; j++)
                if (v[j].t[0] == s[0]) ans.push_back(v[j]);
        } else if (num == 2) {
            for (int j = 0; j < n; j++) {
                if (v[j].t.substr(1, 3) == s) {
                    cnt++;
                    sum += v[j].value;
                }
            }
            if (cnt != 0) printf("%d %d\n", cnt, sum);
        } else if (num == 3) {
            unordered_map<string, int> m;
            for (int j = 0; j < n; j++)
                if (v[j].t.substr(4, 6) == s) m[v[j].t.substr(1, 3)]++;
            for (auto it : m) ans.push_back({it.first, it.second});
        }
        sort(ans.begin(), ans.end(),cmp);
        for (int j = 0; j < ans.size(); j++)
            printf("%s %d\n", ans[j].t.c_str(), ans[j].value);
        if (((num == 1 || num == 3) && ans.size() == 0) || (num == 2 && cnt == 0)) printf("NA\n");
    }
    return 0;
}
```

## 菜鸡的解法
比较函数懒得写了...
```c++
#include<iostream>
#include<vector>
#include<map>
#include<string>
#include<cstdlib>

using namespace std;

struct lnode{
    string id;
    int score;
};

struct knode{
    int rnum,snum;
};

int main(int argc, char const *argv[])
{
    // B123180908127
    int m, n;
    cin >> m >> n;
    // id-score
    string id;
    int score;
    // level-date
    string level;
    string kaochang, date;
    //vector-levle
    map<string, vector<lnode> > lmap;
    map<string, knode> kmap;
    map<string, map<string, int> > dmap;
    for(int i=0; i<m; i++){
        cin >> id >> score;
        level = id[0];
        kaochang = id.substr(1,3);
        date = id.substr(4,6);
        // insert lmap
        lmap[level].push_back(lnode{id, score});
        // insert kmap
        //if(kmap.find(kaochang) == kmap.end()){
            //kmap[kaochang].snum = score;
            //kmap[kaochang].rnum = 1;
        //}
        //else{
            kmap[kaochang].snum += score;
            kmap[kaochang].rnum++;
        //}
        //insert dmap;
        //if(dmap[date].find(kaochang) == dmap[date].end()){
            //dmap[date][kaochang] = 1;
        //}
        //else{
            dmap[date][kaochang]++;
        //}
    }
    int mode;
    string shit;
    for(int j=0; j<n; j++){
        cin >> mode >> shit;
        cout << "Case " << j+1 << endl;
        switch (mode)
        {
        case 1:
            for(int k=0; k<lmap[shit].size(); k++){
                cout << lmap[shit][k].id << " " << lmap[shit][k].score << endl;
            }
            break;
        case 2:
            cout << kmap[shit].rnum <<" "<< kmap[shit].snum << endl;
            break;
        case 3:
            for(auto it = dmap[shit].begin(); it!=dmap[shit].end(); it++){
                cout << it->first << " " << it->second << endl;
            }
            break;
        }
    }
    system("pause");
    return 0;
}
```

# map和vector中的数据赋值问题
在对vector和map容器添加数据的过程中,经常会遇到累加的情况
一开始我认为,对于vector需要先初始化为0,map要首先find键值,再分情况进行赋值(map key-value的初始化)
但是看了题解之后发现不需要进行空判定,直接 += value 即可
**例如map中:**
```c++
       //if(kmap.find(kaochang) == kmap.end()){
            //kmap[kaochang].snum = score;
            //kmap[kaochang].rnum = 1;
        //}
        //else{
            kmap[kaochang].snum += score;
            kmap[kaochang].rnum++;
        //}
```
当键值不存在时,如果进行key-value值的改变,会直接新建键值对,并且这个过程中,value为0.因此可以直接进行赋值操作,而不需要判断key为空
**再例如vector中**:
```c++
struct bing{
    int id;
    int cnt;
};

vector<bing> data(m);
/*
for(int i=0; i<m; i++){
    data[i].id = i;
    data[i].cnt = 0;
}
*/
for(int k=0; k<c; k++){
    for(int j=0; j<m; j++){
        cin >> tmp;
        data[j].id = j;
        data[j].cnt += tmp;
    }
}
```
对于vector中的结构体进行赋值,直接 += 即可,而不需要对其初始化为0
