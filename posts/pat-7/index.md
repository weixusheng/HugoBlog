# PAT-Basic 41~43



B1041-B1043<br>
7.23更新

下午看了俄罗斯的<太空第一步>,之前也看过几个俄系科幻片,不得不说俄国特效做的一点都不比美国拍的那些差,但都有一个通病就是剧情节奏掌握的不是很好,但从故事角度来说,还是很震撼的,不得不佩服苏联的勇气,战斗民族是真的勇,看到最后有一种历经磨难艰险终究胜利的鼓舞,生活也是一样的道理,总会有各种各样难以预测的错误和失败,但只要一直去坚持,一直抱有希望和信心,就一定会造就新的奇迹
<!--more-->

# 1041-考试座位号
这道题提醒了我一个知识点:
对于字符串的复制,不可以直接用等号赋值,需要用到strcpy()
另外在结构体的构造中,尾部最好留有一个指针和一个变量,这样 malloc 还能清晰一点,并且使用方便

## 柳神代码
运用了string-hash 很好的思路,省了结构体
```c++
#include <iostream>
using namespace std;
int main() {
    string stu[1005][2], s1, s2;
    int n, m, t;
    cin >> n;
    for(int i = 0; i < n; i++) {
        cin >> s1 >> t >> s2;
        stu[t][0] = s1;
        stu[t][1] = s2; 
    } 
    cin >> m;
    for(int i = 0; i < m; i++) {
        cin >> t;
        cout << stu[t][0] << " " << stu[t][1] << endl;
    } 
    return 0;
}
```

## 菜鸡的代码
```c++
#include<stdio.h>
#include<stdlib.h>
#include<map>
#include<string.h>

using namespace std;

typedef struct{
    char ida[17];
    int sitid;
}*person,node;

int main(int argc, const char** argv) {
    int num;
    scanf("%d", &num);
    map<int,node>mp1;
    char tempid[17];
    int tryid,sitid;
    for(int i=0; i<num; i++){
        scanf("%s %d %d", &tempid, &tryid, &sitid);
        person person_temp = (person)malloc(sizeof(node));
        person_temp->sitid = sitid;
        strcpy(person_temp->ida, tempid);
        mp1[tryid] = *person_temp;
    }
    int query,queryid;
    scanf("%d", &query);
    for(int k=0; k<query; k++){
        scanf("%d", &queryid);
        printf("%s %d\n", mp1[queryid].ida, mp1[queryid].sitid);
    }
    return 0;
}
```

# 1042-字符统计
一道快乐的哈希白给题,不过这道题虽然AC了,但代码写的实在是冗余的不行
下面总结一下优化的点
1. 截止到目前为止,pat中有许多题会涉及到字母的判断和大小写,而c在<ctype.h>中提供了对于字母处理的函数
2. 对于相同值的情况取更小,可以从后向前进行处理

## 菜鸡的冗余代码
```c++
#include<stdio.h>
#include<stdlib.h>
#include<string>
#include<iostream>

using namespace std;

int main(int argc, const char** argv) {
    string s;
    int max = 0;
    int maxid;
    int tempid;
    getline(cin, s);
    int hashmap[500] = {0};
    for(int i=0; i<s.size(); i++){
        if(s[i]>= 'A' && s[i]<= 'Z'){    //大写字母
            tempid = s[i]+32-'0';
            hashmap[tempid]++;
            if(max < hashmap[tempid]){
                max = hashmap[tempid];
                maxid = tempid;
            }
            if(max == hashmap[tempid]){
                maxid = maxid < tempid ? maxid:tempid;
            }
        }
        else if(s[i]>='a' && s[i]<='z'){
            tempid = s[i]-'0';
            hashmap[tempid]++;
            if(max < hashmap[tempid]){
                max = hashmap[tempid];
                maxid = tempid;
            }
            if(max == hashmap[tempid]){
                maxid = maxid < tempid ? maxid:tempid;
            }
        }
    }
    printf("%c %d",(char)(maxid+'0'), max);
    return 0;
}
```

## 大佬教我写代码
```c++
#include <stdio.h>
#include <ctype.h>

int main()
{
    char c;
    int count[26] = {0}, max = 25;
    while((c = getchar()) != '\n')
        if(isalpha(c))  //判断是否为字母
            count[tolower(c) - 'a']++;
    for(int i = 25; i >= 0; i--)
        if(count[i] >= count[max])
            max = i;
    printf("%c %d", max + 'a', count[max]);
    return 0;
}
```

