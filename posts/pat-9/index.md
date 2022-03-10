# PAT-Basic 53~57



B1053-B1057<br>
7.23更新
<!--more-->

# 1053-住房空置率
这道题不算太难
有一个小知识点: 在打印"%"时,需要%%进行转义输出

## 柳神解法
```c++
#include <iostream>
using namespace std;
int main() {
    int n, d, k, maybe = 0, must = 0;
    double e, temp;
    cin >> n >> e >> d;
    for (int i = 0; i < n; i++) {
        cin >> k;
        int sum = 0;
        for (int j = 0; j < k; j++) {
            cin >> temp;
            if (temp < e) sum++;
        }
        if(sum > (k / 2)) {
            k > d ? must++ : maybe++;
        }
    }
    double mayberesult = (double)maybe / n * 100;  
    double mustresult = (double)must / n * 100;
    printf("%.1f%% %.1f%%", mayberesult, mustresult);   //百分号 % 注意转义
    return 0;
}
```

## 第二种解法
```c++
#include <stdio.h>

int main()
{
    int N, D, K;
    int empty = 0, pempty = 0, lower;
    float e, E;

    scanf("%d %f %d", &N, &e, &D);
    for(int i = 0; i < N; i++)
    {
        lower = 0;
        scanf("%d", &K);
        for(int j = 0; j < K; j++)
        {
            scanf("%f", &E);
            if(E < e)   lower++;
        }
        if(lower > K / 2 && K > D)  empty++;
        else if(lower > K / 2)      pempty++;
    }
    printf("%.1f%% %.1f%%", 100.0 * pempty / N, 100.0 * empty / N);
    return 0;
}
```

# 1054-求平均值*
这道题是真的梦幻,首先考察对于数字的合法输入,让我一点思路都没有
看了题解发现,可以巧妙的运用sprintf和sscanf函数解决,即对字符串格式化读入,格式化读出,并判断两次字符串是否相同
这个方法一定要掌握,很棒
```c++
#include<cstdio>
#include<cstdlib>
#include<cstring>

using namespace std;

int main(int argc, const char** argv) {
    int num;
    scanf("%d", &num);
    char a[50], b[50];
    double temp = 0.0, sum = 0.0;
    bool flag;
    int count = 0;
    for(int i=0; i<num; i++){
        flag = false;
        scanf("%s", &a);        //重点
        sscanf(a, "%lf", &temp);    
        sprintf(b, "%.2f", temp);
        for(int j=0; j<strlen(a); j++){
            if(a[j] != b[j]){
                flag = true;
            }
        }
        if(flag || temp <-1000 || temp >1000){
            printf("ERROR: %s is not a legal number\n", a);
            continue;
        }
        else{
            count ++;
            sum += temp;
        }
    }
    if(count == 1)
        printf("The average of 1 number is %.2f", sum);
    else if(count > 1)
        printf("The average of %d numbers is %.2f", count, sum / count);
    else
        printf("The average of 0 numbers is Undefined");
    return 0;
}
```

# 1055-集体照
这道题我得了20分,虽然两个测试点没过,但也蛮开心的
后来看了题解,发现写的有些复杂...题解在源数据中直接进行跳跃取值的方法属实机智
由于我对特殊的第一排和后面的排分开继续逻辑判断,并用了一个全局的大容量vector,导致冗余很大,完全可以申请一个临时vector
## 大佬题解
```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
using namespace std;
struct node {
    string name;
    int height;
};
int cmp(struct node a, struct node b) {
    return a.height != b.height ? a.height > b.height : a.name < b.name;
}
int main() {
    int n, k, m;
    cin >> n >> k;
    vector<node> stu(n);
    for(int i = 0; i < n; i++) {
        cin >> stu[i].name >> stu[i].height;
    }
    sort(stu.begin(), stu.end(), cmp);
    int t = 0, row = k;
    while(row) {
        if(row == k)
            m = n - n / k * (k - 1);
        else
            m = n / k;
        vector<string> ans(m);  //临时vector
        ans[m / 2] = stu[t].name;
        // 左边一列
        int j = m / 2 - 1;
        for(int i = t + 1; i < t + m; i = i + 2)    //跳跃取值
            ans[j--] = stu[i].name;
        // 右边一列
        j = m / 2 + 1;
        for(int i = t + 2; i < t + m; i = i + 2)
            ans[j++] = stu[i].name;
        // 输出当前排
        cout << ans[0];
        for(int i = 1; i < m; i++)
            cout << " " << ans[i];
        cout << endl;
        t = t + m;
        row--;
    }
    return 0;
}
```

