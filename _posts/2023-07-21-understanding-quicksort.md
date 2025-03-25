---
title: Understanding Quicksort
subtitle: A recursive implementation of the classic sorting algorithm using Python.
tags: algorithm python
---

Quicksort is a divide-and-conquer sorting algorithm with an average time complexity of $$O(n \log(n))$$ and a space complexity of $$O(log(n))$$. The list is traversed and rearranged recursively until each pivot value finds its correct position.

# Partitioning

Before we dive into Quicksort, we need to understand what is partitioning. Given an array of $$n$$ elements, we pick one element that will serve as pivot and place all the smaller elements on its left and all the bigger elements on its right. By doing so, we position the pivot where it should be.

In the following example, we consider an unordered array of integers from 1 to 10. We choose the first value as the pivot. We then rearrange the array so that the smaller values are on its left and the bigger values are on its right.

![Partitioning a list of integers using its first element as the pivot](/assets/images/tlouarn-partitioning.drawio.svg)
*Partitioning a list of integers using its first element as the pivot.*

Note that when moving smaller and bigger elements, their relative order may change: we say that Quicksort is **not stable**. In contrast, a stable sorting algorithm would preserve the relative order of equal elements in the sorted output.

A simple way to partition would be to initialize three subsets, loop through the array and allocate each element to the correct subset. Such a function would do the job but would not be memory-efficient since it would use three intermediary lists and return the partitioned array in a new list.

As you have probably guessed, we can be smarter than that and partition the array **in-place**, that is without creating new lists. To do so, we traverse the array once and swap elements so that all elements smaller than the pivot end up "towards the left" and all the elements bigger than the pivot end up "towards the right". We then put the pivot value at its correct position.

```python
def partition(array: list) -> None:
    """
    In-place partitioning using the first element as the pivot.
    """

    # Choose the first element as the pivot
    pivot = array[0]

    # `index` keeps track of the smaller subset
    # We initialize it with 1
    index = 1

    # Loop through the values (starting after the pivot)
    for i in range(1, len(array)):

        # If we find a value smaller than the pivot,
        # swap it with the current next element
        # after the "smaller" subset
        if array[i] < pivot:
            array[i], array[index] = array[index], array[i]
            index += 1

    # At this stage, `index - 1` represents the last
    # value in the smaller subset; swap it with the pivot
    # This way the pivot is placed just after the smaller subset
    array[0], array[index - 1] = array[index - 1], array[0]
```

# Quicksort

Quicksort works by recursively partitioning the "smaller" and "bigger" subsets. By doing so, we place each pivot at its correct position given the subset we are in, then process the new "smaller" and "bigger" subsets, etc.

Let us now implement Quicksort using in-place partitioning. For that, we need to modify our `partition()` function. Since the function will be used on different subsets of a **same list**, we need to add a start and an end to its signature so that it knows where to start and where to stop. This way, the same function can operate on different segments of a same list from different layers of the call stack. We also need to have it return the position of the pivot: this way, we can call it again on the "smaller" subset (i.e. up until the pivot position) and on the "bigger" subset (i.e. from the pivot position until the end).

We can rewrite it as below:

```python
def partition(array: list, start: int, end: int) -> int:
    """
    In-place partitioning using the first element as the pivot.
    """

    # Choose the first element as the pivot
    pivot = array[start]

    # `index` keeps track of the smaller subset
    # We initialize it with start + 1
    index = start + 1

    # Loop through the values (starting after the pivot)
    for i in range(start + 1, end):

        # If we find a value smaller than the pivot,
        # swap it with the current next element
        # after the "smaller" subset
        if array[i] < pivot:
            array[i], array[index] = array[index], array[i]
            index += 1

    # At this stage, `index - 1` represents the last
    # value in the smaller subset; swap it with the pivot
    # This way the pivot is placed just after the smaller subset
    array[start], array[index - 1] = array[index - 1], array[start]

    # Return the index of the pivot
    return index - 1
```

Finally, we can write the Quicksort algorithm as below:

```python
def quicksort(array: list, start: int, end: int) -> None:
    """
    In-place Quicksort implementation.
    """

    # Base condition
    if start < end:

        # Partition a given subset of the array
        # and store the position of the pivot
        pivot = partition(array, start, end)

        # Recursive call on the "smaller" subset
        quicksort(array, start, pivot)

        # Recursive call on the "bigger" subset
        quicksort(array, pivot + 1, end)
```

The **base condition** of our recursive algorithm is that it only runs when `start` is strictly less than `end`.  The reason for that is: when we reach a  subset of 1 element, the partitioning function will not do anything and return the position of the pivot, which is equal to `start`. At this point the next two calls to `quicksort()` further up the call stack will return nothing, and ultimately this branch of recursion will be closed.

# Complexity analysis

Let's first look at the time complexity. On average, the number of times the array will be divided into subsets is $$log(n)$$, and at each pass we traverse the whole array, which takes $$n$$ operations. The average time complexity is therefore $$O(n log(n))$$.

The worst time complexity happens when the array is already sorted. In that case, each pair of subset will be of type $$[ 1 : n-1 ]$$ and the array will have to be partitioned $$n$$ times. The worst-case time complexity is therefore $$O(n2)$$.

When it comes to space complexity analysis, there are two things to consider: the actual memory used for variables and the memory used by the call stack. Regarding the first one, Quicksort's footprint is minimal since we swap all elements in-place (complexity of $$O(1)$$). Regarding the call stack though, the space complexity is on average $$O(log(n))$$ since we need to divide the array $$log(n)$$ times, and it can be $$O(n)$$ in the worst case.

# Conclusion

In this article we presented the Quicksort algorithm along with its recursive Python implementation. Quicksort is time-efficient due to partitioning and space-efficient due to the partitioning being done in-place.

There is some flexibility with the algorithm: so far we have always selected the first element of the list as pivot, but it is possible to choose the last element, the middle element, a random element, etc. In fact, choosing a random element usually mitigates the risk of encountering a worst-case scenario.

As usual with sorting algorithms, the best algorithm is the one most suited for the data. Although Quicksort is often the fastest algorithm, it can't beat good old BubbleSort on an already-sorted list for instance (but then again, who would want to sort a sorted list 🤔?).

---

References:
* [https://me.dt.in.th/page/quicksort/](https://me.dt.in.th/page/quicksort/)
* [https://www.enjoyalgorithms.com/blog/quick-sort-algorithm](https://www.enjoyalgorithms.com/blog/quick-sort-algorithm)
* [https://cs.stackexchange.com/questions/144424/stability-of-quicksort-algorithm](https://cs.stackexchange.com/questions/144424/stability-of-quicksort-algorithm)
* [https://stackoverflow.com/questions/18262306/quicksort-with-python](https://stackoverflow.com/questions/18262306/quicksort-with-python)