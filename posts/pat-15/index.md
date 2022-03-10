# PAT-Basic 76~80



B1076-B1080<br>
这五道题让我产生了许多反思,由于代码中变量以及语法上的不规范,造成了许多不必要的麻烦,虽然代码确实可以跑起来,但性价比太低了,规范整洁的代码习惯,不仅可以让写代码过程思路清晰,并且debug过程变得轻松很多.小辣鸡以后要重视这方面啦!<br>
7.23更新
<!--more-->

# 1076-Wifi密码
快乐白给题
```c++
#include<cstdio>
#include<vector>
#include<iostream>
using namespace std;

int main(int argc, const char** argv) {
    int num;
    int i=0;
    scanf("%d", &num);
    vector<vector<char> > chos;
    chos.resize(4, vector<char>(2));
    vector<char> res(num);
    getchar();
    for(int i=0; i<num; i++){
        //A-T B-F C-F D-F
        scanf("%c-%c %c-%c %c-%c %c-%c",&chos[0][0], &chos[0][1], &chos[1][0],&chos[1][1], &chos[2][0],&chos[2][1], &chos[3][0],&chos[3][1]);
        for(int k=0; k<4; k++){
            if(chos[k][1] == 'T'){
                res[i] = chos[k][0];
            }
        }
        getchar();
    }
    for(int w=0; w<num; w++){
        switch (res[w])
        {
        case 'A':
            printf("1");
            break;
        case 'B':
            printf("2");
            break;
        case 'C':
            printf("3");
            break;
        case 'D':
            printf("4");
            break;
        }
    }
    return 0;
}
```

# 1077-互评成绩计算
这道题就暴露了我写的代码中很多的问题
首先是变量命名不规范
"count"简写为"cnt"
"temp"简写为"tmp"
其次对于相似功能的变量,应该放在一起命名和赋值,这样方便后期debug阶段查看

## 柳神的代码
```c++
#include <iostream>
using namespace std;
int main() {
    int n, m;
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        int g2, g1 = 0, cnt = -2, temp, maxn = -1, minn = m + 1;
        cin >> g2;
        for (int j = 0; j < n-1; j++) {
            cin >> temp;
            if (temp >= 0 && temp <= m) {
                if (temp > maxn) maxn = temp;
                if (temp < minn) minn = temp;
                g1 += temp;
                cnt++;
            }
        }
        cout << int((((g1 - minn - maxn) * 1.0 / cnt) + g2) / 2 + 0.5) << endl;
    }
    return 0;
}
```

## 菜鸡的代码
```c++
#include<cstdio>
#include<vector>
using namespace std;

int main(int argc, const char** argv) {
    int num, full;
    scanf("%d %d", &num, &full);
    int temp_count, temp_input, teacher, stdscore;
    int avg, max, min;
    bool bool_max, bool_min;
    int res;
    vector<int> sum(num-1);
    for(int i=0; i<num; i++){
        temp_count = 0;
        stdscore = 0;
        bool_max = true;
        bool_min = true;
        max = 0;
        min = full;
        for(int k=0; k<num; k++){
            scanf("%d", &temp_input);
            if(k == 0){
                teacher = temp_input;
            }
            else{
                if(temp_input>= 0 && temp_input<= full){
                    max = temp_input > max ? temp_input:max;
                    min = temp_input < min ? temp_input:min;
                    sum[temp_count++] = temp_input;
                }
            }
        }
        for(int w=0; w<temp_count; w++){
            if(sum[w] == max && bool_max){
                sum[w] = 0;
                bool_max = false;
            }
            if(sum[w] == min && bool_min){
                sum[w] = 0;
                bool_min = false;
            }
            stdscore += sum[w];
        }
        avg = stdscore/(temp_count-2);
        res = (teacher+avg+1)/2;
        printf("%d\n", res);
    }
    return 0;
}
```

