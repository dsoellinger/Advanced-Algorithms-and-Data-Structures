## Randomized Data Structures for Searching



### Dictionary

A **dictionary** is a collection ADT (abstact data type) that focuses on data storage and retrieval. 

- Data is a key-value pair (KVP), a so-called item: (k,v)
- Standard operations:
  - Insert item (k,v) into structure
  - Retrieve data from structure, i.e., check whether it has an item with a given key k and return the pair (k,v)
  - Delete item from structure
- In addition, a reassign replaces the value in one of the (k,v) pairs in the structure 



### Set

A **set** is a collection ADT that allows to store data items and focuses on efficient membership tests.

- Data items
	- A data item is a key or key-value pair with trivial value
	- The order of the data items does not matter
	- Duplicate data items are not permitted
- Standard operations
	- Insert item into structure
	- Delete item from structure
	- Test membership, i.e., check whether it has an item with a given key $k$
- Core set-theoretic operations for two sets $S$, $T$
	- Union: Compute the union of $S$ and $T$
	- Intersection: Compute the intersection of $S$ and $T$
	- Difference: Compute the difference of $S$ and $T$
	- Subset: Check whether $S$ is a subset of $T$



### Theorem (127)

Comparison-based searching among $n$ elements requires $\Omega(log(n))$ comparisons in the worst case.

**Proof:**

Assume that we want to search for the item that has key $k$ among the items $a_1$, $a_2$, ..., $a_n$. A decision tree $T$ for solving this problem must contain at least $n+1$ leaves.

 - One leaf for each $a_i$, if $k$ is the key of $a_i$
 - One additional leaf for "Not found"

Hence, the height of $T$ is at least $log(n+1) \in \Omega(log(n))$



### Height

The **height** of a rooted tree $T$ is the maximum dept of nodes of $T$ with the root of $T$ being at depth 0.



### Balanced-Tree

A rooted binary tree is (height-balanced) if either it has no proper subtrees or if 
- it has two proper subtreees and the heights of both subtrees differ by not more than 1, or if
- it has exactly one proper subtree of height 0
  and if
- all proper subtrees are height-balanced.



### Theorem (130)

If $T$ is a balanced binary tree with $n$ nodes and height $h$ then $h \in \theta(log(n))$.



### Example: Balanced Search Trees

An example for a self-balancing search tree is the so-called AVL tree named after Adelson-Vel'skii and Landis. Insertion, searching and deleting all take $O(log(n))$ time in both the **average case** and the **worst case** where $n$ is the number of nodes in the tree.

However, of course, we need to "pay" for keeping the tree balanced. AVL insertions require $O(1)$ rotations (two rotations at most), while deletions require $O(log(n))$  rotations in the worst case ($O(1) in average). 

Its height is at most $\frac{1}{log(\phi )} log(n) \approx 1.44 \cdot log(n)$.

**Note:** Of course, keeping the tree balances causes some additional overhead. We, therefore, try to relax this requirement by making it less strict.



### Random BST

A **randomly buit binary search tree** with $n$ nodes is a binary search tree built by inserting $n$ items/keys in random order.


We can think about it like computing a random permutation of the items - where each permutation is equally likely - and then inserting the items in that order. Still, different permutions may result in the same tree. For instance, 2 -> 1 -> 3 and 2 -> 3 -> 1.

Furthermore, we need to keep in mind that we have no guarantee that our tree is balanced. It depends on the application whether randomness can be assumed. Otherwise, the resulting tree could be highly skewed. For instance, if we insert 10 numbers in random order then the resulting tree will degenerate to a list with probability $2/10!$

Nevertheless, the more nodes, the less likely the tree is degenrate!



### Lemma (131)

The expected time to build a binary search tree with $n$ nodes randomly is $O(n \cdot log(n))$.

**Note:** The worst-case runtime is $\theta(n^2)$ (for sorted inputs). 

**Proof:**

During the construction of a randomly built BST we perform the same comparisons as a randomized QuickSort, but in a different order. Theorem 91 tells us that the expected number of comparisons made by a randomized QuickSort on an array of $n$ numbers is at most $2n \cdot ln(n)$. Hence, we also get an $O(n \cdot log(n))$ expected-time bound.

Idea: Think about running Quicksort and storing the result of the intermediate steps.



<img src="images/random_binary_search_tree.png" width="500px" />



