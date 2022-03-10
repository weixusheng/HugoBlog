# PAT-Advanced 54-66



1054-1066
<!--more-->

# 1054-The Dominant Color
快乐白给题
```c++
#include<iostream>
using namespace std;

int main(int argc, char const *argv[])
{
    int a, b;
    int data[10000000] = {0};
    long int tmp;
    int cmax = 0;
    long int smax = 0;
    scanf("%d %d", &a, &b);
    for(int i=0; i<b; i++){
        for(int j=0; j<a; j++){
            scanf("%ld", &tmp);
            data[tmp] += 1;
            if(data[tmp] > cmax){
                cmax = data[tmp];
            }
            smax = tmp;
        }
    }
    printf("%ld", smax);
    return 0;
}
```

# 1055-The World’s Richest
这种排序题...真的是要提前构思好,否则写了一大堆,结果发现没排好就尴尬了
考察的是数据分析分组,合理排序的能力
这道题就是需要构造一个年龄的数组,将同一年龄的人放在一堆
```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;

struct node {
    char name[10];
    int age, money;
};
int cmp1(node a, node b) {
    if(a.money!=b.money)
        return a.money>b.money;
    else if(a.age!=b.age)
        return a.age<b.age;
    else return (strcmp(a.name, b.name) <0);
}
int main() {
    int n, k, num, amin, amax;
    scanf("%d %d", &n, &k);
    vector<node> vt(n), v;
    vector<int> book(205, 0);
    for(int i=0; i<n; i++)
        scanf("%s %d %d", vt[i].name, &vt[i].age, &vt[i].money);
    sort(vt.begin(), vt.end(), cmp1);
    for(int i=0; i<n; i++) {
        if(book[vt[i].age] <100) {
            v.push_back(vt[i]);
            book[vt[i].age]++;        
        }    
    }
    for(int i=0; i<k; i++) {
        scanf("%d %d %d", &num, &amin, &amax);
        vector<node> t;
        for(int j=0; j<v.size(); j++) {
            if(v[j].age>=amin&&v[j].age<=amax)
                t.push_back(v[j]); 
        }
        if(i!=0) 
            printf("\n");
        printf("Case #%d:", i+1);
        int flag=0;
        for(int j=0; j<num && j<t.size(); j++) {
            printf("\n%s %d %d", t[j].name, t[j].age, t[j].money);
            flag=1;        
        }
        if(flag==0) 
            printf("\nNone");    
    }
    return 0;
}
```

# 1056-Mice and Rice
花样分组,合并进组
逻辑考察
```c++
#include <iostream>
#include <queue>
#include <vector>
#include <algorithm>
using namespace std;

struct node {
    int weight, index, rank, index0;
};
bool cmp1(node a, node b) {
    return a.index0 < b.index0;
}
int main() {
    int n, g, num;
    scanf("%d%d", &n, &g);
    vector<int> v(n);
    vector<node> w(n);
    for(int i=0; i<n; i++)
        scanf("%d", &v[i]);
    for(int i=0; i<n; i++) {
        scanf("%d", &num);
        w[i].weight=v[num];
        w[i].index=i;
        w[i].index0=num;    
    }
    queue<node> q;
    for(int i=0; i<n; i++)
        q.push(w[i]);
    while(!q.empty()) {
        int size=q.size();
        if(size == 1) {
            node temp=q.front();
            w[temp.index].rank=1;
            break;        
        }
        int group=size/g;
        if(size%g != 0)
            group+=1;
        node maxnode;
        int maxn=-1, cnt=0;
        for(int i=0; i<size; i++) {
            node temp=q.front();
            w[temp.index].rank=group+1;
            q.pop();
            cnt++;
            if(temp.weight > maxn) {
                maxn=temp.weight;
                maxnode = temp;            
            }
            if(cnt==g || i==size-1) {
                cnt=0;
                maxn=-1;
                q.push(maxnode);            
            }        
        }    
    }
    sort(w.begin(), w.end(), cmp1);
    for(int i=0; i<n; i++) {
        if(i!=0) printf(" ");
        printf("%d", w[i].rank);
    }
    return 0;
}
```

