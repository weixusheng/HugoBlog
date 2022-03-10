# PAT-Advanced 1001



甲级的第一道题!<br>
终于不用为了线上考试继续在Win10环境写代码了,亲一口俺最爱的Ubuntu
<!--more-->

# A+B Format
这道题我一开始担心越界,所以写了一个取每一位的函数,但后来看了柳神的题解...发现确实是自己想多了
```c++
#include<cstdlib>
#include<iostream>
#include<cmath>
#include<stack>

using namespace std;

int main(int argc, char const *argv[])
{
    long long a, b, c;
    cin >> a >> b;
    c = a+b;
    if(c < 0){
        cout << "-";
    }
    if(c == 0){
        cout << "0";
        return 0;
    }
    int carry;
    int cnt = 0;
    c = abs(c);
    stack<char> data;
    while(c != 0){
        carry = c%10;
        data.push(carry+'0');
        cnt++;
        if(cnt%3 == 0){
            data.push(',');
        }
        c = c/10;
    }
    if(data.top() == ','){
        data.pop();
    }
    int sizes = data.size();
    for(int i=0; i<sizes; i++){
        cout << data.top();
        data.pop();
    }
    return 0;
}
```

## 柳神代码
就很机制!直接 to_string() 然后用一个巧妙的取余对应值来判断逗号的位置
```c++
#include <iostream>
using namespace std;
int main() {
    int a, b;
    cin >> a >> b;
    string s = to_string(a + b);
    int len = s.length();
    for (int i = 0; i < len; i++) {
        cout << s[i];
        if (s[i] == '-') continue;
        if ((i + 1) % 3 == len % 3 && i != len - 1) cout << ",";
    }
    return 0;
}
```
