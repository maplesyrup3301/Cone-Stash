1. BJT三极管的$Y$等效电路：
```tikz
\usepackage{circuitikz}
\begin{document}
\begin{circuitikz}[american voltages, european resistors]
\ctikzset{
	bipoles/thickness=4,
	resistors/scale=0.75,
	capacitors/scale=0.75,
	csources/scale = 0.75
}

\draw (0, 0) to[short, o-] (1, 0) -- (3, 0) -- (5, 0) -- (7, 0) to[short, -o] (8, 0);
\draw (0, 3) to[short, o-] (1, 3) -- (3, 3);
\draw (5, 3) -- (7, 3) to[short, -o] (8, 3);
\draw (1, 0) to[R, l=$y_{ie}$] (1, 3);
\draw (7, 0) to[R, l_=$y_{oe}$] (7, 3);
\draw (3, 3) to[cI, i>_=$y_{re}\dot V_{c}$] (3, 0);
\draw (5, 3) to[cI, i>=$y_{fe}\dot V_{b}$] (5, 0);
\draw (0, 3) to[open, v>=$\dot V_b$] (0, 0);
\draw (8, 3) to[open, v^>=$\dot V_c$] (8, 0);
\end{circuitikz}
\end{document}
```
1. BJT三极管的$\pi$混合电路：
```tikz
\usepackage{circuitikz}
\begin{document}
\begin{circuitikz}[american voltages, european resistors]
\ctikzset{
	bipoles/thickness=4,
	resistors/scale=0.75,
	capacitors/scale=0.75,
	csources/scale = 0.75
}


\draw (0, 0) to[short, o-] (2, 0) -- (3, 0) -- (5, 0) -- (6, 0) to[short, -o] (7, 0);
\draw (0, 3) to[R=$r_{bb'}$, o-] (2, 3) -- (3, 3) to[R=$r_{b'c}$] (5, 3) to[short, -o] (7, 3);
\draw (2, 3) to[C, l_=$C_{b'e}$] (2, 0);
\draw (3, 3) to[R=$r_{b'e}$] (3, 0);
\draw (5, 3) to[cI, i_>=$g_m\dot V_{b'e}$] (5, 0);
\draw (6, 3) to[R=$r_{ce}$] (6, 0);
\draw (3, 3) -- (3, 4) to[C=$C_{b'c}$] (5, 4) -- (5, 3);
\draw (0, 3) to[open, v>=$\dot V_b$] (0, 0);
\draw (7, 3) to[open, v^>=$\dot V_c$] (7, 0);
\end{circuitikz}
\end{document}
```

