# AAD Exercises

- **Answer Set**: No. 01
- **Full Name**: Fatemeh Karimi Barikarasfi
- **Student Number**: 610301060

## Book Problems

### 7.

> Since the 3-Dimensional Matching Problem is NP-complete, it is natural to expect that the corresponding 4-Dimensional Matching Problem is at least as hard. Let us define 4-Dimensional Matching as follows. Given sets $W$, $X$, $Y$, and $Z$, each of size $n$, and a collection $C$ of ordered 4-tuples of the form $(w_i, x_j, y_k, z_l)$, do there exist $n$ 4-tuples from $C$ so that no two have an element in common?  
> Prove that 4-Dimensional Matching is NP-complete.

**Solution**

4-Dimensional Matching is in NP, since given collection of 4-tuples $C$ can be verified in polynomial time that they form a 4-Dimensional Matching (i.e no two tuples share an element and each elment of $W$, $X$, $Y$, and $Z$ exists exactly in one of these 4-tuples)  
If 3-Dimensional Matching $\leqslant_p$ 4-Dimensional Matching be indicated, we can prove that 4-Dimensional Matching is a NP-Complete problem.  
Now, for the reduction. Given an instance of 3-Dimensional Matching with sets $X$, $Y$, and $Z$ each of size $n$, and a collection of 3-tuples, we costruct an instance of 4-Dimensional Matching as follows:

$$
\begin{aligned}
    W &= X
    \\
    X' &= Y
    \\
    Y' &= Z
    \\
    Z' &= \{a_1, \dots, a_n\} \;\text{(a set of n new elements)}
\end{aligned}
$$

For each $(x, y, z)$ in $C$, create a 4-tuple $(x, y, z, a_i)$ in $C'$ where $a_i$ is the next unused element of $Z'$.  
This reduction runs in polynomial time. We now show it is a correct reduction.
If the 3-Dimentional Matching has a matching $C$ size of n, then for each $(x, y, z)$ in $C$ we take the corresponding 4-Dimentional Matching tuple $(x, y, z, a_i)$ and these n 4-tuples form a 4-Dimensional Matching in the constructed instance.  
Conversely, if the constructed 4-Dimensional Matching instance has a matching $C'$ of size n, it must select one 4-tuple for each $z_i$. Each such 4-tuple corresponds directly to a 3-tuple in $C$, and no two can share an element in $X$, $Y$, and $Z$. Hence the corresponding 3-tuples form a 3-Dimensional Matching in the original instance.

### 16.

> Consider the problem of reasoning about the identity of a set from the size of its intersections with other sets. You are given a finite set $U$ of size $n$, and a collection $A_1,\dots , A_m$ of subsets of $U$. You are also given numbers $c_1,\dots , c_m$. The question is: Does there exist a set $X \subset U$ so that for each $i = 1, 2, \dots, m$, the cardinality of $X ∩ A_i$ is equal to $c_i$? We will call this an instance of the Intersection Inference Problem, with input $U, \{A_i\} ,$ and $\{c_i\}$.  
> Prove that Intersection Inference is NP-complete.

**Solution**

Intersection Inference problem is in NP. A solution X can be verified in polynomial time by computing $|X ∩ A_i|$ for each i and checking if it equals to $c_i$.
We now show that 3-Dimensional Matching $\leq_{P}$ Intersection Inference. An instance of 3-Dimensional Matching consists of three disjoint sets $X$, $Y$, and $Z$ each containing $n$ elements, and a set $T$ triples has one element from each of $X$, $Y$, and $Z$. The question is whether there exists a matching $M \subseteq T$ such that every element of $X \cup Y \cup Z$ occurs in exactly one triple in $M$.  
We will reduce this to an instance of Intersection Inference as follows:

- Let the base set $U = T$
- for each element $j \in X \cup Y \cup Z$, we create a set $A_i$ of these triples that contain $j$.
- $c_j = 1$

We then ask whether there is a set $M \subseteq T$ that hs an intersection of size $1$ with each set $A_j$  
Such sets are those collections of triples for which each element of $j \in X \cup Y \cup Z$ appears in exactly one.

