# Bubble Sort

Bubble sort is the most intuitive sort. You just swap adjacent elements until the array is sorted, meaning that _bubble sort_ is **stable, in-place, and adaptive**.

However, the runtime is bad!

The worst and average case time complexity is $O(n^2)$ - it struggles with arrays in reverse order - while the best case is $O(n)$ - the best case time complexity is $O(n)$ for any adaptive sort though.

Really the only strengths of bubble sort are its simplicity, and the fact that its in place - space complexity is $O(1)$ - but not really worth using.

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

## Cocktail Shaker Sort

This is just an optimization of bubble sort (it's also called bidirectional bubble sort); it performs bubble sort from left-to-right, and then from right-to-left, and so on.

It still has the same qualities: **stable, in-place, adaptive**, time complexity of $O(n^2)$ and $\Omega(n)$, and space complexity of $O(1)$. It's only marginally better empircally.

# Selection Sort

Another intuitive sort. Finds the smallest (or largest) element starting from a left bound and swaps it with the first unsorted element right of that bound.

Selection sort is similar to bubble sort in its performance (exactly the same). So:

Time Complexity: $O(n^2)$ | $\Omega(n)$
Space Complexity: $O(1)$

As well as being: **adaptive, in-place, and stable**

# Insertion Sort

Insertion sort builds the final sorted array one item at a time. You have a right bound, and insert the rightmost item into the now sorted array that's left of that bound, then increase the bound by 1.

Insertion sort is still like the other sorts covered so:

Time Complexity: $O(n^2)$ | $\Omega(n)$
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

# QuickSort

Quicksort divides the array around a pivot (there are a couple [methods]() for choosing good pivots) and recursively sorts the sub-arrays.

It sorts by bringing (does this by swapping) elements less than the pivot to the left of it, and items greater to the right.

Quicksort is arguably the best comparison-based sorting algorithms _empircally_, with an average runtime of $O(n \log n)$. 

However, the worst case time complexity is $O(n^2)$, although this is only if you continously choose a bad pivot. Using the median of first, middle, and last element (median-of-three) can be more efficient than choosing randomly, but will still result in a $O(n^2)$ runtime.



Quicksort uses extra memory by design. In most cases the space complexity is $O(\log n)$, but with bad pivots it's $O(n)$.

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

Almost forgot, but quicksort is **neither adaptive nor stable** (there are stable implementations, but require $O(n)$ space).

Quicksort can use constant space with certain partitioning strategies, specifically with the classic [Hoare]() or [Lomuto]() partition schemes.

###Partition Schemes

#####Hoare's Partition Scheme
1. select a pivot
2. initialize two pointers - one on the left, one on the right
3. when the left pointer finds an element greater than the pivot, and the right pointer finds an element less than the pivot, swap
5. the new partition is where the right pointer crosses over the left, rather than the pivot

#####Lumuto's Partition Scheme
1. select the last element to be the pivot
2. initialize a single pointer to scan through the array from start to end
3. elements less than the pivot are moved to the left side, so that by the end, all elements greater or equal to the pivot are on its right
4. the pivot is then swapped with the first element greater than it

# Merge Sort

The greatest of sorting algorithms and the most intuitive [divide and conquer]() algorithm.

Mergesort divides the array into halves, sorts them, and merges them, it's too easy.

Unlike quicksort, mergesort has a runtime of $O(n \log n)$ for all cases, but still has $O(n)$ space complexity because of the recursive stack - when using linked lists it uses $O(1)$ auxillary space.

Iterative mergesort is still not **in-place**, and both sorting algorithms are **stable**, but **not adaptive**.

There is a **stable** mergesort called [block merge sort]() though.

The implementation below is of top-down mergesort.

```python
def merge_sort(arr):
    if len(arr) > 1:
        mid = len(arr) // 2
        L = arr[:mid]
        R = arr[mid:]

        merge_sort(L)
        merge_sort(R)

        merge(L, R, arr)
```

#### Block Merge Sort (Block Sort)

Blocksort sorts an array by dividing it into blocks of fixed size, sorting each block individually (not neccessarily using mergesort), and then merging the sorted blocks back into a single sorted array using a priority queue.

Apart from having a different name, it's nearly identical to top-down and bottom-up mergesort (same time and space complexities). 

The first difference is that its' **stable**, so it is a good choice for sorting large datasets that cannot fit in memory.

The second difference is that its best case runtime is actually $\Omega (n)$ rather than $O(n)$.

# Heap Sort

Heapsort is pretty easy to implement too (assuming you have a [heap]()). 

In Python, you can just add all the elements to heap (creating the heap is done in $O(n)$ time). Maintaining the heap property causes it to have $O(n \log n)$ though, from the $O(\log n)$ [heapify]() operation, which is done $n$ times.

Time Complexity: $O(n \log n)$ for all cases
Space Complexity: $O(1)$

```python
import heapq

def heapsort(iterable):
    # Create a max-heap from the iterable
    min_heap = [-x for x in iterable]
    heapq.heapify(min_heap)

    # Extract elements from the heap and negate again to restore original values
    return [-heapq.heappop(min_heap) for _ in range(len(min_heap))]
```
Heapsort is good if you want the constant $O(n \log n)$ runtime of algorithms like [merge sort](), without the additional space.

Heapsort is **technically in-place**, but is **not stable**. Standard implementations of heapsort aren't adaptive, but there is [adaptive heapsort]().

#### Adaptive Heapsort

Adaptive Heapsort identifies and preserves already sorted sequences during the heap building phase. 

It reduces the number of comparisons and swaps needed - when building the heap, if a sequence within the array is detected as already sorted, adaptive heapsort avoids unnecessary swaps that would break the sequence. 


# Counting Sort

One of the greatest sorting algorithms (it's cheating).

Counts occurrences of elements and constructs the output array, can easily be done with an array or dictionary.

This means that it's **not in-place or adaptive**, but it is stable!

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

# Radix Sort

One of the downsides of [counting sort](), is *what if the range between the smallest and largest element is quite large*?

Radix sort fixes that issue by sorting numbers by processing individual digits, using [counting sort]() and [bucket sort]() as subroutines.

#####LSD Radix Sort
Starts sorting from the least significant digit and progresses towards the most significant digit. 

LSD is typically used with counting sort and is effective for fixed-length integer sorting.

#####MSD Radix Sort
Starts from the most significant digit and moves towards the least significant digit. 

MSD is typically used with bucket sort and is efficient in sorting variable-length strings or integers.

Like [counting sort](), radix sort is neither **adaptive or in-place** but it is stable!

The time complexity for both is $O(nk)$ where $k$ is the number of digits. Radix sort (and often [counting sort]()) are better when:

1. Elements have a fixed length or small $k$ value.
3. Uniform distribution of key values.
3. Direct comparisons are computationally expensive.

The space complexity is $O(n + k)$.

# Bucket Sort

I had actually never learned bucket sort before. 

Bucket sort distributes elements into buckets, then sorts each bucket individually, either using a different sorting algorithm or by recursively applying the bucket sort algorithm.

One advantage of bucket sort, is that its' time complexity can be linear (assuming close to uniform distribution) $\Omega (n + k)$. 

Its' worst case time complexity is $O(n + k + \frac{n^2}{k} )$ where $k$ is the number of buckets; the worst case space complexity is $O(n+k)$. Its' not **in-place** or **adaptive** either, but it is **stable**.


```python
def bucket_sort(arr):
    # Find maximum value in arr and use length of arr to determine which bucket the element should go in
    max_value = max(arr)
    size = max_value / len(arr)
    
    # Create n empty buckets where n is the length of the array
    buckets_list= [[] for _ in range(len(arr))]
    
    # Insert elements into their respective buckets
    for i in range(len(arr)):
        j = int(arr[i] / size)
        if j != len(arr):
            buckets_list[j].append(arr[i])
        else:
            buckets_list[len(arr) - 1].append(arr[i])
    
    # Sort elements within the buckets using insertion sort
    for i in range(len(arr)):
        insertion_sort(buckets_list[i])
    
    # Concatenate the buckets with sorted elements
    result = []
    for i in range(len(arr)):
        result = result + buckets_list[i]
    
    return result
```

# Pancake Sort

Pancake sort is quite different; it involves flipping entire sub-arrays (stacks of pancakes) up to a chosen position, rather than swapping individual elements.

Pancake sorting isn't useless though, it has practical applications in parallel processor networks and is in a few Leetcode questions (I lied I could only find one):
[969. Pancake Sorting](https://leetcode.com/problems/pancake-sorting/description/)



The runtime complexity of pancake sorting is $O(n^2)$ in the worst case, as for each of the $n$ pancakes, a maximum of $n−1$ flips are needed. However, in practice, the number of flips is usually less. The space complexity is O(1), as the sorting is performed in-place.

```python
def pancake_sort(arr):
    n = len(arr)
    # Start from the complete array and one by one reduce current size by one
    for i in range(n, 1, -1):
        # Find the index of the maximum element in arr[0..i-1]
        mi = arr.index(max(arr[0:i]))
        
        # Move the maximum element to end of current array if it's not
        # already at the end
        if mi != i-1:
            # First move maximum number to beginning
            if mi != 0:
                # flip arr[0..mi]
                arr[:mi+1] = arr[:mi+1][::-1]
            # Now move the maximum number to end by reversing current array
            arr[:i] = arr[:i][::-1]
    return arr
```