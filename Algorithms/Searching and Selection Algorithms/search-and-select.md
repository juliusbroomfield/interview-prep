# Binary Search

Calm little algorithm. Searches a sorted array by dividing it in half repeatedly.

1. If you can eliminate half of your remaining possibilties in each step, you can likely use binary search

2. If the problem is about finding a specific point in a continous range, like finding square root, you can likely use binary search (see [bisection method]())

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

# Exponential Search

An binary search implementation for unbounded arrays or sequences. It first finds an upper bound, and then performs binary search on that bound $(0 \dots i)$.

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

# Boyer - Moore Voting Algorithm

# Median of Medians Search

# Pivot Selection

# Divide & Conquer Select

# Quickselect

# Heapselect
