### Week 3: Problem Assignments

1. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) (Easy)

```
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

2. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) (Easy)

```
    def mergeTwoLists(self, l1, l2):
        # maintain an unchanging reference to node ahead of the return node.
        prehead = ListNode(-1)

        prev = prehead
        while l1 and l2:
            if l1.val <= l2.val:
                prev.next = l1
                l1 = l1.next
            else:
                prev.next = l2
                l2 = l2.next            
            prev = prev.next

        # At least one of l1 and l2 can still have nodes at this point, so connect
        # the non-null list to the end of the merged list.
        prev.next = l1 if l1 is not None else l2

        return prehead.next
```

3. [Delete N Nodes After M Nodes of a Linked List](https://leetcode.com/problems/delete-n-nodes-after-m-nodes-of-a-linked-list/) (Easy)

```
    def deleteNodes(self, head: ListNode, m: int, n: int) -> ListNode:
        current = head
        last = head
        
        while current:
            # initialize mCount to m and nCount to n
            mcount, ncount = m, n
            
            # traverse m nodes
            while current and mcount !=0:
                last = current
                current = current.next
                mcount -=1
                
            # traverse n nodes   
            while current and ncount !=0:
                current = current.next
                ncount -=1
            
            # delete n nodes   
            last.next = current
            
        return head
```

4. [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) (Medium)

```
    def getIntersect(self, head):
        tortoise = head
        hare = head

        # A fast pointer will either loop around a cycle and meet the slow
        # pointer or reach the `null` at the end of a non-cyclic list.
        while hare is not None and hare.next is not None:
            tortoise = tortoise.next
            hare = hare.next.next
            if tortoise == hare:
                return tortoise

        return None

    def detectCycle(self, head):
        if head is None:
            return None

        # If there is a cycle, the fast/slow pointers will intersect at some
        # node. Otherwise, there is no cycle, so we cannot find an entrance to
        # a cycle.
        intersect = self.getIntersect(head)
        if intersect is None:
            return None

        # To find the entrance to the cycle, we have two pointers traverse at
        # the same speed -- one from the front of the list, and the other from
        # the point of intersection.
        ptr1 = head
        ptr2 = intersect
        while ptr1 != ptr2:
            ptr1 = ptr1.next
            ptr2 = ptr2.next

        return ptr1
```

5. [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) (Medium)

```
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        fast, slow = head, head
        
        # Advances first pointer so that the gap 
        # between first and second is n nodes apart
        for _ in range(n): 
            fast = fast.next
        
        if not fast: 
            return head.next
        
        # Move first to the end, maintaining the gap
        while fast.next: 
            fast, slow = fast.next, slow.next
        
        slow.next = slow.next.next
        return head
```

6. [Swapping Nodes in a Linked List](https://leetcode.com/problems/swapping-nodes-in-a-linked-list/) (Medium)

```
    def swapNodes(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        slow, fast = head, head
		
        for _ in range(k - 1):
            fast = fast.next
        first = fast

        while fast.next:
            slow, fast = slow.next, fast.next
		
        first.val, slow.val = slow.val, first.val

        return head
```

7. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) (Hard)

```
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        amount = len(lists)
        interval = 1
        while interval < amount:
            for i in range(0, amount - interval, interval * 2):
                lists[i] = self.merge2Lists(lists[i], lists[i + interval])
            interval *= 2
        return lists[0] if amount > 0 else None

    def merge2Lists(self, l1, l2):
        head = point = ListNode(0)
        while l1 and l2:
            if l1.val <= l2.val:
                point.next = l1
                l1 = l1.next
            else:
                point.next = l2
                l2 = l1
                l1 = point.next.next
            point = point.next
        if not l1:
            point.next=l2
        else:
            point.next=l1
        return head.next
```

