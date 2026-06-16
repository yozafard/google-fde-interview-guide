# Python DSA Patterns Explained for Beginners

This guide explains the patterns in `python_dsa_patterns.ipynb` in beginner-friendly language. Use it before or alongside the notebook: read the idea first, then study the code template, then solve one or two problems using that template.

For problem-by-problem LeetCode notes and runnable solutions from `dsa_problem_set.md`, use `leetcode_problem_set_solutions.ipynb`.

## How to Memorise DSA Patterns

Do not memorise full solutions. Memorise the shape of the problem and the shape of the code.

For each pattern, remember four things:

1. The trigger words in the problem.
2. The data structure or pointer setup.
3. The loop condition.
4. What changes on each step.

A good interview habit is to ask: "What am I repeatedly deciding?" If you can name the repeated decision, the pattern usually becomes obvious.

## 1. Essential Data Structure Operations

### What This Pattern Is

This section is about knowing the basic tools: dictionaries, sets, arrays, strings, stacks, queues, and counters. These are not one single algorithm. They are the building blocks used inside almost every other pattern.

### Use Case

Use a dictionary when you need fast lookup by key, such as checking whether a number has appeared before. Use a set when you only care whether something exists. Use arrays and strings when order matters. Use a stack when the most recent item should be handled first. Use a queue when the oldest item should be handled first.

Common problems:

- Two Sum
- Counting frequencies
- Removing duplicates
- Valid parentheses
- Reversing strings or arrays

### How to Recognise It

Look for phrases like:

- "count occurrences"
- "find duplicates"
- "seen before"
- "matching pair"
- "first unique"
- "valid parentheses"

### How to Memorise It

Use this memory hook:

> Dict answers "what maps to what?" Set answers "have I seen this?" Stack answers "what did I open most recently?" Queue answers "what should I process next?"

If the brute force solution has a nested loop only to check whether something exists, try a dictionary or set.

## 2. Two Pointers

### What This Pattern Is

Two pointers means using two indexes to scan a structure from different positions. Usually one pointer starts at the left and the other starts at the right, or one pointer reads while the other writes.

### Use Case

Use two pointers when the problem involves a sorted array, a palindrome, reversing, partitioning, or removing items in place.

Common problems:

- Check if a string is a palindrome
- Find two numbers that sum to a target in a sorted array
- Remove duplicates from a sorted array
- Reverse an array or string in place
- Move zeroes

### How to Recognise It

Look for phrases like:

- "sorted array"
- "pair that sums to"
- "palindrome"
- "in-place"
- "remove duplicates"
- "reverse"

### How to Memorise It

Use this memory hook:

> Two pointers shrink, chase, or write.

There are three common shapes:

- Shrink: `left` starts at 0, `right` starts at the end.
- Chase: `fast` moves ahead, `slow` follows.
- Write: `read` scans every item, `write` stores only the kept items.

## 3. Sliding Window

### What This Pattern Is

Sliding window is a special form of two pointers for contiguous subarrays or substrings. You keep a window between `left` and `right`, expand it by moving `right`, and shrink it by moving `left`.

### Use Case

Use sliding window when the answer must come from a continuous block of elements.

Common problems:

- Maximum sum of subarray of size `k`
- Longest substring without repeating characters
- Longest substring with at most `k` distinct characters
- Minimum size subarray sum

### How to Recognise It

Look for phrases like:

- "subarray"
- "substring"
- "contiguous"
- "longest"
- "shortest"
- "at most k"
- "window of size k"

### How to Memorise It

Use this memory hook:

> Expand right to include. Shrink left to become valid.

Ask whether the window size is fixed or variable.

- Fixed size: add the new item, remove the item that fell out.
- Variable size: expand until invalid, then shrink until valid again.

## 4. Binary Search

### What This Pattern Is

Binary search repeatedly cuts the search space in half. It is not only for finding a number in a sorted list. It can also find the first valid position, the last valid position, or the minimum value that satisfies a condition.

### Use Case

Use binary search when the input is sorted, or when the answer space is ordered and a yes/no check becomes easier as the candidate changes.

Common problems:

- Find target in sorted array
- Find first or last occurrence
- Search insert position
- Find minimum feasible speed, capacity, or time
- Search rotated sorted array

### How to Recognise It

Look for phrases like:

- "sorted"
- "minimum possible"
- "maximum possible"
- "first occurrence"
- "last occurrence"
- "smallest value that works"
- "can you do better than O(n)?"

### How to Memorise It

Use this memory hook:

> Binary search needs an ordered question.

The key is not the array. The key is the yes/no question. For example: "Can we finish within this time?" If the answer changes from no to yes at some boundary, binary search can find that boundary.

## 5. Linked List Patterns

### What This Pattern Is

Linked list problems are mostly pointer manipulation. Instead of accessing elements by index, you move through nodes using `.next`.

### Use Case

Use linked list patterns when you need to reverse links, detect cycles, find the middle, merge lists, or remove nodes without array indexing.

Common problems:

- Reverse a linked list
- Detect a cycle
- Find the middle node
- Merge two sorted lists
- Remove nth node from end

### How to Recognise It

