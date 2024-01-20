# AAD Exercises

- **Answer Set**: No. 03
- **Full Name**: Fatemeh Karimi Barikarasfi
- **Student Number**: 610301060

## Randomized Algorithms

### 1.

Give a _Las Vegas Algorithm_ to solve _8-Queen_ problem.

**Solution**

Here is the Python code of LVNQueen:

```python
def LVNQueens(n):
    board = [[0 for i in range(n)] for j in range(n)]
    # chech that (row, col) is ok to place next queen or not
    def isSafe(row, col):
        # chech columns
        for i in range(row):
            if board[i][col] == 1:
                return False

        # check Diagonals
        i = row - 1
        j = col - 1
        while i>=0 and j>=0:
            if board[i][j] == 1:
                return False
            i = i - 1
            j = j - 1

        i = row - 1
        j = col + 1
        while i>=0 and j<n:
            if board[i][j] == 1:
                return False
            i = i - 1
            j = j + 1
        return True
    def solve(row):
        if row == n:
            return True

        # choose next column uniformly at random
        random_cols = list(range(n))
        random.shuffle(random_cols)
        for j in random_cols:
            if isSafe(row, j):
                board[row][j] = 1

                if solve(row+1):
                    return True

                board[row][j] = 0
        return False

    if solve(0):
        for i in range(n):
            print(board[i])
    else:
        print('No solution exists')

```

### 2.

Consider the _one-way communication protocol model_ discussed in the class. Let $Eq_n(x, y) : \{0, 1\}^n × \{0, 1\}^n \rarr \{0, 1\}$ be a function defined by $Eq_n(x, y)=1 \harr x=y$.

#### a.

Design a _two-sided error Monte Carlo_ one-way communication protocol that computes $Eq_n$
within $O(log2 n)$ communication complexity.

#### b.

Show that every one-sided error Monte Carlo one-way communication protocol computing
$Eq_n$ must have a communication complexity of at least $n$.

**Solution**

#### a.

Consider the following algorithm:

_Random-Equality(x, y)  
Step 1:  
1: $D_1$ chooses a prime _$p \in [2, n^2]$_ uniformly at random  
Step 2:  
2: $D_1$ computes $s = x\ mod\ p$ and sends $p$ and $s$ to $D_2$  
Step 3:  
3: $D_2$ computes $q=y\ mod\ p$  
if $q=s$ then $D_2$ outputs $1$ (accept)  
else if $q \neq s$ then $D_2$ outputs $0$ (reject)  
end if_

Since $p \le n^2$ we need at most $log_2(n^2)$ bits to send $p$, and $s < p$, to send $s$ we also need $log_2(n^2)$ bits. Thus the communication complexity for this algorithm is $4log_2(n) = O(log_2(n))$.  
If $x = y$ then $x\ mod\ p = y\ mod\ p$, so, Random-Equality(x, y) returns $1$ every time in this case.$(Prob(A(x, y) = 1) = 1)$.

If $x \neq y$ then the algorithm might return $1$ which is wrong. Since the maximum number of prime number $l \in [2, n^2]$ that cause $x\ mod\ p = y\ mod\ p$ since $x \neq y$ is $n - 1$. So the probability of this case is $Prob(A(x, y) = 1) \le \frac{n-1}{\frac{n^2}{Ln(n^2)}}$ (number of prime numbers in $[2, n^2]$ is $\frac{n^2}{Ln(n^2)}$). In this case, the probability that $A(x, y)$ returns $0$ is $Prob(A(x, y) = 0) = 1 - Prob(A(x, y) = 0) \ge 1 - \frac{n-1}{\frac{n^2}{Ln(n^2)}} \ge 1 - \frac{Ln(n^2)}{n}$. For every $n \ge 10$ the probability is greater than $\frac{1}{2}$

#### b.

For every $x \neq y$ an _One-sided-error MC_ algorithm must outpots $0$. $(\forall x \notin Eq_n, Prob(A(x, y) = 0) = 1)$. The argument for this is that for all $u,v \in \{0,1\}^n$, u \neq v$ implies $\overline {C_1}(u) \neq \overline {C_1}(v)$. Let $\overline {C_1}(u) = \overline {C_1}(v)$ for two different $u, v \in \{0, 1\}^n$. Then, assuming that $(C_1, C_2)$ computes $Eq_n$, we obtain the following contradiction:
$$ 0 = Eq_n(u, u) = \overline {C_2}(u, \overline {C_1}(u)) = \overline {C_2}(u, \overline {C_1}(v)) = Eq_n(u, v) = 1$$

### 3.

Give a _Monte Carlo randomized algorithm_ to check if $A × B = C$ for given three matrices $A, B, C$. Analyze your algorithm.

**Solution**
Consider Three matrix $A_{m×k}, B_{k×n}, C_{m×n}$. The following algorithm $A(A, B, C)$ computes the desired solution.

_A(A, B, C):  
Step 1:  
1: $D_1$ chooses uniformly a vector $r = (r_1, \dots , r_n)$ where each $r_i \in {1, 2}$  
Step 2:  
2: $D_1$ computes $m = ABr$ and sends $m$ and $r$ to $D_2$  
Step 3:  
3: $D_2$ computes $x = Cr$  
4: if $x = m$ then $D_2$ outputs $1$  
5: else $x \neq y$ then $D_2$ outputs $0$  
end if_

Now, we show that this is a _Monte Carlo_ algorithm.

1. $AB = C$. In this case the algorithm $A$ always outputs $1$.
   $$ AB = C \rarr ABr = Cr \rarr x = m \rarr Prob(AB = C) = 1$$

2. $AB \neq C$. In this case if $ABr \neq Cr$ then the algorithm returns $0$ which id true. Assume that $AB \neq C$, and $x = m$. We define $D = AB - C$. The matrix $D$ should have at least one element that is not equal to zero. Let it be $d_{ij}$ then:
   $$ Dr = (AB - C)r = Cr - ABr = x - m$$  
   Since $x = m$ then $Dr$ should be vector zero. so for row $i$ we have:
   $$
   \sum_{k=1}^{n} d_{ik}r_{k} = 0
   $$
   $$\sum_{k=1, j\neq j}^{n} d_{ik}r_{k} = -d_{ij}r_{j}$$
   $$
   \sum_{k=1, j\neq j}^{n} -\frac{d_{ik}}{d_{ij}}r_{k} = r_{j}
   $$
   This means that for every $r_i$ where $i \in{1, \dots ,n} - {j}$, there is only one $r_j$ where the equation above will be satisfied. We have selected $r_j$ form ${1, 2}$ so the probability that $r_j$ be that value is at most $\frac{1}{2}$. This means the algorithm will return the wrong answer with probability at most $\frac{1}{2}$.
