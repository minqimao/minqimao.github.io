---
layout: post
title:  LU factorization, Inverse of matrix
date:   2016-04-19
categories: Basis, Share
permalink: LU-decomposition
tags: Basis, Share

# author
author: Minqi Mao
---

## LU Decomposition

Function (MATLAB)

```python
function [L,U,P] = Lu(A)

s=length(A);
U=A;
L=zeros(s,s);
PV=(0:s-1)';
for j=1:s,
    % Pivot Voting (Max value in this column first)
    [~,ind]=max(abs(U(j:s,j)));
    ind=ind+(j-1);
    t=PV(j); PV(j)=PV(ind); PV(ind)=t;
    t=L(j,1:j-1); L(j,1:j-1)=L(ind,1:j-1); L(ind,1:j-1)=t;
    t=U(j,j:end); U(j,j:end)=U(ind,j:end); U(ind,j:end)=t;

    % LU
    L(j,j)=1;
    for i=(1+j):size(U,1)
       c= U(i,j)/U(j,j);
       U(i,j:s)=U(i,j:s)-U(j,j:s)*c;
       L(i,j)=c;
    end
end
P=zeros(s,s);
P(PV(:)*s+(1:s)')=1;
```
## Inverse of matrix

For a given linear system $Ax=b$,

We can find $x$ by

$$
\begin{aligned}
x= A^(-1)b
\end{aligned}
$$

However, when the matrix is large, we can compute the LU decomposition to get the inverse of $A$

That is,
$$
\begin{aligned}
[L, U]= lu(A);
\end{aligned}
$$

Then,
$$
\begin{aligned}
x= inv(U)*inv(L)*b;
\end{aligned}
$$

For matrix, if, instead of b you insert the identity matrix I,
to get the inverse:
$$
\begin{aligned}
inv(A) = inv((U)*inv(L)*I
\end{aligned}
$$


## Reference

Mathworks, [http://cn.mathworks.com/...](http://cn.mathworks.com/matlabcentral/newsreader/view_thread/19700)