# Dynamic Programming
Dynamic Programming is mainly used to solve optimization and counting problems (i.e. a problem that wants you to "minimize this" or "maximize that" or "count the ways to do that"). 

Dynamic Programming tackles problem by identifying overlapping subproblems (if they aren't overlapping, [divide and conquer]() might be a better approach), and tackling them one by one, smallest first, using the answers to small problems to help figure out larger ones, until the whole lot of them is solved.

Look at the Bellman equation below, used for solving optimization problems - [Markov Decision Processes]().
$$U(s) = R(s) + γ \max_{a ⋹ A(s)} \sum P(s' | s, a) \cdot U(s')$$

$U(s)$: The utility (or optimal value) of being in state $s$.\
$R(s)$: The immediate reward for being in state $s$.

The function is recursive, it calls itself! we can make the equation iterative instead! - [Value Iteration]().

1. Initialize an array (or table) to store the value (or utility) of each state, starting with arbitrary values (often zeros).
2. Iterate through each state, updating its value based on the best possible action you can take from that state. This involves considering all possible next states, the rewards of moving to those states, and the future value of those states.
3. Repeat this process until the values converge or don't change significantly between iterations.

$$ V(s)←R(s) + γ \max_{a∈A(s)}​∑s′​P(s′∣s,a)⋅V(s′)$$

By applying DP (in this case, creating something like value iteration), we can solve problems that would otherwise be intractable due to their complexity or the sheer number of possibilities.

After you've formulated the problem recursively, you can look at the follow steps:

## Steps
a) Identify the subproblems
- What are all the different ways your recurisve algorithm can call itself?

b) Choose a memoization data structure
- Find a data structure that can store _every_ subproblems in step (a).

c) Identify dependencies
- Except for the base cases, every subproblem depends on other subproblems - which ones?

d) Find a good evaluation order
- Order the subproblems so that each one comes _after_ the subproblems it depends on.

e) Analyze space and running time
- The summed running times of all possible subproblems is the total running time.

f) Write down the algorithm.

There are more algorithms (specifically DP string algorithms) covered in [DP - String Processing](); there are even more DP algorithms integrated into other sections which I'll list at the end of this file.

---
# Top-Down Dynamic Programming

I'm going to look at a simple **fibonacci sequence** question to introduce the concept of top-down dynamic programming.

### Fibonacci Sequence

You should remember that a number at index $n$ in a fibonacci sequence is:
$$T(n) = T(n - 1) + T(n - 2)$$

Using the [recurrence relation]() though (which I won't [write out]()), we can see that the runtime is $\Theta (ϕ^n)$, the golden ratio to the power of $n$, although in this case it can be approximated as $\Theta (2^n)$.

Going back to our principles of dynamic programming, we can see that there seems to be a lot of overlapping subproblems; we can more easily show this with a recursion tree.
```
                                                        T(n)
                                                       /    \
                                                    T(n-1) T(n-2)
                                                     / \    / \
                                                T(n-2) ... ... ...
```
In this tree, we can clearly see $T(n-2)$ seems to be a repeated subproblem. In fact, it seems that $T(m)$ for all $0 < m < n$ will be repeated. Instead of calculating $T(n-2)$ each time, which'll have a runtime of $O (2^{(n-2)})$, we can store it in a cache (a memoization technique) once it's been calculated giving us a runtime of $O(1)$ for retrieving instead of needlessly calculating in $O (2^n)$ again.

```python
def fib(n, cache):
  if n in (0, 1):
    return n

  if n not in cache:
    return cache[n - 1] + cache[n - 2]

  cache[n]
```

`@lru_cache` technically would be more efficient though, so we can adjust the code again if we wanted:
```python
@lru_cache
def fib(n):
  if n in (0, 1):
    return n
  return fib(n-1) + fib(n-2)
```
Either way, both of these codes are completely more efficient with a runtime of $\Theta (n)$ - simply put, we're populating a cache of size $O(n)$ which will take time $O(n)$, and then returning $cache[n]$ in $O(1)$ time.

Also remember that we don't need to store all the intermediate results in dynamic programming algorithms. For example, in the fibonacci sequence example we only need to keep track of the last two elements cached - this could end up saving us some space.

Other (top-down) DP problems are covered throughout the guide, such as [edit distance]().

# Bottom-Up Dynamic Programming
Bottom-Up Dynamic Programming is quite similar, although different. I'll use the coin changes problem(s) to introduce the idea.

### Coin Change I

The classic coin change problem asks for the minimum number of coins needed to make up a certain amount of money given a list of coin denominations.

Let's quickly identify a situation where the [greedy]() approach will not work.

Imagine we have an array $A = [1, 15, 20]$, where our target amount is $30$. Our greedy solution will greedily choose $20$, skip over $15$, and then choose ten $1s$, even though in reality we can easily make $30$ by using two $15$_s_. 

Let's follow the steps outlined from before:

**a) Identify the subproblems**
- The subproblems involve finding the minimum number of coins required to make up every amount less than or equal to the target amount. For a target amount $n$, the recursive calls vary based on the remaining amount after using each coin.

