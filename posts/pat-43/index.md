# PAT-A1068(背包问题)



终于搞明白这道题啦!
<!--more-->

这道题题目很简单
给定一系列的硬币值, 然后给定一个目标value， 从所有硬币中找出几个, 使得这**几个硬币的和正好等于这个value**, 而且这个硬币序列应该是满足硬币值字典序的**最小序列**.

一开始我一直认为不能用0-1背包,也一直不懂大佬们的题解
今天感觉自己看了一些动态规划的概念,应该会对这道题有一些新的认识...然鹅又研究了一会题解还是不会
只好再去百度查查大佬的博客,虽然那些博客之前都看过,但还是试一试吧...突然看到大佬的一句话,之后想了一下,豁然开朗!
这道题能用01背包方法是因为,可以将金额数值设置为边界上限(即背包问题中的容量)
而在该金额上限下,去计算最大值(背包问题中的金额),如果这个最大值等于上限边界,那么就说明当前的选择是符合条件的
**简单的来说,就是将边界条件和求最优解(最大值)都设置为"金额"这个属性,那么就可以求出是否金额能达到最大值(边界)**
没想到鸭没想到,算法太灵性辣
**递归公式:**
```
F(N, M) = max{ F(N–1, M), F(N–1, M–c(N)) + c(N) }，c(N)表示第N个硬币的面值
```
那么最后要是F(N, M)  == M， 那么就说明我们可以找到这样一组硬币, 使得他们的面值总和恰好等于M。 
记录路径：若has(N, M)为真, 则表示从前面N个硬币中选出一组得到最多的不超过M的币值总和里面包括了第N个硬币。那么如果我们需要找到完整路径, 我们就可以从N、M出发，一直回溯到最后一个硬币。

另外这道题还要求输出的最小序列,这就需要在一开始时对序列进行排序(从大到小),因为选取顺序是从头到尾的,而且**判定式是小于等于因此遇到符合条件的更小的数值时,会更新dp数组**,这样就可以选取到最小序列

对于路线的记录,这里构建了一个**choice二维数组,当符合条件时,利用该数组进行回溯**(妙啊!)

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int dp[10010], w[10010];
bool choice[10010][110];
int cmp1(int a, int b){
    return a>b;
}
int main() {
    int n, m;
    scanf("%d%d", &n, &m);
    for(int i=1; i<=n; i++)
        scanf("%d", &w[i]);
    sort(w+1, w+n+1, cmp1);
    for(int i=1; i<=n; i++) {   //背包递推
        for(int j=m; j>=w[i]; j--) {
            if(dp[j] <=dp[j-w[i]] +w[i]) {
                choice[i][j] = true;
                dp[j] = dp[j-w[i]] +w[i];            
            }        
        }    
    }
    if(dp[m] !=m) 
        printf("No Solution");
    else {
        vector<int> arr;
        int v=m, index=n;
        while(v>0) {    //回溯路线
            if(choice[index][v] ==true) {
                arr.push_back(w[index]);
                v -= w[index];            
            }
            index--;        
        }
        for(int i=0; i<arr.size(); i++) {
            if(i!=0) 
                printf(" ");
            printf("%d", arr[i]);       
        }    
    }
    return 0;
}
```
