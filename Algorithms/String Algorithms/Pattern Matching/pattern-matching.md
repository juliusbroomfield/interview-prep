# L

First, I'll briefly talk about the **brute force** approach, which is done aligning the pattern against the text.

There are $n - m$ pattern checks, so the runtime is $O(nm)$ and $\Omega (n)$.

# Boyer-Moore

Similar to the brute force approach, except after a mismatch (using brute force), we'll shift a certain amount of spaces according to a last occurance table table

**Last Occurrence Table** - tells how far to shift based on mismatched character. It is a form of preprocessing, does not depend on text.

```python
def lastOccuranceTable(pattern):
	table = {} # {char : int}

	for i in range(0, m - 1):
		table[pattern[i]] = i

	return table
```

How to use *last occurance table*?

1. Shift starting index from left to right, but match pattern right to left
2. **Mismatch?**
\
**Case 1:** Character is not in pattern
**Solution:** Shift all the way past
\
**Case 2:** Character is in pattern, but occurs before current index
**Solution:** Match Character in Text with Character in Pattern
\
**Case 3:** Character has already been past
**Solution:** Shift right by 1

Time Complexity: If searching for a single pattern, it runs in $\Omega (n)$. If searching for a all patterns, it's $\Omega(\frac nm + m)$. Otherwise it runs in $\Theta (nm)$.

#### Galil Rule

When a mismatch occurs after a series of matches, the Galil Rule allows the algorithm to "skip" over the parts of the text that have already been matched in the previous attempt.

It keeps track of the segments of the text that were involved in the partial match before a mismatch occurred. Then in the next iteration, the search resumes from the point just after the last confirmed match. By using the Galil Rule, Boyer-Moore can achieve better than linear time complexity in practical scenarios.


One of the only strengths of Boyer-Moore over [brute force]() is its' efficency when there are more characters in the text that are not in the pattern, which is more likely the larger the alphabet is.

# Knuth-Morris-Pratt (KMP)

The core idea of KMP is to preprocess the pattern $W$ to create a failure table, which'll store the length of the longest proper prefix which is also a suffix for the pattern up to that point.

```python
def computeLPSArray(pattern):
    m = len(pattern)
    lps = [0] * m
    length = 0  # length of the previous longest prefix suffix

    # the loop calculates lps[i] for i from 1 to m-1
    i = 1
    while i < m:
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length-1]
                # Note that we do not increment i here
            else:
                lps[i] = 0
                i += 1
    return lps
```

After the preprocessing, we can begin the actual KMP algorithm.

1. **Initialization:** Start with $0$ index in both text $S$ and pattern $W$.

2. **Matching:** Move both pointers forward in $S$ and $W$ as long as characters match.

3. **Mismatch?**
    - **Case 1**: If a mismatch occurs at pattern index $i$, look at $\text{lps}[i-1]$ to decide the next characters in $S$ to match with pattern's beginning characters
    - **Case 2**: If a mismatch occurs at pattern index $0$, just move the text pointer to the next position

4. **Repeat:** Repeat the process until the end of text $S$ or pattern $W$ is reached.

KMP has a much better time complexity than the [brute force]() approach or [Boyer-Moore](). The preprocessing takes time $\Theta (m)$, while the searching takes $\Theta (n)$, resulting in linear time $\Theta(n+m)$.

KMP is especially efficient when the pattern contains repeating subpatterns; it's great when the pattern or the text is large.

#### Z-Algorithm

I didn't include the Z-Algorithm since it doesn't have any benefits over [KMP](), even if it is simpler to understand.

It has a linear runtime like KMP of $\Theta (n+m)$, and space complexity of $O(n)$.

# Rabin - Karp

Rabin-Karp uses a rolling hash for its' pattern matching.

1. **Preprocessing:** Compute the hash value of the pattern $W \rightarrow hash(W)$.
2. Compute the hash value for the initial window of text $S$ of length equal to the pattern length.
3. **Searching:**
    - Slide the window one character at a time across the text. For each new window, calculate its hash value and compare it with $hash(W)$
    - If the hash values match: Verify the actual characters one by one. If all characters match, a match is found.
    - If the hash values do not match: Continue sliding the window across the text without further checks.

#### Rolling Hash
The rolling hash, effectively a [sliding window](), that works by by subtracting the leading digit, adding the trailing digit, and applying the modulo operation.

