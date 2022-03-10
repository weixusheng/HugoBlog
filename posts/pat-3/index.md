# PAT-Basic 13~18



B1013-B1018<br>
我刷的太慢了(虽然题很简单)...但感觉解题速度越来越快了!继续加油!<br>
2020-07-18 优化题解 
<!--more-->

# B1013-数素数
这道题的精髓就是在于如何算素数呀
之前算素数的时候没用sqrt函数,但发现还是直接用函数来的方便
标准模板
```c
m=(int)sqrt((double)s); //首先求出平方根
for(i=2;i<=m;i++){
    if(s%i==0)
        //当前不为素数
    if(i>m){
        //当前为素数
    }
}
```

## C++
```c++
#include <iostream>
#include <vector>
using namespace std;
bool isprime(int a) {
    for (int i = 2; i * i <= a; i++)
        if(a % i == 0) return false;
    return true;
}
int main() {
    int M, N, num = 2, cnt = 0;
    cin >> M >> N;
    vector<int> v;
    while (cnt < N) {
        if (isprime(num)) {
            cnt++;
            if (cnt >= M) v.push_back(num);
        }
        num++;
    }
    cnt = 0;
    for (int i = 0; i < v.size(); i++) {
        cnt++;
        if (cnt % 10 != 1) printf(" ");
        printf("%d", v[i]);
        if (cnt % 10 == 0) printf("\n");
    }
    return 0;
}
```

## C
```c
#include<stdio.h>
#include<stdlib.h>
#include <math.h>

int main(){
    int a, b;   //输入
    scanf("%d %d", &a, &b);
    int count = 0;  //计数
    int m; //平方跟
    int i; //循环计数
    int s=2; //当前判断的数
    int flag;   //判断素数
    //find single num
    while(count < b){
        flag = 0;
        m=(int)sqrt((double)s); //求素数
        for(i=2;i<=m;i++)
            if(s%i==0)
                break;
        if(i>m){
            flag = 0;
            ++count;
        }
        else{
            flag = 1;
        }
        if(flag == 0){ // is single
            if(count >= a){
                printf("%d", s);
                if((count-a+1)%10 == 0 && count!= b){
                    printf("\n");
                }
                else if(count != b)
                {
                    printf(" ");
                }
            }
        }
        s++;
    }
    return 0;
}
```

# B1014-福尔摩斯的约会
不知道为啥...我写的代码就是有一个测试点没法通过,哭了
## 字符类型的判断
直接用ascii判断大小即可
例如判断字符在A和G之间
```c
a1[i]>='A'&&a1[i]<='G'
```
## 格式输出
```c
printf("%02d:", output2);
```
表示把整型数据打印最低两位，如果不足两位，用0补齐
## 字符类型判断
头文件

```c
#include <ctype.h>
```
1. isdigit
判断是否为数字,若为数字则返回1
2. isalpha
判断是否为字母,若为字母返回1

```c
#include <stdio.h>
#include <ctype.h>

int main()
{
    char str1[61], str2[61], str3[61], str4[61];
    scanf("%s %s %s %s", str1, str2, str3, str4);
    int DAY;
    char *weekdays[] = {"MON", "TUE", "WED", "THU", "FRI", "SAT", "SUN"};
    for(DAY = 0; str1[DAY] && str2[DAY]; DAY++)
        if(str1[DAY] == str2[DAY] && str1[DAY] >= 'A' && str1[DAY] <= 'G')
        {
            printf("%s", weekdays[str1[DAY] - 'A']);
            break;
        }
    int HH;
    for(HH = DAY + 1; str1[HH] && str2[HH]; HH++)
        if(str1[HH] == str2[HH])
        {
            if(str1[HH] >= 'A' && str1[HH] <= 'N')
            {
                printf(" %02d", str1[HH] - 'A' + 10);
                break;
            }
            if(isdigit(str1[HH]))
            {
                printf(" %02d", str1[HH] - '0');
                break;
            }
        }
    int MM;
    for(MM = 0; str3[MM] && str4[MM]; MM++)
        if(str3[MM] == str4[MM] && isalpha(str3[MM]))
        {
            printf(":%02d", MM);
            break;
        }

    return 0;
}
/*测试点4无法通过
#include<stdlib.h>
#include<stdio.h>
#include <ctype.h>

int main(){
    char a1[61], a2[61], a3[61], a4[61];
    scanf("%s", a1);
    scanf("%s", a2);
    scanf("%s", a3);
    scanf("%s", a4);    //输入字符串
    int flag = 0; //找到了第一个
    int output1;    //输出星期
    int output2,output3=0;
    char week[7][10] = {"MON","TUE","WED","THU","FRI","SAT","SUN"};
    //判断第一对字符串
    int i=0;
    while(a1[i]!= '\0' && a2[i]!= '\0'){    //第一个字符串
        if(a1[i] == a2[i] && flag == 0 && (a1[i]>='A'&&a1[i]<='G')){ //找到第一个
            output1 = a1[i]-'A';
            printf("%s ", week[output1]);
            flag = 1;
            i = i+1;
        }
        if(a1[i] == a2[i] && flag == 1 && ((a1[i]>='A'&&a1[i]<='N') || (a1[i]>='0'&&a1[i]<='9'))){
            output2 = isdigit(a1[i]);
            if(output2){   //当前为数字
                output2 = a1[i]-'0';
                printf("%02d:", output2);
            }
            else{    //字母
                output2 = a1[i]- 'A' + 10;
                printf("%02d:", output2);
            }
            break;
        }
        i++;
    }   //第一个字符串结束
    i = 0;
    while(a3[i] && a4[i]){
        if((a3[i] == a4[i]) && (a3[i]>='a'&&a3[i]<='z')){
            printf("%02d", i);
            break;
        }
        i++;
    }
    return 0;
}
*/
```

