Chapter 22 p.589

---

## Representations

- G =(V, E)
	- collection of adjacency lists
	- adjacency matrix

- sparse graph
	- $|E|<<|V|^2$
	- use list
- dense graph
	- $|E|\approx|V|^2$
	- use matrix

![[graph repr example.png]]

## list representation

G = (V, E)
- array Adj in length |V| for vertex
- each of u $\in$ Adj is a list containing all vertices it connects
- list of lists (pointers)

If G is a directed graph, ==the sum of the lengths of all the adjacency lists is |E|==.

If G is an undirected graph, ==the sum of the lengths of all the adjacency lists is 2|E|==.
- Same edge appear in both vertex list

Memory require: $\Theta(V+E)$

### weighted graph

given a weight function $w: E\rightarrow\mathcal{R}$  

>[!example]
>given G = (V, E) and w(u, v)
>simply store the weight with the vertex in adjacency list

## matrix representation

$$
a_{ij}=
\begin{cases}
1&if(i,j)\in E\\
0&otherwise
\end{cases}
$$
require $\Theta(V^2)$ memory

also support weighted graph (build a new matrix with weights as entries)

If an edge does not exist, store NIL value as the entry

for unweighted graph, it only needs one bit per entry

---
## Breadth-First Search

one of the simplest algorithms for searching a graph and the archetype for many important graph algorithms

- Prim's minimum-spanning-tree algorithm
- Dijkstra's single-source shortest-paths algorithm

### how

1. given a distinguished source vertex s
2. explore the edges to find every vertex that is reachable from s
3. compute the distance (smallest number of edges) from s to each reachable vertex
4. produce a BF tree with root s that contains all reachable vertices
5. for any vertex v reachable from s, do the same thing

discover degree: 
white -> gray -> black

>[!info]
>black vertex connects either gray or black;
>that is, all vertices adjacent to black vertices have been discovered.
>
>Gray may have some adjacent white, they represent the frontier between discovered and undiscovered vertices.

```BFS(G, s)
# initialize
for each vertex u in G.V - {s}
	u.color = white
	u.d = inf # distance from source to vertex
	u.pi = nil # parent
s.color = gray
s.d = 0
s.pi = nil
Q = empty
Enqueue(Q, s)

# keep dequeueing, enqueue only when the current got some new edges
while Q != empty
	# source
	u = Dequeue(Q)
	# search all adjacent vertices of the source
	for each v in G.Adj[u]
		# if it is new
		if v.color == white
			v.color = gray # mark as discovered
			v.d = u.d + 1 # distance from root
			v.pi = u # record the parent
			Enqueue (Q, v) # stuck it back
	u.color = black # mark as fully discovered
```

![[bfs example.png]]

### analysis

white only once ⇾ # of dequeue = V
dequeue = O(1) ⇾ total dequeue = O(V)

gray once ⇾ # of enqueue < E
total enqueue = O(E)

total running time = O(V+E)
linear in the size of the adj list

### shortest paths