### 17.

> You are given a directed graph $G = (V, E)$ with weights $w_e$ on its edges $e \in E$. The weights can be negative or positive. The Zero-Weight-Cycle Problem is to decide if there is a simple cycle in $G$ so that the sum of the edge weights on this cycle is exactly $0$.  
> Prove that this problem is NP-complete.

**Solution**

The Zero-Weight-Cycle problem is in NP. If we are given a candidate cycle, we can efficiently sum the edge weights along that cycle and verify in polynomial time whether the sum is 0.  
We know that Subset-Sum problem is NP-complete. We will show that Subset-Sum $\leq_{P}$ Zero-Weight-Cycle to prove that Zero-Weight-Cycle is NP-complete.  
In the Subset-Sum problem, we are given a set $S$ of integers and a target integer $T$. The goal is to determine whether some subset of $S$ sums exactly to $T$.  
Given an instance of Subset-Sum, we construct an instance of Zero-Weight-Cycle in which the graph has node $0, 1, \dots , n$ and an edge $(i, j)$ far all pairs $i < j$. The weight of edge $(i, j)$ is equal to $w_j$. Finally, there is an edge $(n, 0)$ of weight $-T$.  
We claim there is a zero-weight cycle in this graph if and only if the Subset Sum instance has a subset summing to $T$. If there is such a subset of $S$, then we define a cycle that starts at $0$, goes through the nodes whose indices are in the subset, and then return to $0$ on the edge $(n, 0)$. The weight of $-T$ on the edge $(n, 0)$ cancels the sum of other edge weights. Conversely, all cycles in the constructed graph must use the edge $(n, 0)$, and so if there is a zero-weight cycle, then the other edges must cancel $-T$.

### 22.

> Suppose that someone gives you a black-box algorithm $\mathcal{A}$ that takes an undirected graph $G = (V, E)$, and a number $k$, and behaves as follows.
>
> - If $G$ is not connected, it simply returns $G$ is not connected.
> - If $G$ is connected and has an independent set of size at least $k$, it returns “yes.”
> - If $G$ is connected and does not have an independent set of size at least $k$, it returns “no.”

> Suppose that the algorithm $\mathcal{A}$ runs in time polynomial in the size of $G$ and $k$. Show how, using calls to $\mathcal{A}$ , you could then solve the Independent Set Problem in polynomial time: Given an arbitrary undirected graph $G$, and a number $k$, does $G$ contain an independent set of size at least $k$?

**Solution**

First, we identify the connected components of the graph $G=(V, E)$ using a simple connectivity algorithm that it takes poly-time to run. In each connected component $C$, we call $\mathcal{A}$ with values $k = 1, \dots , n$; we let $k_c$ is the size of maximum independent set in the component $C$. Doing this takes polynomial time per component, and hence polynomial time overall. Since nodes in different components have no edges between them, we now know that the largest independent set in $G$ has size $\sum_{c} k_c$; thus, we simply compare this value to $k$.

### 24.

> Let $G = (V, E)$ be a bipartite graph; suppose its nodes are partitioned into sets $X$ and $Y$ so that each edge has one end in $X$ and the other in $Y$. We define an $(a, b)$-skeleton of $G$ to be a set of edges $E' \subseteq E$ so that at most $a$ nodes in $X$ are incident to an edge in $E'$ , and at least $b$ nodes in $Y$ are incident to an edge in $E'$.  
> Show that, given a bipartite graph $G$ and numbers $a$ and $b$, it is NP-complete to decide whether $G$ has an $(a, b)$-skeleton.

**Solution**
This problem is in NP. An $E'$ can be verified in polynomial time whether it is an $(a, b)$-skeleton.
We now show that Set-Cover problem which is NP-copmplete, can be reduces to this problem, and then NP-completeness of this problem will be proved. In the Set-Cover problem, we are given a universe $U$ of elements, a collection $S$ of subsets of $U$, and an integer $k$. The problem asks if there exists a subcollection $C ⊆ S$ of sets that covers all of $U$, with $|C| ≤ k$.  
We construct a bipartite graph $G = (X,Y,E)$ as follows:

- $X = S$ (the subsets)
- $Y = U$ (the elements)
- Add an edge $(x,y)$ between set $x ∈ X$ and element $y ∈ Y$ iff $y$ is contained in subset $x$  
  We set the parameters:
- $a = k$ (the size bound for the set cover)
- $b = |U|$ (number of elements to be covered)

Now, any set cover $C ⊆ S$ of size $≤ k$ corresponds directly to a $(k, |U|)$-skeleton in $G$, with the edges between $C$ and $U$. Conversely, any $(k, |U|)$-skeleton must cover all $|U|$ elements, and involves $≤ k$ subsets from $S$. So it corresponds to a valid set cover. Therefore, $G$ has a $(k, |U|)$-skeleton if and only if the original Set Cover instance has a solution of size $≤ k$.

### 25.

> For functions $g_1,\dots, g_l$, we define the function $\max(g_1,\dots, g_l)$ via $$[\max(g_1,\dots, g_l)](x) = \max(g_1(x),\dots, g_l(x)).$$
> Consider the following problem. You are given $n$ piecewise linear, continuous functions $f_1,\dots, f_n$ defined over the interval $[0, t]$ for some integer t. You are also given an integer $B$. You want to decide: Do there exist $k$ of the functions $f_{i_1} ,\dots, f_{i_k}$ so that $$ \int\_{0}^{t} [\max(f*{i*1} ,\dots, f\*{i_k})](x) dx \ge B? $$
> Prove that this problem is NP-complete.

**Solution**

The problem is in NP, since given a subset of functions, we can evaluate the integral and compare to $B$ in polynomial time to verify a solution. We now show that Set-Cover problem which is NP-copmplete, can be reduces to this problem, and then NP-completeness of this problem will be proved. In the Set-Cover problem, we are given a universe $U$ of $n$ elements, a collection $S$ of $m$ subsets of $U$, and an integer $k$. The problem asks if there exists a subcollection $C ⊆ S$ that covers all of $U$, with $|C| ≤ k$.  
We construct an instance of this problem as follows:

- Create $m$ continuous piecewise linear functions $f_1, \dots ,f_m$ defined over $[0,n]$.
- For each set $Si ∈ S$, construct function $f_i$ as:

  - $f_i(x) = 0$ for $x ∈ [0, |S_i|]$
  - $f_i(x) = 1$ for $x ∈ (|S_i|, n]$

- Set parameter $B = n - k$.

Therefore, there exists a size $k$ subset with integral $≥ B$ iff there is a size $k$ set cover in the original instance.

### 39.

> The Directed Disjoint Paths Problem is defined as follows. We are given a directed graph $G$ and $k$ pairs of nodes $(s_1, t_1), (s_2, t_2),\dots, (s_k, t_k)$. The problem is to decide whether there exist node-disjoint paths $P_1, P_2,\dots, P_k$ so that $P_i$ goes from $s_i$ to $t_i$.  
> Show that Directed Disjoint Paths is NP-complete.

**Solution**

Directed-Disjoint-Path is in NP, since a given k $k$ pairs of nodes can be verified in polynomial time whether these are a directed disjoint path.  
We now show that 3-SAT $\leq_{P}$ Directed-Disjoint-Path.  
We construct a directed graph $G$ and node pairs as follows:

Vertices:

- Add a vertex for each variable xi
- Add a vertex for each clause Cj
- Add source vertex s and sink vertex t

Edges:
For each variable $x_i$:

- Add edge $(s, x_i)$
- Add edge $(x_i, ¬x_i)$
- Add edge $(¬x_i, t)$

For each clause $C_j = (l_j ∨ m_j ∨ n_j)$

- Add edges $(l_j, C_j), (m_j, C_j), (n_j, C_j)$

Node pairs:

- For each variable $x_i$, add the pair $(s, t)$

This gives us a graph with $n$ disjoint $(s, t)$ pairs, one for each variable.