# B1015-德才论
这道题算是我目前做过的pat里最难的了
就是逻辑太复杂,给我整迷糊了
巧妙的点有几个
1. 构建数据类型
这道题很典型,即一个实例下对应多个属性,创建一个结构体再好不过了
2. qsort函数
太重要了,尤其是多条件下的排序(之前只用过单条件),简直无敌,必须背下来!
主要就是根据函数最后的return值来进行判定排序先后

## C++
```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
struct node {
    int num, de, cai;
};
int cmp(struct node a, struct node b) {
    if ((a.de + a.cai) != (b.de + b.cai))
        return (a.de + a.cai) > (b.de + b.cai);
    else if (a.de != b.de)
        return a.de > b.de;
    else
        return a.num < b.num;
}
int main() {
    int n, low, high;
    scanf("%d %d %d", &n, &low, &high);
    vector<node> v[4];
    node temp;
    int total = n;
    for (int i = 0; i < n; i++) {
        scanf("%d %d %d", &temp.num, &temp.de, &temp.cai);
        if (temp.de < low || temp.cai < low)
            total--;
        else if (temp.de >= high && temp.cai >= high)
            v[0].push_back(temp);
        else if (temp.de >= high && temp.cai < high)
            v[1].push_back(temp);
        else if (temp.de < high && temp.cai < high && temp.de >= temp.cai)
            v[2].push_back(temp);
        else
            v[3].push_back(temp);
    }
    printf("%d\n", total);
    for (int i = 0; i < 4; i++) {
        sort(v[i].begin(), v[i].end(), cmp);
        for (int j = 0; j < v[i].size(); j++)
            printf("%d %d %d\n", v[i][j].num, v[i][j].de, v[i][j].cai);
    }
    return 0;
}
```

## C
```c
#include<stdlib.h>
#include<stdio.h>

typedef struct _student{
    int id;
    int d;
    int c;
    int rank;
    int sum;
}sstudent, *Student;
int rank(Student s, int h, int l){
    if(s->d<l || s->c<l) return 0;
    else if(s->d>=h && s->c>=h) return 4;
    else if(s->d>=h) return 3;
    else if(s->d>=s->c) return 2;
    else return 1;
}
int comp(const void *a, const void *b){
    Student s1= *(Student*)a;
    Student s2= *(Student*)b;
    if(s1->rank != s2->rank) return s1->rank-s2->rank;
    else if(s1->sum != s2->sum) return s1->sum-s2->sum;
    else if(s1->d != s2->d)     return s1->d - s2->d;
    else if(s1->id != s2->id) return s2->id-s1->id;
    else{
        return 0;
    }
}
int main() {
    int n, l, h,m=0;
    Student students[100000] = {0};
    scanf("%d %d %d", &n, &l, &h);
    for(int i=0; i<n; i++){
        Student s = (Student)malloc(sizeof(sstudent));
        scanf("%d %d %d", &s->id, &s->d, &s->c);
        s->sum = s->d + s->c;
        if((s->rank = rank(s, h, l)) != 0){
            students[m++] = s;
        }
    }
    qsort(students, m, sizeof(Student), comp);
    printf("%d\n", m);
    for(int i = m - 1; i >= 0; i--)
        printf("%d %d %d\n", students[i]->id, students[i]->d, students[i]->c);
    return 0;
}
```

