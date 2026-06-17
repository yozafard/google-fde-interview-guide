# Coding Section Cheatsheet

Use this as a one-page reference before mock interviews and live coding rounds. The goal is not to memorise every line, but to quickly recognise the pattern, state the trade-off, and code a clean first solution.

## Related Obsidian Links

- [[README]]
- [[python_dsa_patterns_explained]]
- [[dsa_problem_set]]
- [[leetcode_problem_set_solutions]]
- [[python_dsa_patterns]]

## Interview Flow

1. Clarify input, output, constraints, duplicates, sorting, mutability, and edge cases.
2. Say the brute force idea and complexity.
3. Identify the pattern and explain why it improves the brute force.
4. Code slowly enough to avoid off-by-one errors.
5. Test with empty, one-element, duplicate, negative, and boundary cases.
6. State final time and space complexity.

## Pattern Picker

| Clue in Problem | Likely Pattern | Useful Structure | Complexity Target |
|---|---|---|---|
| Need fast lookup, count, grouping, duplicates | Hash map / set | `dict`, `set`, `Counter` | `O(n)` |
| Sorted array, pair/triplet, palindrome | Two pointers | `left`, `right` | `O(n)` |
| Longest/shortest contiguous subarray or substring | Sliding window | window counts + `left` | `O(n)` |
| Sorted search, minimum valid value, capacity/speed | Binary search | `lo`, `hi`, predicate | `O(log n)` or `O(n log k)` |
| Tree path, connected components, reachability | DFS / BFS | stack, recursion, queue | `O(V + E)` |
| Top K, merge sorted streams, repeated min/max | Heap | `heapq` | `O(n log k)` |
| Best/number/min ways with overlapping choices | DP | memo or table | varies |

## Python Imports

```python
from collections import Counter, defaultdict, deque
import heapq
from functools import lru_cache
```

## Hash Map / Set

Use when the question asks whether something was seen before, how many times it appears, or where it appeared.

```python
def two_sum(nums, target):
    # Store numbers already seen with their indexes.
    seen = {}

    # Scan once so each lookup is O(1) average time.
    for i, x in enumerate(nums):
        # Check whether the complement already appeared.
        need = target - x
        if need in seen:
            return [seen[need], i]

        # Save the current number for future complements.
        seen[x] = i

    return []
```

```python
def group_anagrams(words):
    # Map each sorted-letter signature to matching words.
    groups = defaultdict(list)

    for word in words:
        # Anagrams share the same sorted character tuple.
        key = tuple(sorted(word))
        groups[key].append(word)

    return list(groups.values())
```

## Two Pointers

Use when the data is sorted or when comparing from both ends removes impossible choices.

```python
def two_sum_sorted(nums, target):
    # Start at both ends of the sorted array.
    left, right = 0, len(nums) - 1

    while left < right:
        # Compare the current pair.
        total = nums[left] + nums[right]
        if total == target:
            return [left, right]

        # Move left up when the sum is too small.
        if total < target:
            left += 1

        # Move right down when the sum is too large.
        else:
            right -= 1

    return []
```

```python
def is_palindrome(s):
    # Compare valid characters from both ends.
    left, right = 0, len(s) - 1

    while left < right:
        # Skip non-alphanumeric characters on the left.
        while left < right and not s[left].isalnum():
            left += 1

        # Skip non-alphanumeric characters on the right.
        while left < right and not s[right].isalnum():
            right -= 1

        # Compare normalized characters.
        if s[left].lower() != s[right].lower():
            return False

        # Move inward after a match.
        left += 1
        right -= 1

    return True
```

## Sliding Window

Use for contiguous ranges. Expand with `right`, shrink with `left` until the window is valid.

```python
def longest_unique_substring(s):
    # Keep the current window's characters.
    seen = set()
    left = 0
    best = 0

    # Expand the window one character at a time.
    for right, ch in enumerate(s):
        # Shrink until the duplicate is removed.
        while ch in seen:
            seen.remove(s[left])
            left += 1

        # Add the new valid character.
        seen.add(ch)

        # Update the best valid window length.
        best = max(best, right - left + 1)

    return best
```

