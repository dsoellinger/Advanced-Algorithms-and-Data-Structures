# Basics of Algorithm Theory

## Asymptotic Notation

| Notation Notation | Definition                                                   |
| ----------------- | ------------------------------------------------------------ |
| Big-O             | $O(f) := \{g:\mathbb{N} \rightarrow \mathbb{R}⁺ \hspace{0.5cm} | \hspace{0.5cm} \exists c \in \mathbb{R^+} \hspace{0.5cm} \exists n_0 \in \mathbb{N} \hspace{0.5cm} \forall n \geq n_0 \hspace{0.5cm} g(n) \leq c \cdot f(n)\}$ |
| Big-Omega         | $\Omega (f) := \{g: \mathbb{N} \rightarrow \mathbb{R⁺} \hspace{0.5cm}| \hspace{0.5cm} \exists c \in \mathbb{R^+} \hspace{0.5cm} \exists n_0 \in \mathbb{N} \hspace{0.5cm} \forall n \geq n_0 \hspace{0.5cm} c \cdot f(n) \leq g(n)\}$ |
| Big-Theta         | $\theta (f) := \{g: \mathbb{N} \rightarrow \mathbb{R⁺} \hspace{0.5cm}  | \hspace{0.5cm} \exists c_1,c_2 \in \mathbb{R⁺} \hspace{0.5cm} \exists n_0 \in \mathbb{N} \hspace{0.5cm} \forall n \geq n_0 \hspace{0.5cm} c_1 \cdot f(n) \leq g(n) \leq c_2 \cdot f(n)\}$ |
| Small-Oh          | $\Omega (f) := \{g: \mathbb{N} \rightarrow \mathbb{R⁺} \hspace{0.5cm}| \hspace{0.5cm} \forall c \in \mathbb{R^+} \hspace{0.5cm} \exists n_0 \in \mathbb{N} \hspace{0.5cm} \forall n \geq n_0 \hspace{0.5cm} g(n) \leq c \cdot f(n)\}$ |
| Small-Omega       | $O(f) := \{g:\mathbb{N} \rightarrow \mathbb{R}⁺ \hspace{0.5cm}| \hspace{0.5cm} \forall c \in \mathbb{R^+} \hspace{0.5cm} \exists n_0 \in \mathbb{N} \hspace{0.5cm} \forall n \geq n_0 \hspace{0.5cm} g(n) \geq  c \cdot f(n)$ |

 

## Sample growth rates

| Function                           | Asymptotic complexity            | Proof                                                        |
| ---------------------------------- | -------------------------------- | ------------------------------------------------------------ |
| $\theta(log_\alpha(n))$            | $\theta(log_\beta (n))$          | $log_\alpha( n) = \frac{1}{\log_\beta(\alpha)} \cdot log_\beta(n)$   for all  $\alpha,\beta \in \mathbb{R⁺} \setminus \{1\}$ and all $n \in N$ |
| $\theta(log(n!))$                  | $\theta(n \cdot log(n))$         | Using Stirling's formula: $n! \approx \sqrt{2\pi n} (\frac{n}{e})^n$ |
| $\theta(\sum_{K=1}^n \frac{1}{n})$ | $\theta(ln(n)) = \theta(log(n))$ | $lim_{n \rightarrow +\infty}(H_n - ln(n)) = \gamma$          |



#### Ackermann Function

The Ackermann function grows extremely rapidly. For example: $A(4,3) = 2^{2^{65536}}-3$  

$A(m,n) := \begin{cases} n+1 \hspace{5cm} \text{if } m=0 \\ A(m-1,1) \hspace{3.5cm}  \text{if } m > 0 \text{ and } n=0\\ A(m-1,A(m,n-1)) \hspace{1cm} \text{if } m > 0 \text{ and } n> 0\end{cases}$ 

#### Inverse Ackermann Function

The inverse Ackermann function grows extremely slowly. It is at most four for any input of practical relevance.

$\alpha(n):= \text{min}\{m \in \mathbb{N}: A(m,m) \geq n \}$

**Real-world occurence of $O(\alpha)$:** Combinatorial complexity of lower envelope of n
line segments.

#### Log-star (Iterated logarithm)

Log-star also grows very slowly. It's less than 6 for any input of practical relevance

$log^*(n):= \begin{cases} 0 \hspace{4.6cm}  \text{if } n \leq 1\\ 1+ log^*(log(n)) \hspace{1.2cm} \text{if } n > 1\end{cases}$ 

**Example:** 

$log^*(2^{16})=1+log^*(2^{2^2})=2+log^*(2^2)=3+log^*(2^1) = 4 + log^*(1)=4$

However, we have $\alpha \in o(log^*)$



#### Growth rate: $k+\epsilon$

A term of the form $k+\epsilon$ means that there is some additive positive constant $\epsilon \in \mathbb{R⁺}$ that needs to be added to $k$. The constant $\epsilon$ may be regarded as arbitary small but it will never equal zero.

Suppose that some algorithm runs in $2^c n^{1+1/c}$ time, where $c \in \mathbb{R^+}$ is a user-chosen constant:

- For $c:=2$, the complexity term equals $4n^{3/2}$, which is in $O(n^{3/2})$

- For $c:=9$, the complexity term equals $2^9n^{10/9}$, which is in $O(n^{10/9})$

- It is easy to see that $1+1/c$ approaches 1 as $c$ approaches infinity. However, $c$ cannot be set to infinity (or made arbitarily large) since then the $2^c$ term would dominate the complexity of our algorithm.

- Hence, the best possible way to express this complexity is to write $O(n^{1+\epsilon})$








