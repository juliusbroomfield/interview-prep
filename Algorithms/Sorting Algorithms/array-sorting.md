### Bubble Sort

Bubble sort is the most intuitive sort. You just swap adjacent elements until the array is sorted, meaning that _bubble sort_ is **stable, in-place, and adaptive**.

However, the runtime is bad!

The worst and average case time complexity is $O(n^2)$, while the best case is $O(n)$ - the best case time complexity is $O(n)$ for any adaptive sort though.

Really the only strengths of bubble sort are its simplicity, and the fact that its in place, so space complexity is $O(1)$, but not really worth using though.

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        # Create a flag to detect if a swap happens
        swapped = False
        # Last i elements are already in place
        for j in range(0, n-i-1):
            # Traverse the array from 0 to n-i-1
            # Swap if the element found is greater than the next element
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
        # IF no two elements were swapped by inner loop, then break
        if not swapped:
            break
    return arr

```

### Cocktail Shaker Sort

This is just an optimization of bubble sort (it's also called bidirectional bubble sort); it performs bubble sort from left-to-right, and then from right-to-left, and so on.

It still has the same qualities: **stable, in-place, adaptive**, time complexity of $O(n^2)$ and $\Omega(n)$, and space complexity of $O(1)$.

### Selection Sort

Another intuitive sort. Finds the smallest (or largest) element starting from a left bound and swaps it with the first unsorted element right of that bound.

Selection sort is similar to bubble sort in its performance (basically exactly similar). So:

Time Complexity: $O(n^2)$, $\Omega(n)$
Space Complexity: $O(1)$

As well as being: **adaptive, in-place, and stable**

### Insertion Sort

Insertion sort builds the final sorted array one item at a time. You have a right bound, and insert the rightmost item into the now sorted array that's left of that bound, then increase the bound by 1.

Insertion sort is still like the other sorts covered so:


Time Complexity: $O(n^2)$, $\Omega(n)$
Space Complexity: $O(1)$

As well as being: **adaptive, in-place, and stable**

It is nearly $O(n)$ for nearly sorted arrays though.

```python
def insertion_sort(arr):
    # Traverse through 1 to len(arr)
    for i in range(1, len(arr)):
        key = arr[i]
        # Move elements of arr[0..i-1], that are
        # greater than key, to one position ahead
        # of their current position
        j = i-1
        while j >=0 and key < arr[j]:
            arr[j+1] = arr[j]
            j -= 1
        # The right bound for the key is found when the while loop ends
        # The key is then placed at its correct position
        arr[j+1] = key
    return arr

```

### QuickSort

Quicksort is probably the most unintuitive sorting algorithm, I still barely understand it.

Quicksort divides the array around a pivot (there are a couple methods for choosing good pivots) and recursively sorts the sub-arrays.

It sorts by bringing (does this by swapping) elements less than the pivot to the left of it, and items greater to the right.

Quicksort has one of the best possible average runtime for comparison based algorithms (divide and conquer sorting algorithms do in general) of $O(n \log n)$. 

However, the worst case time complexity is $O(n)$, although this is only if you continously choose a bad pivot.

Quicksort uses extra memory by design (if done recursively then this is through the call stack). In most cases the space complexity is $O(\log n)$, but with bad pivots it's $O(n)$.

```python
def quick_sort(arr, low = 0, high = None):
    if high is None:
        high = len(arr) - 1
        
    if low < high:
        pivot = arr[low]
        i = low
        for j in range(low+1, high+1):
            if arr[j] < pivot:
                i += 1
                arr[i], arr[j] = arr[j], arr[i]
        arr[low], arr[i] = arr[i], arr[low]
        
        quick_sort(arr, low, i-1)
        quick_sort(arr, i+1, high)
        
    return arr
```

Almost forgot, but quicksort is neither adaptive

### Merge Sort

The greatest of sorting algorithms and the most intuitive [divide and conquer]() algorithm.

Mergesort divides the array into halves, sorts them, and merges them, it's too easy.

Unlike quicksort, mergesort has a runtime of $O(n \log n)$ for all cases, but still has $O(n)$ space complexity because of the recursive stack. However, merge sort can be done iteratively, using $O(1)$ space!

The implementation below is of the iterative mergesort (also called bottom-up mergesort).

```python
def merge(arr, l, m, r):
    n1 = m - l + 1
    n2 = r - m
    L = [0] * n1
    R = [0] * n2

    for i in range(0, n1):
        L[i] = arr[l + i]
    for j in range(0, n2):
        R[j] = arr[m + 1 + j]

    i = 0
    j = 0
    k = l

    while i < n1 and j < n2:
        if L[i] <= R[j]:
            arr[k] = L[i]
            i += 1
        else:
            arr[k] = R[j]
            j += 1
        k += 1

    while i < n1:
        arr[k] = L[i]
        i += 1
        k += 1

    while j < n2:
        arr[k] = R[j]
        j += 1
        k += 1

def iterative_merge_sort(arr):
    n = len(arr)
    curr_size = 1

    while curr_size < n:
        for left_start in range(0, n, 2*curr_size):
            mid = min(n-1, left_start + curr_size - 1)
            right_end = min(n-1, left_start + 2*curr_size - 1)
            merge(arr, left_start, mid, right_end)
        curr_size *= 2

    return arr

```

### Heap Sort

Heapsort is pretty easy to implement too (assuming you have a [heap]()). 

In Python, you can just add all the elements to heap (creating the heap is done in $O(n)$ time). Maintaining the heap property causes it to have $O(n \log n)$ though, from the $O(\log n)$ [heapify]() operation, which is done $n$ times.

Time Complexity: $O(n \log n)$ for all cases
Space Complexity: $O(1)$


### Counting Sort

One of the greatest sorting algorithms (it's a cheat).

Counts occurrences of elements and constructs the output array, can easily be done with an array or dictionary.

Time Complexity: $O(n + k)$ where k is the range of input

Space Complexity: $O(k)$

```python
# This code assumes non-negative integers
def counting_sort(arr):
    if not arr:
        return []
    max_val = max(arr)
    count = [0] * (max_val + 1)
    for num in arr:
        count[num] += 1
    result = []
    for i, c in enumerate(count):
        result.extend([i] * c)
    return result
```

### Radix Sort

One of the downsides of [counting sort](), is what if the range between the smallest and largest element $k$ is quite large.

Radix sort target that issue by sorting numbers by processing individual digits, by using counting sort and [bucket sort]()

Time Complexity: $O(nk)$ where $k$ is the number of digits.
Space Complexity: $O(n + k)$

### Bucket Sort

Another confusing algorithm; bucket sort distributes elements into buckets and then sorts the buckets. 

Time Complexity: $O(n + k + \frac{n^2}{k} )$ where k is the number of buckets.

### Pancake Sort

Don't feel like including this yet

