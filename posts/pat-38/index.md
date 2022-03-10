# PAT-Advanced 36-40



1036-1040
<!--more-->

# 1036-Boys vs Girls
边界查询比较,不算太难
```c++
#include <iostream>
using namespace std;
int main() {
    int n;
    scanf("%d", &n);
    string female, male;
    int femalescore=-1, malescore=101;
    for(int i=0; i<n; i++) {
        string name, sex, num;
        int score;
        cin>>name>>sex>>num;
        scanf("%d", &score);
        if(sex=="F") {
            if(femalescore<score) {
                femalescore=score;
                female=name+" "+num;
            }        
        } 
        else if(malescore>score) {
            malescore=score;
            male=name+" "+num;
        }    
    }
    if(femalescore!=-1)
        cout<<female<<endl;
    else 
        printf("Absent\n");
    if(malescore!=101)
        cout<<male<<endl;
    else 
        printf("Absent\n");
    if(femalescore!=-1&&malescore!=101)
        printf("%d", femalescore-malescore);
    else 
        printf("NA");
    return 0;
}
```

# 1037-Magic Coupon
代码实现上不难,不过逻辑有些难想(贪心算法)
```c++
#include <cstdio>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    int m, n, ans=0, p=0, q=0;
    scanf("%d", &m);
    vector<int> v1(m);
    for(int i=0; i<m; i++)
        scanf("%d", &v1[i]);
    scanf("%d", &n);
    vector<int> v2(n);
    for(int i=0; i<n; i++)
        scanf("%d", &v2[i]);
    sort(v1.begin(), v1.end());
    sort(v2.begin(), v2.end());
    while(p<m && q<n && v1[p] <0 && v2[q] <0) {
        ans+=v1[p] *v2[q];p++;
        q++;
    }
    p=m-1, q=n-1;
    while(p>=0 && q>=0 && v1[p] >0 && v2[q] >0) {
        ans+=v1[p] *v2[q];
        p--; 
        q--;    
    }
    printf("%d", ans);
    return 0;
}
```

# 1038-Recover the Smallest Number
甲级压轴题都好难...哭了,向压轴题低头
这道题用到了贪心算法...不过也太难想了
因为要将多个不同长度的数字字符组成为一个总体值最小的,就用到了**区间相邻相加并排序**的方式来实现
```c++
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

bool cmp0(string a, string b) {
    return a+b<b+a;
}
string str[10010];
int main() {
    int n;
    scanf("%d", &n);
    for(int i=0; i<n; i++)
        cin>>str[i];
    sort(str, str+n, cmp0);
    string s;
    for(int i=0; i<n; i++)
        s+=str[i];
    while(s.length() !=0&&s[0] =='0')
        s.erase(s.begin());
    if(s.length() ==0) 
        cout<<0;
    cout<<s;
    return 0;
}
```

# 1039-Course List for Student
这道题是对stl的考察

## scanf(string)的方法
首先要对string申请空间
```c++
string a;
a.resize(10);
```
之后在scanf中填入字符串的首地址
```c++
scanf("%s", &a[0]);
```

## CODE
```c++
#include<iostream>
#include<map>
#include<vector>
#include<string>
#include<algorithm>
using namespace std;

int main(int argc, char const *argv[])
{
    map<string, vector<int> > data;
    int n, k, no, num;
    string name;
    name.resize(30);
    //cin >> n >> k;
    scanf("%d %d", &n, &k);
    for(int i=0; i<k; i++){
        scanf("%d %d", &no, &num);
        for(int j=0; j<num; j++){
            //cin >> name;
            scanf("%s", &name[0]);
            data[name].push_back(no);
        }
    }
    for(int i=0; i<n; i++){
        //cin >> name;
        scanf("%s", &name[0]);
        sort(data[name].begin(), data[name].end());
        //cout << name << " " << data[name].size();
        printf("%s %d", name.c_str(), data[name].size());
        for(int k=0; k<data[name].size(); k++){
            printf(" %d", data[name][k]);
        }
        printf("\n");
    }
    return 0;
}
```

# 1040-Longest Symmetric String
动态规划经典题
由于动态规划中一大核心思想就是,保存局部值,以便在更大区间时,可以加以利用
而这道题就是,构建一个二维数组,来保存区间是否对称,并且利用两个for循环,在**小区间的基础上对更大区间进行判断**,并更新二维数组的值
虽然看懂了代码,但下次让我写,我可能还是写不出来...
以后多复习吧
```c++
#include<iostream>
#include<vector>
#include<string>
using namespace std;

int data[1010][1010];

int main(int argc, char const *argv[])
{
    string s;
    getline(cin, s);
    int len = s.size(), ans = 1;
    for(int i=0; i<len; i++){
        data[i][i] = 1;
        if(s[i] == s[i+1]){
            data[i][i+1] = 1;
            ans = 2;
        }
    }
    for(int shit = 3; shit <= len; shit++){
        for(int i=0; i<len; i++){
            int j = i+shit-1;
            if(s[i] == s[j] && data[i+1][j-1] == 1){
                data[i][j] = 1;
                ans = shit;
            }
        }
    }
    printf("%d", ans);
    return 0;
}
```
