---
description: Overview of Time Complexity, Space Complexity, and Big O Notation.
---

# Computational Complexity

## <mark style="color:purple;">Introduction</mark>

When it comes to problem-solving, there are often multiple solutions for a single problem. In computer science, an algorithm represents a systematic method for processing data to solve a specific problem. <mark style="color:yellow;">To assess and compare the efficiency of different algorithms, we have two standard measures of performance: time complexity and space complexity.</mark> Together, these metrics constitute what is known as the computational complexity of an algorithm, which refers to the amount of resources required to execute the algorithm. Understanding computational complexity is crucial when it comes to identifying the "best" solution to a particular computer science problem.

## <mark style="color:purple;">Big O Notation</mark>

<mark style="color:yellow;">Big O notation is a type of mathematical notation used to describe how an algorithm behaves as the amount of data it needs to process becomes larger.</mark> It typically involves the symbol $$O$$ and the variable $$n$$. The $$O$$ represents the order of complexity, indicating the upper bound, or worst case scenario. The $$n$$ denotes the size of the input data to the algorithm.&#x20;

The table below uses Big O notation to succinctly break down seven common complexities and their impact on resource usage with increasing input size.

<table><thead><tr><th width="144">Complexity</th><th width="128">Behavior</th><th>Description</th></tr></thead><tbody><tr><td><span class="math">O(n!)</span></td><td>Factorial</td><td>The use of resources increases drastically with each additional input element.</td></tr><tr><td><span class="math">O(c^n)</span></td><td>Exponential</td><td>The use of resources multiplies by <span class="math">c</span> with each additional input element.</td></tr><tr><td><span class="math">O(n^2)</span></td><td>Quadratic</td><td>The use of resources increases in proportion to the square of the input size.</td></tr><tr><td><span class="math">O(n\;log\;n)</span></td><td>Linearithmic</td><td>The use of resources increases slightly faster than linearly with each additional input element.</td></tr><tr><td><span class="math">O(n)</span></td><td>Linear</td><td>The use of resources grows in direct proportion to the input size.</td></tr><tr><td><span class="math">O(log\;n)</span></td><td>Logarithmic</td><td>The use of resources increases logarithmically with each additional input element.<br><br>This means the rate of increase slows as the input size grows.</td></tr><tr><td><span class="math">O(1)</span></td><td>Constant</td><td>The use of resources is the same for any input size.</td></tr></tbody></table>

Figure 1 below illustrates the behavior of each complexity as input size grows. Note that the y-axis includes two types of resources. This is because Big O notation can be used to describe either time complexity or space complexity. The only difference is that time complexity is measured in terms of CPU operations as a function of input size, and space complexity is measured in terms of memory space required, also as a function of input size.

<figure><img src="../.gitbook/assets/complexityGraph (1).png" alt=""><figcaption><p>Figure 1: Complexity Functions Graph</p></figcaption></figure>

...

## <mark style="color:purple;">Time Complexity</mark>

...

## <mark style="color:purple;">Space Complexity</mark>

...







