---
title: 用markdown书写数学公式
date: 2021-07-02 21:20:21
tags:
toc: 1
declare: true
---
首先在所在themes/_config.yml中找到mathjax，将其属性改为true。之后便可以使用数学公式了。<!--more-->
#### 行内与独行
- 行内公式：将公式插入到本行内，符号：`$公式内容$`，如：$xyz$
- 独行公式：将公式插入到新的一行内，并且居中，符号：`$$公式内容$$`，如：$$xyz$$

#### 上标和下标组合
- 上标符号，符号：`^`，如：$x^4$
- 下标符号，符号：`_`，如：$x_1$
- 组合符号，符号：`{}`，如：${x}^{y}_{ij}$

#### 汉字和字体格式
- 文本：`\text{}`，如：$V_{\text{初始}}$
- 加粗：`\mathbf{}`，如：$\mathbf{A}$
- 数学手写体：`\mathcal{}`，如：$\mathcal{A}$
- 黑板粗体：`\mathbb{}`，如：$\mathbb{A}$
- 花体：`\mathscr{}`，如：$\mathscr{A}$
- 罗马体：`\mathrm{}`，如：$\mathrm{A}$


- 下划线：`\underline{}`，如：$\underline{x+y}$
- 上划线：`\overline{}`，如：$\overline{x+y}$
- 标签：`\tag{}`，如：$$\tag{3}$$
- 自动添加标签：`\begin{equation} \end{equation}`，如：$$\begin{equation} x \end{equation}$$
- 上大括号：`\overbrace{}`，如：$\overbrace{a+b+c+d}^{2.0}$
- 下大括号：`\underbrace{}`，如：$a+\underbrace{b+c}_{1.0}+d$
- 上位符号：`\stackrel{上位符号}{基位符号}`，如：$\vec{x}\stackrel{\mathrm{def}}{=}{x_1,\dots,x_n}$
- 上下标位置：`\sideset控制四角上下标，\overset与\underset控制上下标居中`，如：$\sideset{^a_b}{^c_d}A$,$\overset{100}A$,$\underset{100}A$,$\overset{100}{\underset{200}A}$

- 分段函数：$$f(x) = \begin{cases}
    0, & \text{if } x < 0 \\
    x, & \text{if } x \geq 0
\end{cases}$$


#### 占位符
- 两个quad空格，符号：`\qquad`，如：$x \qquad y$
- quad空格，符号：`\quad`，如：$x \quad y$

#### 定界符与组合
- 括号：`() \big(\big) \Big(\Big) \bigg(\bigg) \Bigg(\Bigg)`，如：$() \big(\big) \Big(\Big) \bigg(\bigg) \Bigg(\Bigg)$
- 中括号：`\left[ \right]`，如：$\left[x+y\right]$
- 大号的中括号：`\bigl[ \bigr]`，如：$\bigl[x+y\bigr]$
- 大括号，符号：`\{ \}`，如：$\{x+y\}$
- 自适应括号，符号：`\left \right`，如：$\left(x\right)$，$\left(x_{yz}\right)$
- 组合公式，符号：`{上位公式 \choose 下位公式}`，如：${n+1 \choose k}={n \choose k}+{n \choose k-1}$

#### 四则运算
- 加减运算，符号：`\pm`，如：$x \pm y=z$
- 减加运算，符号：`\mp`，如：$x \mp y=z$
- 乘法运算，符号：`\times`，如：$x \times y=z$
- 点乘运算，符号：`\cdot`，如：$x \cdot y=z$
- 星乘运算，符号：`\ast`，如：$x \ast y=z$
- 除法运算，符号：`\div`，如：$x \div y=z$
- 分式表示，符号：`\frac{分子}{分母}`，如：$\frac{x+y}{y+z}$
- 分式表示，符号：`{分子} \voer {分母}`，如：${x+y} \over {y+z}$
- 绝对值表示，符号：`||`，如：$|x+y|$

#### 高级运算
- 开二次方运算，符号：`\sqrt`，如：$\sqrt x$
- 开方运算，符号：`\sqrt[开方数]{被开方数}`，如：$\sqrt[3]{x+y}$
- 对数运算，符号：`\log`，如：$\log(x)$
- 极限运算，符号：`\lim`，如：$\lim^{x \to \infty}_{y \to 0}{\frac{x}{y}}$
- 求和运算，符号：`\sum`，如：$\sum^{x \to \infty}_{y \to 0}{\frac{x}{y}}$
- 求积运算，符号：`\prod`, 如：$\prod^N_{i=1}x_i$
- 积分运算，符号：`\int`，如：$\int^{\infty}_{0}{xdx}$
- 微分运算，符号：`\partial`，如：$\frac{\partial x}{\partial y}$
- 矩阵表示，符号：`\begin{bmatrix} \end{bmatrix}`，如：$$\begin{bmatrix}
    a & b & c \\
    d & e & f \\
    g & h & i \\
\end{bmatrix}$$

- 梯度算子，符号：`\nabla`，如$\nabla$
- 向上取整，符号：`\lceil x \rceil`，如$\lceil x \rceil$
- 向下取整，符号：`\lfloor x \rfloor`，如$\lfloor x \rfloor$

#### 逻辑运算
- 大于等于运算，符号：`\geq`，如：$x+y \geq z$
- 小于等于运算，符号：`\leq`，如：$x+y \leq z$
- 不等于运算，符号：`\neq`，如：$x+y \neq z$
- 不大于等于运算，符号：`\ngeq`，如：$x+y \ngeq z$
- 不大于等于运算，符号：`\not\geq`，如：$x+y \not\geq z$
- 不小于等于运算，符号：`\nleq`，如：$x+y \nleq z$
- 不小于等于运算，符号：`\not\leq`，如：$x+y \not\leq z$
- 约等于运算，符号：`\approx`，如：$x+y \approx z$
- 恒定等于运算，符号：`\equiv`，如：$x+y \equiv z$

