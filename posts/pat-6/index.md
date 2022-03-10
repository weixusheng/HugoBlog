# PAT-Basic 36~40



B1036-B1040<br>
一个半小时前四道题AC,第五道数学题挂了...我爱数学!
希望25号能拿个满分<br>
7.23更新
<!--more-->

# 1036-跟奥巴马一起编程
跟之前沙漏题一个套路,printf就完事了
```c++
#include<stdio.h>
using namespace std;

int main(int argc, const char** argv) {
    int num;
    char signal;
    scanf("%d %c", &num, &signal);
    int row = (num+1)/2;
    int j;
    for(j=0; j<num; j++){
        printf("%c", signal);
    }
    printf("\n");
    for(int i=0; (i < row-2); i++){
            printf("%c", signal);
            for(int k=0; k<(num-2); k++){
                printf(" ");
            } 
            printf("%c\n", signal);
    }
    for(j=0; j<num; j++){
        printf("%c", signal);
    }
    return 0;
}
```

# 1037-在霍格沃茨找零钱
经典送分白给题
```c++
#include<stdio.h>
using namespace std;

int main(int argc, const char** argv) {
    long int data[3];
    int s1,s2,s3,k1,k2,k3;
    scanf("%d.%d.%d %d.%d.%d", &s1,&s2,&s3,&k1,&k2,&k3);
    long int sum1,sum2;
    int cache = 17*29;
    sum1 = s1*cache + s2*29 + s3;
    sum2 = k1*cache + k2*29 + k3;
    int change = sum2-sum1;
    bool shit = false;
    if(change < 0){
        shit = true;
        change = abs(change);
    }
    data[0] = change/cache;
    change %= cache;
    data[1] = change/29;
    change %= 29;
    data[2] = change;
    if(shit){
        printf("-");
    }
    printf("%d.%d.%d", data[0], data[1], data[2]);
    return 0;
}
```

# 1038-统计同成绩学生
快乐hash
```c++
#include<stdio.h>

int main(int argc, const char** argv) {
    int hashdata[100000] = {0};
    int num;
    scanf("%d",&num);
    int temp;
    for(int i=0; i<num; i++){
        scanf("%d",&temp);
        hashdata[temp]++;
    }
    int query;
    scanf("%d", &query);
    for(int j=0; j<query; j++){
        scanf("%d", &temp);
        if(j != query-1){
            printf("%d ", hashdata[temp]);
        }
        else{
            printf("%d", hashdata[temp]);
        }
    }
    return 0;
}
```

# 1039-到底买不买
这道题跟之前那些操作字符串一个套路
```c++
#include<stdio.h>
#include<string>
#include<iostream>

using namespace std;

int main(int argc, const char** argv) {
    int data[1000] = {0};
    bool shit = false;
    string s1,s2;
    getline(cin, s1);
    getline(cin, s2);
    int sum = s1.size();
    int count = 0;
    for(int j=0 ; j<s1.size(); j++){
        data[s1[j]]++;
    }
    for(int i=0; i<s2.size(); i++){
        if(data[s2[i]] == 0){   //没有找到
            shit = true;
            count++;
        }
        else{
            data[s2[i]]--;
            sum--;
        }
    }
    if(shit){
        printf("No %d", count);
    }
    else{
        printf("Yes %d", sum);
    }
    return 0;
}
```

# 1040-有几个PAT
这道题我一开始想用三指针,不过写着写着就晕了,而且还少算了很多情况
无奈看了一眼题解...发现这道题这么真实吗,就三行代码...
我发现最后一道压轴题,不是数学问题就是排序实现问题,等我再去看看算法笔记
 - 每个A对应的PA组合数量是A之前P的数量，
 - 每个T对应的PAT组合数量是T之前所有A对应的PA组合数量的累加
 - 所有的PAT组合数量是所有T对应的PAT组合数量的累加

感觉这道题有点动态规划那味儿了
```c++
#include <iostream>
#include <string>
using namespace std;

int main() {
    string s;
    cin >> s;
    int len = s.length(), result = 0, countp = 0, countt = 0;
    for (int i = 0; i < len; i++) {
        if (s[i] == 'T')
            countt++;
    }
    for (int i = 0; i < len; i++) {
        if (s[i] == 'P') countp++;
        if (s[i] == 'T') countt--;
        if (s[i] == 'A') result = (result + (countp * countt) % 1000000007) % 1000000007;
    }
    cout << result;
    return 0;
}
```


