# Lecture 7: Sorting III

# Table of Contents

- [1. Introduction to Advanced QuickSort Analysis](#1-introduction-to-advanced-quicksort-analysis)
- [2. Partitioning Strategies & Handling Duplicates](#2-partitioning-strategies-&-handling-duplicates)
  - [Partitioning in QuickSort](#partitioning-in-quicksort)
  - [Analysis of a Specific Partitioning Implementation](#analysis-of-a-specific-partitioning-implementation)
  - [Handling Duplicates](#handling-duplicates)
- [3. Choice of Pivot & Its Impact on Performance](#3-choice-of-pivot--its-impact-on-performance)
- [4. Recurrence Relations & Mathematical Analysis of QuickSort](#4-recurrence-relations-&-mathematical-analysis-of-quicksort)
  - [Deterministic QuickSort Recurrence](#deterministic-quicksort-recurrence)
  - [Best-Case QuickSort Recurrence](#best-case-quicksort-recurrence)
  - [Analysis of Unbalanced Partitions](#analysis-of-unbalanced-partitions)
- [5. Randomized QuickSort & Probability-Based Analysis](#5-randomized-quicksort-&-probability-based-analysis)
  - [Why Randomization Helps](#why-randomization-helps)
  - [Randomized QuickSort Algorithm](#randomized-quicksort-algorithm)
- [6. Paranoid QuickSort](#6-paranoid-quicksort)
    - [Key Claim](#key-claim)
    - [Probability-Based Analysis](#probability-based-analysis)
- [7. Final Optimization Notes on QuickSort](#7-final-optimization-notes-on-quicksort)
  - [Base Case Options](#base-case-options)
  - [Efficient Partitioning](#efficient-partitioning)
  - [Choosing a Pivot](#choosing-a-pivot)
- [8. Finding the kth Smallest Element (Order Statistics via QuickSort)](#8-finding-the-kth-smallest-element-order-statistics-via-quicksort)
    - [Problem Definition](#problem-definition)
    - [Optimized Approach: QuickSelect Algorithm](#optimized-approach-quickselect-algorithm)
    - [Paranoid QuickSelect](#paranoid-quickselect)
    - [Key Takeaways](#key-takeaways)
- [9. Summary and Key Takeaways](#9-summary-and-key-takeaways)

---

## 1. Introduction to Advanced QuickSort Analysis

- **Overview:**  
  QuickSort is a divide-and-conquer algorithm that sorts arrays in place by partitioning the data around a pivot element. Its efficiency hinges on effective partitioning and pivot selection. In this lecture, we delve deeper into the mathematical analysis of QuickSort, exploring its recurrences, probability-based performance, and techniques to handle special cases like duplicates and order statistics.

- **Key Topics Covered:**
  - Advanced partitioning strategies and handling duplicates.
  - Impact of pivot choice on performance.
  - Detailed recurrence relations and their solutions.
  - Randomized QuickSort and expected running time analysis.
  - QuickSelect for finding the kth smallest element.
  - Enhanced methods such as Paranoid QuickSort.

---

## 2. Partitioning Strategies & Handling Duplicates

### Partitioning in QuickSort

- **Core Idea:**  
  Rearrange the array so that all elements less than or equal to a chosen pivot are on one side, and all elements greater than the pivot are on the other.
  
- **Standard Alternatives:**
  - **Lomuto Partition Scheme:** Uses one pointer to separate elements; simpler to implement but may involve more swaps.
  - **Hoare Partition Scheme:** Uses two pointers moving toward each other; usually performs fewer swaps but requires careful handling of indices.

### Analysis of a Specific Partitioning Implementation

The following pseudocode outlines the partitioning procedure used in our analysis. This version assumes an array `A[1..n]` with no duplicates (and `n > 1`), where a sentinel value is used at `A[n+1]` (set to $\infty$):

```pseudo
partition(A[1..n], n, pIndex)   // Assume no duplicates, n > 1
    pivot ← A[pIndex]           // pIndex is the index of the pivot
    swap(A[1], A[pIndex])       // Store pivot in A[1]
    low ← 2                     // Start after pivot in A[1]
    high ← n + 1                // Define: A[n+1] = ∞
    while (low < high) do:
        while (A[low] ≤ pivot) and (low < high) do:
            low ← low + 1
        while (A[high] > pivot) and (low < high) do:
            high ← high - 1
        if (low < high) then:
            swap(A[low], A[high])
    swap(A[1], A[low - 1])
    return low - 1
```

- **Explanation:**
  - **Pivot Selection:** The pivot is chosen from `pIndex` and swapped with `A[1]` to simplify the partitioning.
  - **Pointers:**  
    - `low` starts at index 2, scanning rightwards for elements greater than the pivot.
    - `high` is initialized to `n+1` (with `A[n+1] = ∞`) and moves leftwards, ensuring elements greater than the pivot remain on the right.
  - **Loop:**  
    - The inner loops adjust `low` and `high` such that upon exit:
      - `A[low]` is greater than the pivot.
      - `A[high]` is less than or equal to the pivot.
    - A swap then corrects any misplaced elements.
  - **Final Step:**  
    - The pivot is swapped into its final correct position at `A[low - 1]`.

### Handling Duplicates

- **Issue with Duplicates:**  
  Standard partitioning can degrade to $O(n^2)$ time when many duplicates are present, as the partition does not effectively reduce the problem size.

- **Three-Way Partitioning:**  
  This technique splits the array into three regions:
  1. Elements less than the pivot.
  2. Elements equal to the pivot.
  3. Elements greater than the pivot.
  
  **Options:**
  - **Two-Pass Partitioning:**  
    First perform a standard partition, then make an extra pass to pack duplicates together.
  - **One-Pass Partitioning:**  
    A more complex strategy that maintains four regions to classify elements in a single pass.

- **Example & Benefit:**  
  In an array where all elements are identical, three-way partitioning will group all elements together, preventing unbalanced recursive calls and preserving $O(n \log n)$ expected time.

---

## 3. Choice of Pivot & Its Impact on Performance

- **Pivot Selection Strategies:**
  - **First Element, Last Element, or Middle Element:**  
    Simple, but can lead to worst-case performance on sorted or nearly sorted arrays.
  - **Median-of-Three:**  
    Selects the median of the first, middle, and last elements to improve balance.
  - **Random Pivot:**  
    Reduces the probability of encountering worst-case scenarios by ensuring randomness in partitioning.

- **Mathematical Worst-Case Analysis:**
  - For example, always choosing `A[1]` as the pivot in an already sorted array yields unbalanced partitions, resulting in the recurrence:

    $
    T(n) \\ 
    = T(n - 1) + T(1) + n \\
    = n + (n - 1) + (n - 2) + (n - 3) + \dots \\
    = O(n^2)
    $

- **Balanced Partitioning:**
  - If the pivot is always the median, QuickSort will run in $O(n \log n)$ time.
  - Poor pivot choices cause unbalanced partitions, degrading performance.

---

## 4. Recurrence Relations & Mathematical Analysis of QuickSort

### Deterministic QuickSort Recurrence

- **Worst-Case Recurrence:**  
  In the worst-case (e.g., when the pivot is always the smallest element), the recurrence relation can be expressed as:
  
  $$
  T(n) = T(n - 1) + T(1) + O(n).
  $$
  
  **Expansion:**  
  This recurrence expands approximately to:
  
  $$
  T(n) = O(n) + O(n-1) + \dots + O(1) = O(n^2).
  $$

### Best-Case QuickSort Recurrence

- **Balanced Partitioning Recurrence:**  
  When the pivot splits the array into two equal halves, the recurrence is:
  
  $$
  T(n) = T\left(\frac{n}{2}\right) + T\left(\frac{n}{2}\right) + n. \\
  \\
  T(n) = 2T\left(\frac{n}{2}\right) + n.
  $$
  
  **Applying the Master Theorem:**  
  Here, $a = 2$, $b = 2$, and $f(n) = O(n)$. Since $n^{\log_b a} = n^{\log_2 2} = n$, we have:
  
  $$
  T(n) = \Theta(n \log n).
  $$

### Analysis of Unbalanced Partitions

- **Unbalanced Example:**  
  If one partition always contains $\frac{9}{10}$ of the elements, the recurrence becomes:
  
  $$
  T(n) = T\left(\frac{9n}{10}\right) + T\left(\frac{n}{10}\right) + n.
  $$
  
  A detailed mathematical derivation shows that, even in this case, the running time is bounded by $O(n \log n)$, though the constants may be larger than in the perfectly balanced case.

---

## 5. Randomized QuickSort & Probability-Based Analysis

### Why Randomization Helps

- **Avoiding Worst-Case:**  
  Randomly selecting the pivot minimizes the chance of repeatedly picking the worst pivot, thereby preventing the degenerate $O(n^2)$ scenario.
  
- **Expected Performance:**  
  Randomized QuickSort has an expected running time of $O(n \log n)$, since the probability of encountering highly unbalanced partitions is low.

### Randomized QuickSort Algorithm

**Pseudocode:**

```pseudo
RandomizedQuickSort(A, begin, end)
    if begin < end then:
        pIndex ← Random(begin, end)
        pivotIndex ← 3WayPartition(A, begin, end, pIndex)
        RandomizedQuickSort(A, begin, pivotIndex - 1)
        RandomizedQuickSort(A, pivotIndex + 1, end)
```

- **Explanation:**  
  The random selection of the pivot ensures that, on average, the partitions are balanced, leading to $O(n \log n)$ expected time. Probability theory can be used to analyze the expected number of comparisons and recursive calls.

---

## 6. Paranoid QuickSort

In Paranoid QuickSort, we modify the standard QuickSort algorithm to "reject" bad pivots by repeatedly selecting a pivot until a sufficiently balanced partition is obtained. This helps ensure that the recursion depth remains $O(\log n)$ even in adverse cases.

```pseudo
ParanoidQuickSort(A[1..n], n)
    if (n == 1) then return
    else:
        repeat:
            pIndex ← random(1, n)
            p ← partition(A[1..n], n, pIndex)
        until p > (1/10)*n and p < (9/10)*n
        x ← QuickSort(A[1..p-1], p-1)
        y ← QuickSort(A[p+1..n], n-p)
```

- The algorithm first checks if the array size is 1, in which case it is already sorted.
- It then enters a **repeat loop** where it randomly selects a pivot and partitions the array.
- The loop continues until the pivot’s final position `p` satisfies the condition:
  $$
  \frac{1}{10}n < p < \frac{9}{10}n,
  $$
  ensuring that the pivot divides the array into reasonably balanced subarrays.
- Finally, QuickSort is applied recursively to the left and right subarrays.

#### **Key Claim**

- **Claim:** The **expected** number of iterations to find a "good" pivot (i.e., one that results in a balanced partition) is constant, specifically less than 2.
  
  **Example:**  
  For an array of size 10, a pivot is considered "good" if its position is between 2 and 9. That gives 8 "good" positions out of 10 total.
  - The probability of choosing a good pivot is $p = \frac{8}{10}$.
  - Thus, the expected number of pivot choices needed is:
    $$
    E[\# \text{ choices}] = \frac{1}{p} = \frac{1}{8/10} = \frac{10}{8} < 2.
    $$

This demonstrates that, in expectation, the **repeat loop** in Paranoid QuickSort is executed fewer than 2 times.

#### **Probability-Based Analysis**

A brief recap of key probability concepts used in this analysis:
- **Expectation:** The average outcome over many trials.
- **Linearity of Expectation:** The expected value of a sum of random variables is the sum of their expected values, regardless of any dependencies between them.

Using these concepts, we see that since the expected number of pivot choices is constant (less than 2), the overall recurrence for QuickSort becomes:

$$
E[T(n)] = E[T(k)] + E[T(n - k)] + E[\# \text{ pivot choices}] \cdot n,
$$

which simplifies to:

$$
E[T(n)] \leq E[T(k)] + E[T(n - k)] + 2n.
$$

By solving this recurrence (similar to the balanced partition case), we obtain an expected running time of $O(n \log n)$.

**Key Takeaways:**

- **Paranoid QuickSort** enhances the standard QuickSort by ensuring that only "good" pivots (leading to balanced partitions) are used.
- The **expected number of iterations** to select a good pivot is constant (less than 2), which prevents degradation to $O(n^2)$ time.
- **Probability theory** (using expectation and linearity of expectation) confirms that the randomized pivot selection yields an expected running time of $O(n \log n)$.

These modifications help maintain QuickSort's efficiency even in scenarios where deterministic pivot choices might lead to worst-case behavior.

---

## 7. Final Optimization Notes on QuickSort

Optimizing QuickSort involves several strategies that target both the recursive structure of the algorithm and its partitioning process. Below are some key final notes on how to optimize QuickSort for practical use:

### **Base Case Options**

1. **Recurse All the Way to Single-Element Arrays:**  
   - The algorithm continues recursion until each subarray contains only one element.  
   - This is simple to implement but may involve many recursive calls, especially for small subarrays.

2. **Switch to InsertionSort for Small Arrays:**  
   - Once the subarray size falls below a predetermined threshold (e.g., 10–20 elements), the algorithm switches to InsertionSort.  
   - **Rationale:** InsertionSort is very efficient on small or nearly sorted arrays because each element moves only a constant distance ($O(1)$) to reach its correct position.

3. **Halt Recursion Early, Leaving Small Arrays Unsorted:**  
   - Stop the recursion when subarrays become small, and then perform a final InsertionSort on the entire array.  
   - **Benefit:** This approach leverages the fact that InsertionSort is fast on nearly sorted arrays, reducing the overhead of deep recursion.

### **Efficient Partitioning**

- **Handling Duplicates:**  
  - Standard partitioning algorithms may degrade in performance when many duplicate values are present.  
  - **Tip:** Consider using three-way partitioning to group duplicate elements together, which prevents excessive work on redundant comparisons.

- **In-Place Partitioning:**  
  - Achieve partitioning in one pass by maintaining clear invariants (e.g., elements less than or equal to the pivot to the left, and those greater to the right).  
  - **Trade-off:** While this method is space efficient, it typically is not stable.

- **Stability Considerations:**  
  - In-place partitioning methods are usually not stable, meaning that the relative order of equal elements may not be preserved.  
  - If stability is required, additional modifications or a different sorting algorithm (like MergeSort) may be necessary.

### **Choosing a Pivot**

- **Deterministic vs. Randomized Pivot Selection:**  
  - **Deterministic Pivots:** Often lead to poor performance on certain inputs (e.g., already sorted arrays), as they may consistently produce unbalanced partitions.
  - **Randomized Pivot Selection:**  
    - Randomization is an effective, simple way to ensure that, on average, the pivot divides the array fairly evenly.  
    - This reduces the likelihood of the worst-case $O(n^2)$ time complexity, leading to an expected running time of $O(n \log n)$.

---

## 8. Finding the kth Smallest Element (Order Statistics via QuickSort)

#### **Problem Definition**

- **Objective:**  
  Given an unsorted array, the goal is to find the kth smallest element (for example, the median when $k = \frac{n}{2}$).

#### **Optimized Approach: QuickSelect Algorithm**

Instead of sorting the entire array (which would take $O(n \log n)$ time), QuickSelect leverages the partitioning procedure from QuickSort to "narrow down" the search to the side that contains the kth smallest element. Crucially, QuickSelect only recurses on one subarray—the one that must contain the desired element—thereby avoiding the extra work of sorting both halves.

##### **Pseudocode:**

```pseudo
Select(A[1..n], n, k)
    if (n == 1) then return A[1]
    else:
        Choose random pivot index pIndex.
        p ← partition(A[1..n], n, pIndex)
        if (k == p) then 
            return A[p]
        else if (k < p) then 
            return Select(A[1..p-1], p-1, k)
        else if (k > p) then 
            return Select(A[p+1..n], n-p, k - p)
```

- **Explanation:**
  - **Single Recursive Call:**  
    The algorithm makes only one recursive call based on the result of partitioning:
    - If the pivot's final position `p` is exactly `k`, then the kth smallest element is found.
    - If `k < p`, the kth smallest element lies in the left subarray. Thus, we recursively call `Select` on `A[1..p-1]`.
    - If `k > p`, the kth smallest element lies in the right subarray. We then call `Select` on `A[p+1..n]`, adjusting `k` to `k - p` because we have skipped the first `p` elements.
  - **Key Point:**  
    Only one subarray is recursed into because the kth smallest element can only reside in one of the partitions. Recursing into both sides would effectively sort both partitions, which is unnecessary and would slow down the algorithm.

---

#### **Paranoid QuickSelect**

To further improve the robustness of QuickSelect, we can adopt a "paranoid" approach that rejects bad pivots. This method repeatedly partitions the array until a pivot is chosen that yields a reasonably balanced split—specifically, ensuring that the pivot's position $p$ satisfies:
$$
\frac{n}{10} < p < \frac{9n}{10}.
$$

**Pseudocode for Paranoid QuickSelect:**

```pseudo
ParanoidSelect(A[1..n], n, k)
    if (n == 1) then return A[1]
    else:
        repeat:
            pIndex ← random(1, n)
            p ← partition(A[1..n], n, pIndex)
        until (p > n/10) and (p < 9n/10)
        if (k == p) then 
            return A[p]
        else if (k < p) then 
            return ParanoidSelect(A[1..p-1], p-1, k)
        else if (k > p) then 
            return ParanoidSelect(A[p+1..n], n-p, k - p)
```

- **Key Claim:**  
  The expected number of times the repeat loop is executed is constant—in fact, less than 2 on average.  
  - **Example:**  
    For an array of size 10, a pivot is “good” if its position $p$ satisfies $p > \frac{10}{10}$ and $p < \frac{9 \cdot 10}{10}$, i.e., if $p$ is between 2 and 9. This gives 8 good positions out of 10.  
    - The probability of choosing a good pivot is $\frac{8}{10}$.  
    - Thus, the expected number of pivot choices is:
      $$
      E[\# \text{ choices}] = \frac{1}{8/10} = \frac{10}{8} < 2.
      $$
    This confirms that, in expectation, the repeat loop executes fewer than 2 times.

- **Recurrence Analysis for Paranoid QuickSelect:**

  Given that the repeat loop adds only a constant factor to the work, the recurrence for the running time is:
  
  $$
  E[T(n)] \leq E[T(9n/10)] + O(n)
  $$
  
  Since the recursive call is on at most $9n/10$ elements, unrolling this recurrence gives:
  
  $$
  T(n) = O\left(n \left(1 + \frac{9}{10} + \left(\frac{9}{10}\right)^2 + \dots \right)\right) = O(n),
  $$
  
  using the formula for the sum of a geometric series. Thus, the expected running time for QuickSelect is $O(n)$.

---

#### **Key Takeaways**

- **Single Recursion:**  
  QuickSelect recurses only on the subarray that must contain the kth smallest element, avoiding unnecessary work and making it faster than sorting the entire array.
- **Paranoid Selection:**  
  By repeatedly selecting a pivot until a balanced partition is obtained, we ensure that the recursion depth remains low, and the expected additional work is constant.
- **Efficiency:**  
  With these optimizations, QuickSelect achieves an expected running time of $O(n)$, which is significantly faster than the $O(n \log n)$ time required to sort the entire array.
- **Mathematical Assurance:**  
  Basic probability theory (expectation and linearity of expectation) supports the claim that the number of pivot selections is constant in expectation, ensuring robust performance even in adverse cases.

These insights not only demonstrate the efficiency of QuickSelect but also illustrate the power of combining partitioning with probabilistic analysis to solve order statistics problems optimally.

---

## 9. Summary and Key Takeaways

1. **Partitioning Choices Affect Performance:**
   - Efficient partitioning is crucial for QuickSort's performance.
   - Three-way partitioning can effectively handle duplicates, preventing worst-case $O(n^2)$ behavior in arrays with many equal elements.

2. **Pivot Selection:**
   - The choice of pivot is critical; poor choices can lead to unbalanced partitions and $O(n^2)$ time complexity.
   - Strategies such as median-of-three and randomized pivot selection improve average-case performance.

3. **Recurrence Relations:**
   - Balanced partitions lead to the recurrence $T(n) = 2T(n/2) + O(n)$, which simplifies to $O(n \log n)$.
   - Worst-case scenarios can result in $O(n^2)$, but randomization typically ensures $O(n \log n)$ behavior.

4. **Randomized QuickSort:**
   - Randomization helps avoid consistently poor pivot choices, maintaining efficient average-case performance.
   - Expected running time of $O(n \log n)$ is achieved through probabilistic analysis.

5. **Order Statistics via QuickSelect:**
   - QuickSelect leverages QuickSort’s partitioning to efficiently find the kth smallest element in $O(n)$ expected time.

6. **Enhanced Variants:**
   - Paranoid QuickSort and dual-pivot schemes further improve robustness and practical performance.

7. **Practical Implications:**
   - Understanding the mathematical foundations of QuickSort helps in choosing the right optimizations and implementations based on dataset characteristics and application requirements.

---

These notes provide a detailed, mathematically focused analysis of QuickSort, covering its core concepts, partitioning strategies, performance recurrences, and practical optimizations. Mastery of these topics enables informed decisions on when and how to use QuickSort effectively in real-world applications.