# Week 2: Strings and HashMap

Welcome to the second week of our LeetCode Bootcamp. This week, we will dive into Strings and HashMap in Python, alongside introducing powerful problem-solving patterns like the Hash Map Pattern.

## Class Agenda (2 Hours)

### 1. Python Overview of Strings and HashMap

Please review the following resources:

- [Exercism: Strings](https://exercism.org/tracks/python/concepts/strings)
- [Python.org: String Methods](https://docs.python.org/3/library/stdtypes.html#string-methods)
- [Exercism: Dicts](https://exercism.org/tracks/python/concepts/dicts)
- [Exercism: Dicts Methods](https://exercism.org/tracks/python/concepts/dict-methods)
- [Python.org: Mapping Types](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict)
- [Python.org: Dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries)
- [Python.org: Collections](https://docs.python.org/3/library/collections.html#)

### 2. Pattern Introduction

- Hash Map Pattern
![alt text](./assets/HashMap.png)
- Revisit Sliding Window Technique
[here](/Week%201:%20Lists,%20Arrays%20&%20Sorting/assets/SlidingWindowApproach.png)

### 3. Problems Covered This Week

- [Longest Palindrome](https://leetcode.com/problems/longest-palindrome/description/)(Easy)

```python
from collections import Counter

class Solution:
    def longestPalindrome(self, s: str) -> int:
 
 # Hash Map Approach
 
        char_counts = {}
        for char in s:
            char_counts[char] = char_counts.get(char, 0) + 1

        length = 0
        odd_found = False

        for count in char_counts.values():
            if count % 2 == 0: # even
                length += count
            else: # odd
                length += count - 1 
                odd_found = True

        return length + (1 if odd_found else 0)

# Time Complexity: O(N), where N is the length of the string. We make one pass to count characters.

# Space Complexity: O(1) , since the character set is fixed (e.g., lowercase and uppercase English letters), it's considered constant space.

# Collections Module Approach 

        char_counts = Counter(s)
        print(char_counts)
        length = 0
        
        for count in char_counts.values():
            length += count // 2 * 2
            if length % 2 == 0 and count % 2 == 1:
                length += 1
        return length

# Time Complexity: O(N), similar to the hash map approach, where N is the length of the string.

# Space Complexity: O(1), due to the fixed character set.
```

- [Find all anagrams in a string](https://leetcode.com/problems/find-all-anagrams-in-a-string/description/)(Medium)

```python
from collections import Counter

class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:

        # Sliding Window with HashMap

        if len(p) > len(s): return []

        pCount, sCount = Counter(p), Counter(s[:len(p)-1]) # window p - 1
        res = []
        
        for i in range(len(p)-1, len(s)):
            sCount[s[i]] += 1   # Include current character to the window
            if sCount == pCount:  # Compare window with p's character counts
                res.append(i - len(p) + 1)
            sCount[s[i - len(p) + 1]] -= 1  # Remove the leftmost character from the window
            if sCount[s[i - len(p) + 1]] == 0:
                del sCount[s[i - len(p) + 1]]  # Remove key if count is zero to keep maps comparable

        return res

# Time Complexity: O(n), where n is the length of s. Each character in s is visited twice at most (once added and once removed from the window).

# Space Complexity: O(1), the hash map size is bounded by the alphabet size, which is constant.

        # Sliding Window with Array

        if len(p) > len(s): return []
        
        pCount, sCount = [0]*26, [0]*26
        for char in p:
            pCount[ord(char) - ord('a')] += 1
        
        res = []
        for i in range(len(s)):
            sCount[ord(s[i]) - ord('a')] += 1
            if i >= len(p):
                sCount[ord(s[i-len(p)]) - ord('a')] -= 1
            if sCount == pCount:
                print(sCount)
                print(pCount)
                res.append(i - len(p) + 1)
        
        return res

# Time Complexity: O(n), where n is the length of s. Similar to the hash map approach, each character in s is processed linearly.

# Space Complexity: O(1), the frequency arrays have a fixed length of 26, representing each letter in the alphabet.

# Comparison
# Both methods effectively utilize the sliding window technique to find all anagrams of p in s. The main difference lies in how they track character frequencies—either with hash maps (flexible for any character set) or arrays (optimized for fixed, known character sets like lowercase English letters). The array method slightly edges out in efficiency due to direct index access and a fixed-size character set, though both have the same theoretical time and space complexity.
```

- [Ransom Note](https://leetcode.com/problems/ransom-note/description/)(Easy)

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        # Create a hash map to count occurrences of each character in the magazine
        magazine_counts = {}
        for char in magazine:
            magazine_counts[char] = magazine_counts.get(char, 0) + 1
        
        # Check if the ransom note can be constructed from the magazine
        for char in ransomNote:
            if magazine_counts.get(char, 0) == 0:
                # Character is either not present or not available in sufficient quantity
                return False
            magazine_counts[char] -= 1  # Use one occurrence of the character
        
        return True

# Time Complexity: O(M + N), where M is the length of the magazine string and N is the length of the ransom note string. 

# Space Complexity: O(N), where N is the number of unique characters in the magazine string.
```

- [First Missing Positive](https://leetcode.com/problems/first-missing-positive/description/)(Hard)

```python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        # Create a hash set of all positive integers in the array
        num_set = {num for num in nums if num > 0}
        
        # Start checking from the first positive integer, which is 1
        missing_positive = 1
        
        # While the missing positive is in the set, increment to find the missing one
        while missing_positive in num_set:
            missing_positive += 1
            
        # The first missing positive integer not in the set
        return missing_positive

# Time Complexity: O(N). The algorithm has two main operations: creating a hash set and finding the missing positive.

# Space Complexity: O(N). The hash set can contain at most N positive integers (if all integers in the array are unique and positive).

    # Index mapping

        n = len(nums)
        
        for i in range(n):
            # Swap nums[i] to its correct position nums[nums[i]-1] if it's in the range [1, n]
            while 1 <= nums[i] <= n and nums[nums[i]-1] != nums[i]:
                nums[nums[i]-1], nums[i] = nums[i], nums[nums[i]-1]
        
        # Find the first index 'i' where nums[i] is not 'i+1', return 'i+1'
        for i in range(n):
            if nums[i] != i + 1:
                return i + 1
        
        # If all numbers are in their correct positions, return 'n+1'
        return n + 1
```

## Take-Home Problems

To help solidify your understanding and practice further, here are some take-home problems:

1. [String to integer atoi](https://leetcode.com/problems/string-to-integer-atoi/description/)(Medium)
2. [Longest repeating character replacement](https://leetcode.com/problems/longest-repeating-character-replacement/description/)(Medium)
3. [Palindrome Pairs](https://leetcode.com/problems/palindrome-pairs/description/)(Hard)

Good luck, and happy coding!
