# PAT-Basic 65~68



B1065-B1068<br>
7.23更新
<!--more-->

# 1065-单身狗
这道题双向map加上set就可以解决
```c++
#include<cstdio>
#include<map>
#include<set>
using namespace std;

int main(int argc, const char** argv) {
    int num;
    scanf("%d", &num);
    int s1,s2;
    map<int, int> data;
    for(int i=0; i<num; i++){
        scanf("%d %d", &s1, &s2);
        data[s1] = s2;
        data[s2] = s1;
    }
    int num2, count = 0;
    int temp;
    scanf("%d", &num2);
    set<int> data2;
    set<int> res;
    for(int k=0; k<num2; k++){
       scanf("%d", &temp);
       data2.insert(temp);
    }
    for(auto it = data2.begin(); it!=data2.end(); it++){
        if(data2.find(data[*it]) == data2.end()){
            count++;
            res.insert(*it);
        }
    }
    printf("%d\n", count);
    int flag = 0;
    for(auto it2 = res.begin(); it2!=res.end(); it2++){
        if(flag == 0){
            flag = 1;
        }
        else{
            printf(" ");
        }
        printf("%05d", *it2);
    }
    return 0;
}
```

# 1066-图像过滤
快乐白给打印题
```c++
#include<cstdio>
using namespace std;

int main(int argc, const char** argv) {
    int row, num, shit1, shit2, yeah;
    scanf("%d %d %d %d %d", &row, &num, &shit1, &shit2, &yeah);
    int temp;
    for(int i=0; i<row; i++){
        for(int j=0; j<num; j++){
            scanf("%d", &temp);
            if(j != 0){
                printf(" ");
            }
            if(temp<= shit2 && temp>= shit1){
                printf("%03d", yeah);
            }
            else{
                printf("%03d", temp);
            }
        }
        if(i != row-1){
            printf("\n");
        }
    }
    return 0;
}
```

# 1067-试密码
这道题有两个点,一个是由于输入的字符串中有空格,所以需要使用getline()
另外,在输入第一行的数字之后,如果要进行getline(),需要取到第一行最后的'\n'换行符
```c++
#include<string>
#include<cstdio>
#include<iostream>
using namespace std;

int main(int argc, const char** argv) {
    string password;
    int num;
    cin >> password >> num;
    string input;
    getchar();
    getline(cin, input);
    while(input != "#"){
            if(input == password){
                cout << "Welcome in";
                break;
            }
            else{
                cout << "Wrong password: "<< input << endl;
                if(--num == 0){
                    cout << "Account locked";
                    break;
                }
            }
            getline(cin, input);
    }
    return 0;
}
```

# 1068-万绿丛中一点红*
这道题很不错,输入矩阵数据类型的经典例题
一开始我也没写出来,巧妙在于他将矩阵的不同位置信息,直接写在了一个二维数组中,这样每个点都可以对应使用
另外一点是,对于二维vector的建立,相比与之前的一行定义,我还是更推荐这种写在两行的,结构清晰
第一行代码是定义行,第二行代码是定义列,利用resize()
```c++
vector<vector<int>> v;
v.resize(n, vector<int>(m));
```
```c++
#include <iostream>
#include <vector>
#include <map>
using namespace std;
int m, n, tol;
vector<vector<int>> v;
int dir[8][2] = {{-1, -1}, {-1, 0}, {-1, 1}, {0, 1}, {1, 1}, {1, 0}, {1, -1}, {0, -1}};
bool judge(int i, int j) {
    for (int k = 0; k < 8; k++) {
        int tx = i + dir[k][0]; //确定位置
        int ty = j + dir[k][1];
        if (tx >= 0 && tx < n && ty >= 0 && ty < m && v[i][j] - v[tx][ty] >= 0 - tol && v[i][j] - v[tx][ty] <= tol) return false;
    }
    return true;
}
int main() {
    int cnt = 0, x = 0, y = 0;
    scanf("%d%d%d", &m, &n, &tol);
    v.resize(n, vector<int>(m));
    map<int, int> mapp;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            scanf("%d", &v[i][j]);
            mapp[v[i][j]]++;
        }
    }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (mapp[v[i][j]] == 1 && judge(i, j) == true) {
                cnt++;
                x = i + 1;
                y = j + 1;
            }
        }
    }
    if (cnt == 1)
        printf("(%d, %d): %d", y, x, v[x-1][y-1]);
    else if (cnt == 0)
        printf("Not Exist");
    else
        printf("Not Unique");
    return 0;
}
```
