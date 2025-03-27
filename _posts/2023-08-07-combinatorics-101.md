---
title: Combinatorics 101
subtitle: Formulas for combinations and permutations.
tags: probabilities
---

A **permutation** is an arrangement where order matters. A **combination** is an arrangement where order does not. There are fewer combinations than permutations.

Formulas to count the arrangements of $$k$$ elements among $$n$$:

| Type | Repetitions |   Formula |
| --- |  --- | --- |
| Permutation |  Yes | \\( n^k \\) |
| Permutation | No | \\( \frac{n!}{(n-k)!} \\) |
| Combination | No | \\( \frac{n!}{k! \times (n-k)!} \\) |
| Combination | Yes | \\( \frac{(n+k-1)!}{k! \cdot (n-1)!} \\) | 

# Permutation with repetition

We have a set of $$n$$ elements, we build an ordered list of $$k$$ elements chosen among the set with repetitions allowed. At each of the $$k$$ draws, we can choose among $$n$$ possibilities. The resulting number of permutations is:
\\[ n^k \\]

# Permutation without repetition

We have a set of $$n$$ elements, we build an ordered list of $$k$$ elements chosen among the set with no repetition allowed. For the first draw, we can choose among $$n$$ elements. For the second draw, we can choose among $$(n - 1)$$ elements, etc. The resulting number of permutations is:
\\[ P(n, k) = \frac{n!}{ (n-k)!} \\]

# Combination without repetition

This is the binomial coefficient. It's a combination so the order does not matter. It can be expressed as a permutation altered t
\\[ {n \choose k} = C(n, k) = \frac{P(n, k)}{k!} = \frac{ n! }{k! \times (n-k)!} \\]

# Combination with repetition

This is a tricky one. 
\\[ { n+k-1 \choose k } = \frac{(n+k-1)!}{k! \times (n-1)!} \\]

# Examples

<details>
    <summary>Number of ways the letters A, B and C can be arranged with repetitions</summary>
    <p>$$3^3 = 27$$</p>
</details>

<details>
    <summary>Number of ways the letters A, B and C can be arranged without repetition</summary>
    <p>$$3! = 6$$</p>
</details>

<details>
    <summary>Probability of 3 heads in 5 coin flips</summary>
    <p>$$10/32$$</p>
</details>

<details>
    <summary>How many ways can first and second place be awarded to 10 people?</summary>
    <p>Number of permutations without repetition of 2 in 10: $$\frac{10!}{(10 - 2)!} = 10 \times 9 = 90$$</p>
</details>

<details>
    <summary>How many different ways can 3 red, 4 yellow and 2 blue bulbs be arranged in a string of Christmas tree lights with 9 sockets?</summary>
    <p> We assume all bulbs are individually identified: there are a total of $$9!$$ permutations. We then divide by the number of permutations within each group of bulbs: $$\frac{9!}{3! \times 4! \times 2!} = 1260$$</p>
</details>

<details>
    <summary>How many different sets of 4 letters can be selected from the alphabet?</summary>
    <p>We want the number of combinations without repetitions: $${26 \choose 4} = \frac{26!}{4! \times (26-4)!} = 14950$$</p>
</details>

<details>
    <summary>How many cups of ice-cream can we build by choosing 3 scoops among 5 flavors?</summary>
    <p> This is a combination with repetition. The order of the scoops does not matter, and we can have several times the same flavor. The solution is: $$\frac{(5+3-1)!}{3! \times (5-1)!} = 35$$</p>
</details>

---

References:
- [https://betterexplained.com/articles/easy-permutations-and-combinations/](https://betterexplained.com/articles/easy-permutations-and-combinations/)
- [https://www.intmath.com/counting-probability/3-permutations.php](https://www.intmath.com/counting-probability/3-permutations.php)
- [https://www.mathsisfun.com/combinatorics/combinations-permutations.html](https://www.mathsisfun.com/combinatorics/combinations-permutations.html)