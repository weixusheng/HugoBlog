# 数据库原理及应用 PART-5



数据库范式定义,模式分解,函数依赖,正则覆盖,模式分解
<!--more-->

昨天数据库还剩10节课,以为能看完,再来写总结,可没想到...最后两章这么难
到现在还是有些迷糊,这两章可以说全是理论,集合之间的操作
今天可算学完了,数据库原理撒花

# 数据库范式定义
&emsp; 设计关系数据库时，遵从不同的**规范要求**，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范
&emsp; **越高的范式数据库冗余越小**。

# 模式分解
## 定义
 - 例如可以将关系模式( A,B,C,D) 分解为:( A,B )和( B,C,D ),或( A,C,D )和
( A,B,D ),或( A,B,C )和( C,D ),或( A,B )、( B,C )和( C,D ),或 ( A,D )和( B,C,D )
 - 原模式( R )的所有属性都必须出现在分解后的( R1 , R2 )中: R = R1 并R 2
 - 无损连接分解
对关系模式 R 上的所有可能的关系 r
  ![无损分解](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/2.png)

![无损分解2](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/3.png)

## 目标
目标:设计一个理论
 - 以判断关系模式 R 是否为“好的”形式(不冗余)
 - 当 R 不是“好的”形式时,将它分解成模式集合{ R 1 , R 2 , ..., R n }使得
    - 每个关系模式都是“好的”形式
    - 分解是无损连接分解
 - 理论基于:
    - **函数依赖**( functional dependencies )
    - **多值依赖**( multivalued dependencies )

# 函数依赖
## 定义
设 R 是一个关系模式,且有属性集 α ⊆ R, β⊆ R
 - β函数依赖于α, α函数决定β  (α -> β)
![函数依赖](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/4.png)
 - 函数依赖是码概念的推广
 - 被所有关系实例都满足的函数依赖称为平凡的

## 函数依赖集的闭包
 - 给定函数依赖集 F, 存在其他函数依赖被 F 逻辑蕴含
例如如果 A -> B 且 B -> C ,则可推出 A -> C
被 F 逻辑蕴含的全体函数依赖的集合称为 F 的闭包,用 F<sup>+</sup>表示 F 的闭包
例:  F = { A -> B, B -> C } ,F + = { A -> B, B -> C, A -> C,A -> A, AB -> B, AC -> C, A -> BC, ... }

### 计算闭包
例: R = ( A, B, C, G, H, I )
F = { A -> B, A-> C, CG -> H, CG -> I, B -> H }
计算F<sup>+</sup> : 可用Armstrong公理计算

### Armstrong公理
1. **自反律**
若 β  ⊆ α ,则 α -> β
2. **增补律**
若 α -> β ,则 γα -> γβ
3. **传递律**
若 α -> β 且 β -> γ ,则 α -> γ
4. **合并律**
若 α -> β 与 α -> γ 成立,则 α -> βγ
5. **分解律**
若 α -> βγ 成立,则 α -> β 与 α -> γ
6. **伪传递律**
若 α -> β 与 βγ -> g 成立,则 αγ -> g

Armstrong公理
 - 正确有效的(不会产生错误的函数依赖)
 - 完备的(产生所有成立的函数依赖即 F + )

![Armstrong公理](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/5.png)

### 公式计算过程
![函数依赖集的闭包](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/6.png)

## 属性集的闭包
### 定义
给定一个属性集 α, 在函数依赖集 F 下由 α 函数确定的所有属性的集合为 F 下 α 的闭包(记做 α <sup>+</sup> )
    - 检查函数依赖 α -> β 是否属于 F <sup>+</sup>  <-> β ⊆ α<sup>+</sup>
    - 判断 α 是否为超码: α -> R 属于 F <sup>+</sup>  -> R ⊆ α

### 公式计算过程
![属性集的闭包](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/7.png)

### 例题
![属性集的闭包](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/8.png)

### 属性闭包算法的用途:
 - 测试超码( α -> R )
    - 为检测 α 是否超码,可计算 α<sup>+</sup> 并检查 α<sup>+</sup> 是否包含 R 的所有属性
 - 测试函数依赖( α -> β )
    - 为检测函数依赖 α -> β 是否成立(即是否属于 F<sup>+</sup> ),只需检查是否β⊆ α 即,可计算 α<sup>+</sup>,并检查它是否包含β
 - 计算 F 的闭包( F<sup>+</sup> = ? )
    - 对每个 γ ⊆ R ,计算 γ<sup>+</sup>,再对每个 S ⊆ γ<sup>+</sup>,输出函数依赖 γ -> S

# 正则覆盖
## 定义
F 的正则覆盖(记做 F<sub>c</sub> )是指与 F 等价的"极小的"函数依赖集合
 - F<sub>c</sub> 中任何函数依赖都不包含无关属性
 - F<sub>c</sub> 中函数依赖的左半部都是唯一的

## 计算过程
![正则覆盖](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/9.png)
![正则覆盖](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/10.png)

## 无关属性
![无关属性](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/11.png)

## 公式计算过程
![正则覆盖](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/12.png)

## 例题
![正则覆盖例题](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/13.png)

# 模式分解
## 定义
规范化的目标
 - 以判断关系模式 R 是否为“好的”形式(不冗余,无插入、删除、更新异常)
 - 当 R 不是“好的”形式时,将它分解成模式集合{ R 1 , R 2 , ..., R n }使得
    - 每个关系模式都是“好的”形式
    - 分解是无损连接分解
    - 分解是保持依赖

## 分解要求
![模式分解](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/14.png)
![模式分解](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/15.png)

## 例题
![模式分解](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/16.png)

## 公式判定是否保持依赖
![模式分解](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-5/16.png)



**由于最后两章东西太多了,所以第十章再开一个blog写,不得不说,数据库最后这两章真的让我有些头疼,感觉今天学了一天才差不多明白个大概**

