# @lru_cache and a memoization story

> Why are some things in Python so damn easy?  It's like evil magic.

I've always heard that writing in Lisp is some type of mystical experience.  Like when you stumble upon the power of macros and building a language up to a problem while also writing abstractions to cut the problems up into digestible pieces of the language, it is akin to a spiritual experience.

Having touched a handful of languages (Lisp unfortunately not being one of them), I have had the opportunity to experience both Salem Witch trials-esque revulsion to magic as well as the enjoyment of touching what seems to be a bit of my own magic.  But never have I felt wizard powers pulsing as I did the first time I encountered @lru_cache(maxsize=None)

## Introduction
My introduction to @lru_cache came while reading the editorial to a leetcode problem that was written in python.

I recently have been solving primarily dynamic programming-related problems in an attempt to catch them all.  And by catching them all, I mean banging my head against a wall in order to grok the many different dynamic programming patterns.

Traditionally, the fibonacci sequence is used to illustrate what appears to be the simple transition between a recursive algorithm, a top down dp algorithm (recursion with memoization), and a bottom-up dp algorithm.

Fibonacci numbers create a sequence by adding to themselves all the way down to 0.  Each number is created by adding the two preceeding ones together.  So F_5 is created by adding F_4 and F_3 together.  F_4 is created by adding F_3 and F_2 together.  All the way until 0.

If you have done any digging into recursion, fibonacci numbers really fits this pattern.

Let me show you a coding example.

```python
def fib(n):
    if n <= 1:
        return n

    return fib(n - 1) + fib(n - 2)
```

Calling this function on an input number will build up the call stack until a base case (1 or 0) is reached.
After that base case call returns, the rest of the functions on the call stack will populate with intermediate values until our original function call is returned.

There are MANY tutorials and visualizations on recursion that fill in the blanks in knowledge better than I could.  Consult multiple sources to really see this pattern in action.

## Saving pre-computed results
Recursion can be expensive, as sometimes multiple function calls will be calculated more than once.

When we calculate fib(4), we also need to calculate fib(3) and fib(2)

When we calculate fib(3), we also need to calculate fib(2) and fib(1).

Recursive function calls tend to create a tree of calls that need to be executed to calculate the result. A [search for the fibonacci recursion tree](https://duckduckgo.com/?q=recursion+tree+of+fibonacci+function&atb=v257-1&iax=images&ia=images) can find plenty of images to illustrate this concept.

In order to speed up our algorithm, there are strategies we can use to save these results and reuse them when we encounter them a second time.

LRU cache helps us with a strategy called top-down dynamic programming, which is typically accomplished by simply adding a data structure (hashmap / dictionary) to your function and calling it if a certain parameter exists.

## Naive Top-Down

```python
class Solution:
    def fib(self, n: int) -> int:
        memo = {}
        
        return self.helper(n, memo)
    
    def helper(self, n, memo):
        if n in memo:
            return memo[n]
        
        if n <= 1:
            memo[n] = n
        else:
            memo[n] = self.helper(n - 1, memo) + self.helper(n - 2, memo)
            
        return memo[n]
```

There are different ways to handle the if n <= 1 and else statements, but the gist is here.  

This program should run in O(n) worst case time complexity, because if we are memoizing correctly, the branches of the previously mentioned recursion tree will not need to keep calling deeper if the result has been calculated before.

## Wizard Enters the Chat
In python, you can just slap on a [decorator](https://en.wikipedia.org/wiki/Decorator_pattern) to achieve the same result.

Get your recursive solution, copy / paste a @lru_cache(maxsize=None) and reap the speed benefits of restructuring with logging built in.

```python
@lru_cache(maxsize=None)
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

>>> [fib(n) for n in range(16)]
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610]

>>> fib.cache_info()
CacheInfo(hits=28, misses=16, maxsize=None, currsize=16)
```
