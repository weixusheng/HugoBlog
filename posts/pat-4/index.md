# PAT-Basic 19~28



B1019-B1028<br>
2020-07-18 优化题解<br>
这两天发生了很多事情,所以拖更啦...并且PAT的神奇测试点把我搞自闭了,还好发生了一些快乐的事情,把心态调整了回来
<!--more-->

# 1019 数字黑洞
看到这种长度很长的数,我就有一种想用char来解决的冲动
但是实践表明,在一些逻辑稍微复杂题面前,这是一种很不明智的方法
而直接用除法取余操作来操作每个位,则适用于很多情况,并且出现bug的可能性更小
最好在处理一道题的时候,尽量不去对复杂数进行int和char数组之间的转换
## 菜鸡的笨方法
```c
int cmp_small(const void* a, const void*b){
    char* m = (char*)a;
    char* n = (char*)b;
    return strcmp(a,b);
}

int cmp_big(const void* a, const void*b){
    char* m = (char*)a;
    char* n = (char*)b;
    return strcmp(b,a);
}

int toint(char* a){
    int res1 = 0;
    for(int j=0; j< 4; j++){
        res1 = (a[j]-'0') + res1*10;
    }
    return res1;
}

main(){
    char init_data[5];
    char small[5];
    char big[5];
    int m;
    int n;
    int res = 0;
    int flag = 0;
    scanf("%s", &init_data); //读入初始数到char数组
    //判断是否全部相同
    if((init_data[0] == init_data[1]) && (init_data[0] == init_data[2]) && (init_data[0]== init_data[3])){
        printf("%s - %s = 0 0 0 0",init_data,init_data);
        return 0;
    }
    while(res!=6174){
        strcpy(small,init_data);
        strcpy(big,init_data);
        //非递减排序(递增)
        qsort(big,4,sizeof(init_data[0]),cmp_big);
        m = toint(big); //char 转为 int
        //递减
        qsort(small,4,sizeof(init_data[0]),cmp_small);
        n = toint(small);
        printf("%04d - %04d",m,n);
        res = m-n;
        sprintf(init_data, "%d", res);  //int 转为 char数组
        if(res == 6174){
            printf(" = %04d",res);
        }
        else{
            printf(" = %04d\n",res);
        }
    }
    return 0;
}
```
对于我的这种方法,有几个明显的缺点,首先将int转换成char数组,而这个是大可不必的,完全可以用int数组来解决,其次,对于递增和递减两种数的处理,我竟然写了两个cmp函数...完全可以用一个cmp和一个reverse来解决

## C++ 题解
```c++
#include <iostream>
#include <algorithm>
using namespace std;
bool cmp(char a, char b) {return a > b;}
int main() {
    string s;
    cin >> s;
    s.insert(0, 4 - s.length(), '0');
    do {
        string a = s, b = s;
        sort(a.begin(), a.end(), cmp);
        sort(b.begin(), b.end());   //默认升序(非降序)排列
        int result = stoi(a) - stoi(b);
        s = to_string(result);
        s.insert(0, 4 - s.length(), '0');
        cout << a << " - " << b << " = " << s << endl;
    } while (s != "6174" && s != "0000");
    return 0;
}
```

## AC题解
```c
int cmp(const void *a, const void *b)
{
    return *(int*)b - *(int*)a;
}

int sort(int n)
{
    int digits[4] = {n/1000, n%1000/100, n%100/10, n%10};
    qsort(digits, 4, sizeof(int), cmp);
    return digits[0] * 1000 + digits[1] * 100 + digits[2] * 10 + digits[3];
}

int reverse(int n)
{
    return n/1000 + n%1000/100 * 10 + n%100/10 * 100 + n%10 * 1000;
}

int main()
{
    int N;
    scanf("%d", &N);
    do
    {
        N = sort(N);
        printf("%04d - %04d = %04d\n", N, reverse(N), N - reverse(N));
        N = N - reverse(N);
    }while(N != 0 && N != 6174) ;
    return 0;
}
```

