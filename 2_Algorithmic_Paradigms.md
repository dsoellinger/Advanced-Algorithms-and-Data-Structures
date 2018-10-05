## Algorithmic Paradigms



### Incremental Construction



#### What is incremental construction?

A result $R(\{ x_1, x_2, ..., x_n\})$ that depends on $n$ input items $x_1, x_2, ..., x_n$ is computed by dealing with one element at a time: For $2 \leq i \leq n$, we obtain $R(\{ x_1, x_2, ..., x_i\})$ from $R(\{ x_1, x_2, ..., x_{i-1}\})$ by "inserting" the i-th item $x_i$ into $R(\{ x_1, x_2, ..., x_{i-1}\})$.

#### Important invariant of incremental construction

$R(\{x_1, x_2, ..., x_i\})$ exhibits all the desired properties of the final result $R(\{ x_1, x_2, ..., x_n \})$ restricted to $\{ x_1, x_2, ..., x_i\}$ as input items:  If we would stop incremental construction after having inserted $x_i$ then we would have the correct solution $R(x_1, x_2, ...,x_i\})$ for $\{x_1, x_2, ..., x_i\}$.

Note that once this invariant has been established the overall correctness of an incremental algorithm is a simple consequence. The total complexity is given as a sum of the complexities of the individual "insertions".

Incremental algorithms are particularly well suited for dealing with "online problems", for which data items arrive one after the other.

**Online algorithm:** Can process input data piece-wise
**Offline algorithm:** All input data need to be given from the beginning



#### Example: Insertion Sort

Insertion sort is a classical incremental algorithm: We insert the $i$-th item (of $n$ items) into the sorted list of the first $i-1$ items, thereby transforming it into a sorted list of the first $i$ items.

```python
InsertionSort(array, int low, int high):
	for ( i=low+1; high; ++i ):
		x = A[i]
		j = i
		while ((j >= 1) && (A[j-1] > x) ):
			A[j] = A[j-1]
			--j
		A[j] = x
```



##### Runtime complexity

**Worst case:** 

Occurs if input array is sorted in reverse order. Hence, the number of insert (incl. swaps) operations becomes

