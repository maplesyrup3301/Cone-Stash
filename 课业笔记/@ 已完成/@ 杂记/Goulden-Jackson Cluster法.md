**@ 对戈登-杰克逊 簇（Goulden-Jackson Cluster）法的简单介绍**
## 朴素方法
给定一个**单词** $\overline{w_1w_2\dots w_n}$，其长度为$k(0\lt k\leq n)$的子单词称为该单词的**k-因子**，在不特别指定长度的情况下统称为**因子**
例如：单词$SAP$ 的因子有：$S, A, P, SA, AP, SAP$
注意，因子**可以重复**出现，例如单词$OOZE$中的 1-因子$O$
给定一个**字母表**集合$V$，则其元素组成所有的可能单词集合记为$V^\ast$

考虑包含$d$个字母的有限字母表$V$，假设我们想记录所有长度$\leq R+1$的因子出现情况，那么对于每一个长度$\leq R+1$的单词$w$，我们引入一个变量$x[w]$. $x[w]$服从交换律.
定义单词$w=\overline{w_1w_2\dots w_n}$的权重为：
$$
W(w)=\prod_{r=1}^{R+1}\prod_{i=1}^{n-r+1}x[\overline{w_i\dots w_{i+r-1}}]
$$
例如：当$R=2$，则$W(OOZE)=x[O]^2x[Z]x[E]x[OO]x[OZ]x[ZE]x[OOZ]x[OZE]$

由单词构成的集合称为语言. 语言$\mathcal L$的权重$W(\mathcal L)$定义为语言中所有单词的权重之和
给定一个语言$\mathcal L$和一个字母$v$，$\mathcal Lv$表示由$\mathcal L$所有字母后增加字母$v$形成的新语言
例如：$\mathcal L=\{CAT, DOG\}$，则$\mathcal LS=\{CATS, DOGS\}$

生成函数：
$$
\Phi_R=\sum_{w\in V^\ast}W(w)
$$
储存了所有含有符合条件长度因子的单词的信息
因此，$V^\ast$中恰含有$n_u$个因子$u$的单词数为生成函数中$x[u]^{n_u}$项的系数

若我们欲求 给定长度的 禁语在其中出现了给定次数的 单词的数量，我们首先要计算$\Phi_R$
随后对所有字母$v$令$x[v]=s$，对所有单词$w$令$x[w]=1$，但是若$w$是禁语则令$x[w]=t$
此时，生成函数中的系数$s^nt^m$就是恰包含$m$个禁语因子的长度为$n$的单词数

对长度为$R$的单词$v\in V^\ast$，记$S[v]$为其中以$v$结束的单词形成的子集
$v=\overline{v_1v_2\dots v_R}$，则$S[v]$中的单词或是$v$本身，或长度大于$R$
对于长度大于$R$的单词，若去掉最后一个字母则其属于$S[u]$，其中$u=\overline{iv_1v_2\dots v_{R-1}}$
符号表示为：
$$
S[v]=\{v\}\bigcup_{i\in V}S[\overline{iv_1v_2\dots v_{R-1}}]v_R
$$
由于对长度大于$R$的任意单词$w=\overline{w_1w_2\dots w_n}\in V^\ast$有：
$$
W(w)=W(\overline{w_1w_2\dots w_{n-1}}) \cdot \prod_{r=1}^{R+1}x[w_{n-r+1}\dots w_n]
$$
集合方程组转化为线性代数方程组：
$$
W(S[v])=W(v)+\Big(\prod_{r=1}^Rx[\overline{v_{R-r+1}\dots v_R}]\Big)
\sum_{i\in V}x[\overline{iv_1\dots v_R}]W(S[\overline{iv_1\dots v_{R-1}}])
$$
我们有$d^R$个有$d^R$个未知数$W(S[w])$的线性方程，其明显是有解的，从而得到生成函数为：
$$
\Phi_R=\sum_{w\in V^\ast, length(w)<R}W(w)+\sum_{w\in V^\ast, length(w)=R}W(S[w])
$$

该朴素方法的缺点是显然的，对于有三个字母的英文字母表，我们要解含有$26^2$个方程的方程组，此计算量过大。

## 最基本的GJC方法
考虑有限字母表$V$，有限禁语集合$B$，要求我们找到函数$a(n)$，其值为长度为$n$的不包含任何禁语的单词数
我们将计算生成函数$f(s)=\sum_{n=0}^\infty a(n)s^n$，而不是直接计算$a(n)$
引入单词$w$的权重$W(w)=s^{[w的长度]}$，记$\mathcal L(B)$为语言$\mathcal L$的不包含任何禁语因子的单词的子集，则有：
$$
f(s)=\sum_{w\in\mathcal L(B)}W(w)
$$
用组合数学中$0^0=1$的性质可以写为：
$$
f(s)=\sum_{w\in V^\ast}W(w)0^{[w属于B的因子数]}
$$
随后根据如下性质：
1. $0=1+(-1)$
2. 
$$
0^r=\begin{cases}
1, r=0\\
0, r>0
\end{cases}
$$
可以得到对于任意有限集$A$有
$$
\prod_{a\in A}0=\prod_{a\in A}(1+(-1))=\sum_{S\subset A}(-1)^{|S|}
$$
从而：
$$
f(s)=\sum_{w\in V^\ast}\sum_{S\subset B(w)}(-1)^{|S|}W(w)
$$
$B(w)$表示$w$属于$B$的因子的集合
因此欲求得的生成函数也是一个更大的包含有序对$(w, S)$的集合的权重枚举函数，不妨称这些有序对为**标记词**
在有序对集合中，权重定义为$W(w, S)=(-1)^{|S|}W(w)$

