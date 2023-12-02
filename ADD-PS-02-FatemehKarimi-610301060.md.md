# AAD Exercises

- **Answer Set**: No. 02
- **Full Name**: Fatemeh Karimi Barikarasfi
- **Student Number**: 610301060

## 1.

> Let's consider a special case of Quantified 3-SAT in which the underlying Boolean formula has no negated variables. Specifically, let $\phi(x_1, \dots , x_n) = C_1 \land C_2 \land \dots \land C_k$, where each $C_i$ is disjunction of three terms. We say $\phi$ is monotone if each term in each clause consists of a nonnegated variable-- that is, each term is equal to $x_i$, for some $i$, rather than $\neg x_i$.  
> We define Monotone QSAT to be the decision problem
> $$\exists x_1 \forall x_2 \dots \exists x_{n-2}\forall x_{n-1}\exists x_n \phi(x_1, \dots , x_n)$$  
> where the formula $\phi$ is monotone.  
> Do one of the following two things: (a) prove that Monotone QSAT is
> PSPACE-complete; or (b) give an algorithm to solve arbitrary instances of
> Monotone QSAT that runs in time polynomial in $n$. (Note that in (b), the
> goal is polynomial time, not just polynomial space.)

**Solution**

This problem can be cosidered as a game between two players. Player 1 controls odd-indexed variables while Player 2 controls even-indexed variables. We want to know that can Player 1 force a win? (If at the end $\phi = True$, Player 1 will be win, otherwise Player 2 will be win)  
Player 1 can force a win if and only if each clause $C_i$ contains a variable of odd-indexed variables. If this is the case, Player 1 can win by setting all odd-indexed variables to $True$, If this is not the case, then some clause $C_i$ has no varible from odd-indexed variables. Player 2 can then win by setting all even-indexed variables to $False$. This will cause the clause $C_i$ to evaluate the $\phi = False$, and so, Player 2 win.

## 2.

> Consider the following word game, which we’ll call Geography. You have
> a set of names of places, like the capital cities of all the countries in the world. The first player begins the game by naming the capital city c of the country the players are in; the second player must then choose a city
> c' that starts with the letter on which c ends; and the game continues in this way, with each player alternately choosing a city that starts with the letter on which the previous one ended. The player who loses is the first
> one who cannot choose a city that hasn’t been named earlier in the game. For example, a game played in Hungary would start with “Budapest,”
> and then it could continue (for example), “Tokyo, Ottawa, Ankara, Amsterdam, Moscow, Washington, Nairobi.”
> This game is a good test of geographical knowledge, of course, but even with a list of the world’s capitals sitting in front of you, it’s also a major strategic challenge. Which word should you pick next, to try forcing your opponent into a situation where they’ll be the one who’s ultimately
> stuck without a move?
> To highlight the strategic aspect, we define the following abstract
> version of the game, which we call Geography on a Graph. Here, we have
> a directed graph $G = (V, E)$, and a designated start node $s ∈ V$. Players alternate turns starting from $s$; each player must, if possible, follow an edge out of the current node to a node that hasn’t been visited before. The
> player who loses is the first one who cannot move to a node that hasn’t been visited earlier in the game. (There is a direct analogy to Geography, with nodes corresponding to words.) In other words, a player loses if the game is currently at node $v$, and for edges of the form $(v, w)$, the node $w$ has already been visited.  
> Prove that it is PSPACE-complete to decide whether the first player
> can force a win in Geography on a Graph.

**Solution**  
Geography on a Graph is in PSPACE. We can recursively try all possibilities.  
To show that Geography on a Graph is in PSPACE-complement, we show Q-SAT $\leqslant_p$ Geography on a Graph. We contruct directed graph $G$ such that, for each variable $x_i$, we create a diamond consisting of vertices labeled $a_i, x_i, \neg x_i, b_i$, with directed edges $(a_i, x_i), (a_i, \neg x_i), (x_i, b_i),$ and $(\neg x_i, b_i)$. There is an edge $(b_i, a_{i+1})$ for each $i < n$. Add node $c$, and $c_1, \dots , c_k$, represent the k clauses of $\phi$. Add directed edge $(c_i, x_j)$, if $C_i$ consists of $x_j$, and add directed edge $(c_i, \neg x_j)$, if $C_i$ consists of $\neg x_j$. Add edge $(b_n, c)$. The starting node is $a_1$. Thus, Player 1 always chooses one of $x_i$, or $\neg x_i$, for each odd value of $i$, and Player 2 choose a side of the ith diamond for each even value of $i$. At the end of this process, the current node is $b_n$, and it is Player 2's turn to move.  
The next two moves of the game will now determine which Player wins. These edges model the following contruction: Player 2 chooses a clause of $\phi$, and Player 1 must then choose a term in this clause that has not be chosen. If such moves exist, then Player 1 wins, otherwise Player 2 wins.  
_$\exists x_1 \forall x_2 \dots \exists x_{n-2}\forall x*{n-1}\exists x_n \phi(x_1, \dots , x_n)$ will be $True$ instance if and only if Player 1 has a forced win from $a_1$ in the Geography on a Graph instance on $G$*.

## 3.

> Give a polynomial-time algorithm to decide whether a player has a forced
> win in Geography on a Graph, in the special case when the underlying
> graph $G$ has no directed cycles (in other words, when $G$ is a DAG).

**Solution**

We label the vertices $v_1, v_2, \dots , v_n$ according to a topological ordering. We now define $W_in(j)$ to be equal to $1$ if the player whose turn it is to move force a win starting at node $v_j$, and define $W_in(j)$ to be equal to $0$ if the other can force a win starting at node $v_j$.  
We can initialize $W_in(j) = 0$ for every node $v_j$ with no out-going edges. So, $W_in(n) = 0$. We now use dynamic programming to compute the values of $W_in(j)$ in descending order of $j$. When we get to a particular value of $j$, we may assume that we have already computed $W_in(k)$ for all $k > j$. Now, a player starting from $v_j$ can force a win if and only if there is some node $v_k$ for which $(v_j, v_k)$ is an edge and a player starting from $v_k$ has a forced loss. Thus, $W_in(j) = 1$ if and only if $W_in(k) = 0$ for some $k$ with $(v_j, v_k)$ an edge; and otherwise $W_in(j) = 0$. Thus, the problem is solved in polynomial time.
