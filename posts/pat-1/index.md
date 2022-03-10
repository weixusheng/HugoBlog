# PAT-Basic 01~10



备考7月份的PAT! 冲鸭<br>
B1001-B1010<br>
2020-07-18 优化题解 
<!--more-->

# B1001-害死人不偿命的(3n+1)猜想
没啥好说的...
```c
#include<stdlib.h>
#include<stdio.h>
int main(){
    int input_num;
    scanf("%d",&input_num);
    int count = 0;
    if(input_num <= 1000){
        while(input_num != 1){
            if(input_num%2 == 1){
                input_num = (3*input_num +1)/2;
            }
            else{
                input_num = input_num/2;
            }
            count++;
        }
    }
    printf("%d", count);
}
```

# B1002-写出这个数
## C++
```c++
#include<cstdlib>
#include<string>
#include<iostream>
using namespace std;

int main(int argc, char const *argv[])
{
    string s;
    cin >> s;
    int sum = 0;
    string str[10] =  {"ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu"};
    for(int i=0; i<s.length(); i++){
        sum += s[i]-'0';
    }
    string res = to_string(sum);
    for(int j=0; j<res.length(); j++){
        if(j != 0){
            cout << " "; 
        }
        cout << str[res[j]-'0'];
    }
    system("pause");
    return 0;
}
```

## C
```c
int main(){
    char pinyin[10][5] = {"ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu"};
    char getnum;
    int sum;
    while((getnum = getchar())!= '\n'){
        sum += getnum - '0';
    }
    char res[4];
    sprintf(res, "%d", sum);
    for(int i=0; res[i]!=0; i++){
        //printf("%s\n", res[i]);
        if(i != 0){
            printf(" ");
        }
        printf("%s", pinyin[res[i]-'0']);
    }
    return 0;
}
```
这道题有一个很有用的函数 sprintf,将int打印到一个char数组中,判定结尾为
```c
//res为char数组
res[i]!=0
```
还有一个很好的判断,学到了
```
while((getnum = getchar())!= '\n')
```
这道题思路很好,值得反复推敲

# B1003-我要通过！
我愿称他为一道快乐计数题
并且题目我没读懂,汉语的魅力让我沉醉
```c++
题目解释:
关于上面正确字符串的定义, 这是典型的递归定义, 第1点和第2点是初始条件, 第3点是归纳条件. 来说下显然可以得到的结论, 
正确字符串必定有且只有一个P和一个T, 其余的都是A, 并且P在T的前面, 更具体地, 正确字符串必定是A...APA...ATA...A的模式. 
仔细观察第3点, aPbATca和aPbTc的区别在于前者的P和T之前多了个A, T之后多了a, 可以想象, 面对一个给定字符串, 先要数出P之前有多少个A,
P和T之间有多少个A, T之后有多少个A, 首先判断是否符合第2点, 不符合就在T之后去掉与P之前相同数量的A, 在P和T之间去掉一个A, 
然后再判断剩下来的字符串是否为正确字符串, 重复若干次去掉的操作, 最后肯定归结到是否符合第2点. 但是有更好的办法, 
既然每次都是去掉一个中间A和一个尾部a, 想象最后得到的aPATa形式, 将P前面A的数量设为x, P和T之间A的数量设为1, 那么每增加一个中间A就增加一个尾部a,
假设增加y次得到给定的字符串. 对于给定的字符串, P前面A的数量仍为x, P和T之间A的数量变为1+y, T之后A的数量变为x+xy=x(1+y), 那么给了一个字符串, 
判断 中间A的数量 是否大于0而且 T之后A的数量 是否等于 P前A的数量 乘以 P和T之间A的数量 就可以了. 
```
```c++
#include <iostream>
#include <map>
using namespace std;
int main() {
    int n, p = 0, t = 0;
    string s;
    cin >> n;
    for(int i = 0; i < n; i++) {
        cin >> s;
        map<char, int> m;
        for(int j = 0; j < s.size(); j++) {
            m[s[j]]++;
            if (s[j] == 'P') p = j;
            if (s[j] == 'T') t = j;
        }
        if (m['P'] == 1 && m['A'] != 0 && m['T'] == 1 && m.size() == 3 && t-p != 1 && p * (t-p-1) == s.length()-t-1)
            printf("YES\n");
        else
            printf("NO\n");
    }
    return 0;
}
```

# B1004-成绩排名
没啥好说的...
```c
int main(){
    int stnum,score;
    scanf("%d", &stnum);
    char max_name[20],max_no[20],min_name[20],min_no[20],temp_name[20], temp_no[20];
    int max = -1, min=101;
    for(int i=0; i< stnum; i++){
            scanf("%s %s %d", temp_name, temp_no, &score);
            if(score > max){
                max = score;
                strcpy(max_name,temp_name);
                strcpy(max_no,temp_no);
            }
            if(score < min){
                min = score;
                strcpy(min_name,temp_name);
                strcpy(min_no,temp_no);
            }
    }
    printf("%s %s\n", max_name, max_no);
    printf("%s %s\n", min_name, min_no);
    return 0;
}
```

