# PAT-Advanced 05-10



1005-1010
<!--more-->

# 1005-Spell It Right
相对比较简单,不过要注意测试用例的数据大小
```c++
#include<cstdio>
#include<iostream>
#include<string>
using namespace std;

int main(int argc, char const *argv[])
{
    int shit, sum = 0, tmp;
    string shit2;
    cin >> shit2;
    /* 不能这样用,因为给的测试数据太大了
    while(shit != 0){
        tmp = shit%10;
        sum += tmp;
        shit = shit/10;
    }
    */
    for(int i=0; i<shit2.length(); i++){
        sum += shit2[i]-'0';
    }
    string tb[10] = {"zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine"};
    string ans = to_string(sum);
    cout << tb[ans[0]-'0'];
    for(int i=1; i<ans.length(); i++){
        cout << " " << tb[ans[i]-'0'];
    }
    return 0;
}
```

# 1006-Sign In and Sign Out
快乐白给题
```c++
#include<iostream>
#include<string>
using namespace std;

int main(int argc, char const *argv[])
{
    int num;
    cin >> num;
    string tmpid, f_id, l_id, s1, s2, tmp1, tmp2;
    cin >> tmpid >> s1 >> s2;
    f_id = tmpid;
    l_id = tmpid;
    for(int i=1; i<num; i++){
        cin >> tmpid >> tmp1 >> tmp2;
        if(tmp1 < s1){
            s1 = tmp1;
            f_id = tmpid;
        }
        if(tmp2 > s2){
            s2 = tmp2;
            l_id = tmpid;
        }
    }
    cout << f_id << " " << l_id;
    return 0;
}
```

# 1007-Maximum Subsequence Sum
一个相对简单的动态规划(然而我刚开始并不会)
```c++
#include<vector>
#include<iostream>
using namespace std;

int main(int argc, char const *argv[])
{
    int num;
    cin >> num;
    vector<int> data(num);
    for(int i=0; i<num; i++){
        cin >> data[i];
    }
    int tmp = 0, tmpindex = 0, left = 0, right = num-1, max = -1;
    for(int i=0; i<num; i++){
        tmp += data[i];
        if(tmp < 0){
            tmp = 0;
            tmpindex = i+1;
        }
        else{
            if(tmp > max){
                left = tmpindex;
                right = i;
                max = tmp;
            }
        }
    }
    if(max < 0){
        max = 0;
    }
    cout << max << " " << data[left] << " " << data[right];
    return 0;
}
```

# 1008-Elevator
这道题题解有一个点写的很是精妙
```c++
while(cin >> a)
```
原来这也可以判定输入边界!
```c++
#include <iostream>
using namespace std;

int main() {
    int a, now=0, sum=0;
    cin>>a;
    while(cin >> a) {
        if(a > now)
            sum = sum+6* (a-now);
        else 
            sum = sum+4* (now-a);
        now = a;
        sum += 5;    
    }
    cout << sum;
    return 0;
}
```

# 1009-Product of Polynomials
## 菜鸡的方法
这道题我写的有些麻烦,而且需要注意的是,当系数为0是是不会输出的,所以最后还要去除一下系数为0的项
### earse()函数的使用
在消除系数为0项的过程中,遇到了earse()函数,发现这个函数说道还挺多的
首先earse用迭代器的话,就会有些麻烦
1. 迭代器方式
当使用erase以迭代器方式删除vector中的元素时，vector会自动将被删除元素后边的元素往上挪一位，所以此时指向删除元素的迭代器指向了被删除元素后面的元素，所以在循环中，此时迭代器就不应该 +1
```c++
for (itE = listE.begin(); itE != listE.end();){
    if (currE->start == findV){
        listE.erase(itE);
    }
    else{
        itE++;
    }
}
```
更好的写法
```c++
for (itE = listE.begin(); itE != listE.end();){
    if (currE->start == findV){    
        itE=listE.erase(itE);
    }
    else{
        itE++;
    }
}
```
这段代码就是利用了erase的返回值，当我们用erase删除一个元素后，erase返回的是下一个元素的迭代器，将这个返回值赋给那个原来指向被删除元素的迭代器，所以这个迭代器就指向了下一个元素,保证迭代器的指向安全

2. 利用begin + i的方式
删除data容器中第i个元素(推荐写法)
```c++
data.erase(data.begin() + i)
```