Look for phrases like:

- "linked list"
- "cycle"
- "reverse nodes"
- "middle node"
- "nth from end"
- "merge sorted lists"

### How to Memorise It

Use this memory hook:

> Save next before changing next.

For reversal, always remember:

```python
next_temp = curr.next
curr.next = prev
prev = curr
curr = next_temp
```

For cycle and middle problems, remember:

> Slow moves one step. Fast moves two steps.

## 6. Tree Patterns

### What This Pattern Is

Tree patterns are about visiting nodes in a structured order. Most tree problems are solved with DFS, BFS, or recursion that returns information from children to parents.

### Use Case

Use DFS when you need to go deep into branches, compute height, validate structure, or find paths. Use BFS when you need level-by-level order or shortest distance in an unweighted tree.

Common problems:

- Inorder, preorder, postorder traversal
- Maximum depth
- Level order traversal
- Validate binary search tree
- Lowest common ancestor
- Path sum

### How to Recognise It

Look for phrases like:

- "binary tree"
- "root"
- "leaf"
- "depth"
- "height"
- "level order"
- "ancestor"
- "path from root"

### How to Memorise It

Use this memory hook:

> DFS is a stack. BFS is a queue.

Remember traversal orders:

- Inorder: left, root, right. For a BST, this gives sorted order.
- Preorder: root, left, right. Useful when copying or serialising.
- Postorder: left, right, root. Useful when deleting or computing from children.

For recursion, ask: "What should this node return to its parent?"

## 7. Graph Patterns

### What This Pattern Is

Graph patterns handle nodes connected by edges. Unlike trees, graphs can have cycles, disconnected components, and multiple paths to the same node.

### Use Case

Use BFS for shortest path in an unweighted graph. Use DFS for connected components, cycle detection, and exploring all reachable nodes. Use topological sort when tasks have prerequisites.

Common problems:

- Number of islands
- Clone graph
- Course schedule
- Shortest path in unweighted graph
- Connected components
- Detect cycle

### How to Recognise It

Look for phrases like:

- "network"
- "connections"
- "neighbors"
- "grid of islands"
- "can reach"
- "prerequisites"
- "dependencies"

### How to Memorise It

Use this memory hook:

> Graph equals neighbors plus visited.

Almost every graph solution needs:

```python
graph = adjacency_list
visited = set()
```

For topological sort, remember:

> If something has zero prerequisites, do it first.

## 8. Heap / Priority Queue

### What This Pattern Is

A heap is a data structure that quickly gives you the smallest item. In Python, `heapq` is a min-heap. You can simulate a max-heap by storing negative values.

### Use Case

Use a heap when you repeatedly need the smallest, largest, or top `k` items without fully sorting everything.

Common problems:

- Kth largest element
- Top K frequent elements
- Merge K sorted lists
- Find median from data stream
- Meeting rooms

### How to Recognise It

Look for phrases like:

- "top k"
- "kth largest"
- "k closest"
- "smallest available"
- "merge k sorted"
- "stream of numbers"

### How to Memorise It

Use this memory hook:

> Heap is for repeated best-item removal.

If you only need the top `k`, keep a heap of size `k`. This avoids sorting the whole input.

## 9. Dynamic Programming

### What This Pattern Is

Dynamic programming is used when a problem can be broken into overlapping subproblems. You store answers to smaller problems so you do not recompute them.

### Use Case

Use DP when the problem asks for the best, count, or possibility across choices, and the same subproblem appears many times.

Common problems:

- Climbing stairs
- House robber
- Longest increasing subsequence
- Longest common subsequence
- Coin change
- 0/1 knapsack

### How to Recognise It

Look for phrases like:

- "maximum"
- "minimum"
- "number of ways"
- "can we form"
- "choose or skip"
- "optimal"
- "subsequence"

### How to Memorise It

Use this memory hook:

> DP is recursion with memory, or a table with meaning.

Always define:

1. State: what does `dp[i]` or `dp[i][j]` mean?
2. Transition: how do smaller states build the current state?
3. Base case: what is already known?
4. Answer: where is the final result stored?

For 1D DP, think "previous answers build current answer." For 2D DP, think "two changing inputs."

## 10. Sorting Algorithms

### What This Pattern Is

Sorting algorithms arrange data in order. In interviews, you usually do not need to reimplement sorting unless asked, but you should know how sorting changes the problem.

### Use Case

Sort when order helps you compare neighbors, use two pointers, group similar values, or simplify a messy input.

Common problems:

- Merge intervals
- Meeting rooms
- Sort colors
- Find duplicates
- Two Sum variant using sorted input

### How to Recognise It

Look for phrases like:

- "intervals"
- "overlap"
- "closest"
- "group together"
- "process in order"
- "minimum difference"

### How to Memorise It

Use this memory hook:

> Sorting buys structure for O(n log n).

After sorting, ask:

- Can I compare neighbors?
- Can I use two pointers?
- Can I merge ranges?
- Can I process from smallest to largest?

Know these basics:

- Merge sort: stable, O(n log n), uses extra space.
- Quick sort: often fast, average O(n log n), worst O(n^2).
- Python sort: Timsort, stable, O(n log n).

