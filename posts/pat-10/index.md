# PAT-Basic 58



B1058<br>
7.23更新
<!--more-->

今天只有一道题,上午数学,下午调试路由器,跟学校保安秀了一波操作,然后把礼物送给老师
我才知道原来宽带经销商还有连接多个运营商的
因为github间歇性无法访问,让我很是难受,最后联系技术人员上门操作
才知道其中间歇性的缘由,因为可能移动的缆线可以访问,而一段时间后切换到电信的缆线导致无法访问,dns解析失败,最后gg
其实我一开始预料到了dns的问题,电脑也从添加hosts行,再到后来直接换掉hosts源,但都是无济于事,因为我没想到原来路由器里还有一个dns
最后技术人员告诉我改变一下路由器的dns解析,我改成谷歌的8888,问题就解决啦!
网络真是神奇嗷!(计算机网络没学好...)

晚上想着刷题,结果上来第一题我就傻了...写了好久也没写明白,输入判断太复杂了...

# 1058-选择题*
这道题我错就错在不该用结构体,并且没有想到用set进行比较,而且输出格式还搞错了,这道题很经典
既然今天就做这一道题,那就好好分析一下
首先题目的输入
```
3 4 
3 4 2 a c
2 5 1 b
5 3 2 b c
1 5 4 a b d e
(2 a c) (2 b d) (2 a c) (3 a b e)
(2 a c) (1 b) (2 a b) (4 a b d e)
(2 b d) (1 e) (2 b c) (4 a b c d)
```
难点在于后面的字符输入,首先对于正确选项,需要
```c++
scanf(" %c", &c); //前面有一个空格
```
下一步在读入字符串时,需要首先读入一个"\n"换行符
原因在于,如果是scanf("%s"),那么此时会将回车当作结束,但如果当前是scanf("%c"),那么就会将回车也当作字符进行输入,因此,如果想忽略掉这个回车,就需要特性的scanf("\n")
在输入格式化字符串时,可以对格式的前后进行scanf限定
例如题解中的
```c++
scanf("(%d", &k);
for(int l = 0; l < k; l++) {
    scanf(" %c", &c);
    }
scanf(")");
```
就将括号中的字符进行巧妙的读取
在判定两个集合的元素是否相同时,可以直接用set和"=="进行判断
```c++
set1 == set2
```
最后有一个巧妙的点就是: 没有必要在数组复杂的数据面前使用结构体,虽然结构清晰,但是代码冗余巨大,且逻辑会变得复杂,完全可以用多个一维数组来构造数据,并用下标进行区分

## 题解
```c++
#include <cstdio>
#include <vector>
#include <set>
using namespace std;

int main() {
    int n, m, temp, k;
    scanf("%d%d", &n, &m);
    vector<set<char>> right(m);
    vector<int> total(m), wrongCnt(m);
    for(int i = 0; i < m; i++) {
        scanf("%d%d%d", &total[i], &temp, &k);
        for(int j = 0; j < k; j++) {
            char c;
            scanf(" %c", &c);
            right[i].insert(c);
        }
    }
    for(int i = 0; i < n; i++) {
        int score = 0;
        scanf("\n");
        for(int j = 0; j < m; j++) {
            if(j != 0) scanf(" ");
            scanf("(%d", &k);
            set<char> st;
            char c;
            for(int l = 0; l < k; l++) {
                scanf(" %c", &c);
                st.insert(c);
            }
            scanf(")");
            if(st == right[j]) {
                score += total[j];
            } else {
                wrongCnt[j]++;
            }
        }
        printf("%d\n", score);
    }
    int maxWrongCnt = 0;
    for(int i = 0; i < m; i++) {
        if(wrongCnt[i] > maxWrongCnt) {
            maxWrongCnt = wrongCnt[i];
        }
    }
    if(maxWrongCnt == 0)
        printf("Too simple");
    else {
        printf("%d", maxWrongCnt);
        for(int i = 0; i < m; i++) {
            if(wrongCnt[i] == maxWrongCnt) {
                printf(" %d", i + 1);
            }
        }
    }
    return 0;
}
```

