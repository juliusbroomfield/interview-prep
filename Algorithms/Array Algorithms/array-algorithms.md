There aren't that many algorithms in this selection since most of the interesting ones are in the [sorting]() or [searching]() ones.

# Two Pointers 

This is one of the more important array techniques.

You initialize two pointers, one at the beginning of the array (representing the beginning of an interval), and one at the end (representing the end of that interval). Depending on some operation, you'd bring the pointers inward.

Typically used in reducing a $O(n^2)$ runtime to $O(n)$.

###$ N$- sum

In the original $2$- Sum problem, you're supposed to find two numbers that add up to a target. 
In $N$- sum you're supposed to find $N$ elements that add up to a target.

$N$- sum is a good example of using two pointers.

This can be quickly down by sorting the array (we'll look at $3$- sum in this case), iterating through and choosing each number as a candidate, and then using two pointers to narrow down the other two candidate numbers.

This is the same method for any $N$, except for $N = 2$ where sorting would give us a larger upper asymptotic bound.

A general time complexity formula for $N$- sum is $O(n ^ {(N - 1)})$; the space complexity will always be $O(1)$, although this might depend on the sorting algorithm used.


# Sliding Window

Sliding windows are used for creating subarrays or substrings over a particular range within a larger array or string.

It sliding a 'window' over an array, which can either be of fixed size or dynamic size (expands and contracts based on certain conditions). You would preform some operation or process on the elements within the window, adjusting its size and position to meet the problem's requirements

# Kadane's Algorithm

# Fisher-Yates Shuffle