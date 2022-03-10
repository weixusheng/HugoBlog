# PAT-Basic 81~85



B1081-B1085<br>
第一次模拟考试-94分<br>
第四道题测试点超时,第五道题排序时没转int,这两个点都没想到...确实是坑.<br>
7.23更新
经过上次五道题的深刻反省,感觉现在代码规范了一些,思路也越来越清晰,下一步二刷,总结知识点,冲刺满分!
<!--more-->

# 1081-检查密码
这道题写麻烦了...搞了一堆continue...其实if-else if就可以解决
## 大佬教我写代码
```c++
#include <iostream>
#include <cctype>
using namespace std;

int main() {
    int n;
    cin >> n; getchar();
    for (int i = 0; i < n; i++) {
        string s;
        getline(cin, s);
        if (s.length() >= 6) {
            int invalid = 0, hasAlpha = 0, hasNum = 0;
            for (int j = 0; j < s.length(); j++) {
                if (s[j] != '.' && !isalnum(s[j])) invalid = 1;
                else if (isalpha(s[j])) hasAlpha = 1;
                else if (isdigit(s[j])) hasNum = 1;
            }
            if (invalid == 1) cout << "Your password is tai luan le.\n";
            else if (hasNum == 0) cout << "Your password needs shu zi.\n";
            else if (hasAlpha == 0) cout << "Your password needs zi mu.\n";
            else cout << "Your password is wan mei.\n";
        } else
            cout << "Your password is tai duan le.\n";
    }
    return 0;
}
```

## 菜鸡代码
```c++
#include<cstdio>
#include<iostream>

using namespace std;

int main(int argc, char const *argv[])
{
    int num;
    string s;
    cin >> num;
    getchar();
    bool hasa,hasd,shit;
    for(int i=0; i<num; i++){
        hasa = false;
        hasd = false;
        shit = false;
        s = "";
        getline(cin, s);
        if(s.size() < 6){
            cout << "Your password is tai duan le."<< endl;
            continue;
        }
        for(int j=0; j<s.size(); j++){
            if(isdigit(s[j])){
                hasd = true;
            }
            else if(isalpha(s[j])){
                hasa = true;
            }
            else if(s[j] != '.'){
                shit = true;
            }
        }
        if(shit){
            cout << "Your password is tai luan le." << endl;
            continue;
        }
        if(hasa == false){
            cout << "Your password needs zi mu."<< endl;
            continue;
        }
        if(hasd == false){
            cout << "Your password needs shu zi."<< endl;
            continue;
        }
        cout << "Your password is wan mei."<< endl;
    }
    return 0;
}
```

# 1082-射击比赛
这个没啥好说的...快乐白给题
```c++
#include<cstdio>
#include<iostream>
using namespace std;

int main(int argc, char const *argv[])
{
    int num;
    cin >> num;
    int max = 0, min = 10000;
    int id,x,y,len;
    int idmax,idmin;
    for(int i=0; i<num; i++){
        cin >> id >> x >> y;
        len = x*x + y*y;
        if(len < min){
            idmin = id;
            min = len;
        }
        if(len > max){
            idmax = id;
            max = len;
        }
    }
    printf("%04d %04d", idmin, idmax);
    return 0;
}
```

# 1083-是否存在相等的差
快乐白给题
```c++
#include<iostream>
#include<vector>
#include<cmath>
#include<map>
#include<algorithm>

using namespace std;

struct node{
    int div,cnt;
};

bool cmp(node a, node b){
    return a.div > b.div;
}

int main(int argc, char const *argv[])
{
    int num,tmp,div,cnt=0;
    cin >> num;
    vector<node> ans;
    map<int,int> divid;
    for(int i=0; i<num; i++){
        cin >> tmp;
        div = abs(tmp-(i+1));
        if(divid.find(div) == divid.end()){
            ans.push_back(node{div,1});
            divid[div] = cnt++;
        }
        else{
            ans[divid[div]].cnt++;
        }
    }
    sort(ans.begin(), ans.end(), cmp);
    for(int j=0; j<cnt; j++){
        if(ans[j].cnt > 1){
            printf("%d %d\n",ans[j].div, ans[j].cnt);
        }
    }
    return 0;
}
```

# 1084-外观数列
这道题我认为不应该超时的...可能是++太猛了吧...
```c++
#include<iostream>
#include<string>

using namespace std;

int main(int argc, char const *argv[])
{
    string d,tmp,shit;
    int num, cnt;
    int j;
    cin >> d;
    scanf("%d", &num);
    for(int k=0; k<num-1; k++){
        /* 超时代码
        tmp = "";
        cnt = 1;
        for(int j=0; j<d.size(); j++){
            if(d[j+1] == d[j]){
                cnt++;
            }
            else{
                shit = cnt+'0';
                tmp = tmp + d[j] + shit;
                cnt = 1;
            }
        }
        d = tmp;
        */
        string t;
        for (int i = 0; i < d.length(); i = j) {
            for (j = i; j < d.length() && d[j] == d[i]; j++);
            t += d[i] + to_string(j - i);
        }
        d = t;
    }
    printf("%s", d.c_str());
    return 0;
}
```

# 1085-PAT单位排行
这道题里的这个int整数坑藏的很深,在cmp函数里就要转成int进行比较,最后输出判定rank也要转int
以后要格外注意这种"只要整数部分"的题目
这里用到**transform()函数**,对字符串进行了小写变换
```c++
#include<cstdio>
#include<string>
#include<iostream>
#include<vector>
#include<map>
#include<algorithm>

using namespace std;

struct node{
    string lib;
    double score;
    int pnum;
};

double shitx(double score, string id){
    if(id[0] == 'B'){
        return score/1.5;
    }
    if(id[0] == 'A'){
        return score;
    }
    if(id[0] == 'T'){
        return score*1.5;
    }
}

bool cmp(node a, node b){
    if((int)a.score == (int)b.score){
        if(a.pnum == b.pnum){
            return a.lib < b.lib;
        }
        else{
            return a.pnum < b.pnum;
        }
    }
    else{
        return (int)a.score > (int)b.score;
    }
}

int main(int argc, char const *argv[])
{
    int num,cnt = 0;
    double score;
    string id, lib;
    cin >> num;
    map<string,int> namex;
    vector<node> ans;
    for(int i=0; i<num; i++){
        cin >> id >> score >> lib;
        transform(lib.begin(), lib.end(), lib.begin(), ::tolower);
        if(namex.find(lib) == namex.end()){
            ans.push_back(node{lib,shitx(score,id),1});
            namex[lib] = cnt++;
        }
        else{
            int tmp = namex[lib];
            ans[tmp].pnum++;
            ans[tmp].score = ans[tmp].score + shitx(score,id);
        }
    }
    sort(ans.begin(), ans.end(), cmp);
    int index = 1;
    printf("%d\n", cnt);
    for(int k=0; k<cnt; k++){
        if(k>0 && (int)ans[k-1].score != (int)ans[k].score){
            index = k+1;
        }
        printf("%d %s %d %d\n",index, ans[k].lib.c_str(), (int)ans[k].score, ans[k].pnum);
    }
    return 0;
}
```