**b) Choose a memoization data structure**
- A 1D array can be used for memoization, where the index represents the remaining amount and the value stores the minimum number of coins needed to make up that amount.

**c) Identify dependencies**
- A subproblem (finding the minimum number of coins for amount x) depends on the subproblems for x - coin for each coin in the denominations, plus one coin added (the current coin being considered).

**d) Find a good evaluation order**
- We can start from the base case, which is an amount of 0 (0 coins needed), and move towards the target amount.

**e) Analyze space and running time**
- Space Complexity: $O(n)$, where n is the target amount. The memoization array stores the minimum number of coins needed for each amount up to n.
- Time Complexity: The running time is $\Theta (mn)$, where m is the number of coins. For each amount, we iterate through all coins to find the minimum number of coins needed.

**f) Write the algorithm**
```python
def coinChange(coins, amount):
    dp = [0] + ([float('inf')] * amount)
    for i in range(1, amount + 1):
        for coin in coins:
            if coin <= i:
                dp[i] = min(dp[i], dp[i - coin] + 1)

    if dp[-1] == float('inf'):
        return -1
    return dp[-1]
```

# 2D Dynamic Programming

In CS3510 we started learning 2D DP with a variation of the unique paths problem, so that's what I'll use to introduce 2D Dynamic Programming here - we can follow the same steps I outlined in the intro.

### Unique Paths II

You are given an m x n integer array grid. There is a robot initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

An obstacle and space are marked as 1 or 0 respectively in grid. A path that the robot takes cannot include any square that is an obstacle.

Return the number of possible unique paths that the robot can take to reach the bottom-right corner.

First, let's take a look at the [complete search]() solution.

In the [backtracking]() or complete search approach, you explore every possible paths from the start to the destination, moving only down or right at any point, while avoiding squares that contain obstacles, therefore exhausting every possible paths. 

However, this results in a branching factor of $2$, where the maximum depth of the recursion could potentially be $m + n$, therefore the runtime of this solution would be $\Theta (2^{n + m})$.

Let's get a quicker solution using 2D DP.

**a) Identify the subproblems**
  - Imagine we have a 3 x 3 grid (visualize it in your head buddy). We know logically we can only get to the middle square (at grid[1][1]) 2 ways - from the square above and from the square from the left. In fact, the number of ways to get to any square is dependent on the number of ways to get to the square to its left and above it.

**b) Choose a memoization data structure**
- This is pretty easily given to us, we have a grid, and we want to store the number of unique paths to each space on the grid, so we can use a 2D array (to represent the grid).

**c) Identify dependencies**
  - The number of unique paths to cell (i, j) depends on the sum of unique paths to the cell above it (i-1, j) and to the cell left of it (i, j-1), assuming those cells are not obstacles.

**d) Find a good evaluation order**
- Start from the base case (0, 0), and move towards the target cell (m-1, n-1). The exact evaluation order doesn't matter, we just need to ensure all cells up to row i and column j are evaluated before cell (i, j)

**e) Analyze space and running time**
- Space Complexity: $O(mn)$, where m is the number of rows and n is the number of columns in the grid. The memoization structure needs to store the unique path count for each cell.
- Time Complexity: $\Theta (mn)$, as we need to compute the unique paths for each cell in the grid exactly once.

