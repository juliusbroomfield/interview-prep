Hashing is used used to store and retrieve values based on a key. The key is processed through a hash function to produce an index where the value will be stored or retrieved from.

1. **Hash Function**: A function that takes in a key and returns an index in the hash table. A good hash function distributes keys uniformly across the table.

2. **Collision**: Occurs when two different keys produce the same index. Collisions are inevitable, but there are methods to handle them.

## Collision Resolution Techniques

1. **Chaining**: Each cell in the hash table points to a linked list of keys that hash to that index.

2. **Open Addressing**: When a collision occurs, the hash table searches for the next available slot. There are different methods to do this:

    - **Linear Probing**: If a collision occurs at index $i$, check $i+1$, $i+2$, etc.
    
    - **Quadratic Probing**: If a collision occurs at index $i$, check $i+1^2$, $i+2^2$, etc.
    
    - **Double Hashing**: Use another hash function to decide the next slot.
      
## Python Implementation 
Python uses open addressing, specifically quadratic probing, for hash collisions.

For integers, the hash is the integer itself. Keep this in mind when making a hashmap of only integers, you can just use a list.

## Hashmaps
In Python, **hashmaps** are **dictionaries**.

- **Set/Update (`dict[key] = value`):**
  - **Time Complexity:** $O(1)$
  - Sets or updates the value of `key` in the dictionary.

- **Get (`dict[key]`):**
  - **Time Complexity:** $O(1)$
  - Retrieves the value associated with `key`.

- **Delete (`del dict[key]`):**
  - **Time Complexity:** $O(1)$
  - Removes `key` and its associated value from the dictionary.

- **Clear (`dict.clear()`):**
  - **Time Complexity:** $O(n)$
  - Removes all items from the dictionary.

```python
hashmap = {'name' : id}
```

**Dictionaries have other methods as well:**

- **Copy (`dict.copy()`):**
  - **Time Complexity:** $O(n)$
  - Returns a shallow copy of the dictionary.

- **Keys (`dict.keys()`):**
  - **Time Complexity:** $O(1)$
  - Returns a view of the dictionary's keys.

- **Values (`dict.values()`):**
  - **Time Complexity:** $O(1)$
  - Returns a view of the dictionary's values.

- **Items (`dict.items()`):**
  - **Time Complexity:** $O(1)$
  - Returns a view of the dictionary's items (key-value pairs).

- **Pop (`dict.pop(key[, default])`):**
  - **Time Complexity:** $O(1)$
  - Removes the item with `key` and returns its value or `default` if `key` is not found.

- **Popitem (`dict.popitem()`):**
  - **Time Complexity:** $O(1)$
  - Removes and returns an arbitrary (key, value) pair from the dictionary.

- **Update (`dict.update([other])`):**
  - **Time Complexity:** $O(n)$ (where $n$ is the number of items in `other`)
  - Updates the dictionary with the key/value pairs from `other`, overwriting existing keys.

- **Get (`dict.get(key[, default])`):**
  - **Time Complexity:** $O(1)$
  - Returns the value for `key` if `key` is in the dictionary, else `default`.

### Chainmaps
Chainsmaps are used to combine multiple dictionaries into a single, updateable view.

```python
  from collections import ChainMap
  dict1 = {'a': 1, 'b': 2}
  dict2 = {'b': 3, 'c': 4}
  combined = ChainMap(dict1, dict2)
```

### OrderedDict

An OrderedDict is a dict subclass that remembers the order entries were added. 

As of Python 3.7, regular dicts also maintain insertion order though.

  ```python
  from collections import OrderedDict
  ordered = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
  ```

### Counter

A `Counter` is a dict subclass for counting hashable objects. It's a collection where elements are stored as dictionary keys and their counts as dictionary values.

```python
  from collections import Counter
  counts = Counter(['a', 'b', 'c', 'a', 'b', 'b'])
```


## Sets
Python has sets too, I'll be looking at **sets**, **frozen sets**, and **multisets (bags)**

- **Add (`A.add(x)`):**
  - **Time Complexity:** $O(1)$

- **Remove (`A.remove(x)`):**
  - **Time Complexity:** $O(1)$

- **Discard (`A.discard(x)`):**
  - Removes `x` from `A` if present.
  - **Time Complexity:** $O(1)$

- **Pop (`A.pop()`):**
  - Removes and returns an arbitrary element from `A`
  - **Time Complexity:** $O(1)$

- **Clear (`A.clear()`):**
  - **Time Complexity:** $O(1)$

### Hashsets
The normal, everyday sets.

```python
A = set(['a', 'e', 'i', 'o', 'u'])
B = set(['1', '2', '3', '4', '5'])
```

**Python sets have the same methods you'd expect from mathematical sets:**

- **Union (`A.union(B)` or `A | B`):**
  - **Description:** Returns a set containing all the distinct elements in either $A$ or $B$.
  - **Time Complexity:** $O(m + n)$

- **Intersection (`A.intersection(B)` or `A & B`):**
  - **Description:** Returns a set containing all elements common to both `A` and `B`.
  - **Time Complexity:** $O(min(m, n))$
  - **Intersection Update (`A.intersection_update(B)` or `A &= B`):**
    - **Description:** Updates $A$, keeping only elements found in both $A$ and $B$.
    - **Time Complexity:** $O(m)$ 

- **Difference (`A.difference(B)` or `A - B`):**
  - **Description:** Returns a set containing elements in `A` but not in `B`.
  - **Time Complexity:** $O(m)$
  - **Difference Update (`A.difference_update(B)` or `A -= B`):**
    - **Description:** Removes all elements of $B$ from $A$.
    - **Time Complexity:** $O(m)$

- **Symmetric Difference (`A.symmetric_difference(B)` or `A ^ B`):**
  - **Description:** Returns a set containing elements in either `A` or `B` but not in both.
  - **Time Complexity:** $O(m + n)$
  - **Symmetric Difference Update (`A.symmetric_difference_update(B)` or `A ^= B`):**
    - **Description:** Updates $A$ with the symmetric difference of $A$ and $B$
    - **Time Complexity:** $O(m + n)$

- **Is Subset (`A.issubset(B)`):**
  - **Time Complexity:** $O(m)$

- **Is Superset (`A.issuperset(B)`):**
  - **Time Complexity:** $O(n)$
 
- **Is Disjoint (`A.isdisjoint(B)`):**
  - **Description:** Returns `True` if $A$ and $B$ have no elements in common.
  - **Time Complexity:** $O(min(m, n))$

- **Update (`A.update(B)` or `A |= B`):**
  - **Description:** Adds all elements from $B$ to the set $A$.
  - **Time Complexity:** $O(n)$  

### Frozen Sets
Frozen sets are immutable and static, and allow only query operations on their elements, not inserts or deletions. 

They can be used as dictionary keys or as elements of another set, which isn’t possible with regular sets.

```python
vowels = frozenset({"a", "e", "i", "o", "u"})
```

