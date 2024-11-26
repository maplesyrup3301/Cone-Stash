TikZ支持在LaTex中加入各类图像，本笔记的目的是初步介绍TikZ的用法；
**本笔记需要TikZJax插件方能正确显示**
最基本的操作之一就是作折线，使用`\draw`函数，其语法如下例中所示：
```tikz
\begin{document}
\begin{tikzpicture}
\draw (0, 0) -- (4, 0) -- (4, 4) -- (0, 4) -- cycle;
\end{tikzpicture}
\end{document}
```
对于闭合折线，使用关键词`cycle`比在最后再输入一次第一个点的位置要好
为了简便作画，对于矩形我们可以直接使用关键词`rectangle`，置于对角顶点之间：
```tikz
\begin{document}
\begin{tikzpicture}
\draw (0, 0) rectangle (4, 3);
\end{tikzpicture}
\end{document}
```
类似地还有抛物线：
```tikz
\begin{document}
\begin{tikzpicture}
\draw (0, 0) parabola (4, 3);
\end{tikzpicture}
\end{document}
```
要添加任意曲线，我们可以使用标定点，语法是`.. controls (*, *) [and *]..`，类似于样条
```tikz
\begin{document}
\begin{tikzpicture}
\draw (0, 0) .. controls (2, 4) .. (4, 0);
\end{tikzpicture}
\end{document}
```
要加入圆，使用关键词`circle`
```tikz
\begin{document}
\begin{tikzpicture}
\draw (-1, 0) -- (3, 0);
\draw (0, -1) -- (0, 3);
\draw (1, 1) circle (1.5cm);
\end{tikzpicture}
\end{document}
```

要加入椭圆，使用关键词`ellipse`
```tikz
\begin{document}
\begin{tikzpicture}
\draw (-1, 0) -- (3, 0);
\draw (0, -1) -- (0, 3);
\draw (1, 1) ellipse (1.5cm and 1cm);
\end{tikzpicture}
\end{document}
```
弧的画法稍有不同，语法是`arc (*:*:*)`，参数为起始角、结束角和半径
坐标指示的是弧的起始位置
```tikz
\begin{document}
\begin{tikzpicture}
\draw (-1, 0) -- (3, 0);
\draw (0, -1) -- (0, 3);
\draw (1, 2) arc (30:280:1cm);
\end{tikzpicture}
\end{document}
```
`\draw[*]`中\*处可以添加线的样式，例如：
```tikz
\begin{document}
\begin{tikzpicture}
\draw (-1, 0) -- (3, 0);
\draw (0, -1) -- (0, 3);
\draw[dashed, thick, blue] (1, 1) circle (1.5cm);
\end{tikzpicture}
\end{document}
```
要加入格点，使用关键词`grid`，同时在前面可以指定格点的步长：
```tikz
\begin{document}
\begin{tikzpicture}
\draw[red, thick] (-1, 0) -- (3, 0);
\draw[red, thick] (0, -1) -- (0, 3);
\draw[step=1cm,gray,very thin] (-2,-2) grid (6,6.01);
\end{tikzpicture}
\end{document}
```
填充颜色使用命令`\fill`，可以使用纯色或者渐变色
也可以使用合成命令`\filldraw`，填充的同时作出边框
```tikz
\begin{document}
\begin{tikzpicture}
\draw[step=1cm,gray,very thin] (-2,-2) grid (6,6.01);
\fill[blue!50!white] (0, 1) rectangle (3, 2);
\filldraw[bottom color=blue!50!white, top color=red!50!white, draw=black] (0, 2) rectangle (3, 4);
\end{tikzpicture}
\end{document}
```
下例是坐标轴的画法；其中`node[anchor=...] {*};`决定了点相对于标号的位置
```tikz
\begin{document}
\begin{tikzpicture}
\draw[thick,->] (0,0) -- (4.5,0) node[anchor=north west] {$X$};
\draw[thick,->] (0,0) -- (0,4.5) node[anchor=south east] {$Y$};
\foreach \x in {0,1,2,3,4}
   \draw (\x cm,1pt) -- (\x cm,-1pt) node[anchor=north] {$\x$};
\foreach \y in {0,1,2,3,4}
    \draw (1pt,\y cm) -- (-1pt,\y cm) node[anchor=east] {$\y$};
\end{tikzpicture}
\end{document}
```
`\clip`的作用是将作用限制到一个区域内，因此需要特别加一个`scope`
```tikz
\begin{document}
\begin{tikzpicture}
\draw[thick, ->] (-3, 0) -- (3, 0) node[anchor=west] {$x$};
\draw[thick, ->] (0, -3) -- (0, 3) node[anchor=south] {$y$};
\foreach \x in {-2, -1, 1, 2}
 \draw (\x cm, 2pt) -- (\x cm, 0pt) node[anchor=north] {$\x$};
\foreach \y in {-2, -1, 1, 2}
 \draw (2pt, \y cm) -- (0pt, \y cm) node[anchor=east] {$\y$};
\draw (0, 0) node[anchor=north east] {$O$};

\draw[thick] (0, 0) circle (2cm);
\draw[thick] (3, 2) -- (-2, -3);

\begin{scope}
\clip (0, 0) circle (2cm);
\fill[black, opacity=0.2] (3, 2) -- (-2, 2) -- (-2, -3) -- cycle;
\end{scope}


\end{tikzpicture}
\end{document}
```
作电路图
```tikz
\usepackage{circuitikz}
\begin{document}
\begin{circuitikz}
\draw (0, 0) to[sinusoidal voltage source] (0, 4) to[R, i_=1mA] (4, 4) to[C] (4, 0) to[lamp, o-o] (0,0);
\end{circuitikz}
\end{document}
```


