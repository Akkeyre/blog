---
title: "Combinatorial Properties of the Permanent"
date: 2026-04-13
draft: false
tags: ["math", "combinatorics", "linear algebra"]
math: true
description: "The permanent of a matrix is the forgotten twin of the determinant — identical in definition, wildly different in behaviour. We explore its combinatorial meaning, key identities, and hardness."
---

## Introduction

The **permanent** of a matrix is one of those objects that looks almost identical to something familiar — the determinant — yet behaves in a completely different way. Where the determinant carries a sign on each permutation, the permanent does not. This small difference makes the permanent combinatorially rich and computationally hard.

In this post we develop the theory of the permanent from scratch: its definition, its combinatorial interpretation, key identities and inequalities, its relationship to counting problems, and why computing it is fundamentally harder than computing the determinant.

---

## Definition

Let $A = (a_{ij})$ be an $n \times n$ matrix. The **permanent** of $A$ is defined as

$$\operatorname{perm}(A) = \sum_{\sigma \in S_n} \prod_{i=1}^{n} a_{i,\sigma(i)}$$

where the sum runs over all permutations $\sigma$ in the symmetric group $S_n$.

Compare this to the determinant:

$$\det(A) = \sum_{\sigma \in S_n} \operatorname{sgn}(\sigma) \prod_{i=1}^{n} a_{i,\sigma(i)}$$

The only difference is the factor $\operatorname{sgn}(\sigma) \in \{+1, -1\}$. In the permanent, every permutation contributes positively. This makes the permanent blind to cancellation — and that is precisely what makes it hard.

---

## Combinatorial Interpretation

### Perfect Matchings in Bipartite Graphs

The most important combinatorial interpretation of the permanent is in terms of **perfect matchings**.

Let $G = (U \cup V, E)$ be a bipartite graph with $|U| = |V| = n$. Define the **biadjacency matrix** $A$ by

$$a_{ij} = \begin{cases} 1 & \text{if } (u_i, v_j) \in E \\ 0 & \text{otherwise} \end{cases}$$

Then

$$\operatorname{perm}(A) = \text{number of perfect matchings in } G$$

**Proof.** Each term $\prod_{i=1}^{n} a_{i,\sigma(i)}$ in the permanent equals $1$ if and only if $(u_i, v_{\sigma(i)}) \in E$ for all $i$, i.e., $\sigma$ defines a perfect matching. Otherwise it equals $0$. Summing over all $\sigma$ counts the matchings. $\square$

### Systems of Distinct Representatives

A related interpretation comes from set systems. Given sets $S_1, S_2, \ldots, S_n \subseteq [n]$, a **system of distinct representatives (SDR)** is a choice of distinct elements $x_1 \in S_1, x_2 \in S_2, \ldots, x_n \in S_n$.

If we form the $0$-$1$ matrix $A$ with $a_{ij} = 1$ iff $j \in S_i$, then $\operatorname{perm}(A)$ counts the number of SDRs for the family $(S_1, \ldots, S_n)$.

### Counting Permutations with Forbidden Positions

The **problème des ménages** and the classical **derangement** problem are both special cases of permanent computation.

For derangements: let $J$ be the $n \times n$ all-ones matrix and $I$ the identity. Then

$$D_n = \operatorname{perm}(J - I)$$

counts the number of permutations of $[n]$ with no fixed points.

---

## Key Properties and Identities

### Multilinearity

The permanent is multilinear in the rows (and columns) of $A$: if we scale row $i$ by $\lambda$, the permanent scales by $\lambda$. If row $i$ is a sum $r_i + r_i'$, then