# 1043-输出PATest
这道题也是虽然AC了,但是写的非常麻烦
看了大佬的题解之后...感觉大佬写的太精妙了!
大佬确定了需要固定输出的字符串,在输出中,按照这个字符串模板进行输出,一个字符串内部的for循环就可以搞定
## 菜鸡的冗余代码
```c++
#include<stdio.h>
#include<stdlib.h>
#include<map>
#include<iostream>

using namespace std;

int main(int argc, const char** argv) {
    map<char,int>mp;
    mp['P'] = 0,mp['A'] = 0,mp['T'] = 0;
    mp['e'] = 0,mp['s'] = 0,mp['t'] = 0;
    string s;
    getline(cin, s);
    int count = 0;
    for(int i=0; i<s.size(); i++){
        if(s[i] == 'P'){ //PATest
            mp['P']++;
            count++;
            continue;
        }
        if(s[i] == 'A'){ //PATest
            mp['A']++;
            count++;
            continue;
        }
        if(s[i] == 'T'){ //PATest
            mp['T']++;
            count++;
            continue;
        }
        if(s[i] == 'e'){ //PATest
            mp['e']++;
            count++;
            continue;
        }
        if(s[i] == 's'){ //PATest
            mp['s']++;
            count++;
            continue;
        }
        if(s[i] == 't'){ //PATest
            mp['t']++;
            count++;
            continue;
        }
    }
    int j = 0;
    while(j < count){
        if(mp['P']){
            printf("P");
            mp['P']--;
            j++;
        }
        if(mp['A']){
            printf("A");
            mp['A']--;
            j++;
        }
        if(mp['T']){
            printf("T");
            mp['T']--;
            j++;
        }
        if(mp['e']){
            printf("e");
            mp['e']--;
            j++;
        }
        if(mp['s']){
            printf("s");
            mp['s']--;
            j++;
        }
        if(mp['t']){
            printf("t");
            mp['t']--;
            j++;
        }
    }
    return 0;
}
```

## 大佬教我写代码
```c++
#include <stdio.h>

int main()
{
    char c, *str = "PATest";                /* use as index for count[] */
    int i, flag = 1, count[128] = {0};      /* for each ASCII char */
    /* Read and count numbers for every character */
    while((c = getchar()) != '\n')
        count[(int)c]++;
    /* Print any character in "PATest" if it is still left */
    while(flag)
        for(i = 0, flag = 0; i < 6; i++)
            if(count[(int)str[i]]-- > 0)    /* Check the number left */
                putchar(str[i]), flag = 1;
    return 0;
}
```

## 柳神大佬
用了快乐map!
```c++
#include <iostream>
using namespace std;
int main() {
  int map[128] = {0}, c;
  while ((c = cin.get()) != EOF) map[c]++;
  while (map['P'] > 0 || map['A'] > 0 || map['T'] > 0 || map['e'] > 0 || map['s'] > 0 || map['t'] > 0) {
    if (map['P']-- > 0) cout << 'P';
    if (map['A']-- > 0) cout << 'A';
    if (map['T']-- > 0) cout << 'T';
    if (map['e']-- > 0) cout << 'e';
    if (map['s']-- > 0) cout << 's';
    if (map['t']-- > 0) cout << 't';
  }
  return 0;
}
```

# 头文件 ctype.h
## 判断函数
从上面这两道题,我发现这个头文件很有用处,记录一下
另外对于字符串转变为hash的处理过程,可以用getchar()函数进行处理,而不需要额外开一个string空间
 - int isalnum(int c)
该函数检查所传的字符**是否是字母和数字**。
 - int isalpha(int c)
该函数检查所传的字符**是否是字母**。
 - int iscntrl(int c)
该函数检查所传的字符是否是控制字符。
 - int isdigit(int c)
该函数检查所传的字符**是否是十进制数字**。
 - int isgraph(int c)
该函数检查所传的字符是否有图形表示法。
 - int islower(int c)
该函数检查所传的字符**是否是小写字母**。
 - int isprint(int c)
该函数检查所传的字符是否是可打印的。
 - int ispunct(int c)
该函数检查所传的字符是否是标点符号字符。
 - int isspace(int c)
该函数检查所传的字符是否是空白字符。
 - int isupper(int c)
该函数检查所传的字符**是否是大写字母**。
 - int isxdigit(int c)
该函数检查所传的字符是否是十六进制数字。

## 转换函数
 - int tolower(int c)
该函数把**大写字母转换为小写字母**。
 - int toupper(int c)
该函数把**小写字母转换为大写字母**。
