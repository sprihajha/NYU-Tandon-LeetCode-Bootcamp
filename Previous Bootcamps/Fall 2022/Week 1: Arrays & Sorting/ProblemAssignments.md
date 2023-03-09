### Week 1: Problem Assignments

1. [Running Sum of 1d Array](https://leetcode.com/problems/running-sum-of-1d-array/) (Easy)

```
    def runningSum(self, nums: List[int]) -> List[int]:
        
        # Iterate through nums 1d Array
        for i in range(1, len(nums)):
            # Result at index `i` is sum of result at `i-1` and element at `i`.
            nums[i] += nums[i - 1]
            
        return nums
```

2. [Long Pressed Name](https://leetcode.com/problems/long-pressed-name/) (Easy)

```
    def isLongPressedName(self, name: str, typed: str) -> bool:
        # Find the length of the name and typed inpute strings
        name_length = len(name)
        typed_length = len(typed)
        
        if name_length > typed_length or name[0] != typed[0]:
            return False
        
        i = 0
        for j in range(typed_length):
            if i < name_length and name[i] == typed[j]:
                i += 1
            elif j == 0 or typed[j] != typed[j-1]:
                return False
        return i == name_length
```

3. [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) (Easy)

```
    def containsDuplicate(self, nums: List[int]) -> bool:
        # Track the unique numbers in the list
        numSet = set()

        # Iterate through the list to check for duplicates
        for x in nums: 
            if x in numSet:
                return True
            numSet.add(x);

        return False
```

4. [Video Stitching](https://leetcode.com/problems/video-stitching/) (Medium)

```
    def videoStitching(self, clips: List[List[int]], time: int) -> int:
        clips.sort()
        start = end = 0
        result = 0
        i = 0
        
        while start < time:
            while i < len(clips) and clips[i][0] <= start:
                end = max(end, clips[i][1])
                i += 1
            if end == start:
                return -1
            result += 1
            start = end
        return result
```

5. [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/) (Medium)

```
    def maxProduct(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0

        max_so_far = nums[0]
        min_so_far = nums[0]
        result = max_so_far

        for i in range(1, len(nums)):
            curr = nums[i]
            temp_max = max(curr, max_so_far * curr, min_so_far * curr)
            min_so_far = min(curr, max_so_far * curr, min_so_far * curr)

            max_so_far = temp_max

            result = max(max_so_far, result)

        return result
```

6. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) (Medium)

```
    def maxArea(self, height: List[int]) -> int:
        maxarea = 0
        left = 0
        right = len(height) - 1
        
        while left < right:
            width = right - left
            maxarea = max(maxarea, min(height[left], height[right]) * width)
            if height[left] <= height[right]:
                left += 1
            else:
                right -= 1
                
        return maxarea
```

7. [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) (Hard)

```
    def maxSlidingWindow(self, nums: 'List[int]', k: 'int') -> 'List[int]':
        n = len(nums)
        if n * k == 0:
            return []
        if k == 1:
            return nums
        
        left = [0] * n
        left[0] = nums[0]
        right = [0] * n
        right[n - 1] = nums[n - 1]
        for i in range(1, n):
            # from left to right
            if i % k == 0:
                # block start
                left[i] = nums[i]
            else:
                left[i] = max(left[i - 1], nums[i])
            # from right to left
            j = n - i - 1
            if (j + 1) % k == 0:
                # block end
                right[j] = nums[j]
            else:
                right[j] = max(right[j + 1], nums[j])
        
        output = []
        for i in range(n - k + 1):
            output.append(max(left[i + k - 1], right[i]))
            
        return output
```
