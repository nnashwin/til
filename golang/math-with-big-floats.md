# Math with Big Floats

As a continuation of a project mentioned in this [article](converting-hexadecimal-string.md), I encountered the need to do mathematical operations with the big numbers.

Having encountering rounding errors in other languages when trying to parse json objects that include long number strings, I knew I wanted to keep large numbers in a big.Float form for as long as possible.

I thought this would be potentially difficult in Golang and might require me to do some manual conversions between different types.  However, it turns out the structs for big.Floats and bigInts have a lot of basic mathematical operations that allow you to just call apis to do the work you need.

The fomula for a median function is as follow:
1. Sort all of the values in the array
2. If the number of elements in the array is odd, the middle element (len(array) / 2) is the median
3. If the number of elements in the array is even, then do the following:
    - Add the middle element and the immediately adjacent (middle element - 1) element together 
    - Divide by two

With big.Floats, the procedure looks like this:
```golang
submissions := []*big.Float{} // imagine this full of floats

// note:  the Cmp function from the math/big library returns a -1 when the first element is less than the second, 0 when they are equal, and 1 when the first element is greater than the second
// the < 0 simply says that while the sort is doing comparisons, we want to order the elements by the smallest value first (non-descending order)
sort.SliceStable(submissions, func (i, j int) bool {
    return submissions[i].Cmp(submissions[j]) < 0
})

med := new(big.Float)
subLength := len(submissions)
mid := subLength / 2
if subLength % 2 == 0 {
    // (mid element + mid element-1) / 2
    med.Quo(med.Add(submissions[mid], submissions[mid - 1]), big.NewFloat(2))
} else {
    med = submissions[mid]
}
```

After this was done, I was able to use the median to finish the rest of the calculations I needed for my project.

All in all, I am still amazed by how complete I have found Golang's standard library in comparison to many other languages (Javascript, Rust, etc).  I enjoy languages with strong communities and many ways to solve problems, but when trying to prototype a project quickly, it's nice to be able to work through an idea without stopping to compare multiple user-made libraries.