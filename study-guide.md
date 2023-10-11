# Study Guide

- [Algorithms](#algorithms)
  - [Sorting Algorithms](#sorting-algorithms)
  - [Searching and Selection Algorithms](#searching-and-selection-algorithms)
  - [Array Algorithms](#array-algorithms)
  - [String Algorithms](#string-algorithms)
  - [Linked List Algorithms](#linked-list-algorithms)
  - [Tree Algorithms](#tree-algorithms)
  - [Backtracking](#backtracking)
  - [Math](#math)
- [Technical Knowledge (Python)](#technical-knowledge-python)
  - [Data Structures](#data-structures)
  - [Python Behaviors](#python-behaviors)
  - [Threading](#threading)
  - [Hashing](#hashing)
- [Behavioral Questions](#behavioral-questions)
- [General Guide](#general-guide)
---

# Algorithms

## Sorting Algorithms:

- **Bubble Sort:** Continuously swaps adjacent elements if they are in the wrong order.
   - Time Complexity: Average O(n^2), Best O(n) when optimized.
   - Space Complexity: O(1)
     
- **Selection Sort:** Finds the smallest (or largest) element and swaps it with the first unsorted element
   - Time Complexity: O(n^2) for all cases.
   - Space Complexity: O(1)
     
- **Insertion Sort:** Builds the final sorted array one item at a time.
  
   - Time Complexity: Average O(n^2), Best O(n) for nearly sorted data.
     
   - Space Complexity: O(1)
     
   ```python
      def insertion_sort(array):
        for i in range(1, len(array)):
          key = array[i]
          j = i - 1
          while j >= 0 and array[j] > key:
            array[j + 1] = array[j]
            j -= 1
          array[j + 1] = key
   
     return array
   ```
   
- **QuickSort:** Divides array around a pivot and recursively sorts the sub-arrays.
  
   - Time Complexity: Average O(n log n), Worst O(n^2).
     
   - Space Complexity: Worst Case: O(n), Best Case: O(log n)
     
   ```python
   def quick_sort(arr, low=0, high=None):
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
- **Merge Sort:** Divides the array into halves, sorts them, and merges them.
  
    - Time Complexity: O(n log n) for all cases.
      
    - Space Complexity: O(n), recursive stack
      
   ```python
   def merge_sort(array_to_sort, start_index=0, end_index=None):
       if end_index is None:
           end_index = len(array_to_sort) - 1
   
       if start_index < end_index:
           middle_index = (start_index + end_index) // 2
           merge_sort(array_to_sort, start_index, middle_index)
           merge_sort(array_to_sort, middle_index + 1, end_index)
           
           left_pointer, right_pointer = start_index, middle_index + 1
           
           while left_pointer <= middle_index and right_pointer <= end_index:
               if array_to_sort[left_pointer] <= array_to_sort[right_pointer]:
                   left_pointer += 1
               else:
                   temp_value = array_to_sort[right_pointer]
                   for shift_index in range(right_pointer, left_pointer, -1):
                       array_to_sort[shift_index] = array_to_sort[shift_index - 1]
                   array_to_sort[left_pointer] = temp_value
                   
                   left_pointer += 1
                   middle_index += 1
                   right_pointer += 1
                   
       return array_to_sort
   ```

- **Heap Sort:** Uses a binary heap data structure to sort.
    - Time Complexity: O(n log n) for all cases.
    - Space Complexity: O(1)
- **Counting Sort:** Counts occurrences of elements and constructs the output array.
  
    - Time Complexity: O(n + k) where k is the range of input.
      
    - Space Complexity: O(k)
      
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
   
- **Radix Sort:** Sorts numbers by processing individual digits.
    - Time Complexity: O(nk) where k is the number of digits.
    - Space Complexity: O(n + k)
- **Bucket Sort:** Distributes elements into buckets and then sorts the buckets.
    Time Complexity: Average O(n + n^2/k + k) where k is the number of buckets.

## Searching and Selection Algorithms:

- **Binary Search:** Searches a sorted array by dividing it in half repeatedly.

  - If you can eliminate half of your remaining possibilties in each step, you can likely use binary search
 
  - If the problem is about finding a specific point in a continous range, like finding square root, you can likely use binary search
  
  - Time Complexity: O(log n).
    
    ```python
    class Solution:
      def search(self, nums: List[int], target: int) -> int:
          left, right = 0, len(nums) - 1
          while right >= left:
              mid = left + (right - left) // 2
              if nums[mid] == target: return mid
              elif nums[mid] < target: left = mid + 1
              else: right = mid - 1
  
          return -1
    ```

  - **Practical Example:**
    
    - There are _n_ piles of apples with varying amounts. Given _h_ hours, determine the minimum speed at which all bananas can be eaten, where in one hour, up to a certain number of bananas can be eaten from one pile. If a pile has fewer bananas than the chosen speed, the rest of the hour goes unused.
   
    - The brute force uses tries every possible speed, from 1 to the maximum number of apples in the pile
    
      - Time Complexity: O(n * m)
     
        ```python
        for i in range(1, max(piles) + 1):
            result = 0
            for banana in piles:
                result += math.ceil(banana / i)
            if result <= hours: return i
        ```
        
    - The optimal solution uses binary search for this instead
   
    - Time Complexity O(n log m)
   
      ```python
      class Solution:
        def minEatingSpeed(self, piles: List[int], hours: int) -> int:
        
          # Helper function to calculate total hours required to eat all bananas at a given speed
          def required_hours(speed):
              return sum(math.ceil(pile / speed) for pile in piles)
          
          low, high = 1, max(piles)
          
          # Binary search to find the minimum speed
          while low <= high:
              mid = (low + high) // 2
              if required_hours(mid) <= hours:
                  high = mid - 1
              else:
                  low = mid + 1
                  
          return low
        ```
    
- **Linear Search:** Scans each element in sequence.
  
   - Time Complexity: O(n)
     
   ```python
   arr = [8, 1, 6, 23, 61]
   element = 5
   
   element in arr # Returns False
   ```
   
- **Exponential Search:** Best search when the array size is unknown
  
  - Time Complexity: O(log n)
    
  ```python
  def exponential_search(arr, x):
      if arr[0] == x:
          return 0
  
      i = 1
      while i < len(arr) and arr[i] <= x: i *= 2
  
      # Call binary search for the found range
      return binary_search(arr, i//2, min(i, len(arr)), x)
   ```
    
- **Quick Select:** Finds the k-th smallest element using partitioning logic of QuickSort
  
  - Dutch National Flag Problem: Sort an array of 0s, 1s, and 2s.
    
  - Time Complexity: Average O(n), Worst O(n^2).

- **Boyer-Moore Voting Algorithm (Majority Element):** Given an array, find the element that appears more than **[n / 2]** times

  - Iterate through an array, if the counter is zero, we set the current element as the candidate. If the next element matches the candidate, we increment the counter; otherwise, we decrement it. The majority element will be the final candidate

    - The brute force for this (usually a hashmap to keep count) has linear runtime but takes O(n) space
    
  - Time Complexity: O(n)
 
  - Space Complexity: O(1)


    ```python
    result, count = 0, 0

    for n in nums:
      if count == 0:
        result = n
      count += (1 if n == result else -1)

    return result
    ```

    - Given an array, find all elements (there can only be up to 2) that appear more than **[n / 3]** times
   
      - Realistically, this uses the same algorithm, just with two candidates and two counters
     
      - Time Complexity: O(n)
     
      - Space Complexity: O(1)

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
       
  
## Array Algorithms:

- **Kadane’s Algorithm:** Finds the contiguous sub-array with the largest sum.

  - Basically, we have a sliding window sum (rolling sum), and if the current number is greater than that sum, then we set the sum to that greater number
    
    - This really only makes sense if there are negative numbers in the array 
  
  - Time Complexity: O(n)
 
    - The brute force involves checking every possible subarray and has a O(n ^ 3) runtime
    
- Bit Manipulation: Techniques to manipulate individual bits in numbers; used to find single numbers in arrays, counting set bits, et
  
   - **Find Single Number in Array:**
 
     - Time Complexity: O(n)
     
     ```python
     class Solution:
       def singleNumber(self, nums: List[int]) -> int:
           mask = 0
           for n in nums: mask ^= n
           return mask
     ```

- **N - Sum:**
  
  - 2 Sum: Find two numbers that equal the target
    
    - This can be done really easily using a hashmap (or set, but requires 2 passes)
   
      - Time Complexity: O(n)
     
        - Brute force would be to find every combination, which is O(n ^ 2)
       
  - 3 Sum: Find three numbers that equal the target
 
    -  This can be quickly down by sorting the array, iterating through and choosing each number as a candidate, and then using two pointers to narrow down the other two candidate numbers
   
      - This approaches also allows the triplets smaller than target, or the triplet closest to the target
   
      -  Time Complexity: O(n ^ 2)
     
  - 4 Sum: Find four numbers that equal the target
 
    -  This is the same approach as 3 Sum, except we'll use 2 nested for loops this time
   
      - It's the same method for any N sum, we didn't use it for 2 Sum because sorting the list (O (log n)) would've taken longer than using a hashmap (O (n))
   
      - Time Complexity: O(n ^ 3)
   
        - A general time complexity formula using this algorithm is O(n ^ (N - 1)) where N is the N in N - Sum
       
        - The auxillary space is O(1) for all solutions except 2 Sum
     

## String Algorithms:
_Remember strings are immutuable, usually want to convert to lists -> O(n)_

- Palidromes: Words that reads the same forward and backward

  - This could be done iteratively using two pointers
 
    - Time Complexity: O(n)
   
    ```python
    def is_palindrome(s):
      left, right = 0, len(s) - 1
      while left < right:
          if s[left] != s[right]:
              return False
          left += 1
          right -= 1
      return True
    ```

  - Can also be done recurisvely, however the space complexity is O(n) due to call stack
  ```python
  def is_palindrome(s):
    if len(s) <= 1:
        return True
    if s[0] != s[-1]:
        return False
    return is_palindrome(s[1:-1])
  ```

- Manacher's Algorithm: Finds the longest palindromic substring in a string (This is too complicated to use in interview; simpler approach below).
  
   - Time Complexity: O(n)
 
   - Space Complexity: O(n)
     
   - **Simpler Approach:** Create palidromes by expanding from center; keep track of the longest palidrome you find (each letter can be considered a palidrome)
 
      - Manacher's Algorithm is uses this approach but uses previously computed palidrome radii to skip calculations 
     
      - Time Complexity: O(n ^ 2)
    
      - Space Complexity: O(1)

- Anagrams:
  
  - Check if two strings are anagrams by counting occurances of each character
    
    - Can use a hashmap or array (by characters using ASCII value), either way both are O(1) space - alphabet only has 26 letters
      
    - Time Complexity: O(n)

      ```python
      def anagrams(s1, s2):
        if len(s1) != len(s2):
            return False
        
        counts = [0] * 26  # for 'a' to 'z'
        
        for char in s1:
            counts[ord(char) - ord('a')] += 1
        
        for char in s2:
            counts[ord(char) - ord('a')] -= 1
            if char_count[ord(char) - ord('a')] < 0:
                return False
        
        return sum(counts) == 0
        ```
      
  - Can also sort the arrays and compare if they are equivalent. However, this has O(n log n) runtime (and O(n) space if using built-in sort)

## Linked List Algorithms

- Fast and Slow (Turtoise and Hare) Pointer: Used to detect cycles (cycle algorithm below), middle of linked list, finding kth element from end, intersection point of two lists, etc

  - Middle of Linked List:
 
    - Time Complexity: O(n)
      
    - Space Complexity: O(1)
      
      - Realistically, the "brute force" solution of calculating the length first is still O(n), but this lets you do it one pass

  ```python
  
  class Solution:
    def middleNode(self, head):
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
  ```

- Cycle Detection (Floyd’s Cycle-Finding Algorithm): Determines if a linked list has a cycle.

  - The hashset solution to this is also O(n), but uses O(n) space as well
  
  - Time Complexity: O(n)
    
  - Space Complexity: O(1)

  ```python
  class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        if head is None:
            return False
        slow = head
        fast = head.next
        while slow != fast:
            if fast is None or fast.next is None:
                return False
            slow = slow.next
            fast = fast.next.next
        return True
  ```

- Merge Sorted Linked Lists: Merge two linked lists into one sorted list by comparing each head and splicing.
  
  - **Time Complexity:** O(n + m)
    
    - This is the same as saying O(max(n, m))
      
      - O(n - m) is not the same as O(min(n, m)) though
    
    ```python
    class Solution:
        def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
            curr = dummy = ListNode()
            while list1 and list2:               
                if list1.val < list2.val:
                    curr.next = list1
                    list1, curr = list1.next, list1
                else:
                    curr.next = list2
                    list2, curr = list2.next, list2
                    
            if list1 or list2:
                curr.next = list1 if list1 else list2
                
            return dummy.next
    ```
    
  - Merge K Linked Lists
    
    - Time Complexity: O(n log k)
      
    - Auxillary Space: O(k)
      
    ```python
    class Solution:
        def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        if not lists:
            return None
    
        # Initialize the heap with tuples (node value, node)
        heap = [(l.val, l) for l in lists if l is not None]
        heapq.heapify(heap)
        
        # Dummy node to start the final list
        dummy = ListNode(-1)
        current = dummy
        
        while heap:
            # Get the smallest node from heap using its value for comparison
            val, smallest = heapq.heappop(heap)
            current.next = smallest
            current = current.next
            
            # If this node has a next node, add its tuple to heap
            if smallest.next:
                heapq.heappush(heap, (smallest.next.val, smallest.next))
        
        return dummy.next
     ```
   
- Reverse a Linked List:
  - Time Complexity: O(n).
    
  - **Recursive**: 
    ```python
    class Solution:
      def __init__(self):
          self.root = None
  
      def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
  
          if not head or not head.next: 
              self.root = head
              return head
  
          self.reverseList(head.next)
  
          head.next.next = head
          head.next = None
  
          return self.root
    ```
  - **Iterative**:
    
    
    ```python
    class Solution:
      def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
          prev, curr = None, head
  
          while curr:
              nxt = curr.next
              curr.next = prev
              prev = curr
              curr = nxt
  
          
          return prev
    ```
    
- Find Intersection Point of Two Lists: Using length difference or hash table.
  
  - Using length difference:
    
    - Calculate lengths of both lists, then align the points so that they have an equal number of nodes ahead, and the move them forward to find the intersection
      
  - Time Complexity: O(m + n), where m and n are lengths of two lists.

- Sorting Linked Lists
  
  - The bottom-up approach for merge sort (merge sort is much better than quicksort here) starts by splitting the problem into the smallest subproblem and iteratively merge the result to solve the original problem.
 
    - First, the list is split into sublists of size 1 and merged iteratively in sorted order. The merged list is solved similarly.

    - The process continues until we sort the entire list
    
  - Time Complexity: O(n log n)
    
  - Space Complexity: O(1)
    
    - The top-down approach is O(log n)    

## Tree Algorithms

- **Tree Traversals**: Inorder, Preorder, Postorder, Level order (BFS).
  
  - **DFS Code:**
    ```python
    class Solution:
        def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
            array = []
            
            def traverse(node):
                nonlocal array
                if not node:
                    return
                traverse(node.left)
                array.append(node.val)
                traverse(node.right)
            
            traverse(root)
            return array
    ```
  
   - **BFS Code:**
     
     ```python
     class Solution:
       def levelOrder(self, root: TreeNode) -> List[List[int]]:
           output = []
           queue = deque()
           if root:
               queue.append(root)
   
           while q:
               output = []
   
               for i in range(len(q)):
                   node = q.popleft()
                   val.append(node.val)
                   if node.left:
                       q.append(node.left)
                   if node.right:
                       q.append(node.right)
               output.append(val)
     
           return output
     ```

- **Balanced Binary Tree Check**: Checks if a binary tree is balanced (difference in heights of left and right subtrees is not more than 1 for all nodes).
  
  - **Time Complexity**: O(n)
    
    ```python
    class Solution:
        def isBalanced(self, root: Optional[TreeNode]) -> bool:
            
            def height_and_balance(node):
                if node is None: 
                    return 0, True
                
                left_height, left_balanced = height_and_balance(node.left)
                right_height, right_balanced = height_and_balance(node.right)
                
                balanced = left_balanced and right_balanced and abs(left_height - right_height) <= 1
                
                return max(left_height, right_height) + 1, balanced
            
            return height_and_balance(root)[1]
    ```

- **Valid BST**: Checks if BST is valid (smaller elements on the left, bigger elements on the right).
  
   - Time Complexity: O(n) 
  
    ```python
    class Solution:
        def isValidBST(self, root: Optional[TreeNode]) -> bool:
            
            def search(node, left_bound, right_bound):
                if node is None: return True
        
                if node.val <= left_bound: return False
                if node.val >= right_bound: return False
        
                if not search(node.left, left_bound, node.val): return False
                if not search(node.right, node.val, right_bound): return False
        
                return True
            
            return search(root, float(-inf), float(inf))
    ```

- **Invert Tree**: Flip each nodes left and right child
  
   - Time Complexity: O(n)

  ```python
  class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:

        if root is None: return root

        left = root.left
        right = root.right

        root.left = right
        root.right = left

        self.invertTree(root.left)
        self.invertTree(root.right)

        return root
  ```

## Backtracking

- **Description**: Solves problems recursively, if a partial solution doesn't work at any step, backtrack to a previous step and try a different path. Used in permutations, combinations, mazes, N-Queens, etc
  
   - **Time Complexity**: Time complexity is usually O(b^d), where b is the branching factor (how many options there are), and d is the max depth of recursion.
     
   - **Permutations**:
     
     ```python
     class Solution:
        def permute(self, nums: List[int]) -> List[List[int]]:
            permutations = []
    
            def backtracking(subset = []):
                if len(subset) == len(nums): 
                    permutations.append(subset.copy())
                    return
                
                for n in nums:
                    if n not in subset:
                        subset.append(n)
                        backtracking(subset)
                        subset.pop()
    
            backtracking()
    
            return permutations
     ```
  - **Combinations (Letter Combinations of a Phone Number):**
    
     ```python
        class Solution:
          def letterCombinations(self, digits: str) -> List[str]:
              mapping = {
                  '2': ('a', 'b', 'c'),
                  '3': ('d', 'e', 'f'),
                  '4': ('g', 'h', 'i'),
                  '5': ('j', 'k', 'l'),
                  '6': ('m', 'n', 'o'),
                  '7': ('p', 'q', 'r', 's'),
                  '8': ('t', 'u', 'v'),
                  '9': ('w', 'x', 'y', 'z')
              }
      
              combinations = []
      
              def dfs(current = "", i = 0):
      
                  if i == len(digits): combinations.append(current)
      
                  else:
      
                      for letter in mapping[digits[i]]:
                          current += letter
                          dfs(current, i + 1)
                          current = current[:-1]
          
              dfs()
      
              if len(combinations) == 1 and not combinations[0]: return []
      
              return combinations
     ```

## Math
- Sum of Arithmetic Sequence: Can be used to find the missing number in a range of [1, N]
  
   - Time Complexity: O(n)
     
   - Space Complexity: O(1)
 
     - Another solution uses a set, but uses O(1) space
       
     - An in-place sorting solution would be O(1) space, but O(n log n) time complexity
     
   - **Code:**
     
    ```python
    class Solution:
       def missingNumber(self, nums: List[int]) -> int:
           n = len(nums)
           return ((n * (n + 1)) // 2) - sum(nums)
    ```

- Sieve of Eratosthenes: Used to find all primes smaller than a given number
  
   - Time Complexity: the upper bound is O(n ^ 2), the tight upper bound is somewhere between O(n) and O(n ^ 2)
     
   - **Code:**
     
     ```python
     class Solution:
       def countPrimes(self, n: int) -> int:
           if n <= 2: return 0
           numbers = [False, False] + [True] * (n - 2)
           for p in range(2, int(sqrt(n)) + 1):
               if numbers[p]:
                   for multiple in range(p * p, n, p):
                       numbers[multiple] = False
           
           return sum(numbers)
     ```
- Euclidean Algorithm: Used to find the greatest common divisor
  
  - Time Complexity: O(log(min(a, b))
    
    - The brute force, checking every from number from min(a, b) to a is O(min(a, b))
      
  ```python
  def gcd(a, b):
    while b:
        a, b = b, a % b
    return a
  ```

# Technical Knowledge (Python)

## Data Structures

### List (Dynamic Array)

- **Insertion (append)**: Average \(O(1)\), worst-case \(O(n)\) when resizing
- **Insertion (at an index)**: \(O(n)\)
- **Deletion**: \(O(n)\)
- **Search**: \(O(n)\)

### Dictionary (Hash Table)

- **Access**: Average \(O(1)\), worst-case \(O(n)\) with poor hash function
- **Insertion**: Average \(O(1)\), worst-case \(O(n)\)
- **Deletion**: Average \(O(1)\), worst-case \(O(n)\)
- **Search (key)**: Average \(O(1)\), worst-case \(O(n)\)

### Set (Based on Hash Table)

- **Insertion**: Average \(O(1)\), worst-case \(O(n)\)
- **Deletion**: Average \(O(1)\), worst-case \(O(n)\)
- **Search**: Average \(O(1)\), worst-case \(O(n)\)

### Tuple (Immutable List)
- Will need to use these instead of lists as keys in hashmaps and elements in sets (mutables aren't hashable)
- Good to use these when the list elements aren't changing, it'll give you slightly faster search times (a tuple isn't an array, there is already a Python implementation of an array)
    ```python
    import array
    
    # Create an array of integers
    arr = array.array('i', [1, 2, 3, 4, 5])  # 'i' denotes integer type
    
    # A tuple
    tup = (10, 20, 30, 40, 50)
    ```

### String (Immutable Sequence of Characters)

- **Access**: \(O(1)\)
- **Concatenation**: \(O(n)\)

### Stack (A Last-In-First-Out (LIFO) data structure)

- Can be implemented using list

### Deque (Double-Ended Queue)

- Can be implemented using doubly linked list with head and tail pointers
  
```python
from collections import deque

d = deque([1, 2, 3, 4])

# Append to the right
d.append(5)

# Append to the left
d.appendleft(0)

# Pop from the right
d.pop()

# Pop from the left
d.popleft()
```

### Heap (Binary Heap)

A complete binary tree that satisfies the heap property. **Python heaps are min-heaps**, you'll need to convert each number's sign and convert it back when retrieving.

- **Insert**: \(O(log n)\)
- **Delete Max/Min**: \(O(log n)\)
- **Get Max/Min**: \(O(1)\)

```python
import heapq

# Creating a min heap
heap = []
heapq.heappush(heap, 3); heapq.heappush(heap, 1); heapq.heappush(heap, 4); heapq.heappush(heap, 2)

heap  # Returns: [1, 2, 4, 3]

# Popping the smallest element
heapq.heappop(heap)  # Returns: 1

# Pushing and popping in a single call
heapq.heappushpop(heap, 0)  # Returns: 0

# Getting the n smallest/largest elements
heapq.nsmallest(3, heap)  # Returns: [2, 3, 4]
heapq.nlargest(2, heap)   # Returns: [4, 3]
```

## Python Behaviors

### Pointers

Pointers are variables that store the memory address of another variable. Python does not have explicit pointers like languages such as C or C++, but it does have references:

- In Python, variables refer to values (or objects) in memory. This is similar to a pointer, but you can't perform arithmetic on them or directly access memory addresses.

### Sorting Algorithm

Python's default sorting algorithm for the `sorted()` function and the `list.sort()` method is Timsort.

- **Timsort**: 
  - **Time Complexity**: 
    - **Best**: (O(n log n))
    - **Average**: (O(n log n))
    - **Worst**: (O(n log n))
  - **Space Complexity**: (O(n))

## Threading

Threading is one of the ways to achieve multitasking. It allows a program to run multiple threads (smallest units of a program) in parallel, making better use of available CPU cores. 
**Python threading allows for concurrent execution. However, due to the Global Interpreter Lock (GIL) in the default Python interpreter (CPython), threads cannot execute Python bytecodes in true parallel. This means:**

- Python threads are best for I/O-bound tasks (like file I/O, network I/O) rather than CPU-bound tasks.
- For true parallel execution in CPU-bound tasks, you might want to use multiprocessing or external libraries like Numba or Cython.

## Hashing
Hashing is a technique used to store and retrieve values based on a key. The key is processed through a hash function to produce an index where the value will be stored or retrieved from.

```python
hash()
```

1. **Hash Function**: A function that takes in a key and returns an index in the hash table. A good hash function distributes keys uniformly across the table.

2. **Collision**: Occurs when two different keys produce the same index. Collisions are inevitable, but there are methods to handle them.

### Collision Resolution Techniques

1. **Chaining**: Each cell in the hash table points to a linked list of keys that hash to that index.

2. **Open Addressing**: When a collision occurs, the hash table searches for the next available slot. There are different methods to do this:

    - **Linear Probing**: If a collision occurs at index `i`, check `i+1`, `i+2`, etc.
    
    - **Quadratic Probing**: If a collision occurs at index `i`, check `i+1^2`, `i+2^2`, etc.
    
    - **Double Hashing**: Use another hash function to decide the next slot.
      
### Python Implementation (Create Your Own Hashmap)
Python uses open addressing, specifically quadratic probing, for hash collisions

- Integers
  -  For integers, the hash is the integer itself. Keep this in mind when making a hashmap of only integers, you can just use a list.

This code uses linear probing, which isn't really the best because groups of consec. slots can get filled (clustering).

```python
class MyHashMap:

    def __init__(self):
        self.size = 1000
        self.table = [None] * self.size

    def _hash(self, key):
        return key % self.size

    def put(self, key: int, value: int) -> None:
        index = self._hash(key)
        # If the slot is already occupied, look for the next available slot
        while self.table[index] is not None:
            if self.table[index][0] == key:  # If key already exists, break to overwrite its value
                break
            index = (index + 1) % self.size  # Move to the next slot
        self.table[index] = (key, value)

    def get(self, key: int) -> int:
        # PSEUDOCODE:
        # 1. Compute the index using the hash function.
        # 2. If the slot is occupied but doesn't match the key, move to the next slot.
        # 3. If a slot with the given key is found, return its value.
        # 4. If an empty slot is found or we've checked the whole table, return -1.
        pass

    def remove(self, key: int) -> None:
        # PSEUDOCODE:
        # 1. Compute the index using the hash function.
        # 2. If the slot is occupied but doesn't match the key, move to the next slot.
        # 3. If a slot with the given key is found, set it to a DELETED marker (to maintain the probe sequence).
        # 4. If an empty slot is found or we've checked the whole table, do nothing.
        pass
```

### Dynamic Resizing

For a hash table to be efficient, the load factor (number of entries divided by the number of slots) should be kept below a certain threshold (commonly 0.7). If it goes above this:

- **Expand**: Double the size of the table and rehash all entries.
- **Contract**: If the load factor goes too low, the table might be reduced in size.


# Behavioral Questions
- Tell me about yourself?
- How do you work in a group?
- What teams at Microsoft are you interested in and why?
- What do you do when you have a disagreement with a team member?
- How do you help a client figure out what they want?
- How do you handle making a mistake in your work?
- Tell me about how you balance deadlines with daily tasks.
- Why do you want to work at Microsoft?
- What Microsoft products do you use?
- Tell me about a challenging project you worked on and its technical details.
- How have you met a customer's needs in the past?
- What do you like about technology and why?
- Explain recursion to a child

# General Guide

1. Clarify question
   
  - Listen to the question very closely, you'll need all the information for an optimal solution
    
  - Ask about:
    - Case-sensitive?
    - Number ranges?
    - ASCII only?
    - Sorted, unsorted?
    - Duplicates? How to Handle?
    - Only ints, floats?
    - Only positive? Negative? Increasing? Non-Decreasing?
    - How to handle empty, invalid, or null?
      
2. Find Approach
   
  - "Intuitive Approach"
    
    - Analyze time and space complexity
      
    - Brute force is sometimes only solution (backtracking, greedy, etc)
   
      - Good indicator for this is the best conceivable runtime
     
      
  - "Optimized Approach"
    
    - Probably an entirely different approach, look for unused information
   
    - Sometimes you'll only be able to optimize on space (usually an in-place solution), don't discount it
   
    - **BUDS:** Bottlenecks, unneccessary work, and duplicated work
      
    - Run through list of data structures (specifically ADTs) now are, usually:
      
      1. Hashmaps
         
      2. Stacks & Queues
         
         - In Python, stacks are really just lists, but make sure to point out you're using it as a stack
           
         - You can use a list as a queue too, but point out that removal time (dequeue) is O(n)
           
           - The better data structure here is a doubly-linked list (remember circular array, not worth using though)
              
      3. Sets / Caches (Memoization)
         
      4. Heaps / Trees
         
         - Will mostly be using binary trees, but make sure to clarify (also clarify between binary tree and BST)
           
         - Will mostly use decision trees (N-ary trees) for backtracking
           
         - Even rarer, but should understand tries (this is the data structure to use for a phone book)
           
         - Remember that heaps are not priority queues, they are used to make priority queues, not equivalent
         
    - Break down time and space complexity
    
3. Pseudocode
   
   - You can do this while explaining your approach; try to always maintain some communication with interviewer, ask for a short break if needed
     
   - Psuedocode throgh text (like a comment) or whiteboard - Codility has a whiteboard
     
   - Make unit tests during this time
     
     - Try to break the code; **anything** within constraints is fair game (null cases, empty array, max int, etc)
     
   - Quickly trace through pseudocode before you start coding; you shouldn't be straying from the pseudocode once you start coding

5. Coding
   
   - Prioritize readibility and follow closely to the pseudocode; modularize the code from the beginning
     
   - Try to use helper functions if it simplfies the code, might not need to fill it out in the end
     
   - Usually will have a blank IDE to work with, a good starting point is:
     
   ```python
   IMPORTS 
   '''
   TEST CASES HERE
   PSEUDOCODE HERE
   '''
   def solution():
     return answer

   print(solution(test_cases))
   ```

   - Trace through the code one last time before running the code; running the code is like submitting
