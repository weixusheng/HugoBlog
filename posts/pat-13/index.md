# PAT-Basic 69~73



B1069-B1073<br>
7.23更新
<!--more-->

# 1069-微博转发抽奖
没啥难的,考察基础逻辑输出
```c++
#include<cstdio>
#include<set>
#include<string>
#include<iostream>
using namespace std;

int main(int argc, const char** argv) {
    int M, N, start;
    scanf("%d %d %d", &N, &M ,&start);
    set<string> havegot;
    string temp;
    int num = 0;
    int move = 0;
    for(int i=0; i<N; i++){
        cin >> temp;
        if(i == start-1+num*M+move){
            if(havegot.find(temp) == havegot.end()){
                num++;
                havegot.insert(temp);
                cout << temp << endl;
            }
            else{
                move++;
            }
        }
    }
    if(havegot.size() == 0){
        printf("Keep going...");
    }
    return 0;
}
```

# 1070-结绳
这道题需要先赋值,因为可能只有一根绳子
不知道为什么,额外写判断还是不行,只好先把vector第一个坑先占上了
```c++
#include<cstdio>
#include<vector>
#include<algorithm>
#include<iterator>
using namespace std;

int main(int argc, const char** argv) {
    int num;
    scanf("%d", &num);
    double temp;
    double sum = 0;
    vector<double> data(num);
    for(int i=0; i<num; i++){
        scanf("%lf", &temp);
        data[i] = temp;
    }
    sort(data.begin(), data.end(), less<double>());
    /*
    if(data.size() == 1){
        printf("%d", (int)data[0]);
        return 0;
    }
    */
    sum = data[0];
    for(int k=1; k<data.size(); k++){
        sum += data[k];
        sum = sum/2;
    }
    int res = (int)sum;
    printf("%d", res);
    return 0;
}
```

# 1071-小赌怡情
没啥难的...一次AC
```c++
#include<cstdio>
using namespace std;

int main(int argc, const char** argv) {
    int shit, num;
    scanf("%d %d",&shit, &num);
    int data1, data2, guss, gushit;
    for(int i=0; i<num; i++){
        scanf("%d %d %d %d", &data1, &guss, &gushit, &data2);
        if(gushit > shit){
            printf("Not enough tokens.  Total = %d.\n",shit);
        }
        else{
            if((data1 < data2 && guss == 1) || (data1 > data2 && guss == 0)){
                shit += gushit;
                printf("Win %d!  Total = %d.\n", gushit,shit);
            }
            else{
                shit -= gushit;
                printf("Lose %d.  Total = %d.\n", gushit,shit);
                if(shit == 0){
                    printf("Game Over.");
                    break;
                }
            }
        }
    }
    return 0;
}
```

# 1072-开学寄语
emmmm,这个也不难,一次AC
```c++
#include<cstdio>
#include<set>
#include<vector>
using namespace std;

int main(int argc, const char** argv) {
    int pnum, shitnum;
    set<int> shit;
    scanf("%d %d", &pnum, &shitnum);
    int tempshit;
    for(int i=0; i<shitnum; i++){
        scanf("%d", &tempshit);
        shit.insert(tempshit);
    }
    char name[10];
    int tempnum,tempshit2;
    int count = 0, tempcount, pcount = 0;
    vector<int> data(6);
    for(int k=0; k<pnum; k++){
        tempcount = 0;
        scanf("%s %d", &name, &tempnum);
        for(int h=0; h<tempnum; h++){
            scanf("%d", &tempshit2);
            if(shit.find(tempshit2) != shit.end()){
                count++;
                data[tempcount] = tempshit2;
                tempcount++;
            }
        }
        if(tempcount != 0){
            printf("%s: ",name);
            for(int w=0; w<tempcount; w++){
                if(w != 0){
                    printf(" ");
                }
                printf("%04d",data[w]);
            }
            printf("\n");
            pcount++;
        }
    }
    printf("%d %d",pcount, count);
    return 0;
}
```

# 1073-多选题常见计分法*
这道题就很梦幻了,好久没遇到这么快乐(自闭)的题了
其实本来我代码都快写完了,但我看了一眼我写的三层for循环,又想到没写到的选项判断,我就果断放弃了
这道题的精妙之处就是解决选项判断问题
我一开始想用单独的vector和find进行选项判断,但是感觉好复杂
题解则使用了位运算,这是我根本没想到的(长见识了!)
并且位运算还很是神奇,用到了"{1, 2, 4, 8, 16}"这个数组作为运算base,这就是二进制转十进制的操作
之后将两个十进制的二进制数进行位运算判断
```
对于每到选择题答案a,b,c,d,e依次可用00001，00010，00100，01000，10000表示
例如一道选择题选了a,c则可表示为00101；
令正确答案与考生答案异或：
    1.异或为0则该题做对 例：00101 ^ 00101 = 0
    2.异或非0则该题做错 例：00101 ^ 00010 = 7
      漏选 例：00101 ^ 00001 = 4
此时可以通过与操作对做错和漏选进行区分，漏选和答案或为答案
    1.做错 例：00101 | 00010 = 00111
    2.漏选 例：00101 | 00001 = 00101 与答案相等
最后通过与操作定位错误点
00101 ^ 00010 = 00111
00111 & 00001 = 00001 选错
00111 & 00010 = 00010 选错（对的未选）
00111 & 00100 = 00100 选错
00111 & 01000 = 00000
00111 & 10000 = 00000
```
```c++
#include <vector>
#include <cmath>
#include <cstdio>
using namespace std;

int main() {
    int n, m, optnum, truenum, temp, maxcnt = 0;
    int hash[] = {1, 2, 4, 8, 16}, opt[1010][110] = {0};
    char c;
    scanf("%d %d", &n, &m);
    vector<int> fullscore(m), trueopt(m);
    vector<vector<int> > cnt(m, vector<int>(5));
    for (int i = 0; i < m; i++) {
        scanf("%d %d %d", &fullscore[i], &optnum, &truenum);
        for (int j = 0; j < truenum; j++) {
            scanf(" %c", &c);
            trueopt[i] += hash[c-'a'];
        }
    }
    for (int i = 0; i < n; i++) {
        double grade = 0;
        for (int j = 0; j < m; j++) {
            getchar();
            scanf("(%d", &temp);
            for (int k = 0; k < temp; k++) {
                scanf(" %c)", &c);
                opt[i][j] += hash[c-'a'];
            }
            int el = opt[i][j] ^ trueopt[j];
            if (el) {
                if ((opt[i][j] | trueopt[j]) == trueopt[j]) {
                    grade += fullscore[j] * 1.0 / 2;
                }
                if (el) {
                    for (int k = 0; k < 5; k++)
                        if (el & hash[k]) cnt[j][k]++;
                }
            } else {
                grade += fullscore[j];
            }
        }
        printf("%.1f\n", grade);
    }
    for (int i = 0; i < m; i++)
        for (int j = 0; j < 5; j++)
            maxcnt = maxcnt > cnt[i][j] ? maxcnt : cnt[i][j];
    if (maxcnt == 0) {
        printf("Too simple\n");
    } else {
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < cnt[i].size(); j++) {
                if (maxcnt == cnt[i][j])
                    printf("%d %d-%c\n", maxcnt, i+1, 'a'+j);
            }
        }
    }
    return 0;
}
```
