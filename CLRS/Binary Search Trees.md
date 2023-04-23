chapter 12 p.286

---

For a ==complete binary tree== with n nodes, operations run in $\Theta(lg(n))$ worst-case time. However, if the tree is a ==linear chain== of n nodes, the same operations take $\Theta(n)$ worst-case time.

## What is a BST?

Each node has
- key
- data
- left
- right
- parent

The keys are always stored in a way as to satisfy the ==binary-search-tree property==:
	Let x be a node in a binary search tree. If y is a node in the left subtree of x, then y.key <= x.key. If y is a node in the right subtree of x, then y.key >= x.key

The binary-search-tree property allows us to ==print out all the keys in a binary search tree in sorted order== by a simple recursive algorithm, called an ==inorder tree walk==. This algorithm is so named because it prints the key of the ==root== of a subtree ==between== printing the values in ==its left subtree and printing those in its right subtree==. (Similarly, a preorder tree walk prints the root before the values in either subtree, and a postorder tree walk prints the root after the values in its subtrees.)

```Inorder-Tree-Walk(x)
if x != NIL
	Inorder-Tree-Walk(x.left)
	// until reach the left bottom
	print x.key
	// back one level
	Inorder-Tree-Walk(x.right)
```

It takes $\Theta(n)$ time to walk an n-node BST.

### Searching

```Tree-Search(x, k)
if x == NIL or k == x.key
	return x
if k < x.key
	return Tree-Search(x.left, k)
else
	return Tree-Search(x.right, k)
```

runs in O(height) time

### minimum and maximum

```Tree-Minimum(x)
while x.left != NIL
	x = x.left
return x
```

```Tree-maximum(x)
while x.right != NIL
	x = x.right
return x
```
runs in O(height) time

### successor and predecessor

```Tree-Successor(x)
if x.right != NIL
\\ as long as it is not the right most node, find the smallest one in its right sub
	return Tree-Minimum(x.right)
	
y = x.parent
\\ if it is the right most node, get its left most ancestor's ancestor
while y != NIL and x == y.right
	x = y
	y = y.parent
return y
```

still runs in O(height) time

### insertion

```Tree-Insert(T, z)
y = NIL
x = T.root
while x != NIL
	y = x
	\\ y holds previous and x goes the right direction
	if z.key < x.key
		x = x.left
	else
		x = x.right
\\ hits the bottom, then insert it
z.parent = y
if y == NIL
	T.root = z
else if z.key < y.key
	y.left = z
else
	y.right = z
```

runs in O(height) time

### deletion

- if z has no children, then simply remove it by modifying its parent to replace z with NIL as its child
- if z has one child, then elevate the child to take z's position by modifying z'parent to replace z by z's child
- if z has two children, find z's successor y ==which must be in z's right sub==, and have y take z's position. The rest of z's original right subtree becomes y's new right subtree, same to the left sub.
	- if y is z's right child, replace z by y, leaving y's right child alone
	- otherwise, y lies within z's right subtree by not z's right child.
		- replace y by its own right child, then replace z by y
```Transplant(T, u, v)
if u.parent == NIL
// only the root has not parent
	T.root = v
	// we have u's record, replace the root u by v
else if u == u.parent.left
// we have u's record, replace it
	u.parent.left = v
else 
	u.parent.right = v
if v != NIL
\\ after setting parent's info, we set v's info
	v.parent = u.parent
```

![[BST deletion.png]]

```Tree-Delete(T, z)
if z.left == NIL
// case a
	Transplant(T, z, z.right)
else if z.right == NIL
// case b
	Transplant(T, z, z.left)
else
	y = Tree-Minnimum(z.right)
	// find successor
	if y.parent != z
	// if it is not the right child, swap y and y.right and connect y with both sides of z
		Transplant(T, y, y.right)
		y.right = z.right
		y.right.parent = y
	// if it is the right child, only need to connect the y's left
	Transplant(T, z, y)
	y.left = z.left
	y.left.parent = y
```

O(h)

---
## Randomly built BST

-- incomplete--