首先，我们需要一个适合处理这些奇怪对象的数据结构；
对于单词$w=\overline{w_1w_2\dots w_n}$的因子$\overline{w_i\dots w_j}$，我们记为$[i, j](1\leq i \leq j \leq n)$
从而$(w, S)=(w; [i_1, j_1], \dots, [i_l, j_l])$，显然$[i_r, j_r]\in B, r=1, 2, \dots, l$
我们对$j_r$进行升序排序，由于禁语不相互包含，可以认为$i_r$是互异的
对于两个因子$[i, j], [i', j']$，若它们包含至少一个公共字母，即$i\lt i'\leq j$，则称二者是**交叠的**
令$\mathcal M$为这些标记词的集合

给定一个非空的标记词$(\overline{w_1,\dots,w_2};[i_1, j_1], [i_2, j_2], \dots, [i_l, j_l])$，关于最后一个字母有两种可能
1. $j_l<n$
这说明最后一个字母不是任意一个禁语因子的组成部分，我们可以直接截去它得到一个更短的标记词
2. $j_l=n$
令$k$为满足以下条件的最小正整数：
$[i_k, j_k], [i_{k+1}, k_{k+1}]$交叠，$[i_{k+1}, j_{k+1}], [i_{k+2}, k_{k+2}]$交叠...$[i_{l-1}, j_{l-1}], [i_{l}, k_{l}]$交叠
则移出最后$(n-i_k+1)$个字母和$(l-k+1)$个标记因子可以得到两个标记词：
$(\overline{w_1\dots w_{i_k-1}};[i_1, j_1],\dots,[i_{k-1}, j_{k-1}])$和$(\overline{w_{i_k}\dots w_n};[1, j_k-i_k+1],\dots,[i_l-i_k+1, j_l-i_k+1])$
二者中前者无关紧要，但是后者有一个重要性质：其每一个字母至少属于一个标记因子，且相邻标记因子交叠，我们称其为簇，簇的集合记为$\mathcal C$

从而，对于$\mathcal M$的任何元素，其或是空词，或以一个不属于簇的字母结尾，或以一个簇结尾，因此:
$$
\mathcal M = [空词]\cup \mathcal MV \cup \mathcal M\mathcal C
$$
取其权重得：
$$
W(\mathcal M)=1+W(\mathcal M)ds+W(\mathcal M)W(\mathcal C)
$$
因为$W(\mathcal M)=f(s)$，从而得到：
$$
f(s)=\frac1{1-ds-W(\mathcal C)}
$$

接下来我们解决$W(\mathcal C)$的问题
对于单词$w=\overline{w_1w_2\dots w_n}$，令$H(w)$为其所有真前缀（即不包括单词本身，真后缀同此），$T(w)$为其所有真后缀
例如：$w=WATER$, 则$H(w)=\{W, WA, WAT, WATE\}$
定义$O(u, v)=T(u)\cap H(v)$
例如：$O(PICACA, CACACA)=\{CA, CACA\}$

若$x\in H(v)$，那么$v$有形式$v=xx'$，记$x'=v/x$
我们定义：
$$
u:v=\sum_{x\in O(u, v)}W(v/x)
$$
例如：$SEXSEX:EXSEXS$
$O(SEXSEX, EXSEXS)=\{EX, EXSEX\}$
$v/O(SEXSEX, EXSEXS)=\{SEXS, S\}$
故而$u:v=s^4+s$

现在簇$\mathcal C$可以被表示为：
$$
\mathcal C=\bigcup_{v\in B}\mathcal C[v]
$$
$\mathcal C[v]$表示最后一个标记因子为$v$的簇
给定一个簇属于$\mathcal C[v]$的簇，它或仅包含$v$，或去除$v$后得到一个更小的簇，其可能以任意禁语$u$结尾，其中$O(u, v)$非空
这表示存在$x\in O(u, v)$使得$u=x''x, v=xx'$，移除$v$导致我们去除了$v/x$
相反的，给定一个属于$\mathcal C[u]$的簇和属于$O(u, v)$的一个元素，我们也能重构出属于$\mathcal C[v]$的簇
由此，我们建立了一个双射：
$$
\mathcal C[v]\leftrightarrow\{(v; [1, length(v)])\}\bigcup_{u\in B}\mathcal C[u]\times O(u, v)
$$
取权重得：
$$
W(\mathcal C[v])=(-1)W(v)-\sum_{u\in B}(u:v)W(\mathcal C[u])
$$
这是一个由$|B|$个方程构成的方程组，未知数为$W(\mathcal C[v])$
而且一般是非常分散的，因为大部分禁语之间没有交叠的部分
记$C(v)=\{u\in B|O(u, v)\neq \varnothing\}$，则可以重写为：
$$
W(\mathcal C[v])=-W(v)-\sum_{u \in C(v)}(u:v)W(\mathcal C[u])
$$
解未知数之后可以有:
$$
W(\mathcal C)=\sum_{v\in B}W(\mathcal C[v])
$$
代入方程可得生成函数，将生成函数展开为幂级数可以得到递推公式，用比较系数法即可解出任意$n$对应的值