# 1020 月饼
这道题我AC啦!很开心
这道题有个知识点就是二位数组的cmp函数,并且在cmp函数中对于double类型以及非int型数据的处理,以下是一个必须避免的错误,这种数据类型下不可以直接返回相减的值,而需要进行判断后返明确的return 1 or return -1
```c
int cmp(const void*a , const void*b){
    double m = ((double*)a)[1];
    double n = ((double*)b)[1];
    return (m-n);     //递减
}
```
```c
int cmp(const void*a , const void*b){
    if(((double*)a)[1]>((double*)b)[1])
		return -1;
	else return 1;
}

int main(){
    int n;
    int need;
    double price = 0;
    double data[1000][2];
    scanf("%d %d", &n, &need);
    for(int i=0; i<n; i++){
        scanf("%lf", &data[i][0]);
    }
    for(int j=0; j<n; j++){
        double avg;
        scanf("%lf", &avg);
        data[j][1] = avg/data[j][0];
    }
    qsort(data, n, sizeof(double)*2, cmp); //排序 
    while(need!=0){
        for(int k=0; k<n; k++){
            if(need > data[k][0]){
                need -= data[k][0];
                price+= (data[k][1]*data[k][0]);
            }
            else{
                price+= (data[k][1]*need);
                need = 0;
            }
        }
        break;
    }
    printf("%.2f", price);
}
```

# 1021 个位数统计
这道简单AC,快乐hash就完事了

## C++ 
```c++
#include <iostream>
using namespace std;

int main() {
    string s;
    cin >> s;
    int a[10] = {0};
    for (int i = 0; i < s.length(); i++)
        a[s[i] - '0']++;
    for (int i = 0; i < 10; i++) {
        if (a[i] != 0) 
            printf("%d:%d\n", i, a[i]);
    }
    return 0;
}
```

## C
```c
int main(){
    int hash[10] = {0};
    int max = 0;
    char num[1001];
    scanf("%s", &num);
    int i = 0;
    while(num[i]!= '\0'){
        int cur = num[i]-'0';
        if(cur>max){
            max = cur;
        }
        hash[cur]++;
        i++;
    }
    for(int k=0; k<10; k++){
        if(hash[k]!=0){
            printf("%d:%d",k, hash[k]);
            if(k != max){   //此处判断较为多余
                printf("\n");
            }
        }
    }
    return 0;
}
```

# 1022 D进制的A+B
## C++
**十进制转 N 进制:**
将每一次 t%d 的结果保存在int类型的数组s中，然后将 t/d，直到 t=0 为止
此时s中保存的就是 t 在 D 进制下每一位的结果的倒序，最后倒序输出s数组即可
```c++
#include <iostream>
using namespace std;

int main() {
    int a, b, d;
    cin >> a >> b >> d;
    int t = a + b;
    if (t == 0) {
        cout << 0;
        return 0;
    }
    int s[100];
    int i = 0;
    while (t != 0) {
        s[i++] = t % d;
        t = t / d;
    }
    for (int j = i - 1; j >= 0; j--)
        cout << s[j];
    return 0;
}
```

## 菜鸡题解
我也不知道为啥,有一个测试点通不过...呜呜呜
而且写的还有点复杂,关于进制的处理,我还是不太熟悉,其实完全不必使用int数组
```c
int main(){
    long int a,b,c;
    int res[100000];
    int i = 0;
    int d;
    scanf("%ld %ld %d", &a, &b ,&d);
    c = a+b;
    int temp;
    while(c > d){
        temp = c%d;
        c = c/d;
        res[i] = temp;
        i++;
    }
    res[i] = c;
    for(int k=i; k>=0; k--){
        printf("%d", res[k]);
    }
    return 0;
}
```

## AC题解-C
```c
int main()
{
    int A, B, D, Sum;
    scanf("%d %d %d", &A, &B, &D);
    Sum = A + B;
    int power = 1;
    while(Sum / D >= power) {
        power *= D;
    }
    for(; power > 0; Sum %= power, power /= D){
        printf("%d", Sum / power);
    }
    return 0;
}
*/
```

# 1023 组个最小数
快乐AC,嘿嘿,这道题没啥好说的,主要处理好数的第一位就完事了
```c
int main(){
    int data[10];
    for(int k=0; k<10; k++){
        scanf("%d", &data[k]);
    }
    // find first
    int first = 0;
    for(int i=1; i<10; i++){
        if(data[i]!= 0){
            first = i;
            (data[i])--;
            break;
        }
    }
    printf("%d",first);
    int n;
    for(int j=0; j<10; j++){
        if(data[j]!=0){
            n = data[j];
            for(int h=0; h<n; h++){
                printf("%d", j);
            }
        }
    }
    return 0;
}
```

