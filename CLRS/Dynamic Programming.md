chapter 15 p. 359 - 397

---

- like Divide & Conquer
	- combining the solution to subproblems
- but DP applies when subproblems ==overlap==
	- subproblems ==shares== sub-subproblems
	- D&C will be less efficient since it does extra work

DP solves each subproblem just once and then saves its anser in a table.
>[!info]
>Think about JVM

---
## In optimazation problems

a problem can hava many possible solutions, and each solution has a value. We wish to find the ==optimal value== instead of ==optimal solution== since there may be several solutions that achieve the same value.

---
## Four steps to develop a DP

1. Characterize the structure of an optimal solution
2. Recursively define the value of an optimal solution
3. Compute the value of an optimal solution, typically in a bottom-up fashion
4. Construct an optimal solution from computed information

>[!info]
>Steps 1-3 form the basis of a DP solution to a problem
>
>If we need only the ==value== of an optimal solution, and not the ==solution== itself, then we can omit step 4

---
## Examples
---
### Rod cutting

>[!info]
>given
>- a rod of length $n$ inches in total
>- price table
>	- length $i=1,2,...,n$
>	- price $p_i$
>
| length $i$           | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  |
| ----------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| price $p_i$ | 1   | 5   | 8   | 9   | 10  | 17  | 17  | 20  | 24  | 30 |
>
>return
>-  revenue $r_n$

For some $1\le k\le n$, we have $n=\sum_{j=1}^k i_j$ where $k$ is the number of pieces, and $i$ is the ==distance== between two pieces.

Thus, $r_n=\sum_{j=1}^k p_{i_j}$ 

>[!info]
>Here are some optimal values:
>
>$r_1$ = 1   from 1 = 1 
>$r_2$ = 5   from 2 = 2
>$r_3$ = 8   from 3 = 3  
>$r_4$ = 10 from 4 = 2+2 
>$r_5$ = 13 from 5 = 2+3
>$r_6$ = 17 from 6 = 6
>$r_7$ = 18 from 7 = 1+6 or 2+2+3
>$r_8$ = 22 from 8 = 2+6
>$r_9$ = 25 from 9 = 3+6
>$r_{10}$ = 30 from 10 = 10
>
>Generally:
>$r_n=max(p_n,r_1+r_{n-1},r_2+r_{n-2},...,r_{n-1}+r_1)$



