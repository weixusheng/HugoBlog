# PAT-Basic 86~90



B1086-B1090<br>
第四道题(20分)没想出来...题没太看懂,并且逻辑有些复杂...呜呜呜<br>
别的题都是常规题,都AC了,满分好难...
<!--more-->

# 1086-就不告诉你
这道题有两个知识点,一个是int转string,需要用到 to_string() 函数
第二个就是 string 转 int 的 stoi()函数
```c++
#include <iostream>
#include <stdlib.h>
#include <string>
#include<algorithm>
using namespace std;

int main()
{
    int s1,s2;
    cin >> s1 >> s2;
    int sum = s1 * s2;
    string res = to_string(sum);
    reverse(res.begin(),res.end());
    printf("%d", stoi(res));
    return 0;
}
```

# 1087-有多少不同的值
快乐白给题
```c++
#include<set>
#include<iostream>
#include<cstdlib>
using namespace std;

int main(int argc, char const *argv[])
{
    int num;
    int cnt = 0;
    //double shit;
    cin >> num;
    set<int> res;
    int s;
    for(int i=1; i<=num; i++){
        s = i/2+i/3+i/5;
        if(res.find(s) == res.end()){
            res.insert(s);
            cnt++;
        }
    }
    printf("%d",cnt);
    system("pause");
    return 0;
}
```

# 1088-三人行
这道题需要注意的一个点就是,对于 double = x/y ,如果 x,y 都为 int,那么结果还是int取整数部分
如果想计算除法得到小数,那么需要 x,y 也为 double
对于类似的数字(比较大小)题,要特别注意判断 int(整数) 还是 double(小数) 的情况
```c++
#include<cmath>
#include<iostream>
#include<cstdlib>
using namespace std;

int rev(int a){
    int tmp1;
    int tmp2;
    tmp1 = a%10;
    tmp2 = a/10;
    return (tmp1*10 + tmp2);
}

int main(int argc, char const *argv[])
{
    int m;
    double x, y;
    cin >> m >> x >> y;
    double charg = x/y;
    bool isfind = false;
    double shit[3];
    /* find alpha */
    for(int i=10; i<=99; i++){
        if(fabs(i-rev(i)) == charg*rev(i)){
            shit[0] = i;
            isfind = true;
        }
    }
    shit[1] = rev(shit[0]);
    shit[2] = (shit[1])/y;
    if(isfind){
        printf("%d",int(shit[0]));
        for(int k=0; k<3; k++){
            if(shit[k] > m){
                printf(" Cong");
            }
            if(shit[k] == m){
                printf(" Ping");
            }
            if(shit[k] < m){
                printf(" Gai");
            }
        }
    }
    else{
        printf("No Solution");
    }
    //system("pause");
    return 0;
}
```

# 1089-狼人杀-简单版
这道题心态直接崩了,题读不懂呀,后来读懂了也没有思路
看了题解才有点思路,枚举法好强!没想到做了这么多题,还是会遇到没有思路的题
对于此类数据判定,并且限制条件多的题,需要将程序中的条件提前列好,并且分析终止条件
例如本题中的,两个谎言者,一个狼人谎言,一个人类谎言,以及狼人和普通人的正负区分

## 大佬的方法
大佬直接用正负判定是否符合,太强了!好的思路!
```c++
#include <iostream>
#include <vector>
#include <cmath>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> v(n+1);
    for (int i = 1; i <= n; i++) cin >> v[i];
    for (int i = 1; i <= n; i++) {
        for (int j = i + 1; j <= n; j++) {
            vector<int> lie, a(n + 1, 1);
            a[i] = a[j] = -1;
            for (int k = 1; k <= n; k++)
                if (v[k] * a[abs(v[k])] < 0) lie.push_back(k);
            if (lie.size() == 2 && a[lie[0]] + a[lie[1]] == 0) {
                cout << i << " " << j;
                return 0;
            }
        }
    }
    cout << "No Solution";
    return 0;
}
```

## 菜鸡的方法
相比大佬简洁的判断,我的判断就写的麻烦了很多,主要是逻辑不清晰,只能将不同的情况分开讨论
```c++
#include<cstdlib>
#include<vector>
#include<iostream>
#include<cmath>
using namespace std;

int main(int argc, char const *argv[])
{
    int num, tmp;
    cin >> num;
    vector<int> data(num);
    for(int i=0; i<num; i++){
        cin >> tmp;
        data[i] = tmp;// insert data
    }
    bool shit_l = false, shit_r = false;
    int cnt;
    for(int i=0; i<num; i++){
        for(int j=i+1; j<num; j++){
            shit_r = false;
            shit_l = false;
            cnt = 0;
            for(int k=0; k<num; k++){
                int s = data[k];
                if((abs(s) == i+1 || abs(s) == j+1) && s>0){
                    if(k == i || k == j){
                        shit_l = true;
                    }
                    else{
                        shit_r = true;
                    }
                    cnt++;
                }
                if(s < 0){
                    if(abs(s) != i+1 && abs(s) != j+1){
                        if(k == i || k == j){
                            shit_l = true;
                        }
                        else{
                            shit_r = true;
                        }
                        cnt++;
                    }
                }
            }
            // shit2 is shit
            if(shit_l && shit_r && cnt == 2){
                printf("%d %d",i+1,j+1);
                system("pause");
                return 0;
            }
        }
    }
    printf("No Solution");
    system("pause");
    return 0;
}
```

# 1090-危险品装箱
map-set 的组合使用,快乐AC
```c++
#include<map>
#include<set>
#include<iostream>
#include<cstdlib>
using namespace std;

int main(int argc, char const *argv[])
{
    int m, n, s1, s2;
    cin >> m >> n;
    map<int,set<int> > data;
    for(int i=0; i<m; i++){
        cin >> s1 >> s2;
        data[s1].insert(s2);
        data[s2].insert(s1);
    }
    int cnt;
    for(int k=0; k<n; k++){
        cin >> cnt;
        int tmp;
        bool shit = false;
        set<int> res;
        for(int j=0; j<cnt; j++){
            cin >> tmp;
            res.insert(tmp);
        }
        for(auto it = res.begin(); it!=res.end(); it++){
            set<int> tmpshit = data[*it];
            for(auto it2 = tmpshit.begin(); it2!=tmpshit.end(); it2++){
                    if(res.find(*it2) != res.end()){
                        shit = true;
                        break;
                }
            }
        }
        if(shit){
            cout << "No" << endl;
        }
        else{
            cout << "Yes" << endl;
        }
    }
    //system("pause");
    return 0;
}
```

