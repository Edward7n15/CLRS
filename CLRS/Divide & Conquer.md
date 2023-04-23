chapter 4 p.66

---
- Divide
	- the problem into subproblems that are ==smaller== instances of the ==same== problem
- Conquer
	- the subproblems by solving them ==recursively== (recursive case). If its size is small enough (bottom out), then just solve it in a straightforward manner (base case)
- Combine
	- the solutions to the subproblems into the solution for the original problem

---
## Recurrences

A recurrence is an equation or inequality that describes a function in terms of ==its value on smaller inputs.== 

For example, ==Merge-Sort procedure== can be described as
$$
T(n)=
\begin{cases}
\Theta(1)&\text{if }n=1\\
2T(n/2)+\Theta(n)&\text{if }n>1
\end{cases}
$$
whose solution we claimed to be $T(n)=\Theta(n\,lg\,n)$. (Master Theorem)

And Linear-Search procedure can be described as
$$
T(n)=T(n-1)+\Theta(1)
$$
since the length of the array in each recurrence decreases 1.

>[!info]
>- In the ==substitution method==
>	- we guess a bound and then use mathematical induction to prove our guess correct
>- In the ==recursion-tree method==
>	- we converts the recurrence into a tree whose ==nodes represent the costs== incurred at various levels of the recursion. We use techniques for bounding summations to solve the recurrence.
>- In the ==master method==
>	- it provides bounds for recurrences of the form $T(n)=aT(n/b)+f(n)$
>	- $a\ge1,b>1$ and $f(n)$ is a given function

---
### Technicalities in recurrences

Neglect certain technical details like floor and ceiling.

---
## Examples

### the maximum-subarray problem

- stock price changes day by day
- you know the future prices

solutions
- brute-force solution
	- try every possible pair
	- $(^n_2)$ pairs
	- $\Theta(n^2)$ time
- maximum subarray
	- take the daily-change array as the input
	- find the subarray with the largest sum

In the second method, the input array A in length (n-1), seems we still need to check $(^{n-1}_2)$ subarrays. But using divide-and-conquer, it will be efficient.

1. Find the mid-point and split the array into two subarrays
	1. A\[low, mid] and A\[mid+1, high]
2. an optimal solution A\[i, j] lie in one of the places
	1. entirely in A\[low, mid]
		1. $low\le i \le j \le mid$
	2. entirely in A\[mid+1, high]
		1. $mid < i \le j \le high$
	3. crossing the mid-point
		1. $low\le i \le mid < j \le high$
3. the solution to the first two kinds of arrays can be found recursively
4. the solution to the third kind of array consists of the solutions to the first two kinds of array

```Find-Max-Crossing-Subarray(A, low, mid, high)
left-sum = -inf
sum = 0

// find the left half that start from mid
for i = mid downto low
	sum = sum + A[i]
	if sum > left-sum
		left-sum = sum
		max-left = i

// find the right half that start from mid+1
right-sum = -inf
sum = 0
for j = mid+1 to high
	sum = sum + A[j]
	if sum > right-sum
		right-sum = sum
		max-right = j

// start, end, crossing sum
return (max-left, max-right, left-sum+right-sum)
```

that makes the completed D&C procedure
```Find-Maximum-Subarray(A, low, high)
// base case
if (high == low)
	return (low, high, A[low])
else
	mid = (low + high)/2
	(left-low, left-high, left-sum) = Find-Maximum-Subarray(A, low, mid)
	(right-low, right-high, right-sum) = Find-Maximum-Subarray(A, mid+1, high)
	(cross-low, cross-high, cross-sum) = Find-Crossing-Subarray(A, low, mid, high)
	if ((left-sum >= right-sum) and (left-sum >= cross-sum))
		return (left-low, left-high, left-sum)
	else if ((left-sum <= right-sum) and (left-sum <= cross-sum))
		return (right-low, right-high, right-sum)
	else
		return (cross-low, cross-high, cross-sum)
```

1. When the subarray can be divided, it divides the subarray into two parts.
2. When the subarray bottom ups, 
	1. two elements
		1. left base
		2. right base
		3.  sum of two
	2. three elements
		1. left base
		2. right base
		3. max sum including the middle
3. After comparing, the largest subarray will be fetched

### analyze

base case: $T(1)=\Theta(1)$

when n > 1: $T(n)=\Theta(1)+2T(n/2)+\Theta(n)+\Theta(1)=2T(n/2)+\Theta(n)=\Theta(n\,lg\,n)$  
>[!info]
>split + (left + right) + mid + compare

---
## Strassen's Algorithm for Matrix Multiplication

Matrix Multiplication Rule: $c_{ij}=\sum_{k=1}^na_{ik}\cdot b_{kj}$ 

```Square-matrix-multiply(A, B)
n = A.rows
let C be a new n*n matrix
for i=1 to n
	for j=1 to n
		c_ij = 0
		for k=1 to n
			c_ij = c_ij + a_ik * b_kj
return C
```