**f) Write down the algorithm.**
```python
def uniquePathsWithObstacles(obstacleGrid):
    m, n = len(obstacleGrid), len(obstacleGrid[0])
    if obstacleGrid[0][0] == 1 or obstacleGrid[m-1][n-1] == 1:
        return 0

    # Memoization table initialized with -1 indicating uncomputed paths
    memo = [[-1 for _ in range(n)] for _ in range(m)]

    def dfs(i, j):
        # If out of bounds or on an obstacle, return 0
        if i >= m or j >= n or obstacleGrid[i][j] == 1:
            return 0
        # If at destination, return 1
        if i == m-1 and j == n-1:
            return 1
        # If already computed, return the memoized value
        if memo[i][j] != -1:
            return memo[i][j]
        # Recursively compute the number of paths from the current cell
        memo[i][j] = dfs(i+1, j) + dfs(i, j+1)
        return memo[i][j]

    return dfs(0, 0)
```

Without the obstacles this problem turns into an [inclusion-exclusion]() problem with a much faster solution.

## Matrix Chain Multiplication

Let's look at matrix chain multiplication, another algorithm covered in CS3510, which also happens to be a 2D DP problem by design.

Say we want to multiply three matrices $X$, $Y$ , and $Z$. We could do it like $(XY)Z$ or like $X(YZ)$. 

Which way we do the multiplication doesn’t affect the final outcome but it can affect the running time
to compute it. 

For example, say $X$ is 100 × 20, $Y$ is 20 × 100, and $Z$ is 100 × 20. So, the end result will be a 100 × 20 matrix. If we multiply using the usual algorithm, then to multiply an ℓ× m matrix by an m × n matrix takes time O(ℓmn). 

So in this case, which is better, doing $(XY)Z$ or $X(YZ)$?

Doing $(XY)Z$ will take $100 · 20 · 100 + 100 · 100 · 20 = 4 · 105$ operations.\
Doing $X(YZ)$ will take $20 · 100 · 20 + 100 · 20 · 20 = 8 · 104$ operations. So $X(YZ)$ is more efficient. But what if we have $n$ matrices?

Now we have a DP problem we can solve!

The Matrix Product Parenthesization problem is as follows: 
Suppose we need to multiply a series of matrices: $A_1$ × $A_2$ × $A_3$ $×. . .×$ $A_n$. Given the dimensions of these matrices $(m_0, m_1), . . . ,(mn−1, mn)$, what is the best way to parenthesize them, assuming for simplicity that standard matrix multiplication is to be used (to multiple matrices $a × b$ and $b × c$ we need $O(abc)$ time)?

a) Identify the subproblems
- What are all the different ways your recurisve algorithm can call itself?

b) Choose a memoization data structure
- Find a data structure that can store _every_ subproblems in step (a).

c) Identify dependencies
- Except for the base cases, every subproblem depends on other subproblems - which ones?

d) Find a good evaluation order
- Order the subproblems so that each one comes _after_ the subproblems it depends on.

e) Analyze space and running time
- The summed running times of all possible subproblems is the total running time.

f) Write down the algorithm.

## Longest Increasing Subsequence

Last (for the 2D DP section) we'll take a look at longest increasing subsequence, which is another pretty popular DP problem.

a) Identify the subproblems
- What are all the different ways your recurisve algorithm can call itself?

b) Choose a memoization data structure
- Find a data structure that can store _every_ subproblems in step (a).

c) Identify dependencies
- Except for the base cases, every subproblem depends on other subproblems - which ones?

d) Find a good evaluation order
- Order the subproblems so that each one comes _after_ the subproblems it depends on.

e) Analyze space and running time
- The summed running times of all possible subproblems is the total running time.

f) Write down the algorithm.

# Knapsacks

## 0/1 Knapsack & Bounded Knapsack

## Unbounded Knapsack

---

## All DP Problems
**a)** [Fibonacci Sequence]()\
**b)** [Coin Change II]()\
**c)** [Unique Paths II]()\
**d)** [Matrix Chain Multiplication]()\
**e)** [Longest Increasing Subsequence]()\
**f)** [String Alignment]()\
**g)** [Edit Distance]()\
**h)** [Largest Common Subsequence]()\
**i)** [Floyd Warshall]()
