# PAT-Basic 函数总结



对PAT历年真题中常用的模板函数进行总结
<!--more-->

# 寻找字符串中的某一个字符
```c++
#include<cstring>
if (s2.find(s1[i]) == string::npos && ans.find(toupper(s1[i])) == string::npos){
    ans += toupper(s1[i]);
}
```

# 最大公约数
```c++
int gcd(int a, int b){
    if(b ==0){
        return a;
    }
    else{
        return gcd(b, a%b);
    }
}
```

# 单个字符大小写转换
```c++
#include<iostream>
string a;
a[i] = tolower(a[i]); 
```

# 字符串整体大小写转换
利用transform()函数实现
```c++
transform(str.begin(),str.end(),str.begin(),::tolower);
transform(str.begin(),str.end(),str.begin(),::toupper);
```

# 十进制转二进制
运用**短除法**
```c++
while(scanf("%d",&n)!=EOF)  //EOF: END OF FILE
    {
        i=0;
        while(n>0)
        {
            a[i++]=n%2;
            n=n/2;
        }
        for(j=i-1;j>=0;j--) //注意是反向输出
            printf("%d",a[j]);
        printf("\n");
    }
```

# 求素数
```c++
m=(int)sqrt((double)s); //首先求出平方根
for(i=2;i<=m;i++){
    if(s%i==0)
        //当前不为素数
    if(i>m){
        //当前为素数
    }
}
```

# set自带排序,默认从小到大
# sort默认升序排序

# 手动实现加法进位
```c++
string add(string data1, string data2){
    string s = data1;
    int carry = 0;
    for(int i=s.size()-1; i>=0; i--){
        s[i] = (data1[i]-'0'+data2[i]-'0'+carry)%10 + '0';
        carry = (data1[i]-'0'+data2[i]-'0'+ carry)/10;
    }
    if(carry > 0){
        s = "1" + s;
    }
    return s;
}
```

# 手动实现除法
s为被除数,a为除数
```c++
int len = s.length();
t = (s[0] - '0') / a;
if ((t != 0 && len > 1) || len == 1){
    cout << t;
}
temp = (s[0] - '0') % a;
for (int i = 1; i < len; i++) {
    t = (temp * 10 + s[i] - '0') / a;
    cout << t;
    temp = (temp * 10 + s[i] - '0') % a;
}
cout << " " << temp;
```

# C++格式化输出补0
```c++
//头文件：<iomanip>
//函数：setw(int n)
//函数：setfill(char c)
cout << setw(8) << setfill('0') << 123; //输出 00000123
```
