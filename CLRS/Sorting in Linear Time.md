chapter 8 p.191

---
Comparison sorts: the sorted order is based only on comparisons between the input elements.

We shall prove that any comparison sort ==must== make $\Omega(n\,lg(n))$ comparisons ==in the worst case== to sort n elements. Thus, merge sort and heap sort are ==asymptotically optimal.== 

## decision tree

We can view comparison sorts abstractly in terms of decision trees, which is a ==full binary tree== that represents the comparisons between elements that are performed by a particular sorting algorithm operating on an input of a given size.

In a decision tree, we annotate each internal node by i:j for some i and j in the range $1\le i,j\le n$, where n is the number of elements in the input sequence. Each internal node indicates a comparison $a_i\le a_j$. The left subtree dictates subsequent comparisons once we know that $a_i\le a_j$, and the right subtree dictates subsequent comparisons knowing that $a_i>a_j$. 

We also denote each leaf by a permutation $<\pi(1),\pi(2),...,\pi(n)>$ 

Each of the n! permutations on n elements must appear as one of the leaves of the decision tree for a comparison sort to be correct.

### A lower bound for the worst case

The length of the ==longest path== from the root of a decision tree to any of its reachable leaves represents the ==worst-case== number of comparisons that the corresponding sorting algorithm performs. Consequently, ==the worst-case== number of comparisons for a given comparison sort algorithm equals the ==height of its decision tree==.

==A lower bound on the heights== of all decision trees in which each permutation appears as a reachable leaf is therefore a ==lower bound on the running time== of any comparison sort algorithm.

>[!Theorem 8.1]
>Any comparison sort algorithm requires $\Omega(n\,lg(n))$ comparisons in the worst case

proof: 
- height: h
- number of leaves: l
- number of elements: n

Because 
- each of the n! permutations of the input appears as some leaf, we have $n!\le l$. 
- binary tree of height h has no mare than $2^h$ leaves

We have 
$$
n!\le l\le 2^h
$$
therefor
$$
h\ge lg(n!)=\Omega(n\,lg(n))
$$
---

--incomplete--