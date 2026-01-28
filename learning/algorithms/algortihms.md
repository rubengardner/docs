# 🧠 Interview Patterns Cheat Sheet

## 1️⃣ Two Pointers / Sliding Window

**Pattern:** Solve problems over arrays or strings by maintaining two pointers (start/end) or a window.

**Use When:**
* Subarrays / substrings
* Sorted arrays
* Fixed or variable size windows

**Examples:**
* Two Sum II (sorted array)
* Minimum Size Subarray Sum
* Longest Substring Without Repeating Characters

**Code Snippet (Two pointers):**
```python
# Find two numbers sum to target in sorted array
def twoSum(numbers, target):
    left, right = 0, len(numbers) - 1
    while left < right:
        s = numbers[left] + numbers[right]
        if s == target:
            return [left+1, right+1]
        elif s < target:
            left += 1
        else:
            right -= 1
```

**Code Snippet (Sliding window max length):**
```python
def lengthOfLongestSubstring(s: str) -> int:
    seen = {}
    start = 0
    max_len = 0
    for i, c in enumerate(s):
        if c in seen and seen[c] >= start:
            start = seen[c] + 1
        seen[c] = i
        max_len = max(max_len, i - start + 1)
    return max_len
```

## 2️⃣ Monotonic Stack

**Pattern:** Stack that maintains increasing/decreasing order to solve next greater/smaller element problems.

**Use When:**
* "Next greater/smaller"
* Daily temperatures, histogram problems
* Circular arrays

**Code Snippet:**
```python
def dailyTemperatures(T):
    n = len(T)
    res = [0]*n
    stack = [] # stores indices
    for i in range(n):
        while stack and T[i] > T[stack[-1]]:
            idx = stack.pop()
            res[idx] = i - idx
        stack.append(i)
    return res
```

**Circular array trick:**
```python
for i in range(2*n):
    real_i = i % n
```

## 3️⃣ Binary Search / Search Space

**Pattern:** Use binary search on index, value, or "answer space" for efficiency.

**Use When:**
* Sorted arrays
* Min/max value search
* Threshold or "can I do it in X?" problems

**Example: Koko Eating Bananas**
```python
def minEatingSpeed(piles, h):
    import math
    low, high = 1, max(piles)
    while low < high:
        mid = (low + high)//2
        hours = sum(math.ceil(p/k) for p in piles)
        if hours <= h:
            high = mid
        else:
            low = mid + 1
    return low
```

## 4️⃣ DFS / BFS (Grids & Graphs)

**Pattern:** Traverse connected components using recursion (DFS) or queue (BFS).

**Use When:**
* Number of islands, maze/path problems
* Flood fill, sub-islands
* Graph traversal

**DFS Example (Number of Islands):**
```python
def numIslands(grid):
    rows, cols = len(grid), len(grid[0])
    def dfs(r, c):
        if r < 0 or r >= rows or c < 0 or c >= cols or grid[r][c] == "0":
            return
        grid[r][c] = "0"
        dfs(r+1, c); dfs(r-1, c); dfs(r, c+1); dfs(r, c-1)
    count = 0
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == "1":
                dfs(r, c)
                count += 1
    return count
```

**BFS Example (using queue):**
```python
from collections import deque
def bfs(r, c):
    q = deque([(r,c)])
    while q:
        x, y = q.popleft()
        if 0 <= x < rows and 0 <= y < cols and grid[x][y] == "1":
            grid[x][y] = "0"
            q.extend([(x+1,y),(x-1,y),(x,y+1),(x,y-1)])
```

## 5️⃣ Dynamic Programming

**Pattern:** Solve problems by breaking them into smaller overlapping subproblems.

**Use When:**
* Optimal substructure
* Overlapping subproblems
* Recursive + memoization works

**Examples:**
* Climbing Stairs, Stone Game, House Robber, Knapsack

**Code Snippet (Memoized recursion):**
```python
from functools import lru_cache
def fib(n):
    @lru_cache(None)
    def dfs(k):
        if k <= 1: return k
        return dfs(k-1) + dfs(k-2)
    return dfs(n)
```

**Code Snippet (Bottom-up DP):**
```python
def climbStairs(n):
    dp = [0]*(n+1)
    dp[0], dp[1] = 1, 1
    for i in range(2, n+1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```

## 6️⃣ Heap / Priority Queue

**Pattern:** Use a heap to efficiently find K largest/smallest elements.

**Use When:**
* Top K, streaming median, sliding window max/min

**Python Code:**
```python
import heapq
nums = [3,2,1,5,6,4]
k = 2
min_heap = []
for n in nums:
    heapq.heappush(min_heap, n)
    if len(min_heap) > k:
        heapq.heappop(min_heap)
print(min_heap[0]) # kth largest
```

## 7️⃣ Graph / Topological Sort

**Pattern:** Detect cycles or ordering in a directed graph.

**Use When:**
* Course Schedule, prerequisites, build order
* Cycle detection: DFS + visited set / recursion stack

**Code Snippet (Topological sort + cycle detection):**
```python
from collections import defaultdict
def canFinish(numCourses, prerequisites):
    graph = defaultdict(list)
    for dest, src in prerequisites:
        graph[src].append(dest)
    visited = [0]*numCourses # 0=unvisited,1=visiting,2=visited
    def dfs(u):
        if visited[u] == 1: return False
        if visited[u] == 2: return True
        visited[u] = 1
        for v in graph[u]:
            if not dfs(v): return False
        visited[u] = 2
        return True
    return all(dfs(u) for u in range(numCourses))
```

## 8️⃣ Union-Find / Disjoint Set

**Pattern:** Track connected components efficiently.

**Use When:**
* Islands, friend circles, Kruskal's algorithm
* Merge sets, check connectivity

**Code Snippet:**
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    def union(self, x, y):
        self.parent[self.find(x)] = self.find(y)
```

## 9️⃣ Backtracking / Recursion

**Pattern:** Explore all possibilities recursively with pruning.

**Use When:**
* Subsets, permutations, N-Queens
* Constraints & choices

**Example: Generate Subsets**
```python
def subsets(nums):
    res = []
    def backtrack(path, i):
        if i == len(nums):
            res.append(path[:])
            return
        backtrack(path, i+1)
        backtrack(path + [nums[i]], i+1)
    backtrack([], 0)
    return res
```

## ⚡ Quick Reference: Pattern Recognition

| Pattern                    | Typical Problem Keyword                        |
|----------------------------|------------------------------------------------|
| Two Pointers / Sliding     | Sorted, subarray, substring                    |
| Monotonic Stack            | Next greater/smaller, daily temperatures       |
| Binary Search              | Min/Max speed, Kth element, threshold          |
| DFS / BFS                  | Grid, islands, connected components            |
| DP                         | Optimal sum, subsequence, max/min              |
| Heap / Priority Queue      | Top K, stream, max/min                         |
| Topological Sort / Graph   | Course schedule, prerequisites, build order    |
| Union-Find                 | Connected components, merge sets               |
| Backtracking / Recursion   | Subsets, permutations, combinations            |