```python
def min_subarray_len(nums, target):
    # Track a running window sum.
    left = 0
    total = 0
    best = float("inf")

    # Expand the right edge.
    for right, x in enumerate(nums):
        total += x

        # Shrink while the window still satisfies the target.
        while total >= target:
            best = min(best, right - left + 1)
            total -= nums[left]
            left += 1

    # Return 0 when no valid window exists.
    return 0 if best == float("inf") else best
```

## Binary Search

Use when the answer space is ordered. For "minimum valid X", move left when the predicate is true.

```python
def binary_search(nums, target):
    # Search the inclusive range [lo, hi].
    lo, hi = 0, len(nums) - 1

    while lo <= hi:
        # Check the middle element.
        mid = (lo + hi) // 2
        if nums[mid] == target:
            return mid

        # Discard the left half.
        if nums[mid] < target:
            lo = mid + 1

        # Discard the right half.
        else:
            hi = mid - 1

    return -1
```

```python
def lower_bound(nums, target):
    # Search the first index where nums[i] >= target.
    lo, hi = 0, len(nums)

    while lo < hi:
        mid = (lo + hi) // 2

        # Target must be to the right.
        if nums[mid] < target:
            lo = mid + 1

        # Mid could be the answer, so keep it.
        else:
            hi = mid

    return lo
```

## DFS / BFS

Use DFS for paths and exhaustive traversal. Use BFS for shortest path in an unweighted graph.

```python
def dfs_graph(graph, start):
    # Track nodes already visited.
    seen = set()

    def dfs(node):
        # Stop if this node was already processed.
        if node in seen:
            return

        # Mark before visiting neighbors to avoid cycles.
        seen.add(node)

        # Recursively explore neighbors.
        for nei in graph[node]:
            dfs(nei)

    # Start traversal from the given node.
    dfs(start)
    return seen
```

```python
def shortest_path(graph, start, target):
    # Queue stores node plus distance from start.
    q = deque([(start, 0)])
    seen = {start}

    while q:
        # Process nodes in increasing distance order.
        node, dist = q.popleft()
        if node == target:
            return dist

        # Visit unseen neighbors once.
        for nei in graph[node]:
            if nei not in seen:
                seen.add(nei)
                q.append((nei, dist + 1))

    # Target was unreachable.
    return -1
```

## Heap

Use a heap when you repeatedly need the smallest or largest item without sorting everything.

```python
def top_k_frequent(nums, k):
    # Count each number.
    counts = Counter(nums)

    # Min-heap keeps only the current top k.
    heap = []

    for num, freq in counts.items():
        # Push by frequency so the smallest frequency is removable.
        heapq.heappush(heap, (freq, num))

        # Remove the weakest candidate if heap grows too large.
        if len(heap) > k:
            heapq.heappop(heap)

    # Extract the remaining top-k numbers.
    return [num for freq, num in heap]
```

## Dynamic Programming

Use DP when choices repeat the same subproblems. Define the state before coding.

```python
def climb_stairs(n):
    # Handle the smallest states directly.
    if n <= 2:
        return n

    # prev2 = ways to reach i - 2, prev1 = ways to reach i - 1.
    prev2, prev1 = 1, 2

    # Current ways = previous one-step + previous two-step.
    for _ in range(3, n + 1):
        prev2, prev1 = prev1, prev1 + prev2

    return prev1
```

```python
def coin_change(coins, amount):
    # dp[x] = fewest coins needed to make total x.
    dp = [float("inf")] * (amount + 1)

    # Zero coins are needed to make total 0.
    dp[0] = 0

    for total in range(1, amount + 1):
        for coin in coins:
            # Try using this coin as the last coin.
            if total >= coin:
                dp[total] = min(dp[total], dp[total - coin] + 1)

    # Convert impossible totals back to -1.
    return -1 if dp[amount] == float("inf") else dp[amount]
```

## Complexity Phrases

- Hash map scan: `O(n)` time, `O(n)` space.
- Two pointers: `O(n)` time, usually `O(1)` space.
- Sorting first: `O(n log n)` time.
- BFS / DFS: `O(V + E)` for graphs, `O(n)` for trees.
- Heap top K: `O(n log k)` time, `O(k)` space.
- DP: number of states times work per state.

## Edge Cases To Say Out Loud

- Empty input.
- One item.
- All duplicates.
- No valid answer.
- Multiple valid answers.
- Negative numbers.
- Already sorted or reverse sorted input.
- Very large input and recursion depth.