# 1024 科学计数法
这道题...唉,我的方法就不放上了
题目给出的是这种输入"+1.23400E-03",我想都没想直接开始char[]操作,来提取数中的前后部分,最后好不容易调试完成,发现只通过了一半的测试点...
看了题解才发现,我原来输在了scanf上...于是乎在那个深夜,我自闭了
这道题的知识点都是在输入和输出函数上(感觉乙级考的就是数据的格式化输入输出...)
## C++
**substr(pos, count)方法**
其中pos为起始位置,count为截取字符串数量,如果没有count参数,则一直截取到字符串的末尾

```c++
#include <iostream>
using namespace std;

int main() {
    string s;
    cin >> s;
    int i = 0;
    while (s[i] != 'E') i++;
    string t = s.substr(1, i-1);
    int n = stoi(s.substr(i+1));
    if (s[0] == '-') cout << "-";
    if (n < 0) {
        cout << "0.";
        for (int j = 0; j < abs(n) - 1; j++) cout << '0';
        for (int j = 0; j < t.length(); j++)
            if (t[j] != '.') cout << t[j];
    } else {
        cout << t[0];
        int cnt, j;
        for (j = 2, cnt = 0; j < t.length() && cnt < n; j++, cnt++) cout << t[j];
        if (j == t.length()) {
            for (int k = 0; k < n - cnt; k++) cout << '0';
        } else {
            cout << '.';
            for (int k = j; k < t.length(); k++) cout << t[k];
        }
    }
    return 0;
}
```

## C
```c
int main()
{
    int exponent;   
    char line[10000], *p = line;
    scanf("%[^E]E%d", line, &exponent);
    if(*p++ == '-') putchar('-');     
    if(exponent >= 0)          
    {
        putchar(*p);
        for(p += 2; exponent; exponent--)    
            putchar(*p ? *p++ : '0');
        if(*p)                           
        {
            putchar('.');
            while(*p) putchar(*p++);
        }
    }
    if(exponent < 0)        
    {
        printf("0.");
        for(exponent++; exponent; exponent++)      
            putchar('0');
        for(; *p; p++) if(*p != '.') putchar(*p);  
    }
    return 0;
}
```
## scanf格式化输入
对于char的格式化输入,从这道题中学到了一种新的方法
就是利用字符串匹配(不知道这算不算正则表达式...)
例如在字符串"+1.23400E-03"中,可以使用
```c
scanf("%[^E]E%s",&a, &b);
```
其中"^E"表示字符串读取到E截至,即只读取 +1.23400
此外还可以使用
```c
scanf("%[0-9]E%s",&a, &b);
```
表示只读取数字,得到 123400

# 1025 反转链表
这道题我用的双hash,最后果然就...写懵了
最后之通过了一半的AC(自闭复发)自己的方法就不放了...
看了题解之后不禁扪心自问(结构体它不香吗!)
题解中有一个很有用的细节,就是
```c
#define SWAPNODE(A, B) {Node temp = A; A = B; B = temp;}
```
建立一个全局预编译函数,感觉好有b格嗷!学到了
此外题解中对于链表的建立和排序操作,写的很精妙,一道好题

## 菜鸡的方法
```c++
#include<cstdlib>
#include<vector>
#include<iostream>
#include<algorithm>
#include<map>
#include<iomanip>

using namespace std;

struct node{
    int id;
    int next;
};

int main(int argc, char const *argv[])
{
    int address, num, k;
    int tmpad,tmpid,tmpnext;
    cin >> address >> num >> k;
    vector<node> data(100000);
    map<int,int> mp;
    for(int i=0; i<num; i++){
        cin >> tmpad >> tmpid >> tmpnext;
        data[tmpad] = node{tmpid, tmpnext};
        mp[tmpid] = tmpad;
    }
    vector<int> res;
    while(address != -1){
        res.push_back(data[address].id);
        address = data[address].next;
    }
    // reverse
    for(int i=0; i<res.size()/k; i++){
        reverse(res.begin()+i*k,res.begin()+k*(i+1));
    }
    for(int i=0; i<res.size(); i++){
        if(i == res.size()-1){
            printf("%05d %d -1",mp[res[i]],res[i]);
        }
        else{
            printf("%05d %d %05d\n",mp[res[i]], res[i], mp[res[i+1]]);
        }
    }
    system("pause");
    return 0;
}
```

