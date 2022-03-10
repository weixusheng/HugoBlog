# PAT-Basic 44~52



B1044-B1052<br>
刷完一半啦!继续冲!<br>
7.23更新
<!--more-->

# 1044-火星数字
这道题有点复杂,需要对数字和字母分别判断,并写两个转换函数
 - 对于判断char是否为数字,可以使用isdigit()函数,对于是否为字母,可以使用isalpha()函数
 - 字符串截取 substr(pos,length); pos为起始位置,length为截取长度
 - 数字 string转int: stoi()函数

## atoi()和stoi() 
都是C++的字符处理函数，把数字字符串转换成int输出
头文件都是#include<cstring>
**不同点：**
1. atoi()的参数是 const char* ,因此对于一个字符串str我们必须调用 c_str()的方法把这个string转换成 const char*类型的,而stoi()的参数是const string*,不需要转化为 const char*；
如图：
2. stoi()会做范围检查，默认范围是在int的范围内的，如果超出范围的话则会runtime error！

## CODE
```c++
#include <iostream>
#include <string>
using namespace std;

string a[13] = {"tret", "jan", "feb", "mar", "apr", "may", "jun", "jly", "aug", "sep", "oct", "nov", "dec"};
string b[13] = {"####", "tam", "hel", "maa", "huh", "tou", "kes", "hei", "elo", "syy", "lok", "mer", "jou"};
string s;
int len;
void func1(int t) {
    if (t / 13) cout << b[t / 13];
    if ((t / 13) && (t % 13)) cout << " ";
    if (t % 13 || t == 0) cout << a[t % 13];
}
void func2() {
    int t1 = 0, t2 = 0;
    string s1 = s.substr(0, 3), s2;
    if (len > 4) s2 = s.substr(4, 3);
    for (int j = 1; j <= 12; j++) {
        if (s1 == a[j] || s2 == a[j]) t2 = j;
        if (s1 == b[j]) t1 = j;
    }
    cout << t1 * 13 + t2;
}
int main() {
    int n;
    cin >> n;
    getchar();
    for (int i = 0; i < n; i++) {
        getline(cin, s);
        len = s.length();
        if (isdigit(s[0]))
            func1(stoi(s));
        else
            func2();
        cout << endl;
    }
    return 0;
}
```

# 1045-快速排序
这道压轴题很有意思...(为什么压轴全是数学题哭)
用了两个序列,分别是max序列和min序列
max从前往后取,min序列从后向前取,最后比较两个序列

## 柳神
太强了...我实在是想不出来呀
```c++
#include <iostream>
#include <algorithm>
#include <vector>
int v[100000];
using namespace std;
int main() {
    int n, max = 0, cnt = 0;
    scanf("%d", &n);
    vector<int> a(n), b(n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
        b[i] = a[i];
    }
    sort(a.begin(), a.end());
    for (int i = 0; i < n; i++) {
        if(a[i] == b[i] && b[i] > max)
            v[cnt++] = b[i];
        if (b[i] > max)
            max = b[i];
    }
    printf("%d\n", cnt);
    for(int i = 0; i < cnt; i++) {
        if (i != 0) printf(" ");
        printf("%d", v[i]);
    }
    printf("\n");
    return 0;
}
```

# 第二种方法
```c++
#include <stdio.h>

int main()
{
    int N, count = 0;
    int array[100000], lmax[100000], rmin[100000];
    scanf("%d", &N);
    for(int i = 0; i < N; i++) scanf("%d", array + i);
    for(int i = 0, max = i; i < N; i++)
        lmax[i] = array[i] >= array[max] ? array[max = i] : array[max];
    for(int i = N - 1, min = i; i >= 0; i--)
        rmin[i] = array[i] <= array[min] ? array[min = i] : array[min];
    for(int i = 0; i < N; i++)
    {
        if(array[i] == lmax[i] && array[i] == rmin[i])
            count++;
        else
            array[i] = 0;
    }
    printf("%d\n", count);
    for(int i = 0; i < N && count; i++) if(array[i])
        printf("%d%c", array[i], --count ? ' ' : '\0');
    printf("\n");
    return 0;
}
```