# B1005-继续(3n+1)猜想
## 菜鸡的方法
```c++
#include<cstdlib>
#include<iostream>
#include<set>
#include<vector>
#include<algorithm>

using namespace std;

int main(int argc, char const *argv[])
{
    int num, tmp;
    cin >> num;
    set<int> set1;
    vector<int> data;
    for(int i=0; i<num; i++){
        cin >> tmp;
        data.push_back(tmp);
        while(tmp != 1){
            if(tmp % 2 == 1){
                tmp = (3*tmp+1)/2;
            }
            else{
                tmp = tmp/2;
            }
            if(set1.find(tmp) != set1.end()){
                break;
            }
            set1.insert(tmp);
        }
    }
    vector<int> res;
    for(int i=0; i<num; i++){
        if(set1.find(data[i]) == set1.end()){
            res.push_back(data[i]);
        }
    }
    sort(res.begin(),res.end(),greater<int>());
    for(int j=0; j<res.size(); j++){
        if(j != 0){
            cout << " "; 
        }
        cout << res[j];
    }
    system("pause");
    return 0;
}
```

## 别人的方法
```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int arr[10000];
bool cmp(int a, int b) {return a > b;}
int main() {
    int k, n, flag = 0;
    cin >> k;
    vector<int> v(k);
    for (int i = 0; i < k; i++) {
        cin >> n;
        v[i] = n;
        while (n != 1) {
            if (n % 2 != 0) n = 3 * n + 1;
            n = n / 2;
            if (arr[n] == 1) break;
            arr[n] = 1;
        }
    }
    sort(v.begin(), v.end(), cmp);
    for (int i = 0; i < v.size(); i++) {
        if (arr[v[i]] == 0) {
            if (flag == 1) cout << " ";
            cout << v[i];
            flag = 1;
        }
    }
    return 0;
}
```

