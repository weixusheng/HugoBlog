# PAT-Basic 59~64



B1059-B1064<br>
7.23更新
<!--more-->

# 1059-C语言竞赛
这道题没啥难的,主要是写好素数判断函数和最后的分情况判定
```c++
#include<cstdio>
#include<map>
#include<cmath>

using namespace std;

int is_s(int a){
    int i;
    for(int i=2; i<= sqrt(a); i++){
        if(a%i == 0){
            return false;
        }
    }
    return true;
}

int main(int argc, const char** argv) {
    int num;
    scanf("%d", &num);
    map<int,int> data;
    int tempid,rank;
    for(int i=0; i<num; i++){
        scanf("%d", &tempid);
        data[tempid] = i+1;
    }
    int search;
    scanf("%d", &search);
    for(int j=0; j<search; j++){
        scanf("%d", &tempid);
        rank = data[tempid];
        if(rank == -1){
            printf("%04d: Checked\n",tempid);
        }
        else if(rank == 1){
            printf("%04d: Mystery Award\n",tempid);
            data[tempid] = -1;
        }
        else if(rank == NULL){
            printf("%04d: Are you kidding?\n",tempid);
        }
        else if(is_s(rank)){
            printf("%04d: Minion\n",tempid);
            data[tempid] = -1;
        }
        else{
            printf("%04d: Chocolate\n",tempid);
            data[tempid] = -1;
        }
    }
    return 0;
}
```

# 1060-爱丁顿数
这道压轴数学题,其实没有很难,但我写的有点复杂了...写了个O(n<sup>2</sup>)复杂度的循环
其实一个for循环就能完成判断,最后导致一个测试点没过
```c++
#include<cstdio>
#include<cmath>
#include<vector>
#include<algorithm>
#include<iterator>
#include<iostream>

using namespace std;

int main(int argc, const char** argv) {
    int num;
    scanf("%d", &num);
    vector<int> data(num);
    for(int h=0; h<num; h++){
        scanf("%d", &data[h]);
    }
    sort(data.begin(), data.end(), greater<int>());
    /* 一个测试点超时
    int next = 0, max = 0;
    bool ok = true;
    for(int i=0; i<num; i++){
        for(int j=0; j<i; j++){
            if(data[j] < i+1){
                ok = false;
                break;
            }
        }
        if(ok){
            max = i;
        }
        else{
            break;
        }
    }
    */
    /* 正确方法 */
    int k;
    for(k=0; k<num && k+1 < data[k]; k++);
    printf("%d", k);
    //printf("%d", max);
    return 0;
}
```

# 1061-判断题
快乐白给题
```c++
#include<cstdio>
#include<vector>
using namespace std;

int main(int argc, const char** argv) {
    int pnum, qnum;
    scanf("%d %d", &pnum, &qnum);
    vector<int> score(qnum);
    for(int h=0; h<qnum; h++){
        scanf("%d", &score[h]);
    }
    vector<int> right(qnum);
    for(int w=0; w<qnum; w++){
        scanf("%d", &right[w]);
    }
    int temp_score;
    int temp_choose;
    for(int i=0; i<pnum; i++){
        temp_score = 0;
        for(int k=0; k<qnum; k++){
            scanf("%d", &temp_choose);
            if(temp_choose == right[k]){
                temp_score += score[k];
            }
        }
        printf("%d\n", temp_score);
    }
    return 0;
}
```

# 1062-最简分数
考察最大公约数,和分数比较(转double类型)
之前看到过最大公约数函数,但是不知道什么时候能用上,就没背熟,这道题是经典例子
## 求最大公约数-辗转相除法
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/pat-11/1.png)

## CODE
```c++
#include<cstdio>
using namespace std;

int gcd(int a, int b){
    return b == 0 ? a : gcd(b, a%b);
}

int main(int argc, const char** argv) {
    int s1,s2,s3,s4,shit;
    scanf("%d/%d %d/%d %d", &s1,&s2,&s3,&s4,&shit);
    double shit1 = (double)s1/s2;
    double shit2 = (double)s3/s4;
    double temp;
    if(shit1 > shit2){
        temp = shit2;
        shit2 = shit1;
        shit1 = temp;
    }
    int flag = 0;
    for(int i=0; i<shit; i++){
        double temp_shit = (double)i/shit;
        if(temp_shit<shit2 && temp_shit>shit1 && gcd(i,shit) == 1){
            if(flag == 0){
                flag = 1;
            }
            else{
                printf(" ");
            }
            printf("%d/%d", i,shit);
        }
    }
    return 0;
}
```

# 1063-计算谱半径
快乐白给题
>2020.7.16
这道题有一个很重要的知识点:
**小数点后保留两位四舍五入**的输出
printf函数在进行格式输出(double类型)时,自带四舍五入!但是整数时需要进行+0.5处理
```c++
int temp = int(data + 0.5);
```

## CODE
```c++
#include<cstdio>
#include<cstdlib>
#include<cmath>

using namespace std;

int main(int argc, const char** argv) {
    int num;
    scanf("%d", &num);
    int data1,data2;
    double sum, min = 0;
    for(int i=0; i<num; i++){
        sum = 0;
        scanf("%d %d", &data1, &data2);
        sum = data1*data1 + data2*data2;
        sum = sqrt(sum);
        if(min < sum){
            min = sum;
        }
    }
    printf("%.2f",min);
    return 0;
}
```

# 1064-朋友数
知识点: set默认排序,从小到大
```c++
#include<cstdio>
#include<set>
using namespace std;

int main(int argc, const char** argv) {
    int num;
    scanf("%d", &num);
    set<int> data;
    int temp, sum = 0;
    int count = 0;
    for(int i=0; i<num; i++){
        sum = 0;
        scanf("%d", &temp);
        while(temp > 0){
            sum += temp%10;
            temp = temp/10;
        }
        if(data.find(sum) == data.end()){
            data.insert(sum);
            count++;
        }
    }
    printf("%d\n", count);
    int flag = 0;
    for(auto it = data.begin(); it != data.end(); it++){
        if(flag == 0){
            flag = 1;
        }
        else{
            printf(" ");
        }
        printf("%d", *it);
    }
    return 0;
}
```
