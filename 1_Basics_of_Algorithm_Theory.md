# Basics of Algorithm Theory



### Asymptotic Notation

| Notation Notation | Definition                                                   |
| ----------------- | ------------------------------------------------------------ |
| Big-O             | $O(f) := \{g:\mathbb{N} \rightarrow \mathbb{R}⁺ \hspace{0.5cm} | \hspace{0.5cm} \exists c \in \mathbb{R^+} \hspace{0.5cm} \exists n_0 \in \mathbb{N} \hspace{0.5cm} \forall n \geq n_0 \hspace{0.5cm} g(n) \leq c \cdot f(n)\}$ |
| Big-Omega         | $\Omega (f) := \{g: \mathbb{N} \rightarrow \mathbb{R⁺} \hspace{0.5cm}| \hspace{0.5cm} \exists c \in \mathbb{R^+} \hspace{0.5cm} \exists n_0 \in \mathbb{N} \hspace{0.5cm} \forall n \geq n_0 \hspace{0.5cm} c \cdot f(n) \leq g(n)\}$ |
| Big-Theta         | $\theta (f) := \{g: \mathbb{N} \rightarrow \mathbb{R⁺} \hspace{0.5cm}  | \hspace{0.5cm} \exists c_1,c_2 \in \mathbb{R⁺} \hspace{0.5cm} \exists n_0 \in \mathbb{N} \hspace{0.5cm} \forall n \geq n_0 \hspace{0.5cm} c_1 \cdot f(n) \leq g(n) \leq c_2 \cdot f(n)\}$ |
| Small-Oh          | $\Omega (f) := \{g: \mathbb{N} \rightarrow \mathbb{R⁺} \hspace{0.5cm}| \hspace{0.5cm} \forall c \in \mathbb{R^+} \hspace{0.5cm} \exists n_0 \in \mathbb{N} \hspace{0.5cm} \forall n \geq n_0 \hspace{0.5cm} g(n) \leq c \cdot f(n)\}$ |
| Small-Omega       | $O(f) := \{g:\mathbb{N} \rightarrow \mathbb{R}⁺ \hspace{0.5cm}| \hspace{0.5cm} \forall c \in \mathbb{R^+} \hspace{0.5cm} \exists n_0 \in \mathbb{N} \hspace{0.5cm} \forall n \geq n_0 \hspace{0.5cm} g(n) \geq  c \cdot f(n)$ |

 