## 11. Trie / Prefix Tree

### What This Pattern Is

A trie stores strings character by character. Each path from the root represents a prefix or word.

### Use Case

Use a trie when many words share prefixes and you need fast prefix lookup.

Common problems:

- Implement Trie
- Word search
- Autocomplete
- Search suggestions
- Prefix matching

### How to Recognise It

Look for phrases like:

- "prefix"
- "starts with"
- "dictionary of words"
- "autocomplete"
- "word search"
- "search suggestions"

### How to Memorise It

Use this memory hook:

> Trie turns words into paths.

Each node has:

```python
children = {}
is_end = False
```

`children` says where the next character can go. `is_end` says whether the path is a complete word.

## 12. Big-O Cheat Sheet

### What This Pattern Is

Big-O describes how runtime or memory grows as input size grows. It does not measure exact seconds. It measures scaling.

### Use Case

Use Big-O to compare approaches and explain trade-offs in interviews.

Common complexities:

- O(1): constant work
- O(log n): halving the search space
- O(n): one full scan
- O(n log n): efficient sorting
- O(n^2): nested loops over the same input
- O(2^n): trying all subsets or choices

### How to Recognise It

Look at loops, recursion, and data structures:

- One loop over `n` items is usually O(n).
- Nested loops over `n` items are usually O(n^2).
- Binary search is O(log n).
- Sorting is usually O(n log n).
- Hash maps and sets are usually O(1) average for lookup.

### How to Memorise It

Use this memory hook:

> Count how many times the input can be touched.

For interviews, say both time and space complexity. If you use a dictionary, set, queue, stack, heap, or DP table, mention the extra space.

## 13. Interview Communication Template

### What This Pattern Is

This is the communication pattern for solving problems live. It helps the interviewer follow your thinking and reduces the chance that you solve the wrong problem.

### Use Case

Use it for every coding interview problem, even if the solution feels obvious.

### How to Use It

Follow this order:

1. Clarify the problem.
2. Walk through examples.
3. State the brute force idea.
4. Improve the approach using a pattern.
5. Explain time and space complexity.
6. Code clearly.
7. Test with normal and edge cases.

### How to Memorise It

Use this memory hook:

> Clarify, example, approach, code, test, complexity.

In a live interview, saying your thoughts out loud matters almost as much as writing correct code. The interviewer is evaluating how you reason, not just whether you remember a template.

## Quick Pattern Recognition Table

| Problem clue | Likely pattern |
| --- | --- |
| Sorted array, pair, palindrome | Two pointers |
| Contiguous subarray or substring | Sliding window |
| Sorted search or boundary | Binary search |
| Fast lookup or counting | Dictionary / set |
| Most recent open item | Stack |
| Level order or shortest unweighted path | BFS |
| Explore all paths or components | DFS |
| Top K or kth largest | Heap |
| Best/count/possible over choices | Dynamic programming |
| Prefix or autocomplete | Trie |
| Prerequisites or dependencies | Topological sort |
| Intervals or compare neighbors | Sorting |

## Final Study Method

For each pattern:

1. Read the explanation in this file.
2. Read the matching code in `python_dsa_patterns.ipynb`.
3. Re-type the template from memory.
4. Solve two problems using the pattern.
5. Write down the trigger words that made the pattern obvious.

The goal is not to memorise every problem. The goal is to recognise the pattern quickly and explain why it fits.

## Best Solutions for `dsa_problem_set.md`

This section gives compact, interview-ready Python solutions for the problems listed in `dsa_problem_set.md`. The goal is not to memorise every line. Learn the pattern, the key invariant, and the complexity.

### Arrays and Hashing

```python
from collections import Counter, defaultdict
import heapq

def two_sum(nums, target):
    # Hash map. Time O(n), space O(n).
    seen = {}
    for i, x in enumerate(nums):
        need = target - x
        if need in seen:
            return [seen[need], i]
        seen[x] = i
    return []

def contains_duplicate(nums):
    # Hash set. Time O(n), space O(n).
    return len(nums) != len(set(nums))

def is_anagram(s, t):
    # Frequency count. Time O(n), space O(1) for lowercase English letters.
    return Counter(s) == Counter(t)

def group_anagrams(strs):
    # Canonical key by sorted characters. Time O(n * k log k), space O(n * k).
    groups = defaultdict(list)
    for word in strs:
        groups["".join(sorted(word))].append(word)
    return list(groups.values())

def top_k_frequent(nums, k):
    # Counter + heap helper. Time O(n log k), space O(n).
    count = Counter(nums)
    return heapq.nlargest(k, count.keys(), key=count.get)

def product_except_self(nums):
    # Prefix/suffix without division. Time O(n), space O(1) extra besides output.
    res = [1] * len(nums)
    prefix = 1
    for i, x in enumerate(nums):
        res[i] = prefix
        prefix *= x
    suffix = 1
    for i in range(len(nums) - 1, -1, -1):
        res[i] *= suffix
        suffix *= nums[i]
    return res
```

### Two Pointers