# B1006-换个格式输出整数
[题目](https://pintia.cn/problem-sets/994805260223102976/problems/994805318855278592)
这道题思路很棒,通过除和取余运算,将int数值每位装进一个char数组中,然后进行单个位操作
```c
int main(){
    int j,num;
    scanf("%d", &num);
    int i = 0;
    int res[3] = {0};
    while(num != 0){
        res[i++] = num%10;
        num = num/10;
    }
    for(j=0; j<res[2]; j++){
        printf("%s", "B");
    }
    for(j=0; j<res[1]; j++){
        printf("%s", "S");
    }
    for(j=0; j<res[0]; j++){
        printf("%d", j+1);
    }
    return 0;
}
```

# B1007-素数对猜想
这道数学题让我蒙了呀,暴力超时
原来素数有一种快的求法,我太菜辣
## 求素数
 - m 不必被 2 ~ m-1 之间的每一个整数去除，只需被 2 ~ $\sqrt{m}$ 之间的每一个整数去除就可以了。如果 m 不能被 2 ~ $\sqrt{m}$ 间任一整数整除，m 必定是素数。例如判别 17 是是否为素数，只需使 17 被 2~4 之间的每一个整数去除，由于都不能整除，可以判定 17 是素数。
 - **原因**：因为如果 m 能被 2 ~ m-1 之间任一整数整除，其二个因子必定有一个小于或等于 $\sqrt{m}$，另一个大于或等于 $\sqrt{m}$。例如 16 能被 2、4、8 整除，16=2*8，2 小于 4，8 大于 4，16=4*4，4=$\sqrt{16}$，因此只需判定在 2~4 之间有无因子即可。
[参考文章](http://c.biancheng.net/view/498.html)

```c
int main()
{
    int n,k,prime,nprime;
    int i,j,f;
    scanf("%d",&n);
    k==0;prime=3;nprime=3;
    for(i=5;i<=n;i+=2){
        f=0;
        for(j=2;j*j<=i;j++){
            if(i%j==0){
                f=1;    //求素数
                break;
            }
        }
        if(f==0){
            nprime = i;
            if(nprime-prime==2)
                k++;
            prime=nprime;
        }
    }
    printf("%d\n",k);
    return 0;
}
```

# B1008-数组元素循环右移问题
这道题我有两种解法嘿嘿
第一种方法是用一个大大的数组,提供可以移动的空间,数组内部移动
第二种是用c的qsort函数,数组内部排序
## 大空间移动法
```c
int main(){
    int N,M;
    scanf("%d %d", &N, &M);
    int data[300];
    M = M%N;
    for(int i = 0; i< N; i++){
        scanf("%d", &data[i]);
    }
    int a = N;
    int b = N-M;
    for(int j=0; j<b; j++){
        data[a] = data[j];
        a++;
    }
    for(int k=0; k< N; k++){
        if(k != 0){
            printf(" ");
        }
        printf("%d", data[b]);
        b++;
    }
    return 0;
}
```

## 快排法
```c
int cmp_int(const void* a , const void* b)
{
    return ( *(int*)a - *(int*)b );
}

int main(){
    int N,M;
    scanf("%d %d", &N, &M);
    int data[N];
    M = M%N;
    for(int i = 0; i< N; i++){
        scanf("%d", &data[i]);
    }
    int a = N-M;
    int temp;
    for(int i=0; i<M; i++){
        temp = data[i];
        data[i] = data[a];
        data[a] = temp;
        a++;
    }
    qsort(&data[M], N-M, sizeof(int),cmp_int);
    for(int k = 0; k< N; k++){
        printf("%d", data[k]);
    }
    return 0;
}
```

## reverse()函数
```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;
    vector<int> a(n);
    for (int i = 0; i < n; i++)
        cin >> a[i];
    m %= n;
    if (m != 0) {
        reverse(begin(a), begin(a) + n);
        reverse(begin(a), begin(a) + m);
        reverse(begin(a) + m, begin(a) + n);
    }
    for (int i = 0; i < n - 1; i++)
        cout << a[i] << " ";
    cout << a[n - 1];
    return 0;
}
```

# B1009-说反话
这道题我也有三种解法
没错,其中一种就是我的笨方法
## C++栈
```c++
#include <iostream>
#include <stack>
using namespace std;

int main() {
    stack<string> v;
    string s;
    while(cin >> s) v.push(s);
    cout << v.top();
    v.pop();
    while(!v.empty()) {
        cout << " " << v.top();
        v.pop();
    }
    return 0;
}
```

## 字符串截至法
竟然可以把字符串一个位置直接赋值'\0'截断
这是我没想到的操作,学到了(我太菜辣啊!)
```c
int main(){
    int i;
    char data[81];
    gets(data);
    for(i=strlen(data)-1; i>=0; i--){
        if(data[i] == ' '){
            printf("%s", &data[i+1]);
            printf("%c", data[i]);
            data[i] = '\0';
        }
        if(i==0){
            printf("%s", &data[i]);
        }
    }
}
```

## 笨方法
搞一个超大数组,然后数组内操作,这是我从之前方法学到的
但是跟字符串截断相比...算了不比了
```c
int main(){
    char word[162];
    int word_count = 0;
    gets(word);
    int p = 0;
    int q = 161;
    while(word[p] != '\0'){
        while(word[p]!= ' ' && word[p]!= '\0'){
            word_count++;
            p++;
        }
        for(int i=1; i<word_count+1; i++){
            word[q] = word[p-i];
            q--;
        }
        ++p;
        word_count = 0;
        word[q] = ' ';
        q--;
    }
    int start = 161-p+2;
    for(int k=1; k<p; k++){
        printf("%c", word[start]);
        start++;
    }
    return 0;
}
```

# B1010-一元多项式求导
这道题就是之前说到的用flag判断输出空格
另外getchar()判断输入结尾,属实厉害嗷,学到了
c++就简便很多了,快乐c++
## C++
```c++
#include <iostream>
using namespace std;
int main() {
    int a, b, flag = 0;
    while (cin >> a >> b) {
        if (b != 0) {
            if (flag == 1) cout << " ";
            cout << a * b << " " << b - 1;
            flag = 1;
        }
    }
    if (flag == 0) cout << "0 0";
    return 0;
}
```

## 菜鸡的方法
```c++
#include<cstdlib>
#include<iostream>
using namespace std;

int main(int argc, char const *argv[])
{
    char ch;
    int tmp1, tmp2;
    bool flag = false;
    while(1){
        cin >> tmp1 >> tmp2;
        if(tmp2 != 0){
            if(flag){
                cout << " ";
            }
            else{
                flag = true;
            }
            cout << tmp1*tmp2 << " " << tmp2-1;
        }
        if((ch = getchar()) == '\n'){
            break;
        }
    }
    if (flag == false) cout << "0 0";
    system("pause");
    return 0;
}
```

## C实现
```c
int main(){
    int a,b;
    int flag=0;
    char ch;
    while(1){
        scanf("%d%d",&a,&b);
        if(b!=0){
            if(flag==0) printf("%d",a*b);
            else printf(" %d",a*b);
            printf(" %d",b-1);
            flag=1;
        }
        ch = getchar();
        if(ch == '\n'){
            break;
        }
    }
    if(flag==0) printf("0 0");
    //flag为0代表，所有的输入指数都为0，如果不为0的话
    return 0;//就置1了。
}
```
