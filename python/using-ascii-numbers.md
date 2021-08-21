# Using Ascii Numbers

In my brief 7 years of programming, I haven't found much of a use case for using ascii.

I've learned facts about UTF-8 and other character code formats.  I know that Ascii was originally used because it could be coded on the first 7 bits of a byte.

I've also learned that C++ adds and subtracts 'a' often in their string / int conversions.  But I didn't know how many high level languages utilize ascii.

Until I needed to use it in a problem today.

## Problem Statement
The ascii-using problem today is similar to the [edit distance](https://en.wikipedia.org/wiki/Edit_distance) solved dp pattern used to compare how similar two strings are with a twist.  Instead of trying to solve how many characters are different between the two strings, we need to compare the lowest ASCII sum of deleted characters to make two strings equal.

So with two strings s1 and s2, we want to see which deletions we can make to make the strings equal.  There might be a few different ways to do this, so we want to make the deletions that result in the lowest ascii sum to make them equal.

```python
s1 = 'sea'
s2 = 'eat'

# We return 231 from our function because we will remove s from the first string (ascii code 115) and t from the second string (ascii code 116) to equal 231.
```

## Steps to Solve
I have been learning about the minimum edit distance and related problems for the past two days.  The pattern to solve this problem is nearly identical to that algorithm, so I knew how to set the function up.

NOTE: I'll just be using a naive recursive and top-down dp (memoized) solution to highlight the ascii number part.  Converting the algorithm to a more optimized, bottom-up dp format will probably save more resources.

The following is a top-down dp solution to find the edit distance (with no substitution) between two strings. 

```python
class Solution:
    def minimumDeleteSum(self, s1: str, s2: str) -> int:
        return self.dfs(s1, s2, len(s1), len(s2))
  
    @lru_cache(maxsize=None)
    def dfs(self, s1, s2, m, n):
        if m == 0:
            return n
        
        if n == 0:
            return m
        
        if s1[m - 1] == s2[n - 1]:
            return self.dfs(s1, s2, m - 1, n - 1)
        
        # Note no substitution operation
        return 1 + min(self.dfs(s1, s2, m - 1, n), self.dfs(s1, s2, m, n - 1))
```

After searching around, I discovered that you can obtain the ascii code of a character in python with the ord() function.

We can modify the function above to solve the problem with two changes:
1. When an idx is 0, we should add ALL of the ascii chars in the opposite string to the sum.
   - i.e. if m == 0, we should return all the sum of the ascii characters that are left in the string (sum([ord(c) for c in s2[:n]]))
2. Instead of just adding 1 to the min, we should obtain the minimum value between taking the ascii value of the first string and subtracting by one and taking the ascii value of the second string and subtracting its index by one.
   - Adding the ord value and calling the function with the related string idx subtracted by one is making a sub-decision to find out the ascii summed value resulting from taking that decision at that point in time.
        - So when we have s1 ("sea") and s2("eat"), we will return the minimum value between recursing down the pathway that has us comparing ("se" / "eat") AND ("sea", "ea").
        - We continue to make that same decision until one of the string indexes equals 0.  We only don't make a deletion operation when the tail characters of the strings match (like when we call the function with "sea" and "ea").


The modified solution:
```python
class Solution:
    def minimumDeleteSum(self, s1: str, s2: str) -> int:
        return self.dfs(s1, s2, len(s1), len(s2))
  
    @lru_cache(maxsize=None)
    def dfs(self, s1, s2, m, n):
        if m == 0:
            return sum([ord(c) for c in s2[:n]])
        
        if n == 0:
            return sum([ord(c) for c in s1[:m]])
        
        if s1[m - 1] == s2[n - 1]:
            return self.dfs(s1, s2, m - 1, n - 1)
        
        return min(ord(s1[m - 1]) + self.dfs(s1, s2, m - 1, n), ord(s2[n - 1]) + self.dfs(s1, s2, m, n - 1))
```

It's not the fastest implementation, but it is passable and offer us a lot to learn.

Check out the leetcode problem [here](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/submissions/).