```python
def is_palindrome(s):
    # Opposite-end pointers. Time O(n), space O(1).
    left, right = 0, len(s) - 1
    while left < right:
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1
        if s[left].lower() != s[right].lower():
            return False
        left += 1
        right -= 1
    return True

def two_sum_sorted(numbers, target):
    # Sorted array lets us move the pointer that is too small or too large.
    # Time O(n), space O(1). Returns 1-indexed positions.
    left, right = 0, len(numbers) - 1
    while left < right:
        total = numbers[left] + numbers[right]
        if total == target:
            return [left + 1, right + 1]
        if total < target:
            left += 1
        else:
            right -= 1
    return []

def three_sum(nums):
    # Sort + two pointers. Time O(n^2), space O(1) extra besides output.
    nums.sort()
    res = []
    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        left, right = i + 1, len(nums) - 1
        while left < right:
            total = nums[i] + nums[left] + nums[right]
            if total == 0:
                res.append([nums[i], nums[left], nums[right]])
                left += 1
                right -= 1
                while left < right and nums[left] == nums[left - 1]:
                    left += 1
                while left < right and nums[right] == nums[right + 1]:
                    right -= 1
            elif total < 0:
                left += 1
            else:
                right -= 1
    return res

def max_area(height):
    # Move the shorter wall because width always shrinks. Time O(n), space O(1).
    left, right = 0, len(height) - 1
    best = 0
    while left < right:
        best = max(best, min(height[left], height[right]) * (right - left))
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    return best

def trap(height):
    # Two pointers with left/right max. Time O(n), space O(1).
    left, right = 0, len(height) - 1
    left_max = right_max = water = 0
    while left < right:
        if height[left] < height[right]:
            left_max = max(left_max, height[left])
            water += left_max - height[left]
            left += 1
        else:
            right_max = max(right_max, height[right])
            water += right_max - height[right]
            right -= 1
    return water

def remove_duplicates(nums):
    # Write pointer keeps the next unique position. Time O(n), space O(1).
    if not nums:
        return 0
    write = 1
    for read in range(1, len(nums)):
        if nums[read] != nums[read - 1]:
            nums[write] = nums[read]
            write += 1
    return write
```

### Sliding Window and Kadane

```python
from collections import defaultdict, deque

def max_profit(prices):
    # Track lowest buy so far. Time O(n), space O(1).
    best = 0
    buy = float("inf")
    for price in prices:
        buy = min(buy, price)
        best = max(best, price - buy)
    return best

def max_sub_array(nums):
    # Kadane's algorithm. Time O(n), space O(1).
    best = curr = nums[0]
    for x in nums[1:]:
        curr = max(x, curr + x)
        best = max(best, curr)
    return best

def length_of_longest_substring(s):
    # Variable window with last-seen positions. Time O(n), space O(k).
    last = {}
    left = best = 0
    for right, ch in enumerate(s):
        if ch in last and last[ch] >= left:
            left = last[ch] + 1
        last[ch] = right
        best = max(best, right - left + 1)
    return best

def character_replacement(s, k):
    # Keep window where changes_needed <= k. Time O(n), space O(1).
    count = defaultdict(int)
    left = best = max_count = 0
    for right, ch in enumerate(s):
        count[ch] += 1
        max_count = max(max_count, count[ch])
        while (right - left + 1) - max_count > k:
            count[s[left]] -= 1
            left += 1
        best = max(best, right - left + 1)
    return best

def min_window(s, t):
    # Expand until valid, shrink to minimal. Time O(n), space O(k).
    need = Counter(t)
    missing = len(t)
    left = start = end = 0
    for right, ch in enumerate(s, 1):
        if need[ch] > 0:
            missing -= 1
        need[ch] -= 1
        if missing == 0:
            while left < right and need[s[left]] < 0:
                need[s[left]] += 1
                left += 1
            if end == 0 or right - left < end - start:
                start, end = left, right
            need[s[left]] += 1
            missing += 1
            left += 1
    return s[start:end]

def max_sliding_window(nums, k):
    # Monotonic deque stores candidate indexes. Time O(n), space O(k).
    q = deque()
    res = []
    for i, x in enumerate(nums):
        while q and q[0] <= i - k:
            q.popleft()
        while q and nums[q[-1]] <= x:
            q.pop()
        q.append(i)
        if i >= k - 1:
            res.append(nums[q[0]])
    return res
```

### Stack and Queue