# 1046-划拳
快乐白给题
```c++
#include<stdio.h>
using namespace std;

int main(int argc, const char** argv) {
    int num, acount = 0, bcount = 0;
    int sum;
    scanf("%d", &num);
    int temp1, temp2, temp3, temp4;
    for(int i=0; i<num; i++){
        scanf("%d %d %d %d", &temp1, &temp2, &temp3, &temp4);
        sum = temp1+temp3;
        if(temp2 == sum){
            if(temp2 == temp4){
                continue;
            }
            else{
                acount++;
            }
        }
        if(temp4 == sum){
            bcount++;
        }
    }
    printf("%d %d", bcount, acount);
    return 0;
}
```

# 1047-编程团体赛
快乐白给题
```c++
#include<stdio.h>
using namespace std;

int main(int argc, const char** argv) {
    int num;
    scanf("%d", &num);
    int id, pid, score;
    int maxid, maxscore = 0;
    int hash[10000] = {0};
    for(int i=0; i<num; i++){
        scanf("%d-%d %d", &id, &pid, &score);
        hash[id] += score;
        if(hash[id] > maxscore){
            maxid = id;
            maxscore = hash[id];
        }
    }
    printf("%d %d", maxid, maxscore);
    return 0;
}
```

# 1048-数字加密
这道题有点没说清楚,我以为位数不够不需要补0,加上判断就AC了
```c++
#include<cstdio>
#include<algorithm>
#include<string>
#include<iostream>
using namespace std;

int main(int argc, const char** argv) {
    string a,b;
    cin >> a >> b;
    reverse(a.begin(), a.end());
    reverse(b.begin(), b.end());
    int sum, count;
    if(a.size() > b.size()){
        b.append(a.size()-b.size(),'0');
    }
    else{
        a.append(b.size()-a.size(),'0');
    }
    count = a.size();
    for(int i=0; i<count; i++){
        if((i+1)%2 == 1){   //single
            sum = (a[i]-'0') + (b[i]-'0');
            sum = sum%13;
            if(sum == 10){
                b[i] = 'J';
            }
            else if(sum == 11){
                b[i] = 'Q';
            }
            else if(sum == 12){
                b[i] = 'K';
            }
            else{
                b[i] = sum + '0';
            }
        }
        else{
            sum = (b[i]-'0')-(a[i]-'0');
            if(sum < 0){
                sum += 10;
            }
            b[i] = sum + '0';
        }
    }
    reverse(b.begin(),b.end());
    cout << b; 
    return 0;
}
```

# 1049-数列的片段和
又是一道让我伤心的数学题,需要画图,将数据画到一个方格中就可以发现其中的规律
但是由于题目改的更加严谨了,所以需要写一个我看不懂的代码才能AC
```c++
/* 正经的代码(非AC) */
#include<stdio.h>
int main() {
	int n;
	scanf("%d", &n);        // 数的个数
	double sum = 0, num;    // 和，数
	for (int i = 1; i <= n; ++i) {
		scanf("%lf", &num);
		sum += num * i * (n - i + 1);   // 数×所在片段数
	}
	printf("%.2lf", sum);    // 两位小数
	return 0;
}
/*看不懂但是AC的代码*/
#include <cstdio>
using namespace std;
#define ll long long

ll n,summ,tmpa;
double tpda;

int main(){
    scanf("%lld",&n);
    for(ll i=0;i<n;++i){
        scanf("%lf",&tpda);
        tmpa = ((ll)(tpda*10000)+5)/10;  //从结果上来看，这么做吧原来的浮点数乘以1000再转为整型
        summ += tmpa * (i+1) * (n-i);  //为什么要先乘以一万再加5除以十？是因为最低位会产生精度误差。
    } //加五除十是为了四舍五入
    printf("%lld.%02lld",summ/1000,((summ%1000)+5)/10);
    return 0;
}
```

# 1050-螺旋矩阵*
这道题是典型的画图题,但是其中还混杂了一些数学技巧
由于要求 m*n = N,且 m-n 要求最小,这里需要取N的平方跟,在平方跟的基础上递加1,寻找最小的m

## 二维vector
首先对于一维vector的初始化: 
```c++
vector<int> a(N);
```
如果需要在scanf中对vector读取数据,则必须声明vector长度
这道题需要用到二维vector,正好学习了一下
对于二维的初始化: 
**第一种方法**
```c++
vector<vector<int> > b(m, vector<int>(n));
/* m为行数 n为列数*/
```
**第二种方法-resize()**
```c++
vector<vector<char> > chos;
chos.resize(4, vector<char>(2));
```

## sqrt()
需要注意的一点是,需要将其中的变量转为double
```c++
n = sqrt((double)N);
```