# 1057-Stack
这道题...考察树状数组
以后等我回来再更吧...树状数组有点顶(柳神的方法)
但是也能用**分块思想**做(网上大佬的方法)
不得不说,分块思想对于这种数据集较大的情况,是一种非常nice的解决方案
```c++
#include<stack>
#include<algorithm>
#include<string>
#include<iostream>
using namespace std;

const int maxn = 100010;
const int sqrn = 316;

stack<int> st;

int block[sqrn];
int table[maxn];

void peekm(int k){
    int sum = 0;
    int idx = 0;
    while(sum + block[idx] < k){
        sum += block[idx];
    }
    int num = idx * sqrn;
    while(sum + table[num] < k){
        sum += table[num++];
    }
    printf("%d\n", num);
}

void push(int x){
    st.push(x);
    block[x/sqrn]++;
    table[x]++;
}

void pop(){
    int x = st.top();
    st.pop();
    block[x/sqrn]--;
    table[x]--;
    printf("%d", x);
}

int main(){
    int x, query;
    fill(block, block+316, 0);
    fill(table, table+100010, 0);
    string cmd;
    for(int i=0; i<query; i++){
        cin >> cmd;
        if(cmd == "Push"){
            scanf("%d", x);
            push(x);
        }
        else if(cmd == "Pop"){
            if(st.empty() == true){
                printf("Invalid\n");
            }
            else{
                pop();
            }
        }
        else{
            if(st.empty() == true){
                printf("Invalid\n");
            }
            else{
                int k = st.size();
                if(k%2 == 1){
                    k = (k+1)/2;
                }
                else{
                    k = k/2;
                }
                peekm(k);
            }
        }
    }
    return 0;
}
```

# 1058-A+B in Hogwarts
进制转换,没啥好说的,快乐就完事了
```c++
#include <iostream>
using namespace std;
int main() {
    long long a, b, c, d, e, f;
    scanf("%lld.%lld.%lld %lld.%lld.%lld", &a, &b, &c, &d, &e, &f);
    long long num = c+b*29+a*29*17+f+e*29+d*29*17;
    long long g=num/ (17*29);
    num = num% (17*29);
    printf("%lld.%lld.%lld", g, num/29, num%29);
    return 0;
}
```

# 1059-Prime Factors
这道题要求一个数可拆分为素数相加,因此绝妙的方法就是构建一个**素数表**
```c++
#include <cstdio>
#include <vector>
using namespace std;

vector<int> prime(500000, 1);
int main() {
    for(int i=2; i*i<500000; i++)   //构造一个素数表
        for(int j=2; j*i<500000; j++)
            prime[j*i] =0;
    long int a;
    scanf("%ld", &a);
    printf("%ld=", a);
    if(a==1) printf("1");
    bool state=false;
    for(int i=2; a>=2;i++) {
        int cnt=0, flag=0;
        while(prime[i] ==1 && a%i==0) {
            cnt++;
            a=a/i;
            flag=1;        
        }
        if(flag) {
            if(state) 
                printf("*");
            printf("%d", i);
            state=true;        
        }
        if(cnt>=2)
            printf("^%d", cnt);    
    }
    return 0;
}
```

# 1060-Are They Equal
考察科学计数法...这类题真是有些噩梦,坑点很多,需要慢慢调试
```c++
#include <iostream>
#include <cstring>
using namespace std;

int main() {
    int n, p=0, q=0;
    char a[10000], b[10000], A[10000], B[10000];    //用静态数组是一个很好的选择
    scanf("%d%s%s", &n, a, b);
    int cnta=strlen(a), cntb=strlen(b);
    for(int i=0; i<strlen(a); i++) {
        if(a[i] =='.') {
            cnta=i;
            break;        
        }    
    }
    for(int i=0; i<strlen(b); i++) {
        if(b[i] =='.') {
            cntb=i;
            break;        
        }    
    }
    int indexa=0, indexb=0;
    while(a[p] =='0'||a[p] =='.') 
        p++;
    while(b[q] =='0'||b[q] =='.') 
        q++;
    if(cnta >= p)
        cnta = cnta-p;
    else 
        cnta=cnta-p+1;
    if(cntb >= q)
        cntb=cntb-q;
    else
        cntb=cntb-q+1;
    if(p==strlen(a))
        cnta=0;
    if(q==strlen(b))
        cntb=0;
    while(indexa < n) {
        if(a[p] !='.'&& p<strlen(a))
            A[indexa++] = a[p];
        else if(p>=strlen(a))
            A[indexa++] ='0';
            p++;    
    }
    while(indexb < n) {
        if(b[q] !='.'&&q<strlen(b))
        B[indexb++] = b[q];
        else if(q>=strlen(b))
            B[indexb++] ='0';
            q++;    
    }
    if(strcmp(A, B) == 0 && cnta==cntb)
        printf("YES 0.%s*10^%d", A, cnta);
    else printf("NO 0.%s*10^%d 0.%s*10^%d" , A, cnta, B, cntb);
    return 0;
}
```