```python
def is_valid_parentheses(s):
    # Stack matching. Time O(n), space O(n).
    pairs = {")": "(", "]": "[", "}": "{"}
    stack = []
    for ch in s:
        if ch in pairs.values():
            stack.append(ch)
        elif not stack or stack.pop() != pairs[ch]:
            return False
    return not stack

class MinStack:
    # Store current min beside each value. All operations O(1).
    def __init__(self):
        self.stack = []

    def push(self, val):
        curr_min = val if not self.stack else min(val, self.stack[-1][1])
        self.stack.append((val, curr_min))

    def pop(self):
        self.stack.pop()

    def top(self):
        return self.stack[-1][0]

    def getMin(self):
        return self.stack[-1][1]

def eval_rpn(tokens):
    # Stack operands. Time O(n), space O(n).
    stack = []
    for token in tokens:
        if token not in "+-*/":
            stack.append(int(token))
            continue
        b, a = stack.pop(), stack.pop()
        if token == "+":
            stack.append(a + b)
        elif token == "-":
            stack.append(a - b)
        elif token == "*":
            stack.append(a * b)
        else:
            stack.append(int(a / b))
    return stack[-1]

def daily_temperatures(temperatures):
    # Monotonic decreasing stack of indexes. Time O(n), space O(n).
    res = [0] * len(temperatures)
    stack = []
    for i, temp in enumerate(temperatures):
        while stack and temp > temperatures[stack[-1]]:
            j = stack.pop()
            res[j] = i - j
        stack.append(i)
    return res

def largest_rectangle_area(heights):
    # Monotonic stack. Time O(n), space O(n).
    stack = []
    best = 0
    for i, h in enumerate(heights + [0]):
        start = i
        while stack and stack[-1][1] > h:
            index, height = stack.pop()
            best = max(best, height * (i - index))
            start = index
        stack.append((start, h))
    return best
```

### Binary Search

```python
def search(nums, target):
    # Standard binary search. Time O(log n), space O(1).
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        if nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

def search_insert(nums, target):
    # Lower bound. Time O(log n), space O(1).
    left, right = 0, len(nums)
    while left < right:
        mid = (left + right) // 2
        if nums[mid] < target:
            left = mid + 1
        else:
            right = mid
    return left

def search_range(nums, target):
    # Two lower-bound searches. Time O(log n), space O(1).
    def lower_bound(x):
        left, right = 0, len(nums)
        while left < right:
            mid = (left + right) // 2
            if nums[mid] < x:
                left = mid + 1
            else:
                right = mid
        return left

    start = lower_bound(target)
    if start == len(nums) or nums[start] != target:
        return [-1, -1]
    return [start, lower_bound(target + 1) - 1]

def search_rotated(nums, target):
    # Identify sorted half each step. Time O(log n), space O(1).
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        if nums[left] <= nums[mid]:
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    return -1

def find_min_rotated(nums):
    # Minimum is where rotation boundary lands. Time O(log n), space O(1).
    left, right = 0, len(nums) - 1
    while left < right:
        mid = (left + right) // 2
        if nums[mid] > nums[right]:
            left = mid + 1
        else:
            right = mid
    return nums[left]

def search_matrix(matrix, target):
    # Treat matrix as flattened sorted array. Time O(log(mn)), space O(1).
    if not matrix or not matrix[0]:
        return False
    rows, cols = len(matrix), len(matrix[0])
    left, right = 0, rows * cols - 1
    while left <= right:
        mid = (left + right) // 2
        value = matrix[mid // cols][mid % cols]
        if value == target:
            return True
        if value < target:
            left = mid + 1
        else:
            right = mid - 1
    return False

def min_eating_speed(piles, h):
    # Binary search on answer. Time O(n log max(piles)), space O(1).
    left, right = 1, max(piles)
    while left < right:
        mid = (left + right) // 2
        hours = sum((pile + mid - 1) // mid for pile in piles)
        if hours <= h:
            right = mid
        else:
            left = mid + 1
    return left
```

### Linked Lists

```python
import heapq

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverse_list(head):
    # Iterative pointer reversal. Time O(n), space O(1).
    prev, curr = None, head
    while curr:
        nxt = curr.next
        curr.next = prev
        prev = curr
        curr = nxt
    return prev

def merge_two_lists(list1, list2):
    # Dummy node merge. Time O(n + m), space O(1).
    dummy = tail = ListNode()
    while list1 and list2:
        if list1.val <= list2.val:
            tail.next, list1 = list1, list1.next
        else:
            tail.next, list2 = list2, list2.next
        tail = tail.next
    tail.next = list1 or list2
    return dummy.next

def has_cycle(head):
    # Floyd fast/slow. Time O(n), space O(1).
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            return True
    return False

def detect_cycle(head):
    # After meeting, reset one pointer to head. Time O(n), space O(1).
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            slow = head
            while slow is not fast:
                slow = slow.next
                fast = fast.next
            return slow
    return None

def remove_nth_from_end(head, n):
    # Gap of n between fast and slow. Time O(n), space O(1).
    dummy = ListNode(0, head)
    fast = slow = dummy
    for _ in range(n):
        fast = fast.next
    while fast.next:
        fast = fast.next
        slow = slow.next
    slow.next = slow.next.next
    return dummy.next

def reorder_list(head):
    # Middle, reverse second half, merge alternating. Time O(n), space O(1).
    if not head or not head.next:
        return
    slow = fast = head
    while fast.next and fast.next.next:
        slow = slow.next
        fast = fast.next.next
    second = reverse_list(slow.next)
    slow.next = None
    first = head
    while second:
        tmp1, tmp2 = first.next, second.next
        first.next = second
        second.next = tmp1
        first, second = tmp1, tmp2

def merge_k_lists(lists):
    # Heap by node value. Time O(n log k), space O(k).
    heap = []
    for i, node in enumerate(lists):
        if node:
            heapq.heappush(heap, (node.val, i, node))
    dummy = tail = ListNode()
    while heap:
        _, i, node = heapq.heappop(heap)
        tail.next = node
        tail = tail.next
        if node.next:
            heapq.heappush(heap, (node.next.val, i, node.next))
    return dummy.next
```

