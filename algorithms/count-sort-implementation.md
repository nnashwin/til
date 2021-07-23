# Count Sort Implementation

Corrected my mental model from [this article](https://stackabuse.com/radix-sort-in-python) and the [count sort wikipedia article](https://en.wikipedia.org/wiki/Counting_sort). 

Previously, I understood count sort by an implementation that doesn't use prefix sums.  It would sort correctly, but it had questionable time complexity and I don't think was considered stable (elements with the same number / key remain in the same order).

```python
https://en.wikipedia.org/wiki/Counting_sort
def csort(input, k):
    count = [0] * (k + 1) 
    output = [0] * len(input)

    for i in range(len(input)):
        j = input[i]
        count[j] += 1

    o_idx = 0
    for i in range(len(count)):
        j = input[i]
        while count[j] > 0:
            count[j] -= 1
            output[o_idx] += 1

    return output
```

The previous works, but it doesn't work how the algorithm was really intended.

After figuring out how the prefix sort solution works, I was able to get the following working on my machine:
```python
def csort(input, k):
    count = [0] * (k + 1) 
    output = [0] * len(input)

    for i in range(len(input)):
        j = input[i]
        count[j] += 1

    for i in range(1, k + 1):
        count[i] += count[i - 1]

    for i in range(len(input) - 1, -1, -1):
        j = input[i]
        count[j] -= 1
        output[count[j]] = input[i]

    return output
```

In this method of implementing counting sort, we store a prefix sum at each index of the count array rather than just the occurrences of the digit in the input array.  We then loop backwards through the input array and use the prefix sums in count to assign directly to the output array instead of just slowly incrementing an outside counter.