## CODE
```c++
#include<iostream>
#include<vector>
#include<set>
#include<algorithm>
using namespace std;

int main(int argc, char const *argv[])
{
    int num1, num2;
    int tmp1;
    double tmp2;
    vector<double> res(2000,0);
    vector<double> data1(1000,0);
    vector<double> data2(1000,0);
    set<int> s1;
    set<int> s2;
    cin >> num1;
    for(int j=0; j<num1; j++){
        cin >> tmp1 >> tmp2;
        s1.insert(tmp1);
        data1[tmp1] += tmp2;
    }
    cin >> num2;
    for(int j=0; j<num2; j++){
        cin >> tmp1 >> tmp2;
        s2.insert(tmp1);
        data2[tmp1] += tmp2;
    }
    vector<int> res2;
    set<int> nr;
    int sum;
    double sum2;
    for(auto i=s1.begin(); i!=s1.end(); i++){
        for(auto j=s2.begin(); j!=s2.end(); j++){
            sum = *i+*j;
            sum2 = data1[*i] * data2[*j];
            if(nr.find(sum) == nr.end()){
                nr.insert(sum);
                res2.push_back(sum);
            }
            res[sum] += sum2;
        }
    }
    sort(res2.begin(), res2.end(), greater<int>());
    for(int w = 0; w < res2.size(); w++){
        if(int(res[res2[w]]) == 0){
            res2.erase(res2.begin()+w);
        }
    }
    cout << res2.size();
    for(int k=0; k<res2.size(); k++){
        printf(" %d %.1f", res2[k], res[res2[k]]);
    }
    return 0;
}
```
## 柳神的代码
这个代码就清晰了很多,首先他只保存了第一个数据集,而在输入第二个数据集时,直接进行了计算,并且,对于系数为0的判断,进行了单独计数和最后输出的判断
```c++
#include <iostream>
using namespace std;
int main() {
    int n1, n2, a, cnt=0;
    scanf("%d", &n1);
    double b, arr[1001] = {0.0}, ans[2001] = {0.0};
    for(int i=0; i<n1; i++) {
        scanf("%d %lf", &a, &b);
        arr[a] =b;    
    }
    scanf("%d", &n2);
    for(int i=0; i<n2; i++) {
        scanf("%d %lf", &a, &b);
        for(int j=0; j<1001; j++)
            ans[j+a] +=arr[j] *b;    
    }
    for(int i=2000; i>=0; i--)
        if(ans[i] !=0.0) 
            cnt++; 
    printf("%d", cnt);
    for(int i=2000; i>=0; i--)
        if(ans[i] !=0.0)
            printf(" %d %.1f", i, ans[i]);
    return 0;
}
```

# 1010-Radix
这道题没想到用的是二分法,让我很是惊讶
并且题解中包含了三个新的知识点

## rbegin(),rend()
c.begin() 返回一个迭代器，它指向容器c的第一个元素
c.end() 返回一个迭代器，它指向容器c的最后一个元素的下一个位置
c.rbegin() 返回一个逆序迭代器，它指向容器c的最后一个元素
c.rend() 返回一个逆序迭代器，它指向容器c的第一个元素前面的位置
反向迭代器是一种反向遍历容器的迭代器。也就是从最后一个元素到第一个元素遍历容器。反向迭代器将自增（和自减）的含义反过来了：
对于反向迭代器 **++ 运算将访问前一个元素，而 -- 运算则访问下一个元素**

## pow(int, int)
double pow(double x, double y) 返回 x 的 y 次幂，即 x<sup>y</sup>

## max_element(first, last, comp);
first: 容器比较起始位置
last: 容器比较结束位置
comp: 比较函数(可选)

## CODE
```c++
#include <iostream>
#include <cctype>
#include <algorithm>
#include <cmath>
using namespace std;

long long convert(string n, long long radix) {
    long long sum = 0;
    int index = 0, temp = 0;
    for (auto it=n.rbegin(); it!=n.rend(); it++) {  //反向迭代器
        temp = isdigit(*it) ? *it-'0' : *it-'a'+10;
        sum += temp * pow(radix, index++);    //pow函数,指数运算
    }
    return sum;
}
long long find_radix(string n, long long num) {
    char it = *max_element(n.begin(), n.end()); //max_element : 返回容器中最大值的第一个位置
    long long low= (isdigit(it) ?it-'0': it-'a'+10) +1;     //二分法确定边界
    long long high=max(num, low);
    while (low<=high) {
        long long mid= (low+high) /2;
        long long t = convert(n, mid);  //转换为 mid 进制
        if (t<0||t>num){    //判断是否满足条件
            high=mid-1; //前半部分二分
        } 
        else if(t==num){
            return mid;
        }  
        else {
            low=mid+1;  //后半部分二分
        }
    }
    return -1;
}
int main() {
    string n1, n2;
    long long tag=0, radix=0, result_radix;
    cin >> n1 >> n2 >> tag >> radix;
    result_radix = tag==1 ?find_radix(n2, convert(n1, radix)) :find_radix(n1, convert(n2, radix));
    if (result_radix!=-1) {
        printf("%lld", result_radix);    
    } 
    else {
        printf("Impossible");    
    }  
    return 0;
}
```
