# PAT-Basic 29~35



B1029-B1035<br>
前几天学完了c++和STL的基础,今天开始换c++写,发现解题速度提升很明显,由于c++向下兼容c,所以简直是各种语法糖呀,磨刀不误砍柴工!<br>
2020-07-19 优化题解
<!--more-->

# 1029-旧键盘
这道题收获了一个大坑: 由于这道题其中的一个限制是,没有重复元素,所以我就想当然的用了set,由于set本身有序,而题中要求是按照插入顺序,我就换成了unordered_set,我想这应该没问题了吧,结果就是无法通过...
后来看到一篇blog才发现,**无序set并不是根据insert顺序排列，而是根据元素hash值排列**
字符转大小写用**toupper和tolower**，在cctype头文件里，如果不包含头文件 cctype 仅仅有 iostream 也是可以的
**对于string.find(x),如果没找到,则返回"string::npos"**
```c++
#include<stdio.h>
#include<string>
#include<iostream>
using namespace std;

int main(){
    string s1, s2,ans;
    cin >> s1 >> s2;
    for (int i = 0; i < s1.length(); i++){
        if (s2.find(s1[i]) == string::npos && ans.find(toupper(s1[i])) == string::npos){
            ans += toupper(s1[i]);
        }
    }
    cout << ans;
}
```

# 1030-完美数列
这道题直接用c做的,感觉还算简单,不过最后有一个测试点超时了,原因在于比较算法没有优化...
以后这种寻找最大最小值的函数,还是要多思考
## C++
```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main() {
    int n;
    long long p;
    scanf("%d%lld", &n, &p);
    vector<int> v(n);
    for (int i = 0; i < n; i++)
        cin >> v[i];
    sort(v.begin(), v.end());
    int result = 0, temp = 0;
    for (int i = 0; i < n; i++) {
        for (int j = i + result; j < n; j++) {
            if (v[j] <= v[i] * p) {
                temp = j - i + 1;
                if (temp > result)
                    result = temp;
            } else {
                break;
            }
        }
    }
    cout << result;
    return 0;
}
```

## C
```c++
#include<stdio.h>
#include<stdlib.h>

int cmp(const void *a, const void *b){
    int* m = (int*)a;
    int* n = (int*)b;
    return *m-*n;
}

int main(int argc, const char** argv) {
    int data[100000];
    int N,p;
    scanf("%d %d", &N, &p);
    for(int i=0; i<N; i++){
        scanf("%d", &data[i]);
    }
    qsort(data, N, sizeof(int), cmp);
    int max_count = 0;
    /*
    for(int i=0; i<N; i++){
        for(int j=0; j<N; j++){
            if(data[j] <= 1L*p*data[i]){
                if(max_count < (j-i+1)){
                    max_count = (j-i+1);
                }
            }
        }
    }
    */
    for(int first=0,last=0; last<N && max_count< N-first; first++){
        while(last<N && data[last] <= 1L*data[first]*p){
            last++;
        }
        if(max_count< last-first){
            max_count = last-first;
        }
    }
    printf("%d", max_count);
    return 0;
}
```

# 1031 查验身份证
这道题...我一开始用的string,后来少写一个字母判断的逻辑,结果就崩了...
以后先在纸上写好需要什么判断逻辑和限制,再敲代码
## C++
```c++
#include <iostream>
using namespace std;
int a[17] = {7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2};
int b[11] = {1, 0, 10, 9, 8, 7, 6, 5, 4, 3, 2};
string s;
bool isTrue() {
    int sum = 0;
    for (int i = 0; i < 17; i++) {
        if (s[i] < '0' || s[i] > '9') return false;
        sum += (s[i] - '0') * a[i];
    }
    int temp = (s[17] == 'X') ? 10 : (s[17] - '0');
    return b[sum%11] == temp;
}
int main() {
    int n, flag = 0;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> s;
        if (!isTrue()) {
            cout << s << endl;
            flag = 1;
        }
    }
    if (flag == 0) cout << "All passed";
    return 0;
}
```

## C
```c++
#include<stdio.h> 
#include<string.h>
int main()
{   
    int N;
    scanf("%d",&N);
    int quan[18] = {7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2};
    char jiaoyan[11] = {'1','0','X','9','8','7','6','5','4','3','2'};
    bool flag = true;
    char id[20];
    for(int i = 0;i < N;i++) {
        scanf("%s",id);
        int j,sum = 0;
        for(j = 0;j<17;j++) {
            if(!(id[j] >= '0'&&id[j] <= '9')) break;
            sum += (id[j]-'0')*quan[j];
        }
        if(j<17) {
            flag = false;
            printf("%s\n",id);
        } else{
            if(jiaoyan[sum % 11] != id[17]) {
                flag = false;
                printf("%s\n",id);
            }
        }       
    } 
    if(flag == true) printf("All passed\n");
    return 0;
 } 
```

# 1032-挖掘机技术哪家强
一道经典快乐白给题
```c++
#include<stdio.h>
#include<map>

using namespace std;

int main(int argc, const char** argv) {
    int num;
    int  max = 0;
    int maxid;
    scanf("%d", &num);
    map<int,int> map1;
    int first,second;
    for(int i=0; i<num ;i++){
        scanf("%d %d",&first, &second);
        map1[first] += second;
        if(max < map1[first]){
            maxid = first;
            max = map1[first];
        }
    }
    printf("%d %d", maxid, max);
    return 0;
}
```

