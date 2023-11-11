# Time Complexity

### How fast is your algorithm?

When we want to express how fast an algorithm works, simply measuring
the time it takes to run would not be an accurate representation,
that's because the result is subject to external factors: the
execution time may change from one hardware to another and would also
be affected by the size of the input data. Meaning that it would run
faster on a different computer or take more time to process 5000 items
than just 5.

We need a generalized way of expressing how fast an algorithm is able
to run regardless of the size of the input or other external factors.
And that is what Time Complexity is, it describes the computational
complexity that requires to run an algorithm, is estimated by counting
the number of elemental operations made by the algorithm, assuming
that each elemental operation requires a fixed amount of time.

### Big O notation

For example if we need to find the number 5 in an array of 10
elements:

[ 1, 9, 2, 8, 3, 7, 4, 6, 5, 0 ]

we would need to read every position of the array (that's the
elemental operation) until we find the number 5, in the worst case we
would need to traverse the whole array, meaning that for an array of n
items, we will need to perform n elemental operations to find the
number we are looking for.

The **Big O** notation is used to classify algorithms by
how they behave as the input size increases, in terms of how many
operations they need for every element of input. In the example above,
for an input with **n** elements, the algorithm will need
in the worst case to perform **n** operations to
complete, we say then that it has a time complexity of
**O(n)**.

**O(n)** is a **linear** time complexity,
because the number of operations is directly proportional to the
number of elements in the input. There are diferent time complexities,
the most common are: **constant**
**O(1)**,
**linear**
**O(n)**,
**logarithmic**
**O(log n)** and
**quadratic**
**O(n2)**.

In our number finding example, if our algorithm has psychic powers and
can predict where the number is, it will need only a single read
operation to retrieve the location of the number in the array, no
matter how many elements the input has, it will always require just
one operation. That is constant time complexity or
**O(1)**.

So far we have **O(1)** constant time complexity when we
have a single operation, **O(n)** linear time complexity
when we have to loop the input once, **O(n2)** quadratic
time complexity when we have a loop in a loop, and
**O(log n)** logarithmic time complexity, a common
example of this is a binary search, when the total amount of
operations is less than a normal loop.

### Which is the best one?

We can see in the following list, how the time complexity goes from
best to nevermind.

- **O(1)** best
- **O(log n)** good
- **O(n)** fair
- **O(n log n)** not as bad as quadratic
- **O(n2)** bad
- **O(n3)** worst
- **O(2n)** give up

When comparing algorithms using the
**Big O** notation, is guaranteed that the more
efficient one will be faster eventually as the input size is
increased.

Now we know what time complexity is and how it can help to determine
the efficiency of an algorithm. I hope this was helpful and I'll see
you in the next one.
