## Hard Problems and Approximation Algorithms



### Polynomially solvable

A problem $P$ is **solvable** in **polynomial time** if there exists a polynomial $P$ such that a solution for every instance of $P$ can be obtained in time $O(p)$ where the size of the instance / input forms the argument of the polynomial.



**Example for polynomial time bounds:** $O(n)$, $O(n \cdot log^2(n))$, $O(2^{1000} \cdot n^2 \sqrt{n})$, $O(n^{1000})$

Motivation for distinguishing between polynomial and non-polynomial problems:

- If a problem is not solvable in polynomial time then there is absolutely no hope for an efficient (exact) solution for absolutely all large inputs. Such problems are considered **intractable**.
- Polynomials are solved under complement. If $p$ and $q$ are polynomials then $p \cdot q$ is a polynomial for most "standard" operations (closed under addition, multiplication and composition). For example, if the output of one polynomial-time algorithm is fed into the input of another, the composite algorithm is polynomial. Despite much work by many researchers, no one even knows whether the class NP is closed under complement. That is, does $L \in NP$ imply $\bar{L} \in NP$
- If a problem of size $n$ is solvable in time $f (n)$ on one model of computation then it is also solvable in $f (p(n))$ on another model of computation, for virtually all "natural" models of computation and polynomials $p$.
- The polynomial-time computable problems encountered in practice typically require much less time. Experience has shown that once the first polynomial-time algorithm for a problem has been discovered, more efficient algorithms often follow. Even if the current best algorithm for a problem has a running time of $\theta (n^{100})$, an algorithm with a much better running time will likely soon be discovered.



### Problem Class $P$

The problem class $P$ is the class of all decision problems that are solveable in polynomial time by a deterministic algorithm.



### Non-deterministic Polynomial-time solution

A **non-deterministic algorithm solves a decision problem $P$ in polynomial time** if there is a fixed polynomial $p$ such that for each instance $x$ of $P$ for which the answer is "yes" there is at least one execution of the algorithm that returns "yes" in at most $p(|y|)$ time.

**What is a non-deterministic algorithm?**

Roughly, a non-deterministic algorithm consists of a "guessing" phase and a "verifying phase".

- **Guessing:** An arbitary string $s$ of characters is generated
- **Verifying:** An deterministic algorithm takes the input and the string $s$. It may use or ignore $s$ during its computation. Eventually, it returns "yes" or "no", or it may get in an infinite loop and never halt.

The time consumed by a non-deterministic algorithm is the time needed to write $s$ plus the time consumed by the deterministic verifying phase.



### Problem Class $NP$

The problem class $NP$ is the class of all decision problems that are solveable in polynomial time by non-deterministic algorithms.

It is the class of languages that can be **verified** by a polynomial-time algorithm. In other words, we can easily find a certificate $s$ for an instance if the answer is "yes". But there exists a polynomial-time algorithm to check the validatiy of a proposed certificate $s$.

**Note:** NP stands for "non-deterministic polynomial time"



### Lemma (166)

We have $P \subseteq NP$.

**Sketch of Proof:** Let $P \in P$. All we need to do is to apply a deterministic algorithm that solves $P$ in polynomial time and let it ignore any non-determinism.

**Note:**

It is not clear whether $P$ is a proper subset of $NP$. However, it is widely believed that $P \notin NP$.



### Polynomially reducible

A decision problem P is **polynomially reducible** (or simply **reducible**) to a decision problem $Q$, denote by $\text{P} \leq_p Q$, if there exists a reduction from $\text{P}$ to $Q$ that runs in polynomial time.



### Lemma (168)

For two decision problems $\text{P},Q$, if $\text{P} \leq_p Q$ and $Q \in \text{P}$ then $\text{P} \in P$.

**Proof:** Let $x$ be an instance of P. We apply a polynomial reduction and map $x$ in time $p(|x|)$ to an instance $t(x)$ of $Q$. Since $Q \in \text{P}$, a solution to $t(x)$ can be obtained in time $q(p(|x|))$, where $q(|\alpha|)$ denotes the time needed for solving an instance $\alpha$ of $Q$.



### NP-hard

A problem $Q$ is **NP-hard** if every problem $\text{P} \in NP$ is polynomially reducible to $Q$.



### NP-complete

A problem $Q$ is NP-complete if it is in $NP$ and if it is NP-hard.



### Lemma (171)

We have $\text{NP-complete} \subseteq NP$ and $\text{NP-complete} \subset \text{NP-hard}$.



### Theorem (172)

If some NP-complete problem is in $P$ then $P = NP$.

**Proof:**

If an NP-complete problem P is in $P$ then all problems of $NP$ can be reduced in polynomial time to P and, thus, also solved in polynomial time.



### Theorem (173)

The satisfiability problem of propositional logic, SAT (Satisfiability), is NP-complete.



### NP-complete decision problems



#### Problem: SAT-CNF

**Given:** A propositional formula $A$ which is conjunctive normal form.

**Decide:** Is $A$ satisfiable?

**Note:**

Conjunctive normal form (CNF) is an approach to Boolean logic that expresses formulas as conjunctions of clauses with an AND or OR. Each clause connected by a conjunction, or AND, must be either a literal or contain a disjunction, or OR operator. CNF is useful for automated theorem proving.



#### Problem: 3-SAT-CNF

**Given:** A propositional formula $A$ which is in conjunctive normal form such that every clause consists of exactly (or at most) three literals.

**Decide:** Is $A$ satisfiable?



#### Problem: SubsetSum

**Given:** A set $S$ of $n$ natural numbers and a number $m \in \mathbb{N}$.

**Decide:** Does a subset of the numbers of $S$ add up to exactly $m$?



#### Problem: BinPacking

**Given:** A set $S$ of $n$ objects with size $s_1, s_2, ..., s_n \in \mathbb{Q}$, where $0 < s_i \leq q$, and a number $k \in \mathbb{N}$.

**Decide:** Do the objects fit into $k$ bins of unit capacity.



