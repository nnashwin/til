# Managing Directions on a 2d Matrix

I had worked on some problems that used directions of chesspieces in the past {add link to the knight's tour}, but I never really grokked in which directions certain basic pieces or actors could move on a 2d matrix board.

While solving this [dfs / backtracking problem](https://leetcode.com/problems/word-search/) I noticed that I needed to try out all directions on the board from a spot for each step that matched a certain parameter (the character on that step matched the first character in the word we are searching for).

```python
def exist(self, board: List[List[str]], word: str) -> bool:
        for i in range(len(board)):
            for j in range(len(board[0])):

                # Note how when we have the match of the current character to the first character of the word, we mark it in the data structure and proceed to test out all solutions starting from this place on the board to see if we find the word.
                if board[i][j] == word[0]:
                    char = board[i][j]
                    board[i][j] = "."
                    if self.dfs(board, word[1:], i, j):
                        return True
                        # ... the rest of the code after
```

Having recently practiced many backtracking problems in order to learn the concept, the basic pattern of how the functions should be structured was clear, but I didn't know how to represent the movement on the board should be.

I started by testing out the following list of tuples in my dfs solution:
```python
class Solution
    def __init__(self):
        self.dirX = [1, -1, 0]
        self.dixY = [0, 1, -1]

    def exist(self, board, word):
        # covered earlier, not important to what I learned today

    def dfs(self, board, word, curX, curY):
        if not word:
            # word is empty string because we have solved the puzzle
            return True
        
        for x in self.dirX:
            for y in self.dirY:
                nextX, nextY =  curX + x, curY + y
                if self.isSafe(board, nextX, nextY) and board[nextX][nextY] == word[0]:
                    char = board[nextX][nextY]
                    board[nextX][nextY] = "."
                    if self.dfs(board, word[1:], nextX, nextY):
                        return True
                    board[nextX][nextY] = char
                        
                # rest of backtracking code
```

This kept getting the wrong answers for a couple of reasons.

First, in valid moves for word search on a graph, you will never have a 1 for x and y or a -1 for x and y.  Those are actually diagonals moves.

If I am at E in the following matrix (at position matrix[1][1]), and I add 1 to both indices, I will move to I.

```
matrix[1][1]
[[A, B, C],
 [D, #E#, F],
 [G, H, I]]

 # x + 1, y + 1

matrix[2][2]
 [[A, B, C],
 [D, E, F],
 [G, H, #I#]]
 ```

So I could eliminate those indices from the possibilities.
We can only have four directions, up, down, left, and right.

Then I thought, why don't I just define all of the directions as 2d tuples in a list and loop through the list.

```python
class Solution:
    def __init__(self):
        self.dirs = [(1, 0), (0, 1), (0, -1), (-1, 0)]

    def exist(self, board: List[List[str]], word: str) -> bool:
        # not the focus of this article

    def dfs(self, board, word, curX, curY) -> bool:
        if not word:
            return True
        
        # Note the directions here #
        for d in self.dirs:
            nextX, nextY = curX + d[0], curY + d[1]
            if self.isSafe(board, nextX, nextY) and board[nextX][nextY] == word[0]:
                char = board[nextX][nextY]
                board[nextX][nextY] = "."
                if self.dfs(board, word[1:], nextX, nextY):
                    return True
                board[nextX][nextY] = char
                    
        return False

```

I guess next time I'll verify exactly what movements the problem lets us make on the graph before I jump down the wrong rabbit hole. (And prune that mental pathway earlier!)