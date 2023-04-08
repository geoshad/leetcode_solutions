# Solutions

My notes on Leetcode problems, 2023. Leetcode puts a high emphasis on runtime efficiency and memory usage. Each challenge is scored by the accuracy of its output, and is tested against various cases. I have opted to use Python to complete these problems, due to my enjoyment of the language. 

# Easy Problems

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

The following problem involves a function that checks whether a given integer, `x`, is a palindrome; if it is, `True` is returned. To solve this challenge, the integer is first converted to a string. A second variable is created to hold the reversed string order, to which it is then compared to the original string. 

```
def isPalindrome(self, x):
        string_x = str(x)
        reversed_x = string_x[::-1]
        
        if string_x == reversed_x:
            return True
        else:
            return False
```

## 13. Roman to Integer

The following problem involves a function that passes through a string of a roman numeral, and converts it into its corresponding integer. To make solving this problem easier, a dictionary was created for roman numerals and their matching values. Another variable is created for storing the result. The string is then iterated through, checking for the corresponding value from the dictionary. For example, if `s[i]` is 'X', then `roman_dict[s[i]]` is 10. 

To account for roman numerals such as IV and IX, where a smaller number appears first, the if statement checks if the current numeral is smaller than the next in the input string; the smaller number is then subtracted. Otherwise, the roman value is added to the result.

```
def romanToInt(self, s):
        roman_dict = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000}
        total = 0
        for i in range(len(s)):
            if i < len(s) - 1 and roman_dict[s[i]] < roman_dict[s[i+1]]:
                  total -= roman_dict[s[i]]
            else:
                total += roman_dict[s[i]]
        return total
```

## 14. Longest Common Prefix

The following function takes an array of strings, and prints their longest shared prefix. For exampling, an array of `["dogs", "dogma", "dear"]` would print a shared prefix of "d". To solve this problem, the prefix gets the value of the first index (i.e., "dog"), then the array is iterated through to find prefix commonalities. This is done by checking if the array strings start with the prefix. If not, then the prefix gradually removes letters from its right, until it satisfies the condition. In the case that there are no shared prefixes, the variable will eventually become an empty string. The prefix is then returned.

```
def longestCommonPrefix(self, strs):
        # get first string
        prefix = strs[0]
        for i in strs:
            # loops until conditions are met
            while not i.startswith(prefix):
                # prefix removes letters from right
                prefix = prefix[:-1] 
        return prefix
```

## 20. Valid Parentheses

The following function takes a string of brackets, and returns True if the brackets are paired appropriately, and in correct opening and close order; if not, False is returned. This problem stumped me quite a lot, but after some research, it seemed the use of a stack was appropriate. Each character in the string is iterated through, and the stack keeps track of opening brackets. If the current character is an opening bracket, it is pushed onto the stack. If it is a closing bracket, the function checks if the last opening bracket matches with the closing bracket, according to the dictionary. If there is no matching opening bracket, or the opening and closing brackets do not match, the function returns False to indicate that the input string has invalid pairings. After the loop, if there are any unmatched opening brackets remaining on the stack, False is returned. Otherwise, the function returns True to indicate valid pairings in the string.

```
def isValid(self, s):
        # mapping of bracket values
        bracket_dict = {'(':')', '[':']', '{':'}'}
        # using stack to keep track of bracket ordering
        stack = []

        # loop through the given string
        for bracket in s:
            # if the current bracket is a key in the dict, push onto stack
            if bracket in bracket_dict:
                stack.append(bracket)
            elif len(stack) == 0 or bracket_dict[stack.pop()] != bracket:
                return False
        # will either return True or False
        return len(stack) == 0
```

# Medium Problems

##
