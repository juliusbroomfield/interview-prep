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
      
   ```
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

- Binary Search: Searches a sorted array by dividing it in half repeatedly.
  
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
    
- Linear Search: Scans each element in sequence.
  
   - Time Complexity: O(n)
     
   ```python
   arr = [8, 1, 6, 23, 61]
   element = 5
   
   element in arr # Returns False
   ```
    
- QuickSelect: Finds the k-th smallest element using partitioning logic of QuickSort
  
  - Dutch National Flag Problem: Sort an array of 0s, 1s, and 2s.
    
  - Time Complexity: Average O(n), Worst O(n^2).

- Exponential Search: Best search when the array size is unknown
  
  - Time Complexity: O(log n)
    
  ```python
  def exponential_search(array, n, x):
     low = 0
     high = n - 1
   
     while low <= high:
       mid = low + (high - low) // 2
       if array[mid] == x:
         return mid
       elif array[mid] > x:
         high = mid - 1
       else:
         low = mid + 1
   
     return -1
   ```
  
## Array Algorithms:

- Kadane’s Algorithm: Finds the contiguous sub-array with the largest sum.
  
  - Time Complexity: O(n).
    
- Bit Manipulation: Techniques to manipulate individual bits in numbers; used to find single numbers in arrays, counting set bits, et
  
   - **Find Single Number in Array:**
     
     ```python
     class Solution:
       def singleNumber(self, nums: List[int]) -> int:
           mask = 0
           for n in nums: mask ^= n
           return mask
     ```

## String Algorithms:
_Remember strings are immutuable, usually want to convert to lists -> O(n)_

- Manacher's Algorithm: Finds the longest palindromic substring in a string (Don't use this; simpler code below).
  
   - Time Complexity: O(n)
     
   - **Simpler Approach:** Create palidromes by expanding from center
      - Time Complexity: O(n)

- Anagrams?: Check if two strings are anagrams by counting numbers of each character. Can use a hashmap or array (by using ASCII value), either way both are O(1) space - alphabet only has 26 letters  

## Linked List Algorithms

- Cycle Detection (Floyd’s Cycle-Finding Algorithm): Determines if a linked list has a cycle.
  - Time Complexity: O(n).
    
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
  - Time Complexity: O(m + n), where m and n are lengths of two lists.

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
     
   - **Code:**
     
    ```python
    class Solution:
       def missingNumber(self, nums: List[int]) -> int:
           n = len(nums)
           return ((n * (n + 1)) // 2) - sum(nums)
    ```

- Sieve of Eratosthenes: Used to find all primes smaller than a given number
  
   - Time Complexity: O(sqrt(n) * log(log(n) + n))
     
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

A specialized tree-based data structure that satisfies the heap property. **Python heaps are min-heaps**, you'll need to convert each number's sign and convert it back when retrieving.

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