# B1016-部分A+B
这道题两种方案
第一种是大佬用long int来进行求解,然后/10取余算出来
第二种就是我的char数组,位操作
这道题蛮简单的,没啥好说的
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
/*
long depart(long a, long b){
    long pa;
    for(pa=0; a; a/=10){
        if(a%10 == b){
            pa = pa*10+b;
        }
    }
    return pa;
}
int main(){
    long a,b;
    int pa,pb;
    scanf("%ld %d %ld %d", &a, &pa, &b, &pb);
    printf("%ld", depart(a,pa)+depart(b,pb));
    return 0;
}
*/
int main(){
    char s1[11], s2[11];
    int a,b;
    int res1 = 0, res2 = 0;
    scanf("%s %d %s %d", &s1, &a, &s2, &b);
    int k = strlen(s1);
    for(int i=0; i<strlen(s1); i++){
        if((s1[i]-'0') == a){
            res1 = res1*10+(s1[i]-'0');
        }
    }
    for(int j=0; j<strlen(s2); j++){
        if((s2[j]-'0') == b){
            res2 = res2*10+(s2[j]-'0');
        }
    }
    printf("%d", (res1+res2));
    return 0;
}
```

# B1017-A除以B
这道题就很神奇了,我真是没想到
用算法模拟手动除法方式,两位为单位算,神奇呀!
## C++
```c++
#include <iostream>
using namespace std;
int main() {
    string s;
    int a, t = 0, temp = 0;
    cin >> s >> a;
    int len = s.length();
    t = (s[0] - '0') / a;
    if ((t != 0 && len > 1) || len == 1)
        cout << t;  //判断首位
    temp = (s[0] - '0') % a;
    for (int i = 1; i < len; i++) {
        t = (temp * 10 + s[i] - '0') / a;
        cout << t;
        temp = (temp * 10 + s[i] - '0') % a;
    }
    cout << " " << temp;
    return 0;
}
```

## C
```c
int main()
{
    int B;
    char A[1001], *p = A;
    scanf("%s %d", A, &B);
    int twodigit, remainder = 0;
    for(int i = 0; A[i]; i ++)
    {
        twodigit = remainder * 10 + (A[i] - '0');
        A[i] = twodigit / B + '0';
        remainder = twodigit % B;
    }
    B = remainder;
    if(A[0] == '0' && A[1] != '\0') p++;
    printf("%s %d", p, B);
    return 0;
}
```

# B1018-石头剪刀布
这道题我感觉我做的还不错(嘿嘿),逐渐上道了吧
用到了前面学到的结构体思想
我愿称他为一道开窍题,哈哈哈
```c
typedef struct{
    int win_j;
    int win_s;
    int win_b;
    int lose;
    int balance;
    char win_max;
}play,*player;

void charge_func(char aplay, char bplay, player a, player b){
    if(aplay == bplay){
        (a->balance)++;
        (b->balance)++;
    }
    else if(aplay == 'C' && bplay == 'J'){
        (a->win_s)++;
        (b->lose)++;
    }
    else if(aplay == 'J' && bplay == 'B'){
        (a->win_j)++;
        (b->lose)++;
    }
    else if(aplay == 'B' && bplay == 'C'){
        (a->win_b)++;
        (b->lose)++;
    }
    else if(bplay == 'C' && aplay == 'J'){
        (b->win_s)++;
        (a->lose)++;
    }
    else if(bplay == 'J' && aplay == 'B'){
        (b->win_j)++;
        (a->lose)++;
    }
    else if(bplay == 'B' && aplay == 'C'){
        (b->win_b)++;
        (a->lose)++;
    }
}
char max(int B, int C, int J)   //第一种比较方法
{
    if(B >= C && B >= J) return 'B';
    if(C >  B && C >= J) return 'C';
    /* J > B && J > C */ return 'J';
}

void find_max(player k){    //我的比较方法
    int max = k->win_b;
    k->win_max = 'B';
    if(k->win_s > max){
        max = k->win_s;
        k->win_max = 'C';
    }
    if(k->win_j > max){
        k->win_max = 'J';
    }
}

int main(){
    player a = (player)malloc(sizeof(play));
    player b = (player)malloc(sizeof(play));
    a->win_b = a->win_j = a->win_s = a->balance = a->lose = 0;
    b->win_b = b->win_j = b->win_s = b->balance = b->lose = 0;
    int n;
    scanf("%d", &n);
    char aplay,bplay; //两个玩家的出招
    for(int i=0; i<n; i++){
        scanf(" %c %c", &aplay, &bplay);
        charge_func(aplay, bplay, a, b);
    }
    int win_a = (a->win_b) + (a->win_s) + (a->win_j);
    int win_b = (b->win_b) + (b->win_s) + (b->win_j);
    printf("%d %d %d\n", win_a, a->balance, a->lose);
    printf("%d %d %d\n", win_b, b->balance, b->lose);
    find_max(a);
    find_max(b);
    //char max1,max2;
    //max1 = max(a->win_b, a->win_s, a->win_j);
    //max2 = max(b->win_b, b->win_s, b->win_j);
    printf("%c %c",a->win_max, b->win_max);
    //printf("%c %c",max1, max2);
}
```

今天有点刷不动了,明天继续!
