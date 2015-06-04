title: Chapter1 Introduction to Vectors
categories: math
tags: linear algebra
mathjax: true
---
The heart of linear algebra is in two operations--both with vectors. We add vectors to get $v+w$. We multiply them by numbers $c$ and $d$ to get $cv$ and $dw$. Combining those two operations gives the *linear combination* $cv+dw$.

$$
Linear\;combination \\qquad cv+dw=c\begin{bmatrix}1  \\\\
1
\end{bmatrix}+d\begin{bmatrix}2  \\\\
3
\end{bmatrix}=\begin{bmatrix}c+2d  \\\\
c+3d
\end{bmatrix}
 $$

 The vectors $cv$ lie along a line. When $w$ is not on that line, **the combinations $cv+dw$ fill the whole two-dimensional plane.**


# 1.1 Vectors and Linear Combinations
"You can't add apples and oranges." In a strange way, this is the reason for vectors. We have two separate numbers $v_1$ and $v_2$. That pair produces a **two-dimensional vectors**.

$$
Column\;vector \\qquad cv+dw=v=\begin{bmatrix}v_{1}  \\\\
v_2
\end{bmatrix} \\qquad \begin{array}{c}
v_1=first\;component   \\\\
v_2=second\;component
\end{array}
$$