## 菜鸡的题解
```c++
typedef struct{
    int height;
    string name;
}person, *point_person;

int cmp(person a, person b){
    if(a.height == b.height){
        return a.name < b.name;
    }
    return a.height > b.height;
}

int main(int argc, const char** argv) {
    int N,K,n,m;
    scanf("%d %d\n", &N, &K);
    n = N/K;
    m = N%K;
    int last_row = m+n;
    vector<person> data(N);
    for(int i=0; i<N; i++){
        person new_person;
        cin >> new_person.name >> new_person.height;
        data[i] = new_person;
    }
    sort(data.begin(), data.end(),cmp);
    vector<string> shit(m+n,"0");       //菜鸡的全局vector,并不好用
    int cur = 0, mid;
    int charge =  last_row-K;
    for(int k=0; k<K; k++){
        if(k==0){
            mid = last_row/2 +1;
            --mid;
            shit[mid] = data[cur++].name;
            for(int h=1,max = last_row; last_row>0; last_row-=2){
                if((mid-h)>=0){
                    shit[mid-h] = data[cur++].name;
                }
                if((mid+h)<max){
                    shit[mid+h] = data[cur++].name;
                }
                h++;
            }
            for(auto i = shit.begin(); i<shit.end(); i++){
                if(i == shit.end()-1){
                    printf("%s\n", (*i).c_str());
                }
                else{
                    printf("%s ", (*i).c_str());
                }
            }
        }
        else{
            mid = K/2 +1;
            --mid;
            shit[mid] = data[cur++].name;
            int temp;
            for(int h=1,max = K,temp = K; temp>0; temp-=2){
                if((mid-h)>=0){
                    shit[mid-h] = data[cur++].name;
                }
                if((mid+h)<max){
                    shit[mid+h] = data[cur++].name;
                }
                h++;
            }
            for(auto i = shit.begin(); i<shit.end()-charge; i++){
                if(i == shit.end()-1-charge){
                    printf("%s\n", (*i).c_str());
                }
                else{
                    printf("%s ", (*i).c_str());
                }
            }
        }
    }
    return 0;
}
```

# 1056-组合数的和
快乐白给题
```c++
#include<cstdio>

using namespace std;

int main(int argc, const char** argv) {
    int num;
    scanf("%d", &num);
    int sum = 0;
    int temp;
    for(int i=0; i<num; i++){
        scanf("%d", &temp);
        sum += (temp*10+temp)*(num-1);
    }
    printf("%d", sum);
    return 0;
}
```

# 1057-数零壹
这道题考察 十进制转二进制 的代码实现,运用**短除法**
## 十进制转二进制
```c++
#include<stdio.h>
 
int main()
{
    int n;
    int a[32]={0};
    int i,j;
    while(scanf("%d",&n)!=EOF)  //EOF: END OF FILE
    {
        i=0;
        while(n>0)
        {
            a[i]=n%2;
            n=n/2;
			i++;
        }
        for(j=i-1;j>=0;j--) //注意是反向输出
            printf("%d",a[j]);
        printf("\n");
    }
    return 0;
}
```

## AC题解
```c++
#include<cstdio>
#include<string>
#include<iostream>

using namespace std;

int main(int argc, const char** argv) {
    string s;
    getline(cin,s);
    int sum=0;
    for(int i=0; i<s.size(); i++){
        if(isalpha(s[i])){
            /*
            if(s[i]<='Z' && s[i]>='A'){
                sum += s[i]-'A'+1;
            }
            else{
                sum += s[i]-32-'A'+1;
            }
            */
           sum += tolower(s[i]) - 'a' + 1;  //优化判断
        }
    }
    int count1 = 0, count0 = 0;
    while(sum != 0){
        if(sum%2 == 1){
            count1++;
        }
        else{
            count0++;
        }
        sum = sum/2;
    }
    printf("%d %d", count0, count1);
    return 0;
}
``` 