# 1061-Dating
考察字符串操作,稍微快乐
```c++
#include <iostream>
#include <cctype>
using namespace std;
int main() {
    string a, b, c, d;
    cin>>a>>b>>c>>d;
    char t[2];
    int pos, i=0, j=0;
    while(i<a.length() &&i<b.length()) {
        if (a[i] ==b[i] && (a[i] >='A'&&a[i] <='G')) {
            t[0] = a[i];
            break;        
        }
        i++;    
    }
    i=i+1;
    while (i<a.length() &&i<b.length()) {
        if (a[i] ==b[i] && ((a[i] >='A' && a[i] <='N') || isdigit(a[i]))) {
            t[1] =a[i];
            break;        
        }
        i++;    
    }
    while (j<c.length() &&j<d.length()) {
        if (c[j] ==d[j] && isalpha(c[j])) {
            pos=j;
            break;        
        }
        j++;    
    }
    string week[7] = {"MON ", "TUE ", "WED ", "THU ", "FRI ", "SAT ", "SUN "};
    int m = isdigit(t[1]) ? t[1] -'0' : t[1] -'A'+10;
    cout << week[t[0]-'A'];
    printf("%02d:%02d", m, pos);
    return 0;
}
```

# 1062-Talent and Virtue
英文版 乙级-德才论
考察排序和英文阅读能力(为何要这么难为小菜鸡...哭)
```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

struct node {
    int num, de, cai;
};

int cmp(node a, node b) {
    if ((a.de+a.cai) != (b.de+b.cai))
        return (a.de+a.cai) > (b.de+b.cai);
    else if (a.de!=b.de)
        return a.de>b.de;
    else return a.num<b.num;
}
int main() {
    int n, low, high;
    scanf("%d %d %d", &n, &low, &high);
    vector<node>v[4];
    node temp;
    int total=n;
    for (int i=0; i<n; i++) {
        scanf("%d %d %d", &temp.num, &temp.de, &temp.cai);
        if (temp.de<low||temp.cai<low)
            total--;
        else if (temp.de>=high && temp.cai>=high)
            v[0].push_back(temp);
        else if (temp.de>=high && temp.cai<high)
            v[1].push_back(temp);
        else if (temp.de<high && temp.cai<high && temp.de>=temp.cai)
            v[2].push_back(temp);
        else 
            v[3].push_back(temp);    
    }
    printf("%d\n", total);
    for (int i=0; i<4; i++) {
        sort(v[i].begin(), v[i].end(), cmp);
        for (int j=0; j<v[i].size(); j++)
            printf("%d %d %d\n", v[i][j].num, v[i][j].de, v[i][j].cai);
    }
    return 0;
}
```

# 1063-Set Similarity
白给快乐题
```c++
#include <cstdio>
#include <vector>
#include <set>
using namespace std;
int main() {
    int n, m, k, temp, a, b;
    scanf("%d", &n);
    vector<set<int>> v(n);
    for(int i=0; i<n; i++) {
        scanf("%d", &m);
        set<int> s;
        for(int j=0; j<m; j++) {
            scanf("%d", &temp);
            s.insert(temp);        
        }
        v[i] =s;    
    }
    scanf("%d", &k);
    for(int i=0; i<k; i++) {
        scanf("%d %d", &a, &b);
        int nc=0, nt=v[b-1].size();
        for(auto it=v[a-1].begin(); it!=v[a-1].end(); it++) {
            if(v[b-1].find(*it) == v[b-1].end())
                nt++;
            else 
                nc++;        
        }
        double ans= (double)nc/nt*100;
        printf("%.1f%%\n", ans);    
    }
    return 0;
}
```