It can be shown that the graph $G$ has $n$ disjoint $(s, t)$ paths if and only if $φ$ is satisfiable. The key idea is that paths must go through either $x_i$ or $¬x_i$ representing a truth value assignment that satisfies the clauses.

Since the reduction runs in polynomial time, this shows that Directed-Disjoint-Path is NP-complete.

### 41.

> Given a directed graph $G$, a cycle cover is a set of node-disjoint cycles so that each node of $G$ belongs to a cycle. The Cycle-Cover Problem asks whether a given directed graph has a cycle cover.  
> (a) Show that the Cycle-Cover Problem can be solved in polynomial time. (Hint: Use Bipartite Matching.)
> (b) Suppose we require each cycle to have at most three edges. Show that determining whether a graph $G$ has such a cycle cover is NP-complete.

**Solution**

#### a.

The Cycle-Cover Problem can be solved in polynomial time using bipartite matching as follows:

- 1. Construct a bipartite graph $H$ with vertex sets $L = R = V(G)$.
- 2. Connect nodes $u∈L$ and $v∈R$ with an edge in $H$ if the directed graph $G$ has an edge $(u,v)$.
- 3. Find a maximum matching $M$ in the bipartite graph $H$.
- 4. If $M$ matches all nodes in $L$, then $G$ has a cycle cover consisting of the cycles defined by the matched edges. Otherwise there is no cycle cover.  
     Since bipartite matching can be solved in polynomial time $(O(V(G)E(G)))$, this approach solves the Cycle-Cover Problem in polynomial time.

#### b.

We show the restricted Cycle-Cover Problem with cycle lengths $≤ 3$ is NP-complete via a reduction from 3-SAT:

Given a 3-SAT formula with $n$ variables $and$ m clauses, construct a graph $G$:

- For each variable $x$, add vertices $x_T$ and $x_F$
- For each clause $c_j$, add vertex $c_j$
- Add edges $(x_T, c_j)$ and $(x_F, c_j)$ if $x$ or $¬x$ appears in $c_j$

The graph $G$ has a cycle-cover with cycles of length $≤ 3$ iff the formula is satisfiable.  
Cycle $(x_T, c_j, x_F)$ chooses $x=T$, cycle $(x_F, c_j, x_T)$ chooses $x=F$
All clauses must be covered, so truth assignment satisfies formula
Since a polynomial reduction exists from 3-SAT, the length-restricted Cycle-Cover Problem is NP-complete.

## Nondeterministic Algorithms

### 1.

> Prove that $2SAT ∈ P$.

**Solution**

$$
\begin{aligned}
  f = (x_1 \lor y_1) \land (x_2 \lor y_2) \land \dots \land (x_n \lor y_n)
\end{aligned}
$$

We know that $(x \,\lor\, y)$ and $(\neg x \implies y)$ and $(\neg y \implies x)$ are equal. Replace each term with it's implication equivalent formulas.

$$
\begin{aligned}
	f = [(\neg x_1 \implies y_1), (\neg y_1 \implies x_1)] \land \dots \land [(\neg x_n \implies y_n), (\neg y_n \implies x_n)]
\end{aligned}
$$

Create a directed graph with $2n$ vertices for all $x_i$ and $y_i$. For each term add a directed edge from $\neg x_i$ to $y_i$ and from $\neg y_1$ to $x_1$.  
Now $f$ is not satisfiable if both $\neg x_i$ and $x_i$ are in the same `Strongly Connected Component`, and we can solve an _SCC_ problem using **Kosaraju Algorithm** in $O(n.\log{n} + e)$.

### 2.

> Design a non-deterministic polynomial-time algorithm for each of the following problems:
>
> - a. Selecting the k-th smallest element in an array of n numbers.
> - b. Subset Sum Problem
> - c. Hamiltonian Cycle Problem
> - d. 3-Coloring Problem

**Solution**

#### a.

To determine the $k-th$ smallest element in an array of $n$ elements, first, we sort the array and then return $k-th$ element of sorted array.

