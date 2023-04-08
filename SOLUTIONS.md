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

## 2. Add Two Numbers

Goodness, this problem had me stumped. Many of my solutions were seemingly correct, then suddenly failed a test case check further down the line. 

This problem involved adding the contents of two linked lists together, as if they were two integers. Then, the sum should be returned as its own linked list. I created a helper function to convert the lists to strings, which were then reversed and converted to integers. Personally, the manipulation of strings is much easier to operate than simply using linked list notation. After the strings have been combined, the result is mapped into a list of integers, which is then converted into a linked list.

```
# helper function to convert linked lists to string
def linkedListToString(self, head):
        s = []
        while head:
            s.append(str(head.val))
            head = head.next
        return ''.join(s)

def addTwoNumbers(self, l1, l2):
        # convert lists to string
        s1 = self.linkedListToString(l1)
        s2 = self.linkedListToString(l2)
        
        # reverse the strings and convert to integers
        n1 = int(''.join(s1[::-1]))
        n2 = int(''.join(s2[::-1]))
        
        # add the two integers and convert the result to a list
        result = list(map(int, str(n1+n2)[::-1]))
        
        # convert the list to a linked list
        head = ListNode()
        curr = head
        for value in result:
            curr.next = ListNode(val)
            curr = curr.next
        return head.next
```

## 3. Longest Substring Without Repeating Characters

Initially using a different method, which has a nested for loop to iterate through the string and create substrings of s, between the indices `i` and `j-1` inclusive. The function checks whether the substring contains any repeated characters by comparing the length of the set substring to the length of the substring itself; the creation of the set ensures no repeated characters.

If the substring has no repeated character, the function checks whether its length is greater than the length of the current max_length substring; if so, then max_length is updated to be the value of the current substring. This function ultimately failed the last test case, wherein s is a string of 30,000 characters; the time complexity was too high, at O(n^3).

```
def lengthOfLongestSubstring(self, s):
        max_length = ""
        for i in range(len(s)):
            for j in range(i + 1, len(s) + 1):
                # slice at indexes to create substring of values between
                substr = s[i:j]
                if len(set(substr)) == len(substr):
                    if len(substr) > len(max_length):
                        max_length = substr
        return len(max_length)
```

After researching, I found that a more efficient approach is to use a sliding window, where we maintain a window of characters that contains no repeated characters, and slide the window to the right, updating the maximum length of the non-repeating substring as we go.

```
def lengthOfLongestSubstring(self, s):
        # sliding window
        # left and right boundaries of current window
        left = 0
        right = 0
        
        # currently seen window
        current = set()
        # tracking longest substring
        max_length = 0
        while right < len(s):
            if s[right] not in current:
                # shifting right
                current.add(s[right])
                max_length = max(max_length, right - left + 1)
                right += 1
            else:
                # shifting right
                current.remove(s[left])
                left += 1
        return max_length
```

##
