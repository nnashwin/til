# Second Largest / Smallest Numbers in Some Data Structure

I used to write my 2nd smallest / largest values in an data structure like I write my kth smallest largest / smallest value in my data structure problems.

Here is a simplified and relatively inefficient (O(n log n) worst case) example of finding the kth smallest item in an array:
```python

def findKthSmallest(arr, k):
    arr.sort()
    return arr[k]
```

If the data structure is sorted or can be traversed in a sorted manner, like a binary search tree or a sorted array, we can traverse the tree and use a counter to slowly decrement the k until it is 0.

```python
def findKthSmallest(self, root, k):
    # Note I'm using res here because you can not reliably pass single variables into functions that have shared memory.
    # If we wrap those variables in a data structure, we can reliably pass the reference and keep a persistent structure. 
    res = [k, None]
    self.dfs(root, res)
    return res[1]

# In-Order dfs
def dfs(self, root, res):
    if not root:
        return
    
    if root.left:
        self.dfs(root.left, res)

    res[0] -= 1
    if res[0] == 0:
        res[1] = root
        # cut off the recursion once we find the node
        # previous scopes will still recurse some
        return

    if root.right:
        self.dfs(root.right, res)

```

Although you can apply the same type of algorithm to finding the 2nd smallest / largest value in a data structure, there is a much more straightforward way we can use when the k is not variable (because it is 2).

Note: The actual implementation of the details of the algorithm should vary based on the search space of the data structure you are searching in.  If duplicates are allowed, if the input arr is sorted or not, and what your ideal time / space complexity should all determine which type of approach you choose to solve the problem.

The following is an technique that can be applied to an unsorted array or binary tree that does not have duplicates.

Let's assume the array is full only positive numbers.

```python
def find2ndSmallest(arr):
    smallest = math.Inf
    secondSmallest = math.Inf

    # this can be simplified with the python built in min function.
    # it is written out explicitly for clarity
    for i in range(len(arr)):
        if arr[i] < smallest:
            secondSmallest = smallest
            smallest = arr[i]
        elif arr[i] < secondSmallest:
            secondSmallest = arr[i]
    return (smallest, secondSmallest)
```

Here is an example of using the same type of algorithm while traversing a binary tree (pre-order).

```python
class Solution:
    def findSecondMinimumValue(self, root: Optional[TreeNode]) -> int:
        minV = root.val
        res = [math.inf, math.inf]
        self.dfs(root, minV, res)
        
        print(res)
        
    def dfs(self, root, minV, res):
        if not root:
            return
        
        if root.val < res[0]:
            res[1] = res[0]
            res[0] = root.val
        elif root.val < res[1]:
            res[1] = root.val
        
        self.dfs(root.left, minV, res)
        self.dfs(root.right, minV, res)
```

And the same code with a check to ignore duplicates in the binary tree.

```python
class Solution:
    def findSecondMinimumValue(self, root: Optional[TreeNode]) -> int:
        minV = root.val
        res = [math.inf, math.inf]
        self.dfs(root, minV, res)
        
        print(res)
        
    def dfs(self, root, minV, res):
        if not root:
            return
        
        if root.val < res[0]:
            res[1] = res[0]
            res[0] = root.val
        elif root.val < res[1] and root.val != res[0]:
            res[1] = root.val
        
        self.dfs(root.left, minV, res)
        self.dfs(root.right, minV, res)
```

The cool things about this pattern is that you can apply it to many data structures.
You also can just switch the equalities from < to > to make it work for the second maximum element in a list.
As long as you use a second variable and keep the comparisons relatively the same, you can only use a pointer to make direct comparisons instead of counting down from k.