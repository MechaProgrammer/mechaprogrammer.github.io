## Two sum

[Link to problem](https://leetcode.com/problems/two-sum/)

### Slow solution

Slow solution as in the worst case we iterate `nums` twice (O(n<sup>2</sup>)).

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i1, num1 in enumerate(nums):
            for i2, num2 in enumerate(nums):
                if i1 != i2:
                    sum = num1 + num2
                    if sum == target:
                        return [i1, i2]
```

###  Fast solution

A nice solution by user [__guoqiao__](https://leetcode.com/__guoqiao__/), which is O(N).

The comments are added by me.

```python
class Solution:
    def twoSum(self, nums, target):
        d = {}  # Initialize empty dictionary
        for i, n in enumerate(nums):  # We enumerate through the numbers O(N)
            m = target - n  # Minus the current value from target. We know that if m is found, it is the other pair to the solution.
            if m in d:  # If the m value is found in the dictionary, we know it is the solution
                return [d[m], i]  # Return the indexes
            else:
                d[n] = i  # If the value is not in the dictionary, we add it. The value is key so it is fast to look.
```
