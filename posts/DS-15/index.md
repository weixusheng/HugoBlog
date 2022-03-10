# 动态规划-基本概念



基本概念 | 数塔问题
<!--more-->

# 基本概念
已知问题规模为n的前提A，求解一个未知解B。（我们用A<sub>n</sub>表示“问题规模为n的已知条件”）
此时，如果把问题规模降到0，即已知A<sub>0</sub>，可以得到A<sub>0</sub>->B.

如果从A<sub>0</sub>添加一个元素，得到A<sub>1</sub>的变化过程。即A<sub>0</sub>->A<sub>1</sub>; 进而有A<sub>1</sub>->A<sub>2</sub>; A<sub>2</sub>->A<sub>3</sub>; …… ; A<sub>i</sub>->A<sub>i</sub>+1. 这就是严格的归纳推理，也就是我们经常使用的**数学归纳法**；
**对于A<sub>i</sub>+1，只需要它的上一个状态A<sub>i</sub>即可完成整个推理过程（而不需要更前序的状态）。我们将这一模型称为马尔科夫模型。对应的推理过程叫做“贪心法”**

然而，A<sub>i</sub>与A<sub>i+1</sub>往往不是互为充要条件，随着i的增加，有价值的前提信息越来越少，我们无法仅仅通过上一个状态得到下一个状态，因此可以采用如下方案：

{A<sub>1</sub>->A<sub>2</sub>}; {A<sub>1</sub>, A<sub>2</sub>->A<sub>3</sub>}; {A<sub>1</sub>,A<sub>2</sub>,A<sub>3</sub>->A<sub>4</sub>};……; {A<sub>1</sub>,A<sub>2</sub>,...,A<sub>i</sub>}->A<sub>i</sub>+1. 这种方式就是第二数学归纳法。
**对于Ai+1需要前面的所有前序状态才能完成推理过程。我们将这一模型称为高阶马尔科夫模型。对应的推理过程叫做“动态规划法”**

上述两种状态转移图如下图所示：
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DS-15/1.png)

## 能用动规解决的问题的特点 
能采用动态规划求解的问题的一般要具有3个性质：
1. 最优化原理：如果问题的最优解所包含的子问题的解也是最优的，就称该问题具有最优子结构，即满足最优化原理
2. 无后效性：即某阶段状态一旦确定，就不受这个状态以后决策的影响。也就是说，某状态以后的过程不会影响以前的状态，只与当前状态有关
3. 有重叠子问题：即子问题之间是不独立的，一个子问题在下一阶段决策中可能被多次使用到。（该性质并不是动态规划适用的必要条件，但是如果没有这条性质，动态规划算法同其他算法相比就不具备优势）

## 动规解题的一般思路   
动态规划所处理的问题是**一个多阶段决策问题**，一般由初始状态开始，通过对中间阶段决策的选择，达到结束状态。这些决策形成了一个决策序列，同时确定了完成整个过程的一条活动路线(通常是求最优的活动路线)。如图所示。动态规划的设计都有着一定的模式，一般要经历以下几个步骤
```
初始状态→│决策１│→│决策２│→…→│决策ｎ│→结束状态
```

1. 划分阶段：按照问题的时间或空间特征，把问题分为若干个阶段。在划分阶段时，注意划分后的阶段一定要是有序的或者是可排序的，否则问题就无法求解
2. 确定状态和状态变量：将问题发展到各个阶段时所处于的各种客观情况用不同的状态表示出来。当然，状态的选择要满足无后效性
3. 确定决策并写出状态转移方程：因为决策和状态转移有着天然的联系，状态转移就是根据上一阶段的状态和决策来导出本阶段的状态。所以如果确定了决策，状态转移方程也就可写出。但事实上常常是反过来做，根据相邻两个阶段的状态之间的关系来确定决策方法和状态转移方程
4. 寻找边界条件：给出的状态转移方程是一个递推式，需要一个递推的终止条件或边界条件

**一般，只要解决问题的阶段、状态和状态转移决策确定了，就可以写出状态转移方程（包括边界条件）**