>[!warning]
>Because of the triply-nested for loops, it runs in $\Theta(n^3)$ time.
>
>One may think it takes $\Omega(n^3)$ time, but we have a way to make it in $o(n^3) time.$

Strassen's recursive runs it in $\Theta(n^{lg7})$, so it is in $O(n^{2.81})$. 

### A simple D & C

- assume that n is an exact power of 2 in each of the n\*n matrices
- divide n\*n matrices into four n/2 \* n/2 matrices
- as long as n >= 2, the dimension n/2 is an integer

```Square-matrix-multiply-recursive(A, B)
n = A.rows
let C be a new n*n matrix
if n==1
	c_11 = a_11 * b_11
else partition A, B, and C as in equations
	C_11 = Square-matrix-multiply-recursive(A_11, B_11) 
			 + Square-matrix-multiply-recursive(A_12, B_21)
	C_12 = Square-matrix-multiply-recursive(A_11, B_12) 
			 + Square-matrix-multiply-recursive(A_12, B_22)
	C_21 = Square-matrix-multiply-recursive(A_21, B_11) 
			 + Square-matrix-multiply-recursive(A_22, B_21)
	C_22 = Square-matrix-multiply-recursive(A_21, B_12) 
			 + Square-matrix-multiply-recursive(A_22, B_22)
	return C
```

The beautiful part is that the calculation still work when you ==consider a smaller matrix as an entry of the big matrix.==

### analyze

- base case: $T(1)=\Theta(1)$ 
	- simple multiplication
- recursive case: $\Theta(1)+8T(n/2)+\Theta(n^2)=8T(n/2)+\Theta(n^2)=\Theta(n^3)$ 
>[!info]
>paritioning + 8 parts + matrix addition

==Therefore, the simple D&C is no faster than the brute-force==

More specifically, 
- partitioning 2 matrices
	- $\Theta(2)$
- adding 2 matrices with $n^2/4$ entries
	- $\Theta(n^2/4)=\Theta(n^2)$ since constant factor will be omitted
	- we have 4 matrix additions, but the constant will also be omitted
- but, we cannot omit the constant 8
	- otherwise, the recursion tree will be ==linear==

## Strassen's method

Instead of performing 8 recursive multiplications of n/2 \* n/2 matrices, it performs ==only 7==.

1. Divide the input A and B and output C into n/2 \* n/2 submatrices
	1. $\Theta(1)$
2. Create 10 matrices $S_1\,...\,S_{10}$
	1. each of which is n/2 \* n/2
	2. they are sum of difference of 2 matrices in step 1
3. Using submatrices created in step 1 and the 10 matrices in step 2, recursively compute seven matrix products $P_1\,...\,P_7$
	1. each of which is n/2 \* n/2
4. Compute the desired submatrices $C_{ij}$ of the result C by adding and subtracting various combinations of the $P_i$ matrices
	1. $\Theta(n^2)$

Therefore, when n > 1, $T(n)=7T(n/2)+\Theta(n^2)=\Theta(n^{lg\,7})$.

In step 2, we create
$$
\begin{split}
S_1=B_{12}-B_{22}\\
S_2=A_{11}+A_{12}\\
S_3=A_{21}+A_{22}\\
S_4=B_{21}-B_{11}\\
S_5=A_{11}+A_{22}\\
S_6=B_{11}+B_{22}\\
S_7=A_{12}-A_{22}\\
S_8=B_{21}+B_{22}\\
S_9=A_{11}-A_{21}\\
S_{10}=B_{11}+B_{12}\\
\end{split}
$$
In step 3, we create
$$
\begin{split}
P_1=A_{11}\cdot S_1\\
P_2=S_{2}\cdot B_{22}\\
P_3=S_{3}\cdot B_{11}\\
P_4=A_{22}\cdot S_4\\
P_5=S_{5}\cdot S_6\\
P_6=S_{7}\cdot S_8\\
P_7=S_{9}\cdot S_{10}\\
\end{split}
$$

In step 4, we compute
$$
\begin{split}
C_{11}=P_5+P_4-P_2+P_6\\
C_{12}=P_1+P_2\\
C_{21}=P_3+P_4\\
C_{22}=P_5+P_1-P_3-P_7\\
\end{split}
$$
---
## substitution method for solving recurrences

1. guess the form of the solution
2. use mathematical induction to find the constants and show that the solution works

### Example

we have 
	$T(n)=2T(\lfloor{n/2}\rfloor)+n$ 

we guess the solution is
	$T(n)=O(n\,lg\,n)$

if it is correct, we have
	$T(\lfloor n/2\rfloor)=O(\lfloor n/2\rfloor\,lg\,\lfloor n/2\rfloor)<c\lfloor n/2\rfloor\,lg\,\lfloor n/2\rfloor$ 