### Trees

```python
from collections import deque

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def max_depth(root):
    # DFS height. Time O(n), space O(h).
    if not root:
        return 0
    return 1 + max(max_depth(root.left), max_depth(root.right))

def invert_tree(root):
    # Swap children recursively. Time O(n), space O(h).
    if root:
        root.left, root.right = invert_tree(root.right), invert_tree(root.left)
    return root

def is_same_tree(p, q):
    # Compare both structure and values. Time O(n), space O(h).
    if not p or not q:
        return p is q
    return p.val == q.val and is_same_tree(p.left, q.left) and is_same_tree(p.right, q.right)

def level_order(root):
    # BFS by levels. Time O(n), space O(w).
    if not root:
        return []
    q = deque([root])
    res = []
    while q:
        level = []
        for _ in range(len(q)):
            node = q.popleft()
            level.append(node.val)
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
        res.append(level)
    return res

def is_valid_bst(root):
    # Bounds recursion. Time O(n), space O(h).
    def dfs(node, low, high):
        if not node:
            return True
        if not (low < node.val < high):
            return False
        return dfs(node.left, low, node.val) and dfs(node.right, node.val, high)
    return dfs(root, float("-inf"), float("inf"))

def lowest_common_ancestor(root, p, q):
    # General binary tree LCA. Time O(n), space O(h).
    if not root or root is p or root is q:
        return root
    left = lowest_common_ancestor(root.left, p, q)
    right = lowest_common_ancestor(root.right, p, q)
    return root if left and right else left or right

def right_side_view(root):
    # Last node at each BFS level. Time O(n), space O(w).
    if not root:
        return []
    q = deque([root])
    res = []
    while q:
        for i in range(len(q)):
            node = q.popleft()
            if i == 0:
                res.append(node.val)
            if node.right:
                q.append(node.right)
            if node.left:
                q.append(node.left)
    return res

def kth_smallest(root, k):
    # Inorder traversal of BST is sorted. Time O(h + k), space O(h).
    stack = []
    node = root
    while True:
        while node:
            stack.append(node)
            node = node.left
        node = stack.pop()
        k -= 1
        if k == 0:
            return node.val
        node = node.right

def build_tree(preorder, inorder):
    # Preorder root + inorder split. Time O(n), space O(n).
    index = {value: i for i, value in enumerate(inorder)}
    pre_i = 0

    def helper(left, right):
        nonlocal pre_i
        if left > right:
            return None
        root_val = preorder[pre_i]
        pre_i += 1
        root = TreeNode(root_val)
        mid = index[root_val]
        root.left = helper(left, mid - 1)
        root.right = helper(mid + 1, right)
        return root

    return helper(0, len(inorder) - 1)

class Codec:
    # BFS serialize/deserialize. Time O(n), space O(n).
    def serialize(self, root):
        if not root:
            return ""
        q = deque([root])
        vals = []
        while q:
            node = q.popleft()
            if node:
                vals.append(str(node.val))
                q.append(node.left)
                q.append(node.right)
            else:
                vals.append("#")
        return ",".join(vals)

    def deserialize(self, data):
        if not data:
            return None
        vals = data.split(",")
        root = TreeNode(int(vals[0]))
        q = deque([root])
        i = 1
        while q:
            node = q.popleft()
            if vals[i] != "#":
                node.left = TreeNode(int(vals[i]))
                q.append(node.left)
            i += 1
            if vals[i] != "#":
                node.right = TreeNode(int(vals[i]))
                q.append(node.right)
            i += 1
        return root
```

### Sorting Review

```python
def merge_sort(arr):
    # Stable divide and conquer. Time O(n log n), space O(n).
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    res = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            res.append(left[i])
            i += 1
        else:
            res.append(right[j])
            j += 1
    return res + left[i:] + right[j:]

def quick_sort(arr):
    # In-place quicksort. Average O(n log n), worst O(n^2), space O(log n).
    def partition(low, high):
        pivot = arr[high]
        i = low
        for j in range(low, high):
            if arr[j] <= pivot:
                arr[i], arr[j] = arr[j], arr[i]
                i += 1
        arr[i], arr[high] = arr[high], arr[i]
        return i

    def sort(low, high):
        if low < high:
            p = partition(low, high)
            sort(low, p - 1)
            sort(p + 1, high)

    sort(0, len(arr) - 1)
    return arr

def sort_colors(nums):
    # Dutch National Flag. Time O(n), space O(1).
    low = mid = 0
    high = len(nums) - 1
    while mid <= high:
        if nums[mid] == 0:
            nums[low], nums[mid] = nums[mid], nums[low]
            low += 1
            mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:
            nums[mid], nums[high] = nums[high], nums[mid]
            high -= 1

def merge_intervals(intervals):
    # Sort by start, merge overlaps. Time O(n log n), space O(n).
    intervals.sort()
    merged = []
    for start, end in intervals:
        if not merged or start > merged[-1][1]:
            merged.append([start, end])
        else:
            merged[-1][1] = max(merged[-1][1], end)
    return merged
```