#### 集合运算
- 属于运算，符号：`\in`，如：$x \in y$
- 不属于运算，符号：`\notin`，如：$x \notin y$
- 不属于运算，符号：`\not\in`，如：$x \not\in y$
- 子集运算，符号：`\subset`，如：$x \subset y$
- 子集运算，符号：`\supset`，如：$x \supset y$
- 真子集运算，符号：`\subseteq`，如：$x \subseteq y$
- 非真子集运算，符号：`\subsetneq`，如：$x \subsetneq y$
- 真子集运算，符号：`\supseteq`，如：$x \supseteq y$
- 非真子集运算，符号：`\supsetneq`，如：$x \supsetneq y$
- 非子集运算，符号：`\not\subset`，如：$x \not\subset y$
- 非子集运算，符号：`\not\supset`，如：$x \not\supset y$
- 并集运算，符号：`\cup`，如：$x \cup y$
- 交集运算，符号：`\cap`，如：$x \cap y$
- 差集运算，符号：`\setminus`，如：$x \setminus y$
- 同或运算，符号：`\bigodot`，如：$x \bigodot y$
- 同与运算，符号：`\bigotimes`，如：$x \bigotimes y$
- 空集，符号：`\emptyset`，如：$\emptyset$
- 任意符号，符号：`\forall`，如：$\forall i$
- 存在符号，符号：`\exists`，如$\exists i$

#### 数字符号
- 无穷，符号：`\infty`，如：$\infty$
- 虚数，符号：`\imath`，如：$\imath$
- 虚数，符号：`\jmath`，如：$\jmath$
- 数学符号，符号：`\hat{a}`，如：$\hat{a}$
- 数学符号，符号：`\check{a}`，如：$\check{a}$
- 数学符号，符号：`\breve{a}`，如：$\breve{a}$
- 数学符号，符号：`\tilde{a}`，如：$\tilde{a}$
- 数学符号，符号：`\bar{a}`，如：$\bar{a}$
- 矢量符号，符号：`\vec{a}`，如：$\vec{a}$
- 数学符号，符号：`\acute{a}`，如：$\acute{a}$
- 数学符号，符号：`\grave{a}`，如：$\grave{a}$
- 数学符号，符号：`\mathring{a}`，如：$\mathring{a}$
- 一阶导数符号，符号：`\dot{a}`，如：$\dot{a}$
- 二阶导数符号，符号：`\ddot{a}`，如：$\ddot{a}$
- 上箭头，符号：`\uparrow`，如：$\uparrow$
- 上箭头，符号：`\Uparrow`，如：$\Uparrow$
- 下箭头，符号：`\downarrow`，如：$\downarrow$
- 下箭头，符号：`\Downarrow`，如：$\Downarrow$
- 左箭头，符号：`\leftarrow`，如：$\leftarrow$
- 左箭头，符号：`\Leftarrow`，如：$\Leftarrow$
- 右箭头，符号：`\rightarrow`，如：$\rightarrow$
- 右箭头，符号：`\Rightarrow`，如：$\Rightarrow$
- 底端对齐的省略号，符号：`\ldots`，如：$1,2,\ldots,n$
- 中线对齐的省略号，符号：`\cdots`，如：$x_1^2 + x_2^2 + \cdots + x_n^2$
- 竖直对齐的省略号，符号：`\vdots`，如：$\vdots$
- 斜对齐的省略号，符号：`\ddots`，如：$\ddots$
- 度数符号：`\circ`，如：$A^\circ$

#### 希腊字母
|字母|实现|字母|实现|
|:---:|:---:|:---:|:---:|
|$A$|`A`|$\alpha$|`\alpha`|
|$B$|`B`|$\beta$|`\beta`|
|$\Gamma$|`\Gamma`|$\gamma$|`\gamma`|
|$\Delta$|`\Delta`|$\delta$|`\delta`|
|$E$|`E`|$\epsilon$|`\epsilon`|
|$Z$|`Z`|$\zeta$|`\zeta`|
|$H$|`H`|$\eta$|`\eta`|
|$\Theta$|`\Theta`|$\theta$|`\theta`|
|$I$|`I`|$\iota$|`\iota`|
|$K$|`K`|$\lambda$|`\lambda`|
|$M$|`M`|$\mu$|`\mu`|
|$N$|`N`|$\nu$|`\nu`|
|$\Xi$|`\Xi`|$\xi$|`\xi`|
|$O$|`O`|$\omicron$|`\omicron`|
|$\Pi$|`\Pi`|$\pi$|`\pi`|
|$P$|`P`|$\rho$|`\rho`|
|$\Sigma$|`\Sigma`|$\sigma$|`\sigma`|
|$T$|`T`|$\tau$|`\tau`|
|$\Upsilon$|`\Upsilon`|$\upsilon$|`\upsilon`|
|$\Phi$|`\Phi`|$\phi$|`\phi`|
|$X$|`X`|$\chi$|`\chi`|
|$\Psi$|`\Psi`|$\psi$|`\psi`|
|$\Omega$|`\Omega`|$\omega$|`\omega`|

#### 参考文章
[Markdown数学公式语法](https://www.cnblogs.com/xym4869/p/11282586.html)
[Markdown数学公式语法华为云](https://www.huaweicloud.com/articles/2a62b09a875890613673074063fe9d32.html)
