# PAT-Advanced 1002



这几天一直在思考怎么准备甲级,看了很多经验贴之后,决定首先把"算法笔记"这本书看完吧,因为刚刷到第三道题就是dijkstra(菜鸡惊讶),想了想还是先把这些基本算法实现学明白再说吧...这几天把树看完了,图还剩最短路径和最小生成树<br>
所以今天开始恢复每日更新!<br>
在更新这些算法实现前,先把之前欠下的第二道题补上(嘿嘿,又能水一帖了)
<!--more-->

# 1002 A+B for Polynomials
常规题,数组+hash
```c++
#include<iostream>
#include<cstdlib>

using namespace std;

int main(int argc, char const *argv[])
{
    int cnt = 0;
    double data[1001] = {0};
    int num1, num2;
    int tmp;
    double tmp2;
    cin >> num1;
    for(int i=0; i<num1; i++){
        cin >> tmp >> tmp2;
        if(data[tmp] == 0){
            cnt++;
        }
        data[tmp] += tmp2;
    }
    cin >> num2;
    for(int i=0; i<num2; i++){
        cin >> tmp >> tmp2;
        if(data[tmp] == 0){
            cnt++;
        }
        data[tmp] += tmp2;
    }
    cout << cnt;
    for(int j=1000; j>=0; j--){
        if(data[j]!=0){
            cout << " " << j << " " << data[j];
        }
    }
    system("pause");
    return 0;
}
```
