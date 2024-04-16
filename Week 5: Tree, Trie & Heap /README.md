# Week 5: Tree, Trie & Heap

Welcome to the fifth week of our LeetCode Bootcamp. This week, we will dive into Tree, Trie & Heap in Python, alongside introducing powerful problem-solving patterns.

## Class Agenda (2 Hours)

### 1. Python Overview of Tree, Trie & Heap

Please review the following resources:

- [Programiz: Trees](https://www.programiz.com/dsa/trees)
- [Trie Implementation](https://towardsdatascience.com/implementing-a-trie-data-structure-in-python-in-less-than-100-lines-of-code-a877ea23c1a1)
- [Programiz: Heap](https://www.programiz.com/dsa/heap-data-structure)
- [Python: heapq Module](https://docs.python.org/3/library/heapq.html)
- [Recursion vs Iteration](https://clouddevs.com/python/recursion-and-iteration/)

### 2. Problems Covered This Week

- [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)(Easy)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        # Approach 1: Recursive
        # Base case: if the node is null, return null to signify no node to process
        if not root:
            return None
        
        # Recursively invert the right subtree
        right = self.invertTree(root.right)
        # Recursively invert the left subtree
        left = self.invertTree(root.left)
        
        # Swap the left and right children of the current node
        root.left = right
        root.right = left
        
        # Return the current node as the new root of the subtree
        return root

# Complexity Analysis:

# Time Complexity: O(n) where n is the number of nodes in the tree. Each node is visited once.

# Space Complexity: O(h) where h is the height of the tree. This space is used in the call stack due to recursion. In the worst case of an unbalanced tree, this can be O(n).





        # Approach 2: Iterative
        # Base case: if the tree is empty, return null
        if not root:
            return None
        
        # Initialize a queue for level-order traversal
        queue = collections.deque([root])
        while queue:
            # Pop the first node in the queue
            current = queue.popleft()
            
            # Swap the left and right children of the current node
            current.left, current.right = current.right, current.left
            
            # If the left child exists, add it to the queue for later processing
            if current.left:
                queue.append(current.left)
            
            # If the right child exists, add it to the queue for later processing
            if current.right:
                queue.append(current.right)
        
        # Return the root of the inverted tree
        return root

# Complexity Analysis:

# Time Complexity: O(n), since each node is processed exactly once.

# Space Complexity: O(w) where w is the maximum width of the tree, which corresponds to the maximum size of the queue. 
```

- [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)(Medium)

```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:

        # Initialize an empty list to hold the levels of nodes.
        levels = []
        
        if not root:
            return levels
        
        def helper(node, level):
            # If this is the first time we've reached this level,
            # we need to add a new list for this level in the 'levels' list.
            if len(levels) == level:
                levels.append([])

            # Append the current node's value to the list corresponding to its level.
            levels[level].append(node.val)

            # If there is a left child, process it for the next level.
            if node.left:
                helper(node.left, level + 1)
            # If there is a right child, process it for the next level.
            if node.right:
                helper(node.right, level + 1)
        
        # Start the recursive process with the root node at level 0.
        helper(root, 0)
        # Return the list of levels, with each level containing the values of its nodes.
        return levels

        # The above recursion can be transformed into an iterative process using a queue.
        # Instead of a recursion stack, we use a queue to keep track of nodes and their level.

        # Reset levels for the iterative solution.
        levels = []
        if not root:
            return levels
        
        # The queue will hold tuples of (node, level).
        queue = deque([(root, 0)])
        while queue:
            # Get the next node and its level from the queue.
            node, level = queue.popleft()
            # If the level is not present in the levels list, add a new list for it.
            if len(levels) == level:
                levels.append([])

            # Add the current node's value to its level list.
            levels[level].append(node.val)
            
            # Add the child nodes and their level to the queue.
            if node.left:
                queue.append((node.left, level + 1))
            if node.right:
                queue.append((node.right, level + 1))
        
        # After processing all nodes, return the levels list.
        return levels

```

- [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/description/)(Medium)

```python
class Trie:

    def __init__(self):
        self.root = {}

    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node:
                node[char] = {}
            node = node[char]
        node['#'] = True

    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node:
                return False
            node = node[char]
        return '#' in node

    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            if char not in node:
                return False
            node = node[char]
        return True


# The time complexity of the insert, search, and startsWith operations is O(m), where m is the length of the word or prefix. 

# The space complexity is O(n * m), where n is the total number of words inserted into the trie.       
```

- [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/description/)(Medium)

```python
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        # Create a max-heap with negative distances
        heap = [(-self.distance(point), point) for point in points[:k]]
        heapq.heapify(heap)

        # Process the remaining points
        for point in points[k:]:
            dist = -self.distance(point)
            if dist > heap[0][0]:
                heapq.heappushpop(heap, (dist, point))

        # Extract the k closest points from the heap
        return [point for _, point in heap]

    def distance(self, point: List[int]) -> float:
        return point[0] ** 2 + point[1] ** 2

# Time complexity of this solution is O(n log k), where n is the number of points and k is the number of closest points to return. 

# Space complexity is O(k) for storing the heap.
```

## Take-Home Problems

To help solidify your understanding and practice further, here are some take-home problems:

1. [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)(Medium)
2. [Word Break](https://leetcode.com/problems/word-break/description/)(Medium)
3. [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)(Medium)

Good luck, and happy coding!
