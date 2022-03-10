# PAT-Basic 11~12



B1011-B1012<br>
2020-07-18 优化题解 
<!--more-->

# B1011-A+B 和 C
没啥好说哒
```c
main(){
    long a,b,c,d;
    int num;
    scanf("%d", &num);
    for(int i=1; i<=num; i++){
        scanf("%ld %ld %ld", &a, &b, &c);
        d = a+b;
        if(d>c){
            printf("Case #%d: true\n", i);
        }
        else{
            printf("Case #%d: false\n", i);
        }
    }
    return 0;
}
```

# B1012-数字分类
这道题就很墨迹...而且我的方法还不能全部通过测试点,就很迷
我也不知道我错在哪里了...主要PAT也不提示输出的错误,太不人性化了...
## 收获
 - 在调试我的方法的时候,double输出显示nan
出现这个是因为"在计算除法运算时，分母为0，导致结果是个无穷大的数，无法显示就用NaN代替了"
以后处理数据要注意分母的值
 - !运算符
可以直接使用 !flag 进行取反操作
```
int flag = 0;
flag = !flag; //flag = 1
```

## C++
```c++
#include <iostream>
#include <vector>
using namespace std;
int main() {
    int n, num, A1 = 0, A2 = 0, A5 = 0;
    double A4 = 0.0;
    cin >> n;
    vector<int> v[5];
    for (int i = 0; i < n; i++) {
        cin >> num;
        v[num%5].push_back(num);
    }
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < v[i].size(); j++) {
            if (i == 0 && v[i][j] % 2 == 0) A1 += v[i][j];
            if (i == 1 && j % 2 == 0) A2 += v[i][j];
            if (i == 1 && j % 2 == 1) A2 -= v[i][j];
            if (i == 3) A4 += v[i][j];
            if (i == 4 && v[i][j] > A5) A5 = v[i][j];
        }
    }
    for (int i = 0; i < 5; i++) {
        if (i != 0) printf(" ");
        if (i == 0 && A1 == 0 || i != 0 && v[i].size() == 0) {
            printf("N"); continue;
        }
        if (i == 0) printf("%d", A1);
        if (i == 1) printf("%d", A2);
        if (i == 2) printf("%d", v[2].size());
        if (i == 3) printf("%.1f", A4 / v[3].size());
        if (i == 4) printf("%d", A5);
    }
    return 0;
}
```

## 正确答案
```c
#define MAXN 1000

int n[MAXN];
int n5[MAXN];//配合a5，存储所有被 5 除后余 4 的数字
int k = 0;

int main()
{
    int a1 = 0, a2 = 0, a3 = 0, a5 = 0, count4 = 0;
    int flag1 = 0, flag2 = 0, flag3 = 0, flag4 = 0, flag5 = 0;
    double a4, sum4 = 0;
    int N, i;
    int flag = 1;//配合a2，用来* -1
    scanf("%d",&N);
    for (i = 0; i < N; i++)
    {
        scanf("%d",&n[i]);
    }
    for (i = 0; i < N; i++)
    {
        if (n[i] % 5 == 0 && n[i] % 2 == 0)
        {
            a1 += n[i];
            flag1 = 1;
        }
        if (n[i] % 5 == 1)
        {
            a2 = a2 + flag * n[i];
            flag *= (-1);
            flag2 = 1;
        }
        if (n[i] % 5 == 2)
        {
            a3++;
            flag3 = 1;
        }
        if (n[i] % 5 == 3)
        {
            sum4 = sum4 + n[i];
            count4++;
            flag4 = 1;
        }
        if (n[i] % 5 == 4)
        {
            n5[k++] = n[i];
            flag5 = 1;
        }
    }
    a4 = sum4 / count4;
    a5 = n5[0];
    for (i = 1; i < k; i++)
    {
        if (n5[i] > a5)
        {
            a5 = n5[i];
        }
    }
    if (flag1)
    {
        printf("%d",a1);
    }
    else
    {
        printf("N");
    }
    printf(" ");
    if (flag2)
    {
        printf("%d", a2);
    }
    else
    {
        printf("N");
    }
    printf(" ");
    if (flag3)
    {
        printf("%d", a3);
    }
    else
    {
        printf("N");
    }
    printf(" ");
    if (flag4)
    {
        printf("%.1lf", a4);
    }
    else
    {
        printf("N");
    }
    printf(" ");
    if (flag5)
    {
        printf("%d", a5);
    }
    else
    {
        printf("N");
    }
    return 0;
}
```

## 我的蜜汁答案
```c
int main(){
    int a1=0, a2=0, a3=0, a5=0, cur, temp, flag=0,count=0;
    int useless;
    scanf("%d", &useless);
    double a4=0;
    char ch;
    while(1){
        ch = getchar();
        if(ch == '\n'){
            break;
        }
        scanf("%d", &cur);
        temp = cur%5;
        if(temp == 0 && cur%2 == 0){
            a1 += cur;
        }
        else if(temp == 1){
            if(flag == 0){
                a2 += cur;
            }
            else{
                a2 -= cur;
            }
            flag = !flag;
        }
        else if(temp == 2){
            a3++;
        }
        else if(temp == 3)
        {
            a4 += cur;
            ++count;
        }
        else if(temp = 4)
        {
            if(cur > a5){
                a5 = cur;
            }
        }
    }
    if(a1 == 0){
        printf("N");
    }
    else
    {
        printf("%d", a1);
    }
    if(a2 == 0){
        printf(" N");
    }
    else
    {
        printf(" %d", a2);
    }
    if(a3 == 0){
        printf(" N");
    }
    else
    {
        printf(" %d", a3);
    }
    if(a4 == 0){
        printf(" N");
    }
    else
    {
        a4 = a4/count;
        printf(" %.1f", a4);
    }
    if(a5 == 0){
        printf(" N");
    }
    else
    {
        printf(" %d", a5);
    }
    return 0;
}
```

