# interview-prep

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
    
    ```
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
- QuickSelect: Finds the k-th smallest element using partitioning logic of QuickSort.
  - Dutch National Flag Problem: Sort an array of 0s, 1s, and 2s.
  - Time Complexity: Average O(n), Worst O(n^2).

## Array Algorithms:

- Kadane’s Algorithm: Finds the contiguous sub-array with the largest sum.
  - Time Complexity: O(n).
- Two Pointer Technique: Used to search for a pair in a sorted array.
  - Time Complexity: O(n).

## Linked List Algorithms
- Cycle Detection (Floyd’s Cycle-Finding Algorithm): Determines if a linked list has a cycle.
  - Time Complexity: O(n).
- Reverse a Linked List:
  - Time Complexity: O(n).
  - **Recursive**: 
    ```
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
    
    
    ```
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
- Tree Traversals: Inorder, Preorder, Postorder, Level order (BFS).
  - **DFS Code:**
    ```
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
    
- LCA (Lowest Common Ancestor in a Binary Tree): Finding the common ancestor for two nodes. Several methods exist with varying time complexities.
- Balanced Binary Tree Check: Checks if a binary tree is balanced (difference in heights of left and right subtrees is not more than 1 for all nodes).
  - Time Complexity: O(n).

## Behavioral Questions
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
