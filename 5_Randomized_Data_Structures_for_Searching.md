Randomized Data Structures for Searching



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



### Treaps

A **treap** is a binary tree in which every node stores a priority in addition to the key-value pair such that

- it is a binary search tree on the keys
- it is a max-heap on the priorities where greater number means higher priority



#### Lemma (137)

The structure of a treap is completely determined by the search keys and priorites of its nodes.

**Proof by induction:**

- **IB**
  The base case is a tree with one node which is trivially unique.
- **IH**
  Treaps with $k$ nodes are unique.
- **IS**
  We will know add a new node to our tree with $k$ nodes.
  We know that the structure of a treap is the same as the structure of a binary tree in which the keys are inserted in increasing priority order, the treap with $k+1$ nodes with smallest priority is the same as the treap with $k$ nodes with smallest priority after inserting the $(k+1)$-th node. Since BST insert is a deterministic algorithm, there is only one place the $k$-th node could be inserted. Therefore, the treap with $(k+1)$-th node is unique.



<img src="images/treap_example_1.png" width="500px"/>



#### Operations

- **Search**
	Since a treap is a BST we can apply the algorithm for searching in a BST

- **Insertion**
	- We use the algorithm for insertion into a BST, thus creating a node z
	- In order to repair the heap structure, we use rotations to "bubble" z upwards as long as z has a greater priority than its parent

- **Deletion**
	- Search the node z sought
	- Use rotations to push z downwards until it becomes a leaf, thereby moving its higher-priority child upwards.

- **Split:** Split treap $T$ into two treaps $T_1$, $T_2$ such that all keys of $T_1$ are less than some given key $x$ and all keys of $T_2$ are greater than $x$.
	- Insert a dummy node with key $x$ and priority $+\infty$ into $T$
	- This node will become the root of the new treap and its subtrees form the two treaps $T_1$, $T_2$ sought.  
	



```
bubbleUp(node z, treap T):
	while( z.parent != NIL && z.parent.p > z.p )
		node u = z.parent
		if (z.parent.right == z):
			rotateLeft(z.parent, T)
		else:
			rotateRight(z.parent,T)
		z = u
		if (z.parent == NIL):
			T.root = z
			
	return
```



### Randomized Treap

A **randomized treap** is a treap in which the priorities are independently and uniformly distributed continuous random variables.

**Note:** When inserting a new key-value pair we generate a random real number between, e.g., 0 and 1, and use that number as the priority of the new node. By using reals as priorities we ensure that the probability of two nodes having equal priority is zero. In practice, choosing a random integer from a large range
or a random floating-point number is good enough.



#### Theorem (139)

The expected height of a treap with $n$ nodes is $O(log(n))$. Search, insertion, deletion and split all run in $O(log(n))$ expected time. The expected number of rotations done during an insertion/deletion is only $O(1)$.



### Sorted Linked List

#### Pros

- Truly simple dynamic data structure that is easy to implement
- No need for an a-priory estimate of the number of elements to be stored
- Easy to insert or delete in $O(1)$ time if position is known

#### Cons

- Difficult to get to the middle of the list; binary search does not work
- Search in $o(n)$ time is difficult



### Perfect Skip Lists

**Goal:** Combine the appealing simplicity of sorted lists with a good expected-time behavior!

**Idea:** Add a second list $L_1$ containing only every second item. Then we need at most $\lceil \frac{1}{2}n \rceil$ comparisons on $L_1$ and, with proper links into the first list $L_0$, one additional comparisons on $L_0$.

If $k$ nested lists are used: At most $\lceil \frac{1}{2^{k-1}} n \rceil$ comparisons on the k-th list $L_{k-1}$, plus one additional comparison in each of the lists $L_0, L_1, ..., L_{k-2}$.



#### Search

To search for an item we start on the list at the top level. In the current list we move towards the sentinel until the key of the next item will be greater than the query key. Then we go down and repeat the procedure until we are in the bottom list $L_0$. In the bottom list $L_0$ we either find the item queried or no such item exists.

When searching for $k$:

- If $k = next.k$:   Done
- If $k > next.k$:   Go Right. Stop at sentinel
- If $k < next.k$:   Go down one level from $L_i$ to $L_{i-1}$. Stop at $L_0$.

```
searchSkipList(key x, skiplist T):
	Node u = T.header
	int h = T.height
	
	while(h >= 0):
		while( u.next[h].key < x):
			u = u.next[h]
		--h
		
	return u
```



#### Insertion

- Insert item into full list $L_0$ (lowest level 0)

- Promote it to the next higher level with (independent) probability $p$

  **Note:** Common choices for $p$ are $1/2$ and $1/4$. The choice of $p$ allows a trade-off between space complexity and query speed.

  In case of $p=1/2$: The highest level of a newly can be determined by repeatedly flipping a coin until the coin comes up heads.

```
insertProbabilisticSkipList(key x, skiplist T):
	Node u = T.header;
	int h = T.height;
	int hx = result of coin flips;
	Node v = CreateNode(x, hx);
	while (h >= 0):
		while (u.next[h].key < x):
			u = u.next[h];
		if (h <= hx):
			v.next[h] = u.next[h];
			u.next[h] = v;
		--h;

	++T.counter_of_nodes;

	return v
```



