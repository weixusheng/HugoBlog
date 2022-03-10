# PAT-Basic 考完啦!


没有满分,只考了96(排名40/500+)...但不得不说三个小时的解题过程跟过山车一般,一开始83分,差点以为这次考试要翻车了!不过还好一直沉下心debug...最后前四道题全部AC,但最后一道题有一个测试点超时了,因为方法过于暴力(菜鸡猛锤自己大腿),但我也服吧...毕竟代码能力还有待提高!
最后感觉这个分数还算满意吧!只复习了一个月,从一开始的C都用不明白,到现在能挣扎一下了,而且之前模拟考试一次94,一次96(这次考试没翻车就好嘿嘿)
下一步等证书邮寄到家啦!
让人欣慰的是终于能开个甲级专栏了!
备战九月甲级PAT冲冲冲!
<!--more-->

**下面把这次考试的代码放上吧,蛮有意义的!**
# 第一道题
```c++
#include<cstdlib>
#include<map>
#include<iostream>
#include<string>
#include<vector>
#include<cstdio>

using namespace std;

int main(int argc, char const *argv[])
{
    vector<string> month = {"Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"};
    int d, y,num;
    char m[20];
    cin >> num;
    for(int i=0; i<num; i++){
        scanf("%s %d, %d",&m,&d,&y);
        //find month
        int tmpm;
        string tmp;
        char data[100];
        for(int j=0; j<12; j++){
            if(month[j] == m){
                tmpm = j+1;
                break;
            }
        }
        sprintf(data,"%04d%02d%02d",y,tmpm,d);
        tmp = data;
        int k;
        for(k=0; k<4; k++){
            if(tmp[k] != tmp[7-k]){
                cout << "N ";
                break;
            }
        }
        if(k == 4){
            cout << "Y ";
        }
        cout << tmp << endl;
    }
    system("pause");
    return 0;
}
```

# 第二道题
```c++
#include<cstdlib>
#include<iostream>

using namespace std;

int main(int argc, char const *argv[])
{
    int num, k;
    cin >> num >> k;
    int tmp;
    int l,r;
    int max = 0;
    bool comb = false, has = false;
    for(int i=0; i<num; i++){
        cin >> tmp;
        if(tmp > max){
            max = tmp;
        }
        if(comb == false && tmp>k){
            comb = true;
            l = i;
            r = i;
        }
        else if(comb == true && tmp>k){
            r = i;
        }
        else if(tmp <= k){
            if(comb){
                comb = false;
                printf("[%d, %d]\n",l,r);
                has = true;
            }
            else{
                continue;
            }
        }
    }
    if(comb == true){
        printf("[%d, %d]\n",l,r);
        has = true;
    }
    if(!has){
        printf("%d\n", max);
    }
    system("pause");
    return 0;
}
```

# 第三道题
```c++
#include<iostream>
#include<cstdlib>
#include<algorithm>
#include<string>
#include<vector>
#include<cctype>

using namespace std;

int converts(char a){
    if(isdigit(a)){
        return a-'0';
    }
    else{
        return a-'a'+10;
    }
}

//vector<char> v = {'a','b',c,d,e,f,g,h,i,j,k}

char cons(int a){
    if(a<10){
        return a+'0';
    }
    else
    {
        return 'a'+(a-10);
    }
}

int main(int argc, char const *argv[])
{
    string a,b;
    cin >> a >> b;
    int sum;
    if(a.size()>b.size()){
        sum = a.size();
        int shit = b.size();
        for(int i=0; i<a.size()-shit; i++){
            b = "0"+b;
        }
    }
    else{
        sum = b.size();
        int shit = a.size();
        for(int i=0; i<b.size()-shit; i++){
            a = "0"+a;
        }
    }
    reverse(a.begin(),a.end());
    reverse(b.begin(),b.end());
    char data[1000000];
    int carry = 0;
    int j;
    for(j=0; j<sum; j++){
        int m = converts(a[j]);
        int n = converts(b[j]);
        int shit = (m+n+carry)%30;
        carry = (m+n+carry)/30;
        char shit2 = cons(shit);
        data[j] = shit2;
    }
    if(carry != 0){
        data[j] = carry+'0';
    }
    string res = data;
    // 11feik2ir
    reverse(res.begin(),res.end());
    int cnt = 0;
    for(int i=0; i<res.size(); i++){
        if(res[i] == '0'){
            cnt++;
        }
        else{
            break;
        }
    }
    res = res.substr(cnt,res.size()-cnt);
    //cout << res;
    if(res == ""){
        cout << "0";
    }
    else{
        cout << res;
    }
    system("pause");
    return 0;
}
```

# 第四道题
```c++
#include<string>
#include<iostream>
#include<algorithm>
#include<cstdlib>
#include<cmath>

using namespace std;

bool iss(int a){
    if(a == 1){
        return false;
    }
    int b = (int)sqrt(double(a));
    for(int i=2; i<b; i++){
        if(a%i == 0){
            return false;
        }
    }
    return true;
}

int main(int argc, char const *argv[])
{
    bool has = false;
    string s;
    cin >> s;
    for(int i=0; i<s.size(); i++){
        string a = s.substr(i,s.size()-i);
        if(iss(stoi(a))){
            cout << a << " " << "Yes" << endl;
        }
        else
        {
            cout << a << " " << "No" << endl;
            has = true;
        }
    }
    if(!has){
        cout << "All Prime!";
    }
    system("pause");
    return 0;
}
```

# 第五道题
没有AC,一个测试点超时了
```c++
#include<set>
#include<cstdlib>
#include<iostream>
#include<vector>
#include<algorithm>
#include<cmath>
#include<cstdio>

using namespace std;

int main(int argc, char const *argv[])
{
    int s1,s2;
    //scanf("%d %d",&s1,&s2);
    cin >> s1 >> s2;
    int n, m;
    cin >> n >> m;
    //scanf("%d %d",&n,&m);
    vector<bool> shit(n,false);
    set<int> data1; //输入的
    set<int> data2; //差值 
    data2.insert(abs(s1-s2));
    data1.insert(s1);
    data1.insert(s2);
    vector<vector<int>> res;
    res.resize(n,vector<int>(m));
    for(int i=0; i<n; i++){
        for(int j=0; j<m; j++){
            int tmp;
            cin >> tmp;
            res[i][j] = tmp;
        }
    }
    for(int i=0; i<m; i++){
        for(int j=0; j<n; j++){
            if(!shit[j]){
                int tmp = res[j][i];
                if(data1.find(tmp) == data1.end() && data2.find(tmp) != data2.end()){
                    for(auto k=data1.begin(); k!= data1.end(); k++){
                        data2.insert(abs(*k-tmp));
                        data1.insert(tmp);
                    }
                }
                else{
                    printf("Round #%d: %d is out.\n",i+1,j+1);
                    shit[j] = true;
                }
            }
        }
    }
    bool has = false;
    for(int h=0; h<n; h++){
        if(shit[h] == false){
            has = true;
        }
    }
    if(has){
        printf("Winner(s):");
        for(int w=0; w<n; w++){
            if(!shit[w]){
                printf(" %d",w+1);
                //cout << " " << w+1;
            }
        }
    }
    else{
        printf("No winner.");
    }
    system("pause");
    return 0;
}
```
