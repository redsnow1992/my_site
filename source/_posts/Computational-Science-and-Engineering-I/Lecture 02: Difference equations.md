title: Lecture 02-Difference equations
categories: math
tags: linear algebra
mathjax: true
---
# 1. 一阶微分
$$
\text{Forward:}\Delta_{F}=\frac{U(x+h)-U(x)}{h}\approx{U'(x)+O(h) }   \\\\
\text{Backward:}\Delta_{B}=\frac{U(x)-U(x-h)}{h}\apporx{U'(x)+O(h) }  \\\\
\text{Centered:}\Delta_{C}=\frac{U(x+h)-U(x-h)}{h}\apporx{ U'(x)+O(h^2)}
$$

关于Centered证明（需要用到泰勒展开）：
$$
U(x+h)=U(x)+hU'(x)+\frac{h^2}{2!}U''(x)+\frac{h^3}{3!}U'''(x)     \\\\
U(x-h)=U(x)-hU'(x)+\frac{h^2}{2!}U''(x)-\frac{h^3}{3!}U'''(x)    \\\\
\frac{U(x+h)-U(x-h)}{2h}=U'(x)+\frac{h^2}{6}U'''(x)
$$

# 2.二阶微分