#### Deletion

- Simply search and remove item from structure



#### Lemma (140)

The expected number of times a fair coin is tossed up to and including the first time the coin comes up heads is 2.

**Proof:**

Let $T$ denote this random variable and define an indicator random variable $I_i$ as $I_i := 1$ if the coin ends up being tossed $i$ or more times, and $I_i := 0$ otherwise. 



Probability that we toss at least 1 times: $Pr(I_1=1) = 1$

Probability that we toss at least 2 times: $Pr(I_2=1) = 0.5$     (First toss needs to be tail)

Probability that we toss at least 3 times: $Pr(I_3=1) = 0.5 \cdot 0.5$   (First + second toss need to be tail)

Therefore, we get:

​                                                      $\mathbb{E}(I_i) = Pr(I_i = 1) = \frac{1}{2^{i-1}}$



However, we since we are interested in the expected value of $T$.

$T= \sum_{i=1}^{\infty} I_i$

$\mathbb{E}(T) = \mathbb{E}(\sum_{i=1}^{\infty} I_i) = \sum_{i=1}^{\infty} \mathbb{E}(I_i) = \sum_{i=1}^\infty \frac{1}{2^{i-1}} = \sum_{i=0}^\infty \frac{1}{2^{i}} = 2$

Hence, the expected number of levels for a newly inserted item is 2.

##### Alternative

Pr(Insert at level 0) = $0.5$

Pr(Insert at level 0,1) = $1/4$

Pr(Insert at level 0,1 ... i) = $1/2^{l+1}$

Hence, we get as the required space = $\sum_{i=1}^\infty \frac{1}{2^i} n = 2n$

#### Lemma (141)

Suppose that we use constant-size nodes, with one node per level if an item is stored in that level. Then the expected number of nodes in a skip list storing n items is $2n$ if we disregard header and sentinel nodes.

**Proof:**

Probability of an item being at $L_0$: $\frac{1}{2^0} = 1$
Probability of an item being at $L_1$: $\frac{1}{2} = 0.5$
Probability of an item being at $L_2$: $\frac{1}{4} = 0.25$

Hence, after $n$ inserts $L_0$ has $1$ elements.
Hence, after $n$ inserts $L_1$ has $\frac{n}{2}$ elements.
Hence, after $n$ inserts $L_2$ has $\frac{n}{4}$ elements.

Hence, the expected number of elements in the list becomes:

$\mathbb{E}(\sum_{i=0}^\infty \frac{n}{2^i}) = n \cdot \sum_{i=0}^\infty \frac{1}{2^i} = 2n$

Hence, a linear storage can be expected to suffice for storing a skip list.



#### Lemma (142)

The expected height of a skip list storing $n$ items is at most $log(n) + 2$.



#### Lemma (143)

The expected length of a search path in a skip list storing $n$ items is $2 \cdot log(n) + 2$.



#### Theorem (144)

A skip list storing $n$ items has expected size $O(n)$ and supports search, insertion and deletion in expected time $O(log(n))$.



#### Extension

- We can count the number of edges in a search path, in order to gain access to the $j$-th item stored in the skip list
	- length of edge in $L_0$ is 1
	- length of edge in $L_i$ is the sum of the lengths of the edges in $L_{i-1}$ below
- To get to the j-th node we
	- go right if the sum of the edge lengths so far plus the length of th next edge is less than j
	- go down otherwise
- This makes it easy to get the j-th item in the sorted list in $O(log(n))$ time and set/modify its value. Of course, we need to take care that the list remains still sorted.



#### Sample application: Rope 

A **rope** is a data structure that is used to efficiently store and manipulate a very long string of characters by maintaining a number of short (sub-)strings.
Operations like insertion, deletion and random access shal be supported efficiently.

Ropes are used in many applications that maintain long strings which change over time. For instance, word processors or text editors.

Let $R := s_0 s_1 ... s_{n-1}$ be a string of length $n$ that is stored as a rope and let $R_1$, $R_2$ be rope strings.

**Rope operations:**
​	- **Concat($R_1$, $R_2$):** Return a rope that contains the concatenation of the strings $R_1$, $R_2$
​	- **Split(R,i):** Truncate $R$ to length $i$ and return a rope that contains the remaining characters
​	- **report(R,i,j):** Return a rope that contains the string $s_i,s_{i+1}, ..., s_{j-1}$
​	- **insert(R, $R_1$, i):** Return rope with string $R_1$ inserted at position $i$ of $R$.
​	- **delete(R,i,j):** Return rope without the string $s_i, s_{i+1},..., s_{j-1}$ of $R$

- Insertion and deletion can be realized by means of the first three operations.

- We can split a skip list at any node in $O(log(n))$ expected time
  - Insert a new sentinel and new header at the desired position
  - Cut old pointers and set up new pointers

- We can also join two skip lists with $n_1$ and $n_2$ nodes in $O(log(max\{ n_1, n_2\}))$ expected time:
	- Set up new pointers
	- Delete sentinel and header









