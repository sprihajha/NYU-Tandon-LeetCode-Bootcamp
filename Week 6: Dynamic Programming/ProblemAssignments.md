### Week 6: Problem Assignments

1. [Partition equal subset sum](https://leetcode.com/problems/partition-equal-subset-sum/) (Medium)

```
    def canPartition(self, nums: List[int]) -> bool:
        # find sum of array elements
        total_sum = sum(nums)

        # if total_sum is odd, it cannot be partitioned into equal sum subsets
        if total_sum % 2 != 0:
            return False
        subset_sum = total_sum // 2

        # construct a dp table of size (subset_sum + 1)
        dp = [False] * (subset_sum + 1)
        dp[0] = True
        for curr in nums:
            for j in range(subset_sum, curr - 1, -1):
                dp[j] = dp[j] or dp[j - curr]

        return dp[subset_sum]
```

2. [Longest common subsequence](https://leetcode.com/problems/longest-common-subsequence/) (Medium)

```
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        
        # If text1 doesn't reference the shortest string, swap them.
        if len(text2) < len(text1):
            text1, text2 = text2, text1
        
        
        # The previous and current column starts with all 0's and like 
        # before is 1 more than the length of the first word.
        previous = [0] * (len(text1) + 1)
        current = [0] * (len(text1) + 1)
        
        # Iterate up each column, starting from the last one.
        for col in reversed(range(len(text2))):
            for row in reversed(range(len(text1))):
                if text2[col] == text1[row]:
                    current[row] = 1 + previous[row + 1]
                else:
                    current[row] = max(previous[row], current[row + 1])
            # The current column becomes the previous one, and vice versa.
            previous, current = current, previous
        
        # The original problem's answer is in previous[0]. Return it.
        return previous[0]
```

3. [Word break](https://leetcode.com/problems/word-break/) (Medium)

```
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        word_set = set(wordDict)
        dp = [False] * (len(s) + 1)
        dp[0] = True

        for i in range(1, len(s) + 1):
            for j in range(i):
                if dp[j] and s[j:i] in word_set:
                    dp[i] = True
                    break
        return dp[len(s)]
```

4. [Combination sum IV](https://leetcode.com/problems/combination-sum-iv/) (Medium)

```
    def combinationSum4(self, nums: List[int], target: int) -> int:
        # nums.sort()
        dp = [0 for i in range(target+1)]
        dp[0] = 1

        for comb_sum in range(target+1):

            for num in nums:
                if comb_sum - num >= 0:
                    dp[comb_sum] += dp[comb_sum-num]
                # early stopping
                # else:
                #    break
        return dp[target]
```

5. [Decode ways](https://leetcode.com/problems/decode-ways/) (Medium)

```
    def numDecodings(self, s: str) -> int:
        if s[0] == "0":
            return 0
    
        two_back = 1
        one_back = 1
        for i in range(1, len(s)):
            current = 0
            if s[i] != "0":
                current = one_back
            two_digit = int(s[i - 1: i + 1])
            if two_digit >= 10 and two_digit <= 26:
                current += two_back
            two_back = one_back
            one_back = current
        
        return one_back
```

6. [Unique paths](https://leetcode.com/problems/unique-paths/) (Medium)

```
    def uniquePaths(self, m: int, n: int) -> int:
        d = [[1] * n for _ in range(m)]

        for col in range(1, m):
            for row in range(1, n):
                d[col][row] = d[col - 1][row] + d[col][row - 1]

        return d[m - 1][n - 1]
```

7. [Jump Game](https://leetcode.com/problems/jump-game/) (Medium)

```
def canJump(self, nums: List[int]) -> bool:
        length = len(nums)
        dp = [0] * length
        
        dp[0] = nums[0]
        
        for i in range(1, length - 1):
            
            if dp[i - 1] < i:
                return False
            
            dp[i] = max(i + nums[i], dp[i - 1])
            
            if dp[i] >= length - 1:
                return True
        
        return dp[length - 2] >= length - 1
```