by substituting it into the original form, we got
$$
\begin{split}
T(n)\le 2(c\lfloor n/2\rfloor\,lg\,\lfloor n/2\rfloor)+n\\
\le cn\,lg(n/2)+n\\
=cn\,lg(n)-cn\,lg(2)+n\\
=cn\,lg(n)-cn+n\\
\le cn\,lg(n\\
=O(n\,lg(n))
\end{split}
$$
where $c\ge1$ 

### induction part

Since T(1)=1 and 1 lg(1) = 0, the base case fails to hold for any c.

Then we apply strong induction:

T(2) = 4 $\le$ c 2 lg(2) = 4c
T(3) = 5 < c 3 lg(3) = 4.7c

$\Rightarrow c\ge2$ 

then combine the previous substituting part, we have 
- for c $\ge 1$ and n > 3, $T(n)=O(n\,lg(n))$ holds
- it also holds when $n\,\ge 2$ and $c\ge2$

>[!info]
>https://www.youtube.com/watch?v=J9H0Mlp2pxg&ab_channel=YangXu

### make a good guess

$T(n)=2T(\lfloor n/2 \rfloor + 17)+n$ 
is not that larger than the one we discuss before, so n lg n is a good guess.

Another way to make a good guess is to prove ==loose upper and lower bounds== and then reduce the range of uncertainty.

We might start with $T(n)=\Omega(n)$ and $T(n)=O(n^2)$. What left within, a asymptotically tight solution, is $T(n)=\Theta(n\,lg(n))$.

### Subtleties

We can ignore the ceiling and floor like this
$$
\begin{split}
T(n)=T(\lfloor n/2\rfloor)+T(\lceil n/2\rceil)+1\\
=2T(n/2)+1\\
\le2\cdot (cn/2-d)+1\\
=cn-2d+1\\
\le cn-d\\
=O(n)
\end{split}
$$
as long as d $\ge$ 1

by guessing and substituting $T(n)\le cn-d$

### changing variables

for case like $T(n)=2T(\lfloor\sqrt n\rfloor)+lg\,n$, we can change the variable $n\rightarrow 2^m$

which makes $T(2^m)=2T(2^{m/2})+m$

rename it $S(m)=T(2^m)$ 

then we have $S(m)=2S(m/2)+m=O(m\,lg(m))$ 

by changing back, we have $T(n)=T(2^m)=S(m)=O(m\,lg(m))=O(lg(n)\cdot lg(lg(n)))$ 

---
## recursion-tree 

for example $T(n)=3T(\lfloor n/4\rfloor)+\Theta(n^2)$ 

![[tree diagram analyze.png]]

The first T(n) will be divided into ==3 small parts but in the same structure==, leaving $\Theta(n^2)$ which can be represented by $cn^2$. This transform recursively shows until it bottom ups and reach T(1).

We know the element in level k will be $c{\frac n{4^k}}^2=c\frac {n^2}{16^k}$ and the number of elements will be $3^k$, that means the sum of the level will be ${\frac{3}{16}}^kcn^2$. 

Since there are n elements originally, and it shrinks quarterly each time, the height will be $log_4{n+1}$ (start from 0).

At the bottom, we have $3^{log_4n}=n^{log_43}$ nodes because the number of nodes at level k will be $3^k$ and the bottom is level $log_4n$. 

The summation of the sums of all levels will be
$$
\begin{split}
T(n)=\sum^{log_4{n-1}}_{i=0}(\frac3{16})^icn^2+\Theta(n^{log_43})\\
<\sum^\infty_{i=0}(\frac3{16})^icn^2+\Theta(n^{log_43})\\
=\frac1{1-(3/16)}cn^2+\Theta(n^{log_43})\\
=O(n^2)
\end{split}
$$
since we know the geometric series
$$
S_n=\sum^\infty_{i=0} ar^i=\sum^\infty_{i=1}ar^{i-1}=\frac{a}{1-r}
$$
---
## The master theorem

- $a\ge1$
- $b>1$
- $T(n)=aT(n/b)+f(n)$

we have 
$$
\begin{split}
f(n)=O(n^{log_ba-\epsilon})\Rightarrow T(n)=\Theta(n^{log_ba})\\
f(n)=\Theta(n^{log_ba})\Rightarrow T(n)=\Theta(n^{log_ba}lg(n))\\
f(n)=\Omega(n^{log_ba+\epsilon})\,and\,af(n/b)\le cf(n)\Rightarrow T(n)=\Theta(f(n))
\end{split}
$$
where $\epsilon >0$ and c < 1

It basically means, when the ==recursive function splits into a smaller ones sized in 1/b==, you know that the recursive part will run in $\Theta(n^{log_ba-\epsilon})$ because we have $a^{log_bn}$ nodes in the bottom. 
- if the combine function is significantly smaller, the size doesn't change
- if the combine function is in the same size level as the recursive part, the result will be slightly larger
- if the combine function is significantly greater, that the sum in each level are smaller than the combine function, the result will be defined by the combine function

### prove the master theorem

--incomplete--