### Lemma (132)

The average node depth of a randomly build binary search tree is $O(log(n))$.

**Proof:**

The depth of a node equals the number of comparisons made during the BST construction. Since all permutations of th ekeys are equally likely, the verage node depth $d_n$ is given by

$d_n = \frac{1}{n} \mathbb{E}[\sum_{i=1}^n(\text{# comparisons for node i})] = \frac{1}{n} O(n \cdot log(n)) = O(log(n))$



### Theorem (133)

A randomly built binary search tree with $n$ nodes has an expected height of $\alpha \cdot ln(n)$, where $\alpha = 4.311$ .... is the unique solution within $[2, \infty]$ of the equation $\alpha \cdot ln(\frac{2e}{\alpha})=1$.



### Randomized Binary Search Tree

A binary search tree $T$ with $n$ nodes is a **randomized binary search tree** (RBST) if either $n=0$ or if, for $n > 0$,

- both its left subtree $L$ and right subtree $R$ are independent randomized binary search trees,
- Pr($L$ has $i$ nodes) = $\frac{1}{n}$    for all $0 \leq i \leq n-1$

An immediate consequence of this definition above is that any of the keys of a random BST of size  has the same probability, namely $1/n$, of being the root of the tree.



### Lemma (134)

The expected height of a randomized binary search tree with $n$ nodes is $O(log(n))$.



#### Insertion

In order to produce random BSTs, a newly inserted key should have some chance of becoming the root, or the root of one of the subtrees of the root, and so forth. 

If the tree $T$ is not empty with propability $1/(n+1)$ we place $x$ as the root of the new RBST. Therefore, the new tree will have size $n+1$. With probability $1-1/(n+1) = n/(n+1)$, we recursively insert $x$ in the left or right subtree of $T$ depending on the relation of $x$ with the key at the root of $T$.

```
best_insert(int x, bst T):
	int n, r
	n = T.size
	
	r = random(0,n)
	if(r == n):
		return insert_at_root(x,T)
	if(< T.key):
		T.left = insert(x,T.left)
	else:
		T.right = insert(x,T.right)
		
	return T
```

```
insert_at_root(int x, bst T):
	// use standard BST algorithm to insert x as leaf in T
	
	//perform left/right rotations to move the node containing x all the way up to the root of T
```

```
rotate_left(node w, bst T):
	node u = w.right
	u.parent = w.parent
	if (u != T.root):
		if (u.parent.left == u):
			u.parent.left = w
		else:
			u.parent.right = w
	w.right = u.left
	if(w.right != nill):
		w.parent = u
		u.left = w
	if (w == T.root):
		T.root = u
	
	return
	
rotate_right(node u, bst T):
	node w = u.left
	w.parent = u.parent
	if( u != T.root):
		if (u.parent.left == u):
			u.parent.left = w
		else:
			u.parent.right = w
	u.left = w.right
	if (u.left != nill):
		u.parent = w
		w.right = u
		if (u == T.root):
			T.root = w
			
	return
```



#### Deletion



**Join:**

We make use of a join operation for two RBSTs $L$ and $R$, where all keys in $L$ are assumed to be less than all keys in $R$:

- Let $n_L$ be the size of $L$ and $n_R$ be the size of $R$
- Use root of $L$ as root of the union tree with probability $\frac{n_L}{n_L+n_R}$ and recursively join right subtree of $L$ with $R$
- Use root of $R$ as root of the union tree with probability $\frac{n_R}{n_L+n_R}$ and recursively join $L$ with left subtree of $R$



**Deletion:**

- Search and delete the node that contains the key sought
- Use join operation to join the two subtrees of that node



#### Lemma (135)

Tree is still random after deletion.

```
randomizedJoin(bst L, bst R):
	
	//pick a random number, k, between 1 and (L.size + R.size)
	if (k <= L.size):
		T = L
		T.right = randomizedJoin(L.right, R)
	else:
    	T = R
    	T.left = randomizedJoin(L, R.left)
	
	return T

delete(key x, bst T):
	// search node N such that N.key equals x
	randomizedJoin(N.left, N.right)
	remove(N)
```



### Lemma (135)

Tree is still random after deletion.



#### Theorem (136)

The expected height of a randomized binary search tree with $n$ nodes is $O(log(n))$.
Search, insertion, deletion and join all run in $O(log(n))$ expected time.



