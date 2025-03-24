---
title: Understanding Quicksort
subtitle: Intuition and Python implementation of the classic sorting algorithm.
tags: algorithm python
---

QuickSort is a divide-and-conquer sorting algorithm with an average time complexity of $$O(n \cdot log(n))$$ and a space complexity of $$O(1)$$. The list is traversed and rearranged recursively until each pivot value finds its correct position.

# Partitioning

Before we dive into QuickSort, we need to understand what is partitioning. Given an array of $$n$$ elements, we pick one element that will serve as pivot and place all the smaller elements on its left and all the bigger elements on its right. By doing so, we position the pivot where it should be.

In the following example, we consider an unordered array of integers from 1 to 10. We choose the first value as the pivot. We then rearrange the array so that the smaller values are on its left and the bigger values are on its right.

![]

Note that when moving smaller and bigger elements, their relative order may change: we say that QuickSort is not stable.

A simple way to partition would be to initialize three subsets, go through the array and allocate each element to the correct subset:

```python
def partition(array: list) -> list:
    """
    Partition a list using the first element as the pivot.
    Returns a new list.
    """

    # Initialize 3 subsets
    smaller = []
    equal = []
    bigger = []

    # Choose the first element as the pivot
    pivot = array[0]

    # Loop through the array and allocate
    # each element to its corresponding subset
    for element in array:
        if element < pivot:
            smaller.append(element)
        elif element == pivot:
            equal.append(element)
        elif element > pivot:
            bigger.append(element)

    # Return the concatenation
    return smaller + equal + bigger
```

This function does the job but is not memory-efficient since we use three intermediary lists and return the partitioned array in a new list.

As you've probably guessed, we can be smarter than that and partition the array in-place, i.e. without creating new lists. To do so, we traverse the array once and swap elements so that all elements smaller than the pivot end up "towards the left" and all the elements bigger than the pivot end up "towards the right". We then put the pivot value at its correct position.

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

# QuickSort

QuickSort works by recursively partitioning the "smaller" and "bigger" subsets. By doing so, we place each pivot at its correct position given the subset we are in, then process the new "smaller" and "bigger" subsets, etc.

Below is a simple Python implementation of QuickSort often used for educational purpose. As you see, it is not memory-efficient since each call to quicksort() returns a new list. But it has the benefit of being more readable than the in-place implementation.

```python
def quicksort(array: list) -> list:
    """
    QuickSort implementation using new lists.
    """

    # Base condition
    if len(array) <= 1:
        return array

    # Initialize 3 new lists
    smaller = []
    equal = []
    bigger = []

    # Choose the first element as the pivot
    pivot = array[0]

    # Loop through the array and allocate
    # each element to its corresponding subset
    for element in array:
        if element < pivot:
            smaller.append(element)
        elif element == pivot:
            equal.append(element)
        elif element > pivot:
            bigger.append(element)

    # Return the concatenation with quicksorted subsets
    return quicksort(smaller) + equal + quicksort(bigger)
```

Usage:

```shell
>>> array = [6, 4, 3, 1, 8, 7, 9, 2, 10, 5]
>>> result = quicksort(array)
>>> result
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

Let's now look at QuickSort using in-place partitioning.

For that, we need to modify our `partition()` function. Since the function will be used on different subsets of a same list, we need to add a start and an end to its signature so that it knows where to start and where to stop. This way, the same function can operate on different segments of a same list from different layers of the call stack. We also need to have it return the position of the pivot: this way, we can call it again on the "smaller" subset (i.e. up until the pivot position) and on the "bigger" subset (i.e. from the pivot position until the end).

We can rewrite it as below:

```python
def partition(array: list, start: int, end: int) -> int:
    """
    In-place partitioning using the first element as the pivot.
    """

    # Choose the first element as the pivot
    pivot = array[start]

    # `index` keeps track of the smaller subset
    # We initialize it with 1
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

Finally, we can write the QuickSort algorithm as below:

```python
def quicksort(array: list, start: int, end: int) -> None:
    """
    In-place QuickSort implementation.
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

For the purpose of complexity analysis, we only examine the in-place version of QuickSort since it is the most efficient.

Let's first look at the time complexity. On average, the number of times the array will be divided into subsets is $$log(n)$$, and at each pass we traverse the whole array, which takes $$n$$$$ operations.

The average time complexity is therefore: 

\\[ O(n \cdot log(n)) \\]

The worst time complexity happens when the array is already sorted. In that case, each pair of subset will be of type (\ 1 : n-1 \) and the array will have to be partitioned $$n$$ times.  The worst-case time complexity is therefore:

\\[ O(n2) \\]



Space complexity analysis

There are two things to consider when it comes to space complexity:

    the actual memory used for variables
    the memory used by the call stack

Regarding variables, the footprint is minimum since we swap all elements in-place. Therefore the complexity is $$O(1)$$. Regarding the call stack though, it is on average $$O(log(n))$$ since we need to divide the array $$log(n)$$ times, and it can be $$O(n)$$ in the worst case.

# Conclusion

In this article we presented the QuickSort algorithm as well as a variety of Python implementations. QuickSort time efficiency is due to the partitioning, and its space efficiency is due to the partitioning being done in-place.

There is some flexibility with the algorithm: so far we have always selected the first element of the list as pivot, but it is possible to choose the last element, the middle element, a random element, etc. In fact, choosing a random element usually mitigates the risk of encountering a worst-case scenario.

As usual with sorting algorithms, the best algorithm is the one most suited for the data. Although QuickSort is often the fastest algorithm, it can't beat good old BubbleSort on an already-sorted list for instance (but then again, who would want to sort a sorted list 🤔?).