## sort和qsort的用法与区别
### qsort的用法
```c++
void qsort(void *base, int nelem, int width, int (*fcmp)(const void *,const void *));
/*
*base: 待排序数组首地址
nelem: 数组中待排序元素数量
width: 各元素的占用空间大小
fcmp: 指向函数的指针，用于确定排序的顺序
*/
```
1. 对int类型数组排序 
```c++
int num[100];
int cmp ( const void *a , const void *b )
{
return *(int *)a - *(int *)b;
}
qsort(num,100,sizeof(num[0]),cmp); 
```

2. 对char类型数组排序（同int类型） 
```c++
char word[100];
int cmp( const void *a , const void *b )
{
return *(char *)a - *(char *)b;
}
qsort(word,100,sizeof(word[0]),cmp);
```

3. 对double类型数组排序（特别要注意）
```c++ 
double in[100];
int cmp( const void *a , const void *b )
{
return *(double *)a > *(double *)b ? 1 : -1;
}
//返回值的问题，显然cmp返回的是一个整型，所以避免double返回小数而被丢失。
qsort(in,100,sizeof(in[0]),cmp)； 
```

### sort的用法
头文件：
```c++
#include <algorithm>
using namespace std;
```
说明：sort有默认的比较函数，默认是按升序排列。
```c++
void sort(RanIt first, RanIt last, Pred pr);
/*前两个参数为迭代器指针，分别指向容器的首尾，第三个参数为比较方法。*/
```
1. 默认（不写）：对于内置类型，按照升序排列；
2. 标准库自带函数：functional提供了一堆基于模板的比较函数对象：
```c++
equal_to<Type>、not_equal_to<Type>、greater<Type>、greater_equal<Type>、less<Type>、less_equal<Type>。
升序：sort(begin,end,less<data-type>()); 
降序：sort(begin,end,greater<data-type>()).
```
3. 自定义函数：
```c++
int cmp(const int &a,const int &b){
　　return a>b
}
/*Sort中的cmp函数参数可以直接是参与比较的引用类型。*/
```

### 区别
qsort与sort的区别：
std::sort是一个改进版的qsort.
std::sort函数优于qsort的一些特点：对大数组采取9项取样，更完全的三路划分算法，更细致的对不同数组大小采用不同方法排序。

1. cmp函数和qsort中cmp函数的不同
```c++
​int cmp(const int &a,const int &b){
　　return a>b
}
```
Sort中的cmp函数参数可以直接是参与比较的引用类型，sort可以采用标准库自带的比较函数，而qsort没有。
1. **cmp函数比较时qsort用“-”，而sort用”>”**。
2. sort函数是c++中标准模板库的的函数，在qsort()上已经进行了优化，根据情况的不同可以采用不同的算法，所以较快。
在同样的元素较多和同样的比较条件下，sort()的执行速度都比qsort()要快。另外，sort()是类属函数，可以用于比较任何容器，任何元素，任何条件。使用时需调用algorithm
3. 另外在使用sort函数中的greater和less时,需要iterator头文件
```c++
#include<iterator>
```

## CODE
这道矩阵打印很经典,之前在leetcode上也遇到过,今天算是差不多弄懂了,难点不在于矩阵打印的模板,而是如何分析一个规律性打印,并转换为代码实现的过程
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/pat-8/1.png)
```c++
#include <algorithm>
#include <cmath>
#include <vector>
#include <cstdio>
using namespace std;

int cmp(int a, int b) {return a > b;}
int main() {
    int N, m, n, t = 0;
    scanf("%d", &N);
    for (n = sqrt((double)N); n >= 1; n--) {
        if (N % n == 0) {
            m = N / n;
            break;
        }
    }
    vector<int> a(N);
    for (int i = 0; i < N; i++)
        scanf("%d", &a[i]);
    sort(a.begin(), a.end(), cmp);
    vector<vector<int> > b(m, vector<int>(n));
    int level = m / 2 + m % 2;
    for (int i = 0; i < level; i++) {
        for (int j = i; j <= n - 1 - i && t <= N - 1; j++)
                b[i][j] = a[t++];
        for (int j = i + 1; j <= m - 2 - i && t <= N - 1; j++)
                b[j][n - 1 - i] = a[t++];
        for (int j = n - i - 1; j >= i && t <= N - 1; j--)
                b[m - 1 - i][j] = a[t++];
        for (int j = m - 2 - i; j >= i + 1 && t <= N - 1; j--)
                b[j][i] = a[t++];
    }
    for (int i = 0; i < m; i++) {
        for (int j = 0 ; j < n; j++) {
            printf("%d", b[i][j]);
            if (j != n - 1) printf(" ");
        }
        printf("\n");
    }
    return 0;
}
```