# 1064-Complete Binary Search Tree
考察二叉搜索树
**搜索树的中序遍历就是从小到大的排列**
```c++
/*
二叉搜索树的中序遍历,转层序遍历
*/
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn = 1100;
//num存储有序数，即是完全二叉树的中序排列
//CBT存放完全二叉树的层次遍历 

int n,num[maxn],CBT[maxn],index = 0;

void inOrder(int root){
    if(root > n) return;
    inOrder(root*2);
    CBT[root] = num[index++];
    inOrder(root*2+1);
}

int main(){
    scanf("%d",&n);
    for(int i = 0; i < n; i++){
        scanf("%d",&num[i]);
    }
    sort(num,num+n);
    inOrder(1);
    for(int i = 1; i <= n; i++){
        printf("%d",CBT[i]);
        if(i < n) printf(" ");
    }
    return 0;
}
```

# 1065-A+B and C
没想到啊...还考这种数据范围的题,核心点在于,溢出的话,得到的结果就会为相反符号(+-)数
因为A、B的大小为[-2^63, 2^63]，用long long 存储A和B的值，以及他们相加的值sum：
如果A > 0, B < 0 或者 A < 0,  B > 0，sum是不可能溢出的
 - **如果A > 0, B > 0**，sum可能会溢出，sum范围理理应为(0, 2^64 – 2]**溢出得到的结果应该是[-2^63, -2] 是个负数**，所以sum < 0时候说明溢出了
 - **如果A < 0, B < 0**，sum可能会溢出，同理，**sum溢出后结果是大于0**的，所以sum > 0 说明溢出了

```c++
#include <cstdio>
using namespace std;
int main() {
    int n;
    scanf("%d", &n);
    for(int i=0; i<n; i++) {
        long long a, b, c;
        scanf("%lld %lld %lld", &a, &b, &c);
        long long sum=a+b;
        if(a>0 && b>0 && sum<0) {
            printf("Case #%d: true\n", i+1);
        } 
        else if(a<0 && b<0 && sum>=0){
            printf("Case #%d: false\n", i+1);
        } 
        else if(sum>c) {
            printf("Case #%d: true\n", i+1);
        } 
        else {
            printf("Case #%d: false\n", i+1);
        }    
    }
    return 0;
}
```

# 1066-Root of AVL Tree
考察...二叉平衡树的旋转(大脑一片空白,这也考吗...)
菜鸡记得常回来看看...
```c++
#include <iostream>
using namespace std;

struct node {
    int val;
    struct node*left, *right;
};
node* rotateLeft(node*root) {
    node*t = root->right;
    root->right = t->left;
    t->left = root;
    return t;
}
node* rotateRight(node*root) {
    node*t = root->left;
    root->left = t->right;
    t->right = root;
    return t;
}
node*rotateLeftRight(node*root) {
    root->left = rotateLeft(root->left);
    return rotateRight(root);
}
node*rotateRightLeft(node*root) {
    root->right = rotateRight(root->right);
    return rotateLeft(root);
}

int getHeight(node*root) {
    if(root==NULL) 
        return 0;
    return max(getHeight(root->left), getHeight(root->right)) +1;
}
node*insert(node*root, int val) {
    if(root==NULL) {
        root = new node();
        root->val = val;
        root->left = root->right = NULL;
    } 
    else if(val<root->val) {
        root->left = insert(root->left, val);
        if(getHeight(root->left) - getHeight(root->right) ==2)
            root = val < root->left->val ? rotateRight(root) :rotateLeftRight(root);    
        } 
        else {
            root->right = insert(root->right, val);
        if(getHeight(root->left) -getHeight(root->right) ==-2)
            root=val > root->right->val ? rotateLeft(root) :rotateRightLeft(root);    
        }
        return root;
}
int main() {
    int n, val;
    scanf("%d", &n);
    node*root = NULL;
    for(int i=0; i<n; i++) {
        scanf("%d", &val);
        root = insert(root, val);    
    }
    printf("%d", root->val);
    return 0;
}
```