$\text{# inserts } = 1 + 2 + ... + n = \frac{n(n+1)}{2} \in O(n^2)$

**Best case:**

Occurs if the array is already sorted.

$\text{# inserts } = 1 + 1 + ... + 1 = n \in O(n)$

**Average case:**

On average, we expect that each element is less than half the elements to its left. Therefore, the running time would be half of the worst-case running time. Hence, we have an average complexity of $\theta(n^2)$



#### Example: Convex Hull


##### Problem: ConvexHull

**Given:** A set of $S$ of $n$ points in the Euclidean plane $\mathbb{R}^2$
**Compute:** The convex hull $CH(S)$, i.e., the smallest convex super set of $S$.

##### Lemma (58)

- The convex hull of a set $S$ of points in $\mathbb{R}^2$ is a convex polygon
- Two distinct points $p,q \in S$ define an edge of $CH(S)$ if and only if all points of $S \setminus \{ p,q\}$ lie on one side of the line through $p,q$.



##### Incremental Construction

- Sort the points according to their $x$-coordinates and re-number accordingly:
  $S := \{ p_1, p_2, ..., p_n\}$. (Suppose that all x-coordinates are distinct)

- Compute $CH(\{p_1, p_2, p_3 \})$

- Suppose that $CH(\{p_1, p_2, p_3\})$

  - Compute the supporting lines of $CH(\{ p_1, p_2, ..., p_{i-1}\})$ and $p_i$
  - Split $CH(\{ p_1, p_2, ..., p_{i-i}\})$ into two parts at the two vertices of $CH(\{p_1, p_2, ..., p_{i-1}\})$ where the supporting lines touch
  - Discard that part of $CH(\{ p_1, p_2, ..., p_{i-1} \})$ which faces $p_i$



<img src="images/convex_hull_algorithm.jpg" width="500px" />



**Algorithm**

```
INCREMENTAL CONVEX HULL (S)

Sort the n belongs to S points by their x-coordinate
CH  ←  triangle (p1, p2, p3)
for (4 ≤ i ≤ n ) do
        j ←  Index of the rightmost point of CH

        // find the upper tangency point
        u = j
        while (pih4 is not tangent to CH) do
                if (u ≠  j) then
                        remove h4  from CH
                u = u -1

        // find the lower tangency point
        I = j
        while (pihl is not tangent to CH) do 
                if ( I ≠ u) then
                        remove hi from CH
                I = I + 1

        INSERT pi in CH between hu and hi
```



**Runtime complexity**

We know that sorting can be done in $O(n \cdot log(n))$ time. The construction of $CH(\{p_1, p_2, p_3\})$ takes $O(1)$ time. Inserting $p_i$ into $CH(\{p_1, p_2, ..., p_{i-1}\})$ will result in discarding one or more vertices of $CH(\{p_1, p_2, ..., p_{i-1}\})$ and, thus, takes $O(i)$ time.
Hence, we get $O(n \cdot log(n)) + O(1) + O(4+5+...+n) = O(n^2)$ as total time complexity.

But wait, is this a tight upper bound? Definitely not, since we need to take into account that whenever we remove an item from list, it doesn't have to be proceeded in the future. In other words, $m_4 + m_5 + ... + m_n < n$. Hence, the insert operation of an element $p_i$ runs in amortized time $O(1)$ and the total complexity of the incremental construction algorithm is $O(n \cdot log(n))$.

##### Theorem (59)

The convex hull of $n$ points  in a plane can be computed in worst-case optimal time $O(n \cdot log(n))$.



### Greedy Paradigm

A *greedy algorithm* attempts to solve an optimization problem by repeatedly making the locally best choice in a hope to arrive at the global optimum. However, a greedy algorithm may but not end up in the optimum.

Typically, if greedy algorithms are applicable to a problem then the problem tends to exhibit the following two properties:

-  **Optimal substructure:** The optimum solution to a problem consists of optimum solutions to its sub-problems.
- **Greedy choice property:** Choices may depend on prior choices, but must not depend on future choices; no choice is reconsidered

If a greedy algorithm does indeed produce the optimum solution then it's likely that the algoirthm of choice tends to be faste rthan other algorithms.

**Examples:** Kruskal's algorithm / Prim's algorithm for computing minimum spanning trees



#### Selection Sort

Selection sort is a classical greedy algorithm. We sort the array by repeatedly searching the k-smallest item and move it forward to make it the k-th item of the array.

```
SelectionSort(array A[], int low, int high):
	for (i = low; high; ++i):
		j_min = i
		for (j = low+1; high; ++j):
			if (A[j] < A[j_min]): 
				j_min = j
		if (j_min != i): 
			Swap(A[i], A[j_min])
```



**Runtime complexity:**

Obviously, selection sort runs in $O(n^2)$ time in both the average and the worst-case. The only advantage is that it requires only $O(n)$ write operations of array elements while insertion sort may consume $O(n^2)$ write operations.



#### Huffmann Coding

Huffmann coding is a so-called variable-length encoding scheme. While fixed-coding schemes (like ASCII) use a fixed number of bits to encode each symbol, variable-length encodes symbols that occur

- more frequently with shorter bit strings
- less frequently with longer bit strings

Obviously, such a code needs to be design in a smart way. If one would assign, say $1$ to "a" and $11$ to "q" then an encoding string that starts with $11$ cannot be decoded unambiguously.



##### Prefix code

Consider a set $\Omega$ of symbols. A *prefix code* for $\Omega$ is a function $c$ that maps every $x \in \Omega$ to a binary string, such that $c(x)$ is not a prefix of $c(y)$ for all $x,y \in \Omega$ with $x \neq y$.

If a code has this property we say that it has the *prefix property*.



<img src="images/prefix_code.png" width="600px" />



##### Lemma (61)

Let $T$ be the binary tree that represents $c$. If $c$ is a prefix code for $\Omega$ then only the leaves $T$ represent symbols of $\Omega$.



##### Average number of bits per symbol

Consider a set $\Omega$ of symbols and a frequency function $f: \Omega \rightarrow \mathbb{R}^+$. The *average number of bits per symbol* of a prefix code $c$ is given by 

$\hspace{7cm} ANBS(\Omega, c, f) := \sum_{\omega \in \Omega} f(\omega) \cdot |c(\omega)|$ 

where $|c(\omega)|$ denotes the number of bits used by $c$ to encode $\omega$.



##### Optimum prefix code

A prefix code $c*$ for $\Omega$ is *optimum* if it minimizes $ANBS(\Omega, c,f)$ for a given frequency $f$.



##### Lemma (64)

If a prefix code $c*$ is optimum then the binary tree that represents $c*$ is a **full** tree. (every non-leaf node of $T$ has two children)



##### Lemma (65)

The lowest-frequency symbol of $\Omega$ appears at the lowest level of the tree that represents an optimum prefix code $c*$.



##### Huffman's greedy template

- Create two leaves for the two lowest-frequency symbols $s_1, s_2 \in \Omega$
- Recursively build the encoding tree for $\Omega \cup \{ s_{12}\}  \setminus \{s_1, s_2\}$ with $f(s_{12}) := f(s_1) + f(s_2)$ where $s_{12}$ is a new symbol that does not occur in $\Omega$.



##### Theorem (66)

Huffman's greedy algorithm computes an optimum prefix code $c*$ for $\Omega$ relative to  a given frequency $f$ of the symbols of $\Omega$.



#### Job Scheduling



##### Problem: JobScheduling

**Given:** A set $J$ of $n$ jobs, where job $i$ starts at time $s_i$ and finishes at time $f_i$. Two jobs $i$ and $j$ are $compatible$ if they do not overlap time-wise. For example, if either $f_i \leq s_j$ or $f_j \leq s_i$.

**Compute:** A maximum subset $J'$ of $J$ such that the jobs of $J'$ are mutually compatible.



##### There are different methods for sorting our jobs

- **Fewest conflicts:** Pick jobs according to smallest number of incompatible jobs
- **Shortest job duration:** Pick jobs according to ascending order of $f_i - s_i$
- **Earliest start time:** Pick jobs according to ascending ordre of $s_i$
- **Earliest finish time:** Pick jobs according to ascending order of $f_i$



##### Lemma (67)

Picking jobs according to **earliest finish time** allows to compute an optimum solution to **JobScheduling** in $O(n \cdot log(n))$ time for $n$ jobs.



##### Earliest finish time algorithm

```
# A => List of jobs

A ← sort_according_to_finish_time(A)
for j = 1 to n:
	if (job j compatible with A)
	A ← A ∪ {j}
return A
```



**Proof by contradiction:**

We now assume that our Greedy-strategy does **not** produce the correct result.

Suppose that an optimum solution has $m$ jobs while a greedy approach picks $k < m$ jobs $i_1, i_2, ..., i_k$. 

Now, we are going to show that such a set (not equal to the optimum solution) cannot exist.

Therefore, we first need to label the sets as follows:

Let $x$ be the largest-possible number such that $i_1=j_1$, $i_2 = j_2$, ..., $i_x = j_x$ over all optimum solutions $j_1, j_2$, ..., $j_m$. Obviously, we have $x < m$.

Next, we assume that there is a job $i_{x+1}$ that finishes before $j_{x+1}$. So, why not exchanging the job with $f_{j_{x+1}}$ of our optimum solution? Although, the optimum solution would remain optimal, it would violate the maximiality of $x​$.

The optimum solution still remains optimal, but it violates the maximality of $x$.



<img src="images/greedy_proof.png" width="1000px" />



##### Problem: ProcessorScheduling

**Given:** A set $J$ of $n$ jobs, where job $i$ starts at time $s_i$ and finishes at time $f_i$. Two jobs $i$ and $j$ are compatible if they do not overlap time-wise.

**Compute:** An assignment of a all jobs to a minimum number of processors such that no two incompatible jobs run on the same processor.



##### Lemma (68)

Assigning jobs to earliest start time allows to compute an optimum solution to ProcessorScheduling in $O(n \cdot log(n))$ time.