## C
```c
#define SWAPNODE(A, B) {Node temp = A; A = B; B = temp;}

typedef struct node{
    int addr;
    int data;
    int next;
}node, *Node;

int main()
{
    int A, N, K;
    node nodes[100000] = {0};
    Node np[100000] = {0}, *p;
    scanf("%d %d %d", &A, &N, &K);
    for(int i = 0; i < N; i++)
    {
        np[i] = nodes + i;
        scanf("%d %d %d", &np[i]->addr, &np[i]->data, &np[i]->next);
    }
    for(int i = 0; i < N; i++)
    {
        for(int j = i; j < N; j++)
            //这个判定写的很好!值得借鉴
            if(np[j]->addr == (i ? np[i - 1]->next : A)) 
            {
                SWAPNODE(np[i], np[j]);
                break;
            }
        if(np[i]->next == -1)  
            N = i + 1;
    }
    for(int i = 0; i < N / K; i++)
    {
        p = np + i * K;
        for(int j = 0; j < K / 2; j++)
            SWAPNODE(p[j], p[K - j - 1]);
    }
    for(int i = 0; i < N - 1; i++)
        printf("%05d %d %05d\n", np[i]->addr, np[i]->data, np[i + 1]->addr);
    printf("%05d %d -1\n", np[N - 1]->addr, np[N - 1]->data);
    return 0;
}
```

## C++
虽然C的方法十分花里胡哨,但是C++实在是太舒服了
这里值得注意的是,用到了begin(list)的带参数用法,返回参数的起始位置,和list.begin()是一样的
```c++
#include <iostream>
#include <algorithm>
using namespace std;

int main() {
    int first, k, n, temp;
    cin >> first >> n >> k;
    int data[100005], next[100005], list[100005];
    for (int i = 0; i < n; i++) {
        cin >> temp;
        cin >> data[temp] >> next[temp];
    }
    int sum = 0;//不一定所有的输入的结点都是有用的，加个计数器
    while (first != -1) {
        list[sum++] = first;
        first = next[first];
    }
    for (int i = 0; i < (sum - sum % k); i += k)
        reverse(begin(list) + i, begin(list) + i + k);
    for (int i = 0; i < sum - 1; i++)
        printf("%05d %d %05d\n", list[i], data[list[i]], list[i + 1]);
    printf("%05d %d -1", list[sum - 1], data[list[sum - 1]]);
    return 0;
}
```

# 1026 程序运行时间
这道白给题有一个知识点,double的四舍五入: 加上 0.5 之后转 int
```c
int_num = (int)(double_num + 0.5);
```
```c
int main(){
    double s1,s2;
    scanf("%lf %lf", &s1, &s2);
    double dvalue = s2-s1;
    long int d;
    d = (int)(dvalue/100+0.5);
    /*
    c中需要在前面int两端加上括号,而c++不需要,可以直接写为:
    d = int(dvalue/100+0.5);
    */
    int hour = 0, minutes = 0, second = 0;
    second = d%60;
    d /= 60;
    minutes = d%60;
    hour = d/60;
    printf("%02d:%02d:%02d",hour,minutes,second);
}
```

# 1027 打印沙漏
这道题我虽然AC了,但是心很累,为何我写的这么啰嗦
哦,原来是数学没学好,那没事了
```c
int main(){
    int s;
    char signal;
    scanf("%d %c", &s, &signal);
    int sum = s;
    s = (s+1)/2;
    int minus;
    int shit = 0,count = 0,minus = 1;;
    for(int i=1; s >= minus; i++){
        s = s-minus;
        minus = 2*i +1;
        count++;
    }
    int k, w, j;
    for(k=0; k < count; k++){
        for(j=0; j< k; j++){
            printf(" ");
        }
        for(w=0; w<((count-k)*2-1); w++){
            printf("%c",signal);
            shit++;
        }
        printf("\n");
    }
    for(k=count-2; k>=0; k--){
        for(j=k; j>0; j--){
            printf(" ");
        }
        for(w=((count-k)*2-1); w>0; w--){
            printf("%c",signal);
            shit++;
        }
        printf("\n");
    }
    printf("%d", sum-shit);
    return 0;
}
```

## 题解
 - 绝对值
 - M阶的沙漏需要用到的字符个数为2M<sup>2</sup>-1

