# Week 4: Graphs, Stacks & Queues

Welcome to the fourth week of our LeetCode Bootcamp. This week, we will dive into Graphs, Stacks and Queues in Python, alongside introducing powerful problem-solving patterns.

## Class Agenda (2 Hours)

### 1. Python Overview of Graphs, Stacks & Queues

Please review the following resources:

- [Programiz: Graph](https://www.programiz.com/dsa/graph)
- [Programiz: Stack](https://www.programiz.com/dsa/stack)
- [Programiz: Queue](https://www.programiz.com/dsa/queue)
- [Python.org: Using Lists as Stacks & Queues](https://docs.python.org/3/tutorial/datastructures.html#using-lists-as-stacks)
- [Python.org: Collections Deque](https://docs.python.org/3/library/collections.html#deque-objects)

### 2. Problems Covered This Week

- [Find the town judge](https://leetcode.com/problems/find-the-town-judge/description/)(Easy)

```python
class Solution:
    def findJudge(self, n: int, trust: List[List[int]]) -> int:
        if len(trust) < N - 1:
            return -1
        
        trusts = [0] * (n + 1) # outgoing edges / outdegree
        trusted_by = [0] * (n + 1) # indegree / incoming edges
        
        for a, b in trust:
            trusts[a] += 1
            trusted_by[b] += 1
        
        for i in range(1, n + 1):
            if trusts[i] == 0 and trusted_by[i] == n - 1:
                return i
        
        return -1

# Time Complexity: O(n + t), where n is the number of people and t is the length of the trust array. We iterate over the trust array once to update the trusts and trusted_by arrays, and then we iterate from 1 to n to find the town judge.

# Space Complexity: O(n), where n is the number of people. We use two arrays of size n + 1 to keep track of the trust relationships.

        if len(trust) < N - 1:
            return -1
        
        trusted_by = [0] * (n + 1)
        
        for a, b in trust:
            trusted_by[a] -= 1
            trusted_by[b] += 1
        
        for i in range(1, n + 1):
            if trusted_by[i] == n - 1:
                return i
        
        return -1

# Time Complexity: O(n + t), where n is the number of people and t is the length of the trust array. We iterate over the trust array once to update the trusted_by array, and then we iterate from 1 to n to find the town judge.

# Space Complexity: O(n), where n is the number of people. We use an array of size n + 1 to keep track of the net trust scores.
```

- [Number of islands](https://leetcode.com/problems/number-of-islands/description/)(Medium)

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid or not grid[0]:
            return 0

        m, n = len(grid), len(grid[0])
        parent = [i for i in range(m * n)]
        rank = [0] * (m * n)

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        def union(x, y):
            root_x, root_y = find(x), find(y)
            if root_x != root_y:
                if rank[root_x] < rank[root_y]:
                    parent[root_x] = root_y
                elif rank[root_x] > rank[root_y]:
                    parent[root_y] = root_x
                else:
                    parent[root_y] = root_x
                    rank[root_x] += 1

        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    if i > 0 and grid[i-1][j] == '1':
                        union(i * n + j, (i-1) * n + j)
                    if j > 0 and grid[i][j-1] == '1':
                        union(i * n + j, i * n + j - 1)

        islands = set()
        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    islands.add(find(i * n + j))

        return len(islands)

# Time Complexity: O(m * n), where m and n are the dimensions of the grid.

# Space Complexity: O(m * n) to store the parent and rank arrays.
```

- [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)(Easy)

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []

        mapping = {")": "(", "}": "{", "]": "["}

        for char in s:
            if char in mapping:

                top_element = stack.pop() if stack else '#'

                if mapping[char] != top_element:
                    return False
            else:
                stack.append(char)
                
        return not stack

# Time complexity : O(n) because we simply traverse the given string one character at a time and push and pop operations on a stack take O(1) time.

# Space complexity : O(n) as we push all opening brackets onto the stack and in the worst case, we will end up pushing all the brackets onto the stack. e.g. ((((((((((.
```

- [Implement Queue using stacks](https://leetcode.com/problems/implement-queue-using-stacks/description/)(Easy)

```python
class MyQueue:
    def __init__(self):
        self.stack1 = []  # Main stack
        self.stack2 = []  # Auxiliary stack

    def push(self, x: int) -> None:
        # Move all elements from stack1 to stack2
        while self.stack1:
            self.stack2.append(self.stack1.pop())
        
        # Push the new element to stack1
        self.stack1.append(x)
        
        # Move all elements back to stack1
        while self.stack2:
            self.stack1.append(self.stack2.pop())

    def pop(self) -> int:
        if not self.empty():
            return self.stack1.pop()

    def peek(self) -> int:
        if not self.empty():
            return self.stack1[-1]

    def empty(self) -> bool:
        return len(self.stack1) == 0

# Time Complexity:

# Push: O(n) per operation, where n is the number of elements in the queue.
# Pop and Peek: O(1) per operation.
# Empty: O(1) per operation.

# Space Complexity: O(n), where n is the number of elements in the queue.
    def __init__(self):
        self.input_stack = []   # For push operation
        self.output_stack = []  # For pop and peek operations

    def push(self, x: int) -> None:
        self.input_stack.append(x)

    def pop(self) -> int:
        self.peek()
        return self.output_stack.pop()

    def peek(self) -> int:
        if not self.output_stack:
            while self.input_stack:
                self.output_stack.append(self.input_stack.pop())
        if self.output_stack:
            return self.output_stack[-1]

    def empty(self) -> bool:
        return len(self.input_stack) == 0 and len(self.output_stack) == 0

# Time Complexity:

# Push: O(1) per operation.
# Pop and Peek: Amortized O(1) per operation. In the worst case, when output_stack is empty, we need to transfer all elements from input_stack to output_stack, which takes O(n) time. However, this happens rarely, and on average, each element is transferred from input_stack to output_stack only once, resulting in an amortized time complexity of O(1).
# Empty: O(1) per operation.

# Space Complexity: O(n), where n is the number of elements in the queue.
```

- [Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/description/)

```python
class Solution:
    def deckRevealedIncreasing(self, deck: List[int]) -> List[int]:
        N = len(deck)
        index = collections.deque(range(N))
        ans = [None] * N

        for card in sorted(deck):
            print(card)
            ans[index.popleft()] = card
            print(ans)
            if index:
                index.append(index.popleft())
                print(index)

        return ans

# Time Complexity: O(Nlog⁡N), where N is the length of deck.

# Space Complexity: O(N).
```

## Take-Home Problems

To help solidify your understanding and practice further, here are some take-home problems:

1. [Bus routes](https://leetcode.com/problems/bus-routes/description/)(Hard)
2. [Decode String](https://leetcode.com/problems/decode-string/description/)(Medium)
3. [Number of people aware of a secret](https://leetcode.com/problems/number-of-people-aware-of-a-secret/description/)(Medium)

Good luck, and happy coding!
