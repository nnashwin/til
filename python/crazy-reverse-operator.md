# Crazy Reverse Operator

I really thought I knew about slices due to my experience with Golang.  I really did.  I was wrong.

Recently I've been working through competitive programming problems and doing my best to integrate spaced repetition with solving problems in order to really learn solid problem solving techniques.  So far, it has worked pretty well, and my python knowledge has increased immensely during the process.

However, there has always been one operator I see in some of the cheekiest solutions that has eluded me.  The [::-1] operator.

Whether it be due to my exhaustion with looking up optimized solutions to problems I hadn't solved or just too much information for me to learn at one time, for the life of me I could not understand why some people used l[::-1] in many of their solutions.  At first I thought they were trying to return the last index from the list (which is l[-1]).  

Then I thought perhaps they were trying to return the last index from a 2D list (which is l[-1][-1]).

Turns out they were doing a clever reverse.

```python
l = [1, 2, 3]
print(l[::-1])
# => [3, 2, 1]

reversedList = l[::-1]
print(reversedList[::-1])
# => [1, 2, 3]

m = [[1, 2, 3], [0, 0, 0], [0, 0, 0]]
print(m[::-1])
# => [[0, 0, 0], [0, 0, 0], [3, 2, 1]]

reversedMatrix = m[::-1]
print(reversedMatrix[::-1])
# => [[1, 2, 3], [0, 0, 0], [0, 0, 0]]

```

It just goes to show why consistency in any learning endeavor is important.  You can only learn so much each day.  Understanding and expertise needs to be built gradually over a long amount of time.