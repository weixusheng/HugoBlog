# PAT-Advanced 22-25



1022-1025<br>
甲级慢慢上道啦!
<!--more-->

# 1022-Digital Library
考察 map一对多映射,只需要构造 map<string,vector<int> > 结构 
```c++
#include <iostream>
#include <map>
#include <set>
using namespace std;
map<string, set<int> >title, author, key, pub, year;
void query(map<string, set<int> >&m, string&str) {
    if(m.find(str) !=m.end()) {
        for(auto it=m[str].begin(); it!=m[str].end(); it++)
            printf("%07d\n", *it);    
    } 
    else 
        cout<<"Not Found\n";
}
int main() {
    int n, m, id, num;
    scanf("%d", &n);
    string ttitle, tauthor, tkey, tpub, tyear;
    for(int i=0; i<n; i++) {
        scanf("%d\n", &id);
        getline(cin, ttitle);
        title[ttitle].insert(id);
        getline(cin, tauthor);
        author[tauthor].insert(id);
        while(cin >> tkey) {
            key[tkey].insert(id);
            charc=getchar();
            if(c=='\n') 
                break;        
        }
        getline(cin, tpub);
        pub[tpub].insert(id);
        getline(cin, tyear);
        year[tyear].insert(id);
    }
    scanf("%d", &m);
    for(int i=0; i<m; i++) {
        scanf("%d: ", &num);
        string temp;
        getline(cin, temp);
        cout<<num<<": "<<temp<<"\n";
        if(num==1) 
            query(title, temp);
        else if(num==2) 
            query(author, temp);
        else if(num==3) 
            query(key, temp);
        else if(num==4) 
            query(pub,temp);
        else if(num==5) 
            query(year, temp);
    }
    return 0;
}
```

# 1023-Have Fun with Numbers
快乐的手动进制运算,驾轻就熟
```c++
#include <cstdio>
#include <string>
#include<iostream>
using namespace std;

int book[10];
int main() {
    string s;
    getline(cin,s);
    int flag=0, len = s.size();
    for(int i=len-1; i>=0; i--) {
        int temp = s[i] -'0';
        book[temp]++;
        temp = temp*2+flag;
        flag=0;
        if(temp>=10) {
            temp=temp-10;
            flag=1;        
        }
        s[i] = (temp+'0');
        book[temp]--;    
    }
    int flag1=0;
    for(int i=0; i<10; i++) {
        if(book[i] !=0)
            flag1=1;    
    }
    printf("%s", (flag==1||flag1==1) ?"No\n" : "Yes\n");
    if(flag==1) 
        printf("1");
    cout << s;
    return 0;
}
```

# 1024-Palindromic Number
跟上道题一样的套路,对于回文数的判断,直接逆置直接比较就好
```c++
#include<string>
#include<algorithm>
#include<iostream>

using namespace std;

bool isshit(string s){
    string s2  = s;
    reverse(s.begin(),s.end());
    if(s == s2){
        return true;
    }
    return false;
}

int main(int argc, char const *argv[])
{
    string s,s2;
    int num, carry;
    cin >> s >> num;
    bool shit;
    shit = isshit(s);
    if(shit){
        cout << s << endl;
        cout << "0";
        return 0;
    }
    for(int j=1; j<=num; j++){
        s2 = s;
        reverse(s2.begin(), s2.end());
        int tmp;
        carry = 0;
        for(int i=s.size()-1; i>=0; i--){
            tmp = (s[i]-'0')+(s2[i]-'0');
            tmp += carry;
            carry = 0;
            if(tmp >10){
                carry = 1;
            }
            tmp = tmp%10;
            s[i] = tmp+'0';
        }
        if(carry == 1){
            s = "1"+s;
        }
        shit = isshit(s);
        if(shit || j == num){
            cout << s << endl;
            cout << j;
            break;
        }
    }
    return 0;
}
```

# 1025-PAT Ranking
这道题让我很开心,直接ac啦,甲级也有点慢慢上道了呢!
对于这种排序,搞结构体肯定没有错
```c++
#include<iostream>
#include<vector>
#include<string>
#include<algorithm>
#include<cstring>

using namespace std;

struct node{
    string id;
    int score;
    int area;
    int rank;
    int localrank;
};

bool cmp(node a, node b){
    if(a.score == b.score){
        return a.id < b.id;
    }
    else{
        return a.score > b.score;
    }
}

int main(int argc, char const *argv[])
{
    int a, num;
    cin >> a;
    vector<vector<node> > data(a);
    vector<node> res;
    string id;
    int score;
    int pre;
    int rank;
    int tmprank;
    for(int i=0; i<a; i++){
        cin >> num;
        for(int j=0; j<num; j++){
            cin >> id >> score;
            data[i].push_back({id, score, i, 0, 0});
        }
        sort(data[i].begin(), data[i].end(),cmp);
        pre = 101;
        rank = 1;
        for(auto it = data[i].begin(); it!=data[i].end(); it++){
            if(it->score != pre){
                pre = it->score;
                it->localrank = rank; 
                tmprank = rank;
            }
            else{
                it->localrank = tmprank;
            }
            rank++;
        }
        for(int k=0; k<num; k++){
            res.push_back(data[i][k]);
        }
    }
    sort(res.begin(), res.end(), cmp);
    printf("%d\n", res.size());
    pre = 101;
    rank = 1;
    for(int i=0; i<res.size(); i++){
        //1234567890005 1 1 1
        if(res[i].score == pre){
            res[i].rank = tmprank;
        }
        else{
            res[i].rank = rank;
            tmprank = rank;
            pre = res[i].score;
        }
        rank++;
        node shit = res[i];
        printf("%s %d %d %d\n", shit.id.c_str(), shit.rank, shit.area+1, shit.localrank);
    }
    return 0;
}
```