### Graphs

```python
from collections import defaultdict, deque

def num_islands(grid):
    # DFS flood fill. Time O(mn), space O(mn) worst case.
    if not grid:
        return 0
    rows, cols = len(grid), len(grid[0])
    count = 0

    def dfs(r, c):
        if r < 0 or c < 0 or r == rows or c == cols or grid[r][c] != "1":
            return
        grid[r][c] = "0"
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == "1":
                count += 1
                dfs(r, c)
    return count

class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

def clone_graph(node):
    # DFS with old-node to new-node map. Time O(V + E), space O(V).
    if not node:
        return None
    clones = {}

    def dfs(curr):
        if curr in clones:
            return clones[curr]
        copy = Node(curr.val)
        clones[curr] = copy
        copy.neighbors = [dfs(nei) for nei in curr.neighbors]
        return copy

    return dfs(node)

def pacific_atlantic(heights):
    # Reverse flow from oceans inward. Time O(mn), space O(mn).
    rows, cols = len(heights), len(heights[0])
    pac, atl = set(), set()

    def dfs(r, c, seen, prev):
        if (r, c) in seen or r < 0 or c < 0 or r == rows or c == cols or heights[r][c] < prev:
            return
        seen.add((r, c))
        for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
            dfs(r + dr, c + dc, seen, heights[r][c])

    for c in range(cols):
        dfs(0, c, pac, heights[0][c])
        dfs(rows - 1, c, atl, heights[rows - 1][c])
    for r in range(rows):
        dfs(r, 0, pac, heights[r][0])
        dfs(r, cols - 1, atl, heights[r][cols - 1])
    return list(pac & atl)

def can_finish(num_courses, prerequisites):
    # Kahn topological sort. Time O(V + E), space O(V + E).
    graph = defaultdict(list)
    indegree = [0] * num_courses
    for course, prereq in prerequisites:
        graph[prereq].append(course)
        indegree[course] += 1
    q = deque([i for i, deg in enumerate(indegree) if deg == 0])
    taken = 0
    while q:
        node = q.popleft()
        taken += 1
        for nei in graph[node]:
            indegree[nei] -= 1
            if indegree[nei] == 0:
                q.append(nei)
    return taken == num_courses

def find_order(num_courses, prerequisites):
    # Same as Course Schedule, but return the order.
    graph = defaultdict(list)
    indegree = [0] * num_courses
    for course, prereq in prerequisites:
        graph[prereq].append(course)
        indegree[course] += 1
    q = deque([i for i, deg in enumerate(indegree) if deg == 0])
    order = []
    while q:
        node = q.popleft()
        order.append(node)
        for nei in graph[node]:
            indegree[nei] -= 1
            if indegree[nei] == 0:
                q.append(nei)
    return order if len(order) == num_courses else []

def count_components(n, edges):
    # Union find. Time near O(V + E), space O(V).
    parent = list(range(n))
    rank = [0] * n

    def find(x):
        while x != parent[x]:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        ra, rb = find(a), find(b)
        if ra == rb:
            return False
        if rank[ra] < rank[rb]:
            parent[ra] = rb
        elif rank[ra] > rank[rb]:
            parent[rb] = ra
        else:
            parent[rb] = ra
            rank[ra] += 1
        return True

    components = n
    for a, b in edges:
        if union(a, b):
            components -= 1
    return components

def valid_tree(n, edges):
    # A tree has n - 1 edges and one connected component.
    return len(edges) == n - 1 and count_components(n, edges) == 1

def ladder_length(begin_word, end_word, word_list):
    # BFS over wildcard buckets. Time O(n * m^2), space O(n * m).
    words = set(word_list)
    if end_word not in words:
        return 0
    buckets = defaultdict(list)
    for word in words | {begin_word}:
        for i in range(len(word)):
            buckets[word[:i] + "*" + word[i + 1:]].append(word)
    q = deque([(begin_word, 1)])
    seen = {begin_word}
    while q:
        word, dist = q.popleft()
        if word == end_word:
            return dist
        for i in range(len(word)):
            for nei in buckets[word[:i] + "*" + word[i + 1:]]:
                if nei not in seen:
                    seen.add(nei)
                    q.append((nei, dist + 1))
    return 0
```

### Heaps

