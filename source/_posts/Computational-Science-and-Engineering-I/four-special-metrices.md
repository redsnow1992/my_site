title: Four Special Metrices
date: 2015/06/01 23:41
categories: math
tags: linear algebra
mathjax: true
---
# 1. Fixed Matric
$$
K_4=
\begin{bmatrix}
2 & -1 & 0 & 0     \\\\
-1 & 2 & -1 & 0     \\\\
0 & -1 & 2 & -1     \\\\
0 & 0 & -1 & 2   
\end{bmatrix}
$$
性质：
1. $ K=K^T $ ，对称性
2. Sparse，稀疏性；虽然当$n=4$的时候不明显，若$n=100$，则立马可以发现K是稀疏的。
3. Constant diagonal（常三角对称）
4. $K_n$是可逆的。

**关于$ K_4 $的可逆性：**
将$K_4$转化为上三角矩阵如下：
$$
U_4=
\begin{bmatrix}
2 & -1 & 0 & 0     \\\\
0 & 3/2  & -1 & 0     \\\\
0 & 0  & 4/3 & -1     \\\\
0 & 0  & 0 & 5/4   
\end{bmatrix}
$$
# 2. Circular Matrix
$$
C_4=
\begin{bmatrix}
2 & -1 & 0 & -1     \\\\
-1 & 2 & -1 & 0     \\\\
0 & -1 & 2 & -1     \\\\
-1 & 0 & -1 & 2   
\end{bmatrix}
$$
只是不具有*可逆性*。因为存在矩阵( $ (\begin{bmatrix}  1 & 1 & 1 & 1 \end{bmatrix})^T $)使其为0。

# 3. Free-Fixed Matrix
$$
T_4=
\begin{bmatrix}
1 & -1 & 0 & -1     \\\\
-1 & 2 & -1 & 0     \\\\
0 & -1 & 2 & -1     \\\\
-1 & 0 & -1 & 2   
\end{bmatrix}
$$
# 4. Free-Free Matrix
$$
T_4=
\begin{bmatrix}
1 & -1 & 0 & -1     \\\\
-1 & 2 & -1 & 0     \\\\
0 & -1 & 2 & -1     \\\\
-1 & 0 & -1 & 1   
\end{bmatrix}
$$

**以上四个矩阵的特性:**
* $K,T$ 是正定矩阵
* $C,B$ 是半正定矩阵