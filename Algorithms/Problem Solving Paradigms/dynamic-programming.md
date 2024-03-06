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

---
# Top-Down Dynamic Programming

I'm going to look at a simple **fibonacci sequence** question to introduce the concept of top-down dynamic programming, and then I'll move on to a more complicated question.

You should remember that a number at index $n$ in a fibonacci sequence is:
$$T(n) = T(n - 1) + T(n - 2)$$

Using the recurrence relation though (which I won't write out), we can see that the runtime is $\Theta (ϕ^n)$, the golden ratio to the power of $n$, although in this case it can be approximated as $\Theta (2^n)$.

Going back to our principles of dynamic programming, we can see that there seems to be a lot of overlapping subproblems; we can more easily show this with a recursion tree.
```
                                                        T(n)
                                                       /    \
                                                    T(n-1) T(n-2)
                                                     / \    / \
                                                T(n-2) ... ... ...
```
In this tree, we can clearly see $T(n-2)$ seems to be a repeated subproblem. In fact, it seems that $T(m)$ for all $0 < m < n$ will be repeated. Instead of calculating $T(n-2)$ each time, which'll have a runtime of $O (2^{(n-2)})$, we can store it in a cache once it's been calculated giving us a runtime of $O(1)$ for retrieving instead of needlessly calculating in $O (2^n)$ again.

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
Either way, both of these codes are completely more efficient with a runtime of $\Theta (n)$.

# Bottom-Up Dynamic Programming

## ... all coin changes ...

# 2D Dynamic Programming

## Matrix Chain Multiplication

## Unique Paths

## Longest Increasing Subsequence

# Knapsacks

## 0/1 Knapsack & Bounded Knapsack

## Unbounded Knapsack

{Floyd Warshall, Edit Distance, Largest Common Subsequence}