```python
import heapq
from collections import Counter

def find_kth_largest(nums, k):
    # Min-heap of size k. Time O(n log k), space O(k).
    heap = nums[:k]
    heapq.heapify(heap)
    for x in nums[k:]:
        if x > heap[0]:
            heapq.heapreplace(heap, x)
    return heap[0]

def k_closest(points, k):
    # Sort is simple and strong for interviews. Time O(n log n), space O(n).
    return sorted(points, key=lambda p: p[0] * p[0] + p[1] * p[1])[:k]

class MedianFinder:
    # Max heap for lower half, min heap for upper half. Add O(log n), find O(1).
    def __init__(self):
        self.low = []
        self.high = []

    def addNum(self, num):
        heapq.heappush(self.low, -num)
        heapq.heappush(self.high, -heapq.heappop(self.low))
        if len(self.high) > len(self.low):
            heapq.heappush(self.low, -heapq.heappop(self.high))

    def findMedian(self):
        if len(self.low) > len(self.high):
            return -self.low[0]
        return (-self.low[0] + self.high[0]) / 2

def least_interval(tasks, n):
    # Greedy formula. Time O(t), space O(1).
    counts = Counter(tasks).values()
    max_count = max(counts)
    num_max = sum(1 for c in counts if c == max_count)
    return max(len(tasks), (max_count - 1) * (n + 1) + num_max)
```

### Tries

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.word = None
        self.is_end = False

class Trie:
    # Insert/search/prefix are O(word length).
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
        node.is_end = True

    def search(self, word):
        node = self._find(word)
        return bool(node and node.is_end)

    def startsWith(self, prefix):
        return self._find(prefix) is not None

    def _find(self, text):
        node = self.root
        for ch in text:
            if ch not in node.children:
                return None
            node = node.children[ch]
        return node

class WordDictionary:
    # Trie + DFS for '.' wildcard.
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word):
        node = self.root
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
        node.is_end = True

    def search(self, word):
        def dfs(i, node):
            if i == len(word):
                return node.is_end
            ch = word[i]
            if ch == ".":
                return any(dfs(i + 1, child) for child in node.children.values())
            return ch in node.children and dfs(i + 1, node.children[ch])
        return dfs(0, self.root)

def find_words(board, words):
    # Trie prunes backtracking. Time bounded by board paths, space O(total word chars).
    root = TrieNode()
    for word in words:
        node = root
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
        node.word = word

    rows, cols = len(board), len(board[0])
    res = []

    def dfs(r, c, node):
        if r < 0 or c < 0 or r == rows or c == cols:
            return
        ch = board[r][c]
        if ch == "#" or ch not in node.children:
            return
        nxt = node.children[ch]
        if nxt.word:
            res.append(nxt.word)
            nxt.word = None
        board[r][c] = "#"
        dfs(r + 1, c, nxt)
        dfs(r - 1, c, nxt)
        dfs(r, c + 1, nxt)
        dfs(r, c - 1, nxt)
        board[r][c] = ch

    for r in range(rows):
        for c in range(cols):
            dfs(r, c, root)
    return res
```

### Dynamic Programming

```python
from bisect import bisect_left

def climb_stairs(n):
    # Fibonacci-style DP. Time O(n), space O(1).
    if n <= 2:
        return n
    a, b = 1, 2
    for _ in range(3, n + 1):
        a, b = b, a + b
    return b

def rob(nums):
    # Choose rob current + prev2, or skip current. Time O(n), space O(1).
    prev2 = prev1 = 0
    for x in nums:
        prev2, prev1 = prev1, max(prev1, prev2 + x)
    return prev1

def coin_change(coins, amount):
    # Unbounded knapsack min coins. Time O(amount * coins), space O(amount).
    dp = [float("inf")] * (amount + 1)
    dp[0] = 0
    for total in range(1, amount + 1):
        for coin in coins:
            if total >= coin:
                dp[total] = min(dp[total], dp[total - coin] + 1)
    return dp[amount] if dp[amount] != float("inf") else -1

def length_of_lis(nums):
    # Patience sorting tails. Time O(n log n), space O(n).
    tails = []
    for x in nums:
        i = bisect_left(tails, x)
        if i == len(tails):
            tails.append(x)
        else:
            tails[i] = x
    return len(tails)

def unique_paths(m, n):
    # 2D paths optimized to 1D. Time O(mn), space O(n).
    dp = [1] * n
    for _ in range(1, m):
        for c in range(1, n):
            dp[c] += dp[c - 1]
    return dp[-1]

def longest_common_subsequence(text1, text2):
    # 2D DP optimized to 1D. Time O(mn), space O(n).
    dp = [0] * (len(text2) + 1)
    for a in text1:
        prev = 0
        for j, b in enumerate(text2, 1):
            temp = dp[j]
            if a == b:
                dp[j] = prev + 1
            else:
                dp[j] = max(dp[j], dp[j - 1])
            prev = temp
    return dp[-1]

def word_break(s, word_dict):
    # dp[i] means s[:i] can be segmented. Time O(n^2), space O(n).
    words = set(word_dict)
    dp = [False] * (len(s) + 1)
    dp[0] = True
    for i in range(1, len(s) + 1):
        for j in range(i):
            if dp[j] and s[j:i] in words:
                dp[i] = True
                break
    return dp[-1]

def min_distance(word1, word2):
    # Edit distance. Time O(mn), space O(n).
    prev = list(range(len(word2) + 1))
    for i, a in enumerate(word1, 1):
        curr = [i] + [0] * len(word2)
        for j, b in enumerate(word2, 1):
            if a == b:
                curr[j] = prev[j - 1]
            else:
                curr[j] = 1 + min(prev[j], curr[j - 1], prev[j - 1])
        prev = curr
    return prev[-1]
```
