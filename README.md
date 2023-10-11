# Algorithms

## Sorting Algorithms:

- **Bubble Sort:** Continuously swaps adjacent elements if they are in the wrong order.
   - Time Complexity: Average O(n^2), Best O(n) when optimized.
     
- **Selection Sort:** Finds the smallest (or largest) element and swaps it with the first unsorted element
   - Time Complexity: O(n^2) for all cases.
     
- **Insertion Sort:** Builds the final sorted array one item at a time.
   - Time Complexity: Average O(n^2), Best O(n) for nearly sorted data.
- **QuickSort:** Divides array around a pivot and recursively sorts the sub-arrays.
   - Time Complexity: Average O(n log n), Worst O(n^2).
- **Merge Sort:** Divides the array into halves, sorts them, and merges them.
    - Time Complexity: O(n log n) for all cases.
- **Heap Sort:** Uses a binary heap data structure to sort.
    - Time Complexity: O(n log n) for all cases.
- **Counting Sort:** Counts occurrences of elements and constructs the output array.
    - Time Complexity: O(n + k) where k is the range of input.
- **Radix Sort:** Sorts numbers by processing individual digits.
    - Time Complexity: O(nk) where k is the number of digits.
- **Bucket Sort:** Distributes elements into buckets and then sorts the buckets.
    Time Complexity: Average O(n + n^2/k + k) where k is the number of buckets.

## Searching and Selection Algorithms:

- Binary Search: Searches a sorted array by dividing it in half repeatedly.
  
  - Time Complexity: O(log n).
    
  - Code:
    
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
  
  - Time Complexity: O(n).
    
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
     
   - ** Code:**
     
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


# Behavioral Questions
- Tell me about yourself?
- How do you work in a group?
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
