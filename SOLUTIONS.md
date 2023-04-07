# Solutions

My notes on Leetcode problems, 2023. Each challenge is scored by the accuracy of its output, and is tested against various cases. I have opted to use Python to complete these problems due to my enjoyment of the language.

## 1. Two Sum

Two Sum involves finding two numbers in a given array, `nums`, that sum to a `target` value, and return them. Solving this problem required two nested `for` loops to simultaneously iterate through the array's indexes. For each iteration, an if statement would check if the current index values summed to the target value. From there, either the index values, or an empty set are returned, depending on the result.

```
def twoSum(self, nums, target):
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                if nums[i] + nums[j] == target:
                    return [i, j]
        return []
```

## 9. Palindrome Number