```
NSort(A[1, ..., n])
  Array B[1, ..., n]  <-- {0}
  for i <-- 1 to n do
    j <-- choose({1, ..., n})
    if B[j] != 0 then
      failure()
    endif
      B[j] <-- A[j]
  end for
  for i <-- 1 to n-1 do
    if B[i] > B[i+1] then
      failure()
    end if
  end for
  return B
  success()
  end

NKElement(A[1, ..., n], k)
  B <-- NSort(A[1, ..., n])
  return B[k]
```

#### b.

Given a set of $n$ elements $S = \{S_1, \dots , S_n \}$ and a target integer $k$. We want to determine whether there exists a subset of $S$ in which sum of the elements equal to $k$. We solve this algorithm nondeterministically and recursively.

```
NSubsetSum(S, k)
  if k = 0 then
    return YES
    succes()
  if |S| = 0 then
    return NO
    failure()
  x <-- choose(S)
  NSubsetSum(S - {x}, k-x)
```

#### c.

Given a graph $G = (V, E)$, first, we create all the permutations of $V$, and then determine whether a permutation is a Hamiltonian Cycle.

```
NHamiltonianCycle(V, E)
  Array X[1, ..., |V|] <-- NULL
  for i <-- 1 to |V| do
    j <-- choose({1, ..., |V|})
    if X[j] != NULL then
      failure()
    end if
    X[j] <-- V[i]
  end for
  for i <-- 1 to |V| do
    if (X[i], X[i+1]) not in E then
      failure()
    end if
  end for
  if (X[n], X[1]) in E then
    success()
  else if
    failure()
```

#### d.

Given a graph $G = (V, E)$, first, we create all the permutations of 3 coloreng, and then determine whether a permutation is valid.

```
N3Coloring(G = (V, E))
  Array COLOR[1, ..., |V|] <-- Null
  for i <-- 1 to |V| do
    COLOR[i] <-- choose({black, white, red})
  end for

  for e=(u, v) in E do
    if COLOR[u] == COLOR[v] then
      failure()
    end if
  end for
  success()

```

### 3.

> Devise a non-deterministic polynomial-time algorithm to solve the decision version of the optimization 0/1−knapsack problem.

**Solution**

```
N01Knapsak(v[1, ..., n], w[1, ..., n], V, W)
  Array X[1, ..., n] <-- {0}
  for i <-- 1 to n do
    X[i] <-- choose({0, 1})
  end for

  weight <-- 0
  value <-- 0
  for i <-- 1 to n do
    if X[i] == 1 then
      weight <-- weight + w[i]
      value <-- value + v[i]
    end if
  end for

  if (weight <= W) and (value >= V) then
    success()
  else if
    failure()
  end if

```

### 4.

> Devise a non-deterministic polynomial-time algorithm to solve the decision version of the optimization min-vertex cover problem.

**Solution**

We do this nondeterministically and recursively.

```
NMinVertexCover(G=(V, E), k)
  if |E| == 0 then
    print YES
    success()
  if |E| != 0 and k == 0
    print NO
    failure()
  v <-- choose(V={v_1, v_2, ..., v_n})
  NMinVertexCover(G=(V-v, E-{e| e=(u, v) in E}), k-1)

```

## Reduction

### 5.

> . Consider the following chain of polynomial reductions:  
> $$ 3SAT \leq*{P} K-Clique \leq*{P} k−Vertex-Cover \leq\_{P} Subset-Sum $$
> Also, suppose that the following instance of 3SAT problem is given:
> $$ φ = (x2 ∨ x3 ∨ x4) ∧ (x1 ∨ x2 ∨ x4) ∧ (x2 ∨ x3 ∨ x5) ∧ (x1 ∨ x3 ∨ x5) $$
> a. Give a yes-instance assignment of variables for φ.  
> b. Give a no-instance assignment of variables for φ.  
> c. Employing the above chain of polynomial reductions, construct a corresponding instance for each of the above mentioned problems (starting from φ).

**Solution**

#### a.

$x_1 = x_2 = x_3 = x_4 = x_5 = False$

#### b.

$x_1 = x_2 = x_3 = x_4 = x_5 = True$
