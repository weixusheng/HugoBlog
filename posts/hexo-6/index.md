# Hexo-Mathjax渲染Bug


Hexo Mathjax大括号公式渲染问题
<!--more-->

# 双斜杠"\\"换行失败
## 解决方式
更换渲染引擎
```bash
npm uninstall --save hexo-renderer-marked
npm install --save hexo-renderer-kramed
```

多行公式需要使用两个"$$"
```bash
$$\left \lbrace\begin{matrix}
    h_\Theta(X)>0.5, set h_\Theta(X) = 1\\
    h_\Theta(X)<0.5, set h_\Theta(X) = 0
\end{matrix} \right.$$
```
$$\left \lbrace\begin{matrix}
    h_\Theta(X)>0.5, set h_\Theta(X) = 1\\
    h_\Theta(X)<0.5, set h_\Theta(X) = 0
\end{matrix} \right.$$

# 左括号渲染失败
```bash
$U=\{ A_1,A_2,\cdots,A_n \} $ #渲染失败
```

## 解决方案
不要使用
```
\{ \}
```
使用
```
\lbrace \rbrace
```
