There aren't that many algorithms in this section since most of the interesting ones are in the [sorting]() or [searching]() ones.

# Two Pointers 

This is one of the more important array techniques.

You initialize two pointers, one at the beginning of the array (representing the beginning of an interval), and one at the end (representing the end of that interval). Depending on some operation, you'd bring the pointers inward.

Typically used in reducing a $O(n^2)$ runtime to $O(n)$.

I'll go in-depth on this technique using the $N$- sum problem.

### $ N$- sum

In the original $2$- Sum problem, you're supposed to find two numbers that add up to a target. 
In $N$- sum you're supposed to find $N$ elements that add up to a target.

This can be quickly down by sorting the array (we'll look at $3$- sum in this case), iterating through and choosing each number as a candidate, and then using two pointers to narrow down the other two candidate numbers.

This is the same method for any $N$, except for $N = 2$ where sorting would give us a larger upper asymptotic bound.

A general time complexity formula for $N$- sum is $O(n ^ {(N - 1)})$; the space complexity will always be $O(1)$, although this might depend on the sorting algorithm used.


# Sliding Window

Sliding windows are used for creating subarrays or substrings over a particular range within a larger array or string.

It sliding a 'window' over an array, which can either be of fixed size or dynamic size (expands and contracts based on certain conditions). You would preform some operation or process on the elements within the window, adjusting its size and position to meet the problem's requirements

I'll go in-depth here on some more examples taken from [Competitive Programming 3]().

### Smallest Sub-array with Sum ≥ S
Problem: Find the minimum length of a sub-array whose sum is greater than or equal to a given value S.

Solution: Start with two pointers (beginning and end of the window) at the start of the array. Expand the window by moving the end pointer to the right, adding elements to the sum until it is ≥ S. Then, contract the window from the left by moving the beginning pointer to the right, subtracting elements from the sum, until the sum is < S. Update the minimum length throughout. Repeat until the end of the array is reached.

### Smallest Sub-array Covering All Values from 1 to K
Problem: Find the smallest sub-array size so that the sub-array contains all integers in the range [1..K].

Solution: Use two pointers to form a window and a frequency counter for numbers 1 to K. Expand the window by moving the end pointer to include more elements until all numbers from 1 to K are present. Then, contract the window from the beginning to remove excess numbers while checking if the condition still holds. Update the minimum length when all numbers are included.

### Maximum Sum of Sub-array of Size K
Problem: Find the maximum sum of any contiguous sub-array of size K.

Solution: Initialize the sum with the first K elements. Then slide the window across the array by adding the next element (entering the window) and subtracting the first element of the previous window (leaving the window). Keep track of the maximum sum encountered during the sliding.

### Minimum of Each Sub-array of Size K
Problem: Find the minimum of each possible sub-array of size K.

Solution: Use a deque (double-ended queue) to keep track of the indices of potential minimums in a sorted order (smallest at the front). As the window slides, remove indices from the front if they are out of the window's range and from the back if the new element is smaller. The front of the deque gives the minimum of the current window. This maintains an ascending order within the deque for each window, allowing constant time access to the minimum.

# Kadane's Algorithm

Kadane's algorithm is really simple, it's for finding the contiguous subarray with the largest sum.

Basically, we have a [sliding window]() sum (rolling sum), and if the current number is greater than that sum, then we set the sum to that greater number

This really only makes sense if there are negative numbers in the array.

The time complexity is $\Theta (n)$; the brute force involves checking every possible contiguous subarray and has a runtime of $\Theta (n ^ 3)$.

# Fisher-Yates Shuffle

The Fisher-Yates shuffle is used to create a random permutation of an array (or any sequence).

The algorithm iterates through the array from the last element to the first, swapping each element with another randomly selected element that comes before it (including itself). 

1. Start at the end of the array.
2. Select a random element from the section of the array that comes before the current element 
3. Swap the selected element with the current element.
4. Move one position to the left (towards the start of the array) and repeat the selection and swap process.
5. Continue until you reach the beginning of the array.

Fisher-Yates has a time complexity of  $\Theta (1)$ and space complexity of $O(1)$ and was included for giggles.
