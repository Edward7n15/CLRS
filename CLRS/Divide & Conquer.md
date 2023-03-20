

--incomplete--

## Strassen's Algorithm for Matrix Multiplication

Matrix Multiplication Rule: $c_{ij}=\sum_{k=1}^na_{ik}\cdot b_{kj}$ 

Square-matrix-multiply(A, B)
	n = A.rows
	let C be a new n\*n matrix
	for i=1 to n
		for j=1 to n
			c_ij = 0
			for k=1 to n
				c_ij = c_ij + a_ik \* b_kj
	return C

>[!warning]
>Because of the triply-nested for loops, it runs in $\Theta(n^3)$ time.
>
>One may think it takes $\Omega(n^3)$ time, but we have a way to make it in $o(n^3) time.$

Strassen's recursive runs it in $\Theta(n^{lg7})$, so it is in $O(n^{2.81})$. 

### A simple D & C

- assume that n is an exact power of 2 in each of the n\*n matrices
- divide n\*n matrices into four n/2 \* n/2 matrices
- as long as n >= 2, the dimension n/2 is an integer

Square-matrix-multiply-recursive(A, B)
	n = A.rows
	let C be a new n\*n matrix
	if n\==1
		c_11 = a_11 \* b_11
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