# 1078-字符串压缩与解压
这道题我一开始写的有点麻烦,主要担心越界,但是发现题解直接i+1了,因为字符串最后还有一个终结符
```c++
#include<cstdio>
#include<string>
#include<iostream>
using namespace std;

int main(int argc, char const *argv[])
{
    char mode;
    string data;
    scanf("%c", &mode);
    getchar();
    int temp_count;
    int temp_num = 0;
    if(mode == 'C'){
        getline(cin, data);
        temp_count = 1;
        for(int i=0; i<data.size(); i++){
            if(data[i] == data[i+1]){
                temp_count++;
            }
            else{
                if(temp_count!=1){
                    printf("%d",temp_count);
                    temp_count = 1;
                }
                printf("%c",data[i]);
            }
        }
    }
    else if(mode == 'D'){
        getline(cin,data);
        for(int i=0; i<data.size(); i++){
            if(isdigit(data[i])){
                temp_num = temp_num*10 + data[i]-'0';
            }
            else{
                if(temp_num!= 0){
                    for(int a=0 ;a<temp_num-1; a++){
                        printf("%c", data[i]);
                    }
                }
                printf("%c", data[i]);
                temp_num = 0;
            }
        }
    }
    return 0;
}
```

# 1079-延迟的回文数*
这道题有一个超级重点的知识点,就是对于较大数的加法,这里使用string进行位加法,类似于程序实现手动加法步骤
应该作为一个函数模板掌握,另外对于限制次数的操作,可以使用 while(n--) 实现
```c++
#include<cstdio>
#include<string>
#include<algorithm>
#include<iostream>

using namespace std;

string rev(string data){
    reverse(data.begin(), data.end());
    return data;
}

/* 重点 */
string add(string data1, string data2){
    string s = data1;
    int carry = 0;
    for(int i=s.size()-1; i>=0; i--){
        s[i] = (data1[i]-'0'+data2[i]-'0'+carry)%10 + '0';
        carry = (data1[i]-'0'+data2[i]-'0'+ carry)/10;
    }
    if(carry > 0){
        s = "1" + s;
    }
    return s;
}

int main(int argc, char const *argv[])
{
    string origin, s;
    cin >> origin;
    int n = 10;
    if(origin == rev(origin)){
        cout << origin << " is a palindromic number.\n";
        return 0;
    }
    while(n--){
        s = add(origin, rev(origin));
        cout << origin << " + " << rev(origin) << " = " << s << endl;
        if(s == rev(s)){
            cout << s << " is a palindromic number.\n";
            return 0;
        }
        origin = s;
    }
    cout << "Not found in 10 iterations.\n";
    return 0;
}
```

# 1080-MOOC期终成绩
这道题虽然不难(想明白之后感觉不难...)但蕴含的知识点很多,看懂题解之后,我花了半个小时一遍一遍的看代码
并且反思为什么自己写不出来这样的code
总结出几个点,首先是变量命名不规范,其次是对于判断功能写的拖泥带水,冗余代码很多,最后导致思路越写越乱
因此应该在读懂题意的条件下,认真分析每一处数据的限制条件,以及解决方法,这样才能思路清晰的一气呵成
另外要善于用vector的push_back(),结构体的新建实例化可以直接 node{s, score, -1, -1, 0}
```c++
#include<map>
#include<vector>
#include<iostream>
#include<algorithm>
using namespace std;

struct node{
    string name;
    int gp,gm,gf,g;
};

bool cmp(node a, node b){
    return a.g != b.g ? a.g > b.g : a.name < b.name; 
}

int main(int argc, char const *argv[])
{
    int p, m, n, score, cnt = 1;
    string s;
    cin >> p >> m >> n;
    vector<node> v, ans;
    map<string, int> idx;
    for(int i=0; i<p; i++){
        cin >> s >> score;
        if(score >= 200){
            v.push_back(node{s, score, -1, -1, 0});
            idx[s] = cnt++;
        }
    }
    for(int i=0 ;i<m; i++){
        cin >> s >> score;
        if(idx[s] != 0){
            v[idx[s]-1].gm = score;
        }
    }
    for(int i=0; i<n; i++){
        cin >> s >> score;
        int temp = idx[s]-1;
        if(idx[s] != 0){
            v[temp].gf = v[temp].g = score;
            if(v[temp].gm > v[temp].gf){
                v[temp].g = (int)(v[temp].gm*0.4 + v[temp].gf*0.6 + 0.5);
            }
        }
    }
    for(int i=0; i<v.size(); i++){
        if(v[i].g >= 60){
            ans.push_back(v[i]);
        }
    }
    sort(ans.begin(), ans.end(),cmp);
    for(int i=0; i<ans.size(); i++){
        printf("%s %d %d %d %d\n", ans[i].name.c_str(), ans[i].gp, ans[i].gm, ans[i].gf, ans[i].g);
    }
    return 0;
}
```