```python
def rabin_karp(text, pattern):
    # Base value for the rolling hash function
    base = 256
    
    # A prime number to modulate the hash values
    prime = 101
    
    n, m = len(text), len(pattern)
    h, p, t = 1, 0, 0  # h: hash value for base^m, p: hash value for pattern, t: hash value for text
    
    # The value of h would be "pow(d, M-1)%q"
    for i in range(m-1):
        h = (h * base) % prime
    
    # Calculate the hash value of pattern and first window of text
    for i in range(m):
        p = (base * p + ord(pattern[i])) % prime
        t = (base * t + ord(text[i])) % prime
    
    # Slide the pattern over text one by one
    for i in range(n - m + 1):
        # Check the hash values of current window of text and pattern
        if p == t:
            # If the hash values match, then only check for characters one by one
            match = True
            for j in range(m):
                if text[i + j] != pattern[j]:
                    match = False
                    break
            if match:
                print("Pattern found at index:", i)
        
        # Calculate hash value for next window of text: Remove leading digit,
        # add trailing digit
        if i < n - m:
            t = (base * (t - ord(text[i]) * h) + ord(text[i + m])) % prime
            
            # We might get negative value of t, converting it to positive
            if t < 0:
                t = t + prime

```

It runs in $\Theta(n + m)$ time, assuming that the hash function evenly distributes the strings. However, in the worst case it runs in $O(nm)$ when there are multiple collisions.

Rabin-Karp is very effective when searching for multiple pattern searches in the same text, since the hash values for the text can be reused.


# Finite Automata Algorithm

The [G.O.A.T.]() pattern matching algorithm.

#### Deterministic Finite Automaton
The DFA is constructed so that it contains a state for every possible prefix of the pattern, including the pattern itself. For each state (representing a prefix) and each possible input character, the DFA defines a transition to the next state based on the longest prefix of the pattern that is also a suffix of the string (similar to [LPS]()) formed by appending this character to the current prefix.

1. **Preprocessing**
    - Create the DFA from the pattern
2. **Searching**
    - Begin at the initial state of the DFA and process each character of the text in turn.
    - For each character, move to the next state by following the DFA's transition function.
    - If the final state (representing the entire pattern) is reached, a match is found at the current position minus the pattern length.

Finite Automata has a time complexity of $\Theta(n)$, however its' preprocessing takes time $O(m|Σ|)$, where $m$ is the length of the pattern and $|Σ|$ is the size of the input alphabet. The only drawback of finite automata is its preprocessing and the size of DFA, which is on the order of $O(m|Σ|)$.

```python
def build_dfa(pattern, alphabet):
    m = len(pattern)
    dfa = {ch: [0] * m for ch in alphabet}
    dfa[pattern[0]][0] = 1
    
    x = 0
    for j in range(1, m):
        for ch in alphabet:
            dfa[ch][j] = dfa[ch][x]  # Copy mismatch cases
        dfa[pattern[j]][j] = j + 1  # Set match case
        x = dfa[pattern[j]][x]  # Update restart state
    
    return dfa

def finite_automata_search(text, pattern):
    alphabet = set(text)  # Assuming text contains all alphabet characters
    m, n = len(pattern), len(text)
    dfa = build_dfa(pattern, alphabet)
    state = 0
    
    for i in range(n):
        state = dfa[text[i]][state]
        if state == m:
            print(f"Pattern found at index {i - m + 1}")
```

This makes finite automata extremely efficient for large texts and when the pattern is fixed, since the DFA needs to be constructed only once.

### Aho-Corsack Algorithm

I'll quickly go over Aho-Corsack, since it doesn't have too many benefits over algorithms like [KMP](), [Boyer Moore]() (using the Galil rule), or [finite automata]().

First we build a [trie]() from the set of patterns. Each node in the trie represents a character in one of the patterns, and each path from the root to a leaf represents a pattern. This trie is then enhanced with two additional types of edges:

- Failure links: These are fallback pointers. If the algorithm is currently at a node corresponding to a prefix of a pattern and the next character in the text does not match any child of this node, the failure link points to the longest suffix of this prefix that is also a prefix of any pattern.

- Output links: These point to other nodes in the trie that represent patterns that are suffixes of the current pattern. This ensures that all pattern matches are found, including those that overlap.

Once the enhanced trie is constructed, the algorithm processes the input text character by character, following trie edges that correspond to the text characters. When a character does not match any outgoing edge of the current node, the algorithm follows the failure link to continue the search. Matches are reported whenever the current node has an output link or represents the end of a pattern.

The Aho-Corsack algorithm has a runtime of $\Theta(n+m+z)$.