实际应用中可以按以下几个简化的步骤进行设计：
 - 分析最优解的性质，并刻画其结构特征
 - 递归的定义最优解
 - 以自底向上或自顶向下的记忆化方式（备忘录法）计算出最优值
 - 根据计算最优值时得到的信息，构造问题的最优解

## 算法实现的说明
动态规划的主要难点在于理论上的设计，也就是上面4个步骤的确定，一旦设计完成，实现部分就会非常简单。

使用动态规划求解问题，最重要的就是确定动态规划三要素：
 - 问题的阶段
 - 每个阶段的状态
 - 从前一个阶段转化到后一个阶段之间的递推关系

**递推是自底向上**
**递归是自顶而下**

**递推关系必须是从次小的问题开始到较大的问题之间的转化**，从这个角度来说，动态规划往往可以用递归程序来实现，不过**因为递推可以充分利用前面保存的子问题的解来减少重复计算，所以对于大规模问题来说，有递归不可比拟的优势**，这也是动态规划算法的核心之处

确定了动态规划的这三要素，整个求解过程就可以用一个**最优决策表**来描述，最优决策表是一个二维表，其中行表示决策的阶段，列表示问题状态，表格需要填写的数据一般对应此问题的在某个阶段某个状态下的最优值（如最短路径，最长公共子序列，最大价值等），填表的过程就是根据递推关系，从1行1列开始，以行或者列优先的顺序，依次填写表格，最后根据整个表格的数据通过简单的取舍或者运算求得问题的最优解
```
f(n,m)=max{f(n-1,m), f(n-1,m-w[n])+P(n,m)}
```

## 算法实现的步骤
1. 创建一个一维数组或者二维数组，保存每一个子问题的结果，具体创建一维数组还是二维数组看题目而定，基本上如果题目中给出的是一个一维数组进行操作，就可以只创建一个一维数组，如果题目中给出了两个一维数组进行操作或者两种不同类型的变量值，比如背包问题中的不同物体的体积与总体积，找零钱问题中的不同面值零钱与总钱数，这样就需要创建一个二维数组(需要创建二维数组的解法，都可以创建一个一维数组运用**滚动数组**的方式来解决，即一位数组中的值不停的变化)
2. 设置数组边界值，**一维数组就是设置第一个数字，二维数组就是设置第一行跟第一列的值，特别的滚动一维数组是要设置整个数组的值**，然后根据后面不同的数据加进来变换成不同的值
3. 找出状态转换方程，也就是说找到每个状态跟他上一个状态的关系，根据状态转化方程写出代码
4. 返回需要的值，一般是数组的最后一个或者二维数组的最右下角

# 数塔问题
## 问题描述
将一些数字排成数塔的形状，其中第一层一个数字，第二层2个数字，……第n层n个数字，现在要从第一层走到第n层，每次只能走向下一层的两个数字中的一个，问：最后将路径上所有数字相加后得到的和最大是多少？

 - 输入：
```bash
5
        5
       8 3
     12 7 16
    4 10 11 6
  9  5  3  9  4
```

 - 输出：
```bash
44
# 44是5 -> 3 -> 16 -> 11 -> 9得到的
```

## Code
```c++
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn = 1000;

int f[maxn][maxn], dp[maxn][maxn];

int main() {
	int n;
	scanf("%d",&n);
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= i; j++) {
			scanf("%d",&f[i][j]);
		}
	}
	for (int j = 1; j <= n; j++) {
		dp[n][j] = f[n][j];//最下面一行为边界
	}
	for (int i = n - 1; i >= 1; i--) {
		for (int j = 1; j <= i; j++) {
			dp[i][j] = max(dp[i + 1][j], dp[i + 1][j + 1]) + f[i][j];
		}//这里自底向上求和，当前元素和下面一层左边和右边大的那个相加。
	}
	printf("%d\n",dp[1][1]);//dp[1][1]是从1，1到底层的最大和，也是本题要求的
	return 0;
}
```