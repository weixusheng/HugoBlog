# AI-CSP


Constraint Satisfaction Problems
<!--more-->

搜索问题，是一种规划问题(planning problem)的最优解
约束满足问题（constraint satisfaction problems）(CSPSs).与搜索问题不同，CSP是一种**识别问题**（identification problem），对这种问题，我们只需要识别一个状态是否是目标状态，不用管我们如何到达那个目标。CSPs由三个要素定义：
 - **变量Variables**：CSP拥有一个集合，其中有N个变量$X_1,X_2,...X_N$，每一个都可以从定义的一组值中取一个值。
 - **域Domain**：一个CSP变量所有可能取值的集合${x_1,x_2...x_d}$
 - **约束Constrains**：约束定义了变量取值的限制条件，这可能与其他变量有关。

