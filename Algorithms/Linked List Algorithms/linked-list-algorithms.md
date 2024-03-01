# Floyd’s Cycle-Finding Algorithm

## Fast and Slow (Turtoise and Hare) Pointer

This technique is used to detect cycles (cycle algorithm below), middle of linked list, finding $k^{th}$ element from the end (at index $\frac {n}{2}$), intersection point of two lists, etc.

## Cycle Detection

Floyd’s Cycle-Finding Algorithm is used to determine if a linked list has a cycle.

Two pointers (the "tortoise" and the "hare") start at the head of the list. The tortoise advances one step at a time, while the hare moves two steps at a time. If there is no cycle, the hare will reach the end of the sequence. If a cycle exists, the hare and tortoise will eventually meet within the cycle.

The time complexity is $\Theta (n)$, with a space complexity of $O(1)$. The hashset solution is also $\Theta(n)$, but uses $O(n)$ space as well.

```python
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

# Merge (Merge K Linked Lists)
  
How might we merge two linked lists into one sorted list?

If $K = 2$ we can compare each head and splice into one list.
```python
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]):
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

This has a time complexity of $\Theta (n + m)$, and uses no additional space.

However if $K > 2$, then it'll be best to use a [heap]() to manage which head to choose next, giving us a time complexity of $\Theta (n \log k)$ and space complexity of $O(k)$. Not using a heap and checking each head manually, will have a time complexity of $\Theta (N^{k-1})$.

```python
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

# Reverse

We can reverse a linked list iteratively using 3 adjacent pointers and reversing the first two, preserving the 3rd.

```python
  def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
      prev, curr = None, head

      while curr:
          nxt = curr.next
          curr.next = prev
          prev = curr
          curr = nxt

      
      return prev
```

We can also do it recursively, essentially have a reversed portion of the list point to the head of that portion (making it the tail).

```python
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:

      if not head or not head.next: 
          root = head
          return head

      reverseList(head.next)

      head.next.next = head
      head.next = None

      return root
```

Both solutions have a runtime of $O(n)$, but reversing recursively has a space complexity of $O(n)$. If they ask this question they'll typically be looking for both though.
   
# Intersection Point

How would we find the intersection point of two linked lists?

1. Calculate Lengths: Iterate through both linked lists to determine their lengths, 
2. Calculate Difference: Find the difference $D$ in lengths, where
$D = ∣L_{1}−L{2}∣$
3. Advance the pointer in the longer list by $D$ nodes. This way, both pointers have the same number of nodes to traverse until the end of their respective lists.
4. Move both pointers through the two lists, the first node where both pointers meet is the intersection point.
5. If no intersection is found by the time both pointers reach the end of their lists, then the lists do not intersect.

This algorithm has a runtime of $\Theta (n + m)$, the [brute force]() solution would have a runtime of $\Theta(nm)$.
   
# Sorting 

Sometimes, interviewers might ask to sort a linked list. I chose $3$ sorting algorithms to look at, each with different advantages.
   
## Merge Sort

Merge Sort is the most efficient and practical sorting algorithm for linked lists. 

**Step 1: Splitting the List**
The first part of merge sorting a linked list is dividing the list into two halves, using the ["tortoise and hare" technique](), where the slow pointer will be at the midpoint

**Step 2: Recursive Sorting**
Once the list is split into two halves, the merge sort algorithm recursively sorts each half, stopping only until the sublists are single elements.

**Step 3: Merging Sorted Lists**
This involves taking two sorted sublists and [merging]() them into a single sorted list. 

It has the same qualities as the traditional [merge sort]() used for arrays.


## Bubble Sort

Bubble Sort is probably the simplest and easiest sorting algorithm to apply to singly linked list. [Selection sort]() and [insertion sort]() are simple to apply to linked lists too though.

It has the same qualities as the traditional [bubble sort]() used for arrays.

```python
    def bubbleSort(self): 
        count = 0
        start = self.head 
        while start != None: 
            count+= 1
            start = start.next
   
        # Traverse through all nodes of linked list 
        for i in range(0, count): 
   
            # Last i elements are already in place 
            start = self.head 
            while start != None: 
   
                # Swap adjacent nodes 
                ptr = start.next
                if ptr != None: 
                    if start.data > ptr.data: 
                        self.swapNodes(start.data, ptr.data) 
   
                start = start.next
```

Both Mergesort and Bubblesort can be used for singly, doubly, and circular linked lists. However, in circular linked lists you will have to break the loop first.