# 1033-旧键盘打字
这道题也是不算难,但这逻辑给我整的不会写了,改了好久才AC
```c++
#include<stdio.h>
#include<map>
#include<string>
#include<iostream>

using namespace std;

int main(int argc, const char** argv) {
    bool has = false;
    string s1;
    getline(cin,s1);
    string s2;
    getline(cin,s2);
    bool shitup = false;
    if(s1.find('+') != string::npos){
        shitup = true;
    }
    for(int j=0; j<s2.size(); j++){
        char a = s2[j];
        if(a>='A' && a<='Z' && shitup){
            continue;
        }
        if(s1.find(a) == string::npos){
            if(a>='a' && a<='z'){
                if(s1.find(a-32) != string::npos){
                    continue;
                }
            }
            printf("%c",a);
            has = true;
        }
    }
    if(has == false){
        printf("\n");
    }
    return 0;
}
```

# 1034-有理数四则运算
这道题我认输,刷到现在为止感觉最难受的一道题,真要是考试遇到这样的题,我心态就gg了
如果总结这道题的技巧,那就是提前在纸上写好完整逻辑
另外将题目中的逻辑,以及输出进行模块化,这道题以后常回家看看
## 最大公约数
一个很重要的计算模板
```c++
int gcd(int a, int b){
    if(b == 0){
        return a;
    }
    else{
        return gcd(b, a%b);
    }
}
```

## CODE
这个题打印的思路很棒，直接在函数内部按步骤打印，而不需要返回完整的字符串最后一起打印
```c++
#include <stdio.h>
#include <iostream>
#include <cmath>
using namespace std;

long long a, b, c, d;
long long gcd(long long t1, long long t2) {
    return t2 == 0 ? t1 : gcd(t2, t1 % t2);
}
void func(long long m, long long n) {
    if (m * n == 0) {
        printf("%s", n == 0 ? "Inf" : "0");
        return ;
    }
    bool flag = ((m < 0 && n > 0) || (m > 0 && n < 0));
    m = abs(m); n = abs(n);
    long long x = m / n;
    printf("%s", flag ? "(-" : "");
    if (x != 0) printf("%lld", x);
    if (m % n == 0) {
        if(flag) printf(")");
        return ;
    }
    if (x != 0) printf(" ");
    m = m - x * n;
    long long t = gcd(m, n);
    m = m / t; n = n / t;
    printf("%lld/%lld%s", m, n, flag ? ")" : "");
}
int main() {
    scanf("%lld/%lld %lld/%lld", &a, &b, &c, &d);
    func(a, b); printf(" + "); func(c, d); printf(" = "); func(a * d + b * c, b * d); printf("\n");
    func(a, b); printf(" - "); func(c, d); printf(" = "); func(a * d - b * c, b * d); printf("\n");
    func(a, b); printf(" * "); func(c, d); printf(" = "); func(a * c, b * d); printf("\n");
    func(a, b); printf(" / "); func(c, d); printf(" = "); func(a * d, b * c);
    return 0;
}
```

# 1035-插入与归并*
度过了上一题对自己的否定,看到这道题之后我再次进入了人生的思考之中...
原来还考排序算法的实现啊...然后现去复习了一会排序
这道题有一个很精妙的处理技巧,那就是对于插入排序,由于前半段有序,而题中要求输出再迭代一轮的数据,题中直接进行有序段长度加1,利用sort函数进行排序
不过对于归并排序,就只能一遍遍模拟了,利用了一个flag来控制循环,使得最后多运行一次
这是一道很棒的题!对排序的理解有很大帮助,要多复习这道题
>> 22号模拟考的这道题，我当场就傻掉了...太难了...

```c++
#include<cstdio>
#include<algorithm>
using namespace std;
 
int main(){
	int n;
	scanf("%d",&n);
	int a[n];
	int b[n];
	for(int i=0 ;i<n ;i++){
		scanf("%d",&a[i]);
	}
	for(int i=0 ;i<n ;i++){
		scanf("%d",&b[i]);
	}
	int i,j;
	for(i=0 ;i<n-1 && b[i+1]>=b[i] ;i++); //i记录了第一个相同元素的位置 
	for(j=i+1 ;j<n && a[j]==b[j];j++);	//j记录了最后一个相同元素的位置+1 
	
	if(j==n){	//如果最后一个相同的位置是n-1 那么说明是插入排序 
		printf("Insertion Sort\n");
		sort(a,a+i+2);	
	}else{
		printf("Merge Sort\n");
		int k = 1;	//下一次进行排序的子序列长度是2*k 
        int flag = 1;
       	//设置flag变量后，模拟归并过程。直到达到给出的状态，然后再进行一次归并，将结果输出即可。 
	    while(flag) {
            flag = 0;
            for (i = 0; i < n; i++) {
                if (a[i] != b[i])
                    flag = 1;
            }    
			k = k * 2;
            for (i = 0; i < n / k; i++){
            	sort(a + i * k, a + (i + 1) * k);
			}
			sort(a + n / k * k, a + n);
        }
		
	}
	for(int i=0 ;i<n-1 ;i++){
		printf("%d ",a[i]);
	}
	printf("%d\n",a[n-1]);
	return 0;
} 
```
