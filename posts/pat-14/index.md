# PAT-Basic 74~75



B1074-B1075<br>
今天只有两道题...因为都是我最怕的快乐数据题<br>
7.23更新
<!--more-->

# 1074-宇宙无敌加法器*
这道题我写了80行代码...题解是35行
我太菜,主要是没想明白怎么处理开始的0,最后用的vector存储有效数字
看了题解发现字符串还能有这种补0的迷幻操作,学到了...谢谢大佬
最后的判断0开始输出简直是点睛之笔!
而我的方法就很笨了,需要vector慢慢转数据呜呜呜

## 题解方法
```c++
#include<cstdio>
#include<vector>
#include<iostream>
using namespace std;

int main() {
    string s, s1, s2, ans;
    int carry = 0, flag = 0;
    cin >> s >> s1 >> s2;
    ans = s;
    string ss1(s.length() - s1.length(), '0');
    s1 = ss1 + s1;
    string ss2(s.length() - s2.length(), '0');
    s2 = ss2 + s2;
    for(int i = s.length() - 1; i >= 0; i--) {
        int mod = s[i] == '0' ? 10 : (s[i] - '0');
        ans[i] = (s1[i] - '0' + s2[i] - '0' + carry) % mod + '0';
        carry = (s1[i] - '0' + s2[i] - '0' + carry) / mod;
    }
    if (carry != 0) ans = '1' + ans;
    for(int i = 0; i < ans.size(); i++) {
        if (ans[i] != '0' || flag == 1) {
            flag = 1;
            cout << ans[i];
        }
    }
    if (flag == 0) cout << 0;
    return 0;
}
```

## 菜鸡的方法
有两个测试点没过 分数: 16/20
```c++
#include<cstdio>
#include<vector>
#include<iostream>
using namespace std;

vector<int> shit1(21);
vector<int> data1(21);
vector<int> data2(21);

vector<int> num_convert(int num, vector<int> data){
    int temp;
    int sum = 0;
    while(num > 0){
        temp = num %10;
        data[++sum] = temp;
        num = num/10;
    }
    data[0] = sum;
    return data;
}

int main(int argc, const char** argv) {
    int shit, num1, num2;
    scanf("%d %d %d", &shit, &num1, &num2);
    vector<int> res(20);
    shit1 = num_convert(shit,shit1);
    data1 = num_convert(num1,data1);
    data2 = num_convert(num2,data2);
    int size1 = data1[0];
    int size2 = data2[0];
    int leftshit, tempsum;
    int j = 1;
    int inshit = 0;
    while(size1 > 0 || size2 > 0){
        tempsum = 0;
        if(size1 >0 && size2 >0){
            tempsum = data1[j] + data2[j];
            size1--;
            size2--;
        }
        else{
            if(size1 > 0){
                tempsum = data1[j];
                size1--;
            }
            else{
                tempsum = data2[j];
                size2--;
            }
        }
        if(inshit){
            tempsum += inshit;
        }
        int convert_shit1;
        if(shit1[j] == 0){
            convert_shit1 = 10;
        }
        else{
            convert_shit1 = shit1[j];
        }
        if(tempsum >= convert_shit1){
            inshit = tempsum/convert_shit1;
        }
        else{
            inshit = 0;
        }
        leftshit = tempsum % convert_shit1;
        res[j] = leftshit;
        j++;
    }
    int h = j-1;
    if(inshit){
        res[j] = 1;
        h = j;
    }
    for(; h>0; h--){
        printf("%d", res[h]);
    }
    if(j == 1){
        printf("0");
    }
    return 0;
}
```

# 1075-链表元素分类
这道题我搞了很多一维vector...有点创建上瘾了
就是没想到二维vector和结构体,可能之前用结构体有阴影了
因为只得到了17分,完成度太低,就不放自己写的代码了
这道题有一个点很值得学习,一个是vector的push_back()
最后用vector.size()遍历,这样不需要提前分配空间,省了很多事
之前学习过,如果需要scanf中输入二维vector中,需要提前分配空间
```c++
vector<vector<int> > data;
data.resize(m, vector<int>(n));
```
而使用push_back生成的非预分配二维vector
```c++
vector<int> v[3];
v[i].push_back(data);
```
在最后的输出中,由于当前的next地址和下一节点地址相同,那么可以在一行内输出
这样不仅输出清晰,而且还可以有效解决最后一个地址是-1的特殊情况
精妙的方法!

## CODE
```c++
#include<cstdio>
#include<map>
#include<vector>
#include <iostream>
using namespace std;

struct node {
    int data, next;
}list[100000];      // hash vector<node>
vector<int> v[3];

int main() {
    int start, n, k, a;
    scanf("%d%d%d", &start, &n, &k);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a);
        scanf("%d%d", &list[a].data, &list[a].next);
    }
    int p = start;
    while(p != -1) {
        int data = list[p].data;
        if (data < 0)
            v[0].push_back(p);
        else if (data >= 0 && data <= k)
            v[1].push_back(p);
        else
            v[2].push_back(p);
        p = list[p].next;
    }
    int flag = 0;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < v[i].size(); j++) {
            if (flag == 0) {
                printf("%05d %d ", v[i][j], list[v[i][j]].data);
                flag = 1;
            } else {
                printf("%05d\n%05d %d ", v[i][j], v[i][j], list[v[i][j]].data);
            }
        }
    }
    printf("-1");
    return 0;
}
```
