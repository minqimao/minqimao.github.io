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

```matlab
function [L,U,P] = Lu(A)
%  LU factorization.
%   
% [L,U,P] = Lu(A) returns unit lower triangular matrix L, upper
% triangular matrix U, and permutation matrix P so that P*A = L*U.
%
% example,
%   A=rand(9,9);
%
%   [L,U,P] = Lu(A);
%   sum(sum(abs(P*A- L*U)))
%   
%   [L,U,P] = lu(A);
%   sum(sum(abs(P*A- L*U)))

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

For a given linear system 

Ax=b,

We can find x by

x= A^(-1)b

However, when the matrix is large, we can compute the LU decomposition to get the inverse of $A

That is,

[L, U]= lu(A);

Then,

x= inv(U)*inv(L)*b;

For matrix, if, instead of b you insert the identity matrix I,
to get the inverse:

inv(A) = inv((U)*inv(L)*I

$$
\begin{equation}
   P(X = x_i) = p_i, i = 1, 2, ...,n
\end{equation}
$$


```python
marineEnt = -0.4*np.log2(0.4)-0.6*np.log2(0.6)
print "the entropy of the marine organism: %.20f" % marineEnt
# the entropy of the marine organism: 0.97095059445466858072
```

## Reference

Mathworks, [mathworks.com/...](http://cn.mathworks.com/matlabcentral/newsreader/view_thread/19700)

Function, [mathworks.com/...](http://www.mathworks.com/matlabcentral/fileexchange/37459-matrix-inverse-using-lu-factorization/content/Lu.m)