# 1051-复数乘法
这道题也挺有意思(难)的
坑点主要在精度范围,如果**保留两位小数,则当小于0.01时,会表示为0.00,这时需要赋值为0,并且需要用到绝对值abs()函数**
```c++
#include<cstdio>
#include<cmath>
using namespace std;

int main(int argc, const char** argv) {
    double s1,s2,s3,s4;
    scanf("%lf %lf %lf %lf",&s1, &s2, &s3, &s4);
    double A,B;
    A = s1*s3*cos(s2+s4);
    B = s1*s3*sin(s2+s4);
    if(abs(A) < 0.01){
        A = 0;
    }
    if(abs(B) < 0.01){
        B = 0;
    }
    if(B >= 0){
        printf("%.2f+%.2fi", A, B);
    }
    else{
        printf("%.2f%.2fi", A, B);
    }
    return 0;
}
```

# 1052-卖个萌
这道题一个点是格式输入
对于"[╮][╭][o][~\][/~][<][>]"字符的输入
可以使用类似正则的表达式取字符
```c++
if(c == '['){
    scanf("%[^]]", &data);
}
```
这里意味着从 "[" 开始取字符串取到 "]" 为止
并且对于char数组中添加字符串,需要赋初始值为'\0'
这样可以在字符串取出时,遇到结束符
```c++
char data[3][10][5] = {'\0'};
```
另外对于最内层的字符串,需要使用 *data[m][n] 访问

## 柳神解法
这vector-string和substr()用的是真的酷炫,学到了!
另外注意转义字符:  @\/@ 的 '\' 是转义字符，想要输出 '\' 就要用 '\\' 表示
```c++
#include <iostream>
#include <vector>
using namespace std;
int main() {
    vector<vector<string> > v;
    for(int i = 0; i < 3; i++) {
        string s;
        getline(cin, s);
        vector<string> row;
        int j = 0, k = 0;
        while(j < s.length()) {
            if(s[j] == '[') {
                while(k++ < s.length()) {
                    if(s[k] == ']') {
                        row.push_back(s.substr(j+1, k-j-1));
                        break;
                    }
                }
            }
            j++;
        }
        v.push_back(row);
    }
    int n;
    cin >> n;
    for(int i = 0; i < n; i++) {
        int a, b, c, d, e;
        cin >> a >> b >> c >> d >> e;
        if(a > v[0].size() || b > v[1].size() || c > v[2].size() || d > v[1].size() || e > v[0].size() || a < 1 || b < 1 || c < 1 || d < 1 || e < 1) {
            cout << "Are you kidding me? @\\/@" << endl;
            continue;
        }
        cout << v[0][a-1] << "(" << v[1][b-1] << v[2][c-1] << v[1][d-1] << ")" << v[0][e-1] << endl;
    }
    return 0;
}
```

## 第二种解法
```c++
#include<cstdio>
#include<cstring>
using namespace std;

int main(int argc, const char** argv) {
    int num;
    int n[5];
    char c;
    char data[3][10][5] = {'\0'};
    for(int i=0; i<3; i++){
        for(int index=0; (c = getchar())!='\n';){
            if(c == '['){
                scanf("%[^]]", data[i][index++]);
            }
        }
    }
    scanf("%d", &num);
    for(int j=0; j<num; j++){
        for(int k=0; k<5; k++){
            scanf("%d", n+k);
            --n[k];
        }
        if(n[0] < 0 || n[0] > 9 || !(*data[0][n[0]])
        || n[1] < 0 || n[1] > 9 || !(*data[1][n[1]])
        || n[2] < 0 || n[2] > 9 || !(*data[2][n[2]])
        || n[3] < 0 || n[3] > 9 || !(*data[1][n[3]])
        || n[4] < 0 || n[4] > 9 || !(*data[0][n[4]]))
            puts("Are you kidding me? @\\/@");
        else{
            printf("%s(%s%s%s)%s\n", data[0][n[0]], data[1][n[1]],data[2][n[2]], data[1][n[3]], data[0][n[4]]);
        }
    }
    return 0;
}
```