这绝对值用的我服了...学到了
```c
#define ABS(X) ((X) >= 0 ? (X) : -(X))
int main()
{
    char c;
    int N, M;
    scanf("%d %c", &N, &c);
    for(M = 1; 2 * M * M - 1 <= N; M++) ;
    M--;   
    for(int i = 0; i < 2 * M - 1; i ++)
    {
        for(int j = 0; j < M - 1 - ABS(M - 1 - i); j++)
            putchar(' ');
        for(int j = 0; j < 2 * ABS(M - 1 - i) + 1; j++)
            putchar(c);
        putchar('\n');
    }
    printf("%d", N - 2 * M * M + 1);
    return 0;
}
```

# 1028 人口普查
这道题鸭,我自己写有一个测试点就是过不去,去看了眼题解,差点没给我气昏过去
strcmp()这函数鸭,我服辣
## C++
```c++
#include <iostream>
using namespace std;

int main() {
    int n, cnt = 0;
    cin >> n;
    string name, birth, maxname, minname, maxbirth = "1814/09/06", minbirth = "2014/09/06";
    for (int i = 0; i < n; i++) {
        cin >> name >> birth;
        if (birth >= "1814/09/06" && birth <= "2014/09/06") {
            cnt++;
            if (birth >= maxbirth) {
                maxbirth = birth;
                maxname = name;
            }
            if (birth <= minbirth) {
                minbirth = birth;
                minname = name;
            }
        }
    }
    cout << cnt;
    if (cnt != 0) cout << " " << minname << " " << maxname;
    return 0;
}
```

## 菜鸡的方法
用了快乐结构体,结果还用笨了
```c
typedef struct {
    char name[100];
    int age;
}person,*node;

int main(){
    int num,year,month,day;
    int max,maxd=0,max_day = 200*365,count = 0;
    int min,mind = max_day;
    scanf("%d", &num);
    node data[100000];
    char temp_name[100];
    int init_day = 2014*365 + 9*30 + 6;
    int temp_age,sumday;
    for(int i=0; i<num; i++){
        scanf("%s %d/%d/%d", &temp_name, &year, &month, &day);
        sumday = (year*365 + 30*month + day);
        temp_age = init_day-sumday;
        node newp = (node)malloc(sizeof(person));
        if(temp_age <= max_day && temp_age>0){
            strcpy(newp->name, temp_name);
            newp->age = temp_age;
            data[i] = newp;
            if(temp_age > maxd){
                max = i;
                maxd = temp_age;
            }
            if(temp_age < mind){
                min = i;
                mind = temp_age;
            }
            count++;
        }
    }
    if(count == 0){
        printf("0");
    }
    else{
        printf("%d %s %s",count, data[max]->name, data[min]->name);
    }
    return 0;
```

## 题解
题解是真的机智,首先对于日期的比较,直接strcmp
然后将日期和名字放到同一个char数组中
```c
int main()
{
    int N, count = 0;
    char cur[17], eldest[17] = {'9'}, youngest[17] = {'0'};
    scanf("%d", &N);
    for(int i = 0; i < N; i++)
    {
        scanf("%s %s", cur + 11, cur);
        if(strcmp(cur, "1814/09/06") >= 0 && strcmp(cur, "2014/09/06") <= 0)
        {
            if(strcmp(cur, eldest) <= 0)
                memcpy(eldest, cur, 17);
            if(strcmp(cur, youngest) >= 0)
                memcpy(youngest, cur, 17);
            count++;
        }
    }
    if(count)
        printf("%d %s %s", count, eldest + 11, youngest + 11);
    else
        printf("0");
    return 0;
}
```

## strcmp
strcmp() 用来比较字符串(区分大小写）
原型为：int strcmp(const char *s1, const char *s2);
s1, s2 为需要比较的两个字符串。字符串大小的比较是以ASCII码表上的顺序来决定，此顺序亦为字符的值。strcmp()首先将s1 **第一个字符值**减去s2 **第一个字符值**，若差值为0则再继续比较下个字符，若差值不为0 则将差值返回。
例如字符串"Ac"和"ba"比较则会返回字符"A"(65)和'b'(98)的差值(－33)。
```c
#include <string.h>
main(){
    char *a = "aBcDeF";
    char *b = "AbCdEf";
    char *c = "aacdef";
    char *d = "aBcDeF";
    printf("strcmp(a, b) : %d\n", strcmp(a, b));
    printf("strcmp(a, c) : %d\n", strcmp(a, c));
    printf("strcmp(a, d) : %d\n", strcmp(a, d));
}
```
输出结果：
```c
strcmp(a, b) : 32
strcmp(a, c) :-31
strcmp(a, d) : 0
```
