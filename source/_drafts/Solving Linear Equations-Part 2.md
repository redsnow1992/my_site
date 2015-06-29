title: Solving Linear Equations-Part 2
categories: math
tags: linear algebra
mathjax: true
---
# Rules for Matrix Operations
When $A$ has $m$ rows and $n$ columns, it is an "$m$ by $n$" matrix. Matrices can be added if their shapes are the same.
$$
\begin{bmatrix}
1 & 2\\\\
3 & 4\\\\
0 & 0
\end{bmatrix}+\begin{bmatrix}
2 & 2\\\\
4 & 4\\\\
9 & 9
\end{bmatrix}=\begin{bmatrix}
3 & 4\\\\
7 & 8\\\\
9 & 9
\end{bmatrix}\qquad and\qquad2\begin{bmatrix}
1 & 2\\\\
3 & 4\\\\
0 & 0
\end{bmatrix}=\begin{bmatrix}2 & 4\\\\
6 & 8\\\\
0 & 0
\end{bmatrix}
$$
Matrix addition is easy. The serious question is *matrix multiplication*. When can we multiply $A$ times $B$, and what is the product $AB$? We cannot multiply when $A$ and $B$ are $3$ by $2$. They don't pass the following test:
> To multiply AB: If A has n columns, B must have n rows.

In every case $AB$ is filled with dot products. For the top corner, the $(1,1)$ entry for $AB$ is $(row\; 1\; of\; A)\cdot(column\; 1\; of\; B)$.

$$
\begin{bmatrix}\*\\\\
a\_{i1} & a\_{i2} & \cdots &  & a\_{i5}\\\\
\*\\\\
\*
\end{bmatrix}\begin{bmatrix}\* & \* & b\_{1j} & \* & \* & \*\\\\
 &  & b\_{2j}\\\\
 &  & \vdots\\\\
\\\\
 &  & b\_{5j}
\end{bmatrix}=\begin{bmatrix} &  & \*\\\\
\* & \* & (AB)\_{ij} & \* & \* & \*\\\\
 &  & \*\\\\
 &  & \*
\end{bmatrix}
$$