$$\operatorname{perm}(A) = \operatorname{perm}(A\text{ with row } i = r_i) + \operatorname{perm}(A\text{ with row } i = r_i')$$

### Expansion by Row

Just like the Laplace expansion for determinants:

$$\operatorname{perm}(A) = \sum_{j=1}^{n} a_{ij} \cdot \operatorname{perm}(A_{ij})$$

where $A_{ij}$ is the $(n-1) \times (n-1)$ matrix obtained by deleting row $i$ and column $j$. Note: there is **no** $(-1)^{i+j}$ sign factor.

### Symmetry

Unlike the determinant, the permanent is symmetric under permutation of rows **or** columns independently (since both just reindex the sum). In particular, $\operatorname{perm}(A) = \operatorname{perm}(A^T)$.

### Scaling

$$\operatorname{perm}(DAE) = \left(\prod_{i} d_i\right)\left(\prod_j e_j\right) \operatorname{perm}(A)$$

for diagonal matrices $D = \operatorname{diag}(d_1, \ldots, d_n)$ and $E = \operatorname{diag}(e_1, \ldots, e_n)$.

---

## The Van der Waerden Conjecture

One of the most celebrated results about permanents is the resolution of the **Van der Waerden conjecture**, proved independently by Egorychev and Falikman in 1981.

A matrix $A$ is **doubly stochastic** if all entries are non-negative, all row sums equal $1$, and all column sums equal $1$.

**Theorem (Egorychev–Falikman, 1981).** If $A$ is an $n \times n$ doubly stochastic matrix, then

$$\operatorname{perm}(A) \geq \frac{n!}{n^n}$$

with equality if and only if $A = J_n = \frac{1}{n} \mathbf{1}\mathbf{1}^T$, the matrix with all entries $\frac{1}{n}$.

This bound, conjectured by van der Waerden in 1926, remained open for over 50 years. The value $\frac{n!}{n^n}$ comes from

$$\operatorname{perm}(J_n) = n! \cdot \left(\frac{1}{n}\right)^n = \frac{n!}{n^n}$$

since each of the $n!$ permutations contributes $\frac{1}{n^n}$.

### Consequence for Counting

By Birkhoff's theorem, every doubly stochastic matrix is a convex combination of permutation matrices. The Van der Waerden bound gives a lower bound on perfect matchings in regular bipartite graphs: a $d$-regular bipartite graph on $n + n$ vertices has biadjacency matrix $\frac{1}{d} A$ doubly stochastic (after scaling), giving at least $\frac{n! \cdot d^n}{n^n}$ perfect matchings.

---

## Ryser's Formula

The naive computation of $\operatorname{perm}(A)$ by summing over all $n!$ permutations is clearly infeasible for large $n$. The best known exact formula is due to **Ryser (1963)**:

$$\operatorname{perm}(A) = (-1)^n \sum_{S \subseteq [n]} (-1)^{|S|} \prod_{i=1}^{n} \sum_{j \in S} a_{ij}$$

This runs in $O(2^n \cdot n)$ time — exponential, but far better than $n!$.

**Derivation sketch.** By inclusion-exclusion on which columns are covered:

$$\operatorname{perm}(A) = \sum_{\sigma \in S_n} \prod_i a_{i,\sigma(i)}$$

Expanding $\prod_i \sum_j a_{ij} x_j$ and extracting the coefficient of $x_1 x_2 \cdots x_n$ via inclusion-exclusion yields Ryser's formula.

---

## Computational Complexity: #P-Hardness

One of the deepest facts about the permanent is its computational hardness.

**Theorem (Valiant, 1979).** Computing the permanent of a $0$-$1$ matrix is **#P-complete**.

This is a landmark result. The class **#P** consists of counting problems associated with NP decision problems — for example, counting the number of satisfying assignments of a Boolean formula. Valiant showed that counting perfect matchings in bipartite graphs is complete for this class.

### Why Doesn't the Determinant Suffer This Fate?

The determinant can be computed in $O(n^3)$ time via Gaussian elimination. The key difference is **cancellation**: the $\pm$ signs in the determinant allow massive cancellation that reduces the computation to solving a linear system. The permanent has no such cancellation — every term is positive, so no algebraic shortcut is known.

Formally, this connects to the conjecture $\mathbf{P} \neq \mathbf{\#P}$, which would imply no polynomial-time algorithm for the permanent exists. This is believed to be true but remains unproven, and is in some sense a stronger statement than $\mathbf{P} \neq \mathbf{NP}$.

### Approximate Counting

Since exact computation is hard, a natural question is whether we can **approximate** $\operatorname{perm}(A)$ efficiently.

**Theorem (Jerrum–Sinclair–Vigoda, 2001).** There exists a fully polynomial randomized approximation scheme (FPRAS) for the permanent of a non-negative matrix: for any $\varepsilon > 0$, a $(1 \pm \varepsilon)$-approximation can be computed in time polynomial in $n$ and $1/\varepsilon$.

This uses a Markov chain Monte Carlo approach on the space of matchings, with the key technical ingredient being rapid mixing of the Markov chain.

---

## The Permanent and the Hafnian

For **symmetric** matrices, a related quantity is the **hafnian**:

$$\operatorname{haf}(A) = \frac{1}{2^n n!} \sum_{\sigma \in S_{2n}} \prod_{j=1}^{n} a_{\sigma(2j-1), \sigma(2j)}$$

The hafnian counts perfect matchings in **general** (non-bipartite) graphs, just as the permanent counts them in bipartite graphs. Both are #P-hard to compute.

---

## Problems

### Problem 1

**Let $J_n$ be the $n \times n$ all-ones matrix. Compute $\operatorname{perm}(J_n)$ and give a combinatorial interpretation.**

**Solution.**

By definition,

$$\operatorname{perm}(J_n) = \sum_{\sigma \in S_n} \prod_{i=1}^{n} (J_n)_{i,\sigma(i)} = \sum_{\sigma \in S_n} \prod_{i=1}^n 1 = \sum_{\sigma \in S_n} 1 = n!$$

**Combinatorial interpretation.** $J_n$ is the biadjacency matrix of the complete bipartite graph $K_{n,n}$, where every left vertex is connected to every right vertex. A perfect matching in $K_{n,n}$ is simply a bijection from $\{u_1, \ldots, u_n\}$ to $\{v_1, \ldots, v_n\}$, and there are exactly $n!$ such bijections.

Alternatively: $\operatorname{perm}(J_n)$ counts the number of SDRs for the family $S_1 = S_2 = \cdots = S_n = [n]$, which is the number of ways to pick $n$ distinct elements from $[n]$ in order — again $n!$.

---

### Problem 2

**Let $C_n$ be the $n \times n$ circulant matrix with $(C_n)_{ij} = 1$ if $j \equiv i$ or $j \equiv i+1 \pmod{n}$, and $0$ otherwise. That is, each row has exactly two $1$s in adjacent (cyclically) columns. Show that $\operatorname{perm}(C_n)$ counts the number of ways to place $n$ non-attacking rooks on the cycle graph $C_n \times C_n$, and compute $\operatorname{perm}(C_n)$ for small $n$.**

**Solution.**

**Combinatorial setup.** $C_n$ is the biadjacency matrix of a bipartite graph $G$ where left vertex $u_i$ is connected to right vertices $v_i$ and $v_{i+1 \bmod n}$. A perfect matching selects, for each $u_i$, exactly one of its two neighbours, such that no right vertex is chosen twice.

This is equivalent to placing $n$ non-attacking rooks on the $n \times n$ board where the allowed squares in row $i$ are columns $i$ and $i+1 \pmod{n}$ — exactly the support of $C_n$.

**Computation.** Each perfect matching corresponds to a choice function $f: [n] \to \{0, 1\}$ where $f(i) = 0$ means $u_i \to v_i$ and $f(i) = 1$ means $u_i \to v_{i+1}$. The matching is valid iff no two rows map to the same column.

Row $i$ maps to column $i + f(i) \pmod{n}$. Validity requires $i + f(i) \not\equiv j + f(j) \pmod{n}$ for $i \neq j$.

For small $n$:

- **$n = 2$:** $C_2 = \begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix}$. The permutations are $\sigma = \text{id}$ and $\sigma = (12)$, both valid. So $\operatorname{perm}(C_2) = 2$.

- **$n = 3$:** Each row has exactly $2$ choices. Total functions: $2^3 = 8$. We need to check which give distinct column assignments mod $3$.

  The column assigned by row $i$ is $i + f(i) \pmod 3$. Enumerating all 8 choices $(f(1), f(2), f(3)) \in \{0,1\}^3$:

  | $f$ | Columns | Valid? |
  |---|---|---|
  | $(0,0,0)$ | $1,2,3$ | ✓ |
  | $(1,0,0)$ | $2,2,3$ | ✗ |
  | $(0,1,0)$ | $1,3,3$ | ✗ |
  | $(0,0,1)$ | $1,2,1$ | ✗ |
  | $(1,1,0)$ | $2,3,3$ | ✗ |
  | $(1,0,1)$ | $2,2,1$ | ✗ |
  | $(0,1,1)$ | $1,3,1$ | ✗ |
  | $(1,1,1)$ | $2,3,1$ | ✓ |

  So $\operatorname{perm}(C_3) = 2$.

- **$n = 4$:** By a similar enumeration (or by Ryser's formula), $\operatorname{perm}(C_4) = 2$.

**Pattern.** For all $n \geq 2$, $\operatorname{perm}(C_n) = 2$.

**Proof.** A valid matching corresponds to an injective map $i \mapsto i + f(i) \pmod{n}$, i.e., a permutation $\sigma$ with $\sigma(i) \in \{i, i+1\}$ for all $i$. This means each element is either a fixed point or mapped forward by one step cyclically.

Suppose row $i$ is mapped forward: $f(i) = 1$, so $\sigma(i) = i+1$. Then row $i+1$ must be mapped to a column other than $i+1$, so $f(i+1) = 1$ as well. By induction, once one row maps forward, all subsequent rows must map forward cyclically. This forces $f \equiv 0$ (all fixed) or $f \equiv 1$ (all shifted). Both are valid and give the identity permutation and the cyclic shift $\sigma = (1\, 2\, \cdots\, n)$ respectively.

Therefore $\operatorname{perm}(C_n) = 2$ for all $n \geq 2$. $\square$

---

## Further Reading

- L. G. Valiant, *The Complexity of Computing the Permanent*, Theoretical Computer Science, 1979.
- G. P. Egorychev, *Proof of the van der Waerden conjecture for permanents*, 1981.
- M. Jerrum, A. Sinclair, E. Vigoda, *A polynomial-time approximation algorithm for the permanent of a matrix with nonnegative entries*, JACM, 2004.
- H. J. Ryser, *Combinatorial Mathematics*, MAA, 1963.
