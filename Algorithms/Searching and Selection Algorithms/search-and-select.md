# Binary Search

Calm little algorithm. Searches a sorted array by dividing it in half repeatedly.

1. If you can eliminate half of your remaining possibilties in each step, you can likely use binary search

2. If the problem is about finding a specific point in a continous range, like finding square root, you can likely use binary search (like [bisection method]())

Time Complexity: $O(\log n)$ | $\Omega (1)$


```python
  def binarysearch(nums: List[int], target: int) -> int:
      left, right = 0, len(nums) - 1
      while right >= left:
          mid = left + (right - left) // 2
          if nums[mid] == target: return mid
          elif nums[mid] < target: left = mid + 1
          else: right = mid - 1

      return -1
```
## Ternary Search

A search similar to binary search that divides the array into $3$ intervals instead of $2$.

Like binary search it has a runtime of $O (\log n)$.

It does technically have a runtime of $O(\log_{3}n)$, however it still has an upper asymptotic bound of $O(\log n)$.

# Exponential Search

An binary search implementation for unbounded arrays or sequences. It first finds an upper bound, and then performs binary search on that bound $[0 \dots i]$.

Time Complexity: $O(\log i)$ where $i$ is where the index would be of the target

  ```python
  def exponential_search(arr, x):
      if arr[0] == x:
          return 0
  
      i = 1
      while i < len(arr) and arr[i] <= x: i *= 2
  
      # Call binary search for the found range
      return binary_search(arr, i//2, min(i, len(arr)), x)
   ```

# Interpolation Search

An improved binary search that chooses where to split the array based on the distribution of values in the array.

It estimates the position of the target value using the formula: 

$pos = low + \left[ \frac{(x - arr[low]) \cdot (high - low)}{arr[high] - arr[low]} \right]$

where $x$ is the target value. 

Interpolation search is great for searching in a uniformly distributed sorted array. In these cases, it has a time complexity of $O(\log \log n)$, although it has a worst case time complexity of $O(n)$.

# Boyer - Moore Voting Algorithm

The Boyer-Moore Voting Algorithm (not the [pattern matching]() one), is often used for finding the **majority element**, or the element that appears more than $\frac {n}{2}$ times.

1. We iterate through an array, if the counter is zero, we set the current element as the candidate. 

2. If the next element matches the candidate, we increment the counter; otherwise, we decrement it

3. The majority element will be the final candidate

The intuitive solution for this (using a hashmap to keep count) has linear runtime but takes $O(n)$ space.

```python
result, count = 0, 0

for n in nums:
  if count == 0:
    result = n
  count += (1 if n == result else -1)

return result
```

Boyer-Moore can be expanded to find $k$ elements that appears more than $\frac {n}{k+1}$ times.

The code below is to find $2$ elements that appear more than $\frac {n}{3}$ times.

```python
def majorityElement(nums):
 if not nums:
     return []

 count1, count2, candidate1, candidate2 = 0, 0, 0, 1

 for n in nums:
     if n == candidate1:
         count1 += 1
     elif n == candidate2:
         count2 += 1
     elif count1 == 0:
         candidate1, count1 = n, 1
     elif count2 == 0:
         candidate2, count2 = n, 1
     else:
         count1, count2 = count1 - 1, count2 - 1

 return [n for n in (candidate1, candidate2)
             if nums.count(n) > len(nums) // 3]
```

# Median of Medians Search

This is a pivot selection method for [quick select](), but shouldn't be used for [quicksort]().

1. Split the array into subarrays of 5 elements each (the last subarray may have fewer than 5 elements).

2. Sort each subarray and find its median; the medians of all subarrays form a new array.

3. Apply the algorithm algorithm to the new array of medians to find the median of these medians.

4. The median of medians is our pivot.

This median searching algorithm is approximate, but optimal, as the median is guaranteed to be between the $30^{th}$ and $70^{th}$ percentiles.

It has a runtime of $\Theta (n)$, and uses auxillary space of $O (\log n)$.

# Quickselect

Quickselect is used to find the $k^{th}$ order statistic or the $k^{th}$ smallest element in an unordered list. 

Quickselect is just a variant of [quicksort]() where we recurse on one side rather than both.

It's more efficient than sorting the entire array when $n$ is large and $k$ is small. On average its runtime is $O(n)$, while in the worst cases it's $O(n^2)$. Using [median of medians]() brings it back down to $O(n)$.


### Heap-select

You can find the $k^{th}$ smallest by building a [heap](), removing $k$ elements, and returning the $k^{th}$ element removed.

It outperforms quickselect when $k$ is small, with a runtime of $\Theta (n + k \log n)$.

### Merge-select

I learned about a [divide-and-conquer]() algorithm based on [merge sort]() for finding the $i^{th}$ order statistic, but was not able to find much about it online. 
