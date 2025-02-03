# Lecture 6: Sorting II


# Table of Contents

- [Part 1: MergeSort and Space Complexity Analysis](#part-1-mergesort-and-space-complexity-analysis)
  - [1. Introduction to MergeSort](#1-introduction-to-mergesort)
    - [Divide-and-Conquer Overview](#divide-and-conquer-overview)
    - [Recursive Definition & Pseudocode](#recursive-definition--pseudocode)
  - [2. Space Complexity of MergeSort](#2-space-complexity-of-mergesort)
    - [Temporary Arrays and Recurrence](#temporary-arrays-and-recurrence)
    - [Diagrammatic Explanation](#diagrammatic-explanation)
  - [3. Optimizing MergeSort to Reduce Space Usage](#3-optimizing-mergesort-to-reduce-space-usage)
    - [Iterative (Bottom-Up) MergeSort](#iterative-bottom-up-mergesort)
    - [Further Optimisation: $O(n)$ space usage](#further-optimisation-on-space-usage)
    - [In-Place MergeSort Approaches](#in-place-mergesort-approaches)
    - [Hybrid Sorting Techniques](#hybrid-sorting-techniques)
    - [Comparison with QuickSort on Space Efficiency](#comparison-with-quicksort-on-space-efficiency)
- [Part 2: Intro to QuickSort](#part-2-intro-to-quicksort)
  - [1. Introduction to QuickSort](#1-introduction-to-quicksort)
  - [2. QuickSort Algorithm Breakdown](#2-quicksort-algorithm-breakdown)
    - [Algorithm Steps](#algorithm-steps)
    - [Pseudocode](#pseudocode)
    - [Partitioning Strategies in QuickSort](#partitioning-strategies-in-quicksort)
  - [3. Algorithm Invariant Analysis](#3-algorithm-invariant-analysis)
    - [1. Proof and Explanation](#1-proof-and-explanation)
    - [2. Invariant Maintenance at the End of Each Iteration](#2-invariant-maintenance-at-the-end-of-each-iteration)
    - [3. Conclusion of Partitioning](#3-conclusion-of-partitioning)
  - [4. Handling Duplicates in QuickSort](#4-handling-duplicates-in-quicksort)
    - [The Problem with Duplicates](#the-problem-with-duplicates)
    - [Three-Way Partitioning](#three-way-partitioning)
    - [Three-Way Partitioning Pseudocode](#three-way-partitioning-pseudocode)
    - [Key Takeaways](#key-takeaways)
  - [5. QuickSort Performance Analysis](#5-quicksort-performance-analysis)
    - [Time Complexity](#time-complexity)
    - [Space Complexity](#space-complexity)
    - [Stability](#stability)
  - [6. Comparison: QuickSort vs. MergeSort](#6-comparison-quicksort-vs-mergesort)
    - [When to Use QuickSort vs. MergeSort](#when-to-use-quicksort-vs-mergesort)
  - [7. Modern Optimizations & Real-World Implementations](#7-modern-optimizations-&-real-world-implementations)
- [Summary and Key Takeaways](#summary-and-key-takeaways)
- [Appendix](#appendix)
  - ['Merge' Function for Iterative Mergesort](#merge-function-for-iterative-mergesort)
  - ['Merge' Function for Two-Array Alternating Implementation](#merge-function-for-two-array-alternating-implementation)
  - [Quicksort: Alternative Partitioning Strategies](#quicksort-alternative-partitioning-strategies)

---
# Lecture 6: Sorting II

These notes cover two of the most important sorting algorithms: MergeSort and QuickSort. We will explore their principles, pseudocode, performance analysis (both time and space), and discuss key trade-offs and optimizations. The notes are divided into two parts.

---

## Part 1: MergeSort and Space Complexity Analysis

### 1. Introduction to MergeSort

**MergeSort** is a classic Divide-and-Conquer algorithm that sorts an array by recursively splitting it into two halves, sorting each half, and then merging the two sorted halves into one fully sorted array.

#### **Divide-and-Conquer Overview**

1. **Divide:** Split the input array into two roughly equal halves.
2. **Conquer:** Recursively sort each half.
3. **Combine:** Merge the two sorted halves to produce a sorted array.

#### **Recursive Definition & Pseudocode**

```pseudo
MergeSort(A, n)
    if (n == 1) then return A
    else:
        X ← MergeSort(A[1 .. n/2], n/2)
        Y ← MergeSort(A[n/2+1 .. n], n/2)
        return Merge(X, Y)
```

*Example Walkthrough:*  
Given an array A, MergeSort first divides it into two halves, sorts each recursively, and then merges the sorted halves to produce the final sorted output.

### 2. Space Complexity of MergeSort

A key question is: **How much extra space does MergeSort require?**

#### **Temporary Arrays and Recurrence**

- **Merge Operation:**  
  When merging two sorted subarrays, temporary arrays are used to hold the elements of the two halves. For an array of size `n`, if each half is of size `n/2`, then merging requires additional space proportional to `n`.

- **Recurrence Relation for Space Usage:**
  
  $$
  S(n) = 2S\left(\frac{n}{2}\right) + O(n)
  $$

  This recurrence shows that at each level of recursion, $O(n)$ space is used for the temporary arrays. Since the recursion tree has a depth of $O(\log n)$, the total space complexity in the worst case is:

  $$
  O(n \log n)
  $$

#### **Diagrammatic Explanation**

Imagine the recursion tree for MergeSort:  
- At the root (level 0), the entire array of size `n` is processed, requiring $O(n)$ extra space for merging.
- At level 1, there are 2 nodes, each processing an array of size `n/2` (total space still $O(n)$).
- This continues for $\log n$ levels.  
- The extra space used at each level does not add up (because the space is reused) but the worst-case auxiliary space required remains $O(n)$ for each merge.

*Note:* The recurrence relation above assumes that the temporary arrays are allocated anew at each merge step. In practice, optimizations may reduce the overall extra space usage, sometimes to $O(n)$ total auxiliary space.

### 3. Optimizing MergeSort to Reduce Space Usage

#### **Iterative (Bottom-Up) MergeSort**

To avoid the overhead of recursion and reduce space complexity, an iterative version of MergeSort (also known as bottom-up MergeSort) is often used. This approach iteratively merges subarrays of increasing size.

**Pseudocode:**

```pseudo
IterativeMergeSort(A, n)
    size ← 1
    while size < n do:
        for left ← 1 to n - size step 2*size do:
            mid ← left + size - 1
            right ← min(left + 2*size - 1, n)
            Merge(A, left, mid, right)
        size ← size * 2
```

- **Explanation:**  
  - Start with subarrays of size 1 (individual elements, which are trivially sorted).  
  - Iteratively merge adjacent subarrays to form sorted subarrays of size 2, then 4, 8, and so on.  
  - The merge process itself takes $O(n)$ time per pass, and the number of passes is $O(\log n)$.

- **Space Analysis:**  
  In the standard recursive MergeSort, each merge operation may allocate new temporary arrays, leading to a space complexity of $O(n)$ per level of recursion. With recursive calls, this can accumulate to $O(n \log n)$ total extra space if temporary arrays are not reused. In the iterative (bottom-up) approach, however, the extra space needed for merging is often allocated once for a temporary array of size $n$, resulting in an overall auxiliary space complexity of $O(n)$.

#### **Further Optimisation: $O(n)$ space usage**

A further optimization reduces space usage by using only a single temporary array for the entire sorting process, instead of allocating new arrays at each merge step. Two common strategies are discussed:

**Iteration 1: Using One Temporary Array**

The idea is to pass a pre-allocated temporary array (`tempArray`) to all recursive (or iterative) merge operations.

**Pseudocode:**

```pseudo
MergeSort(A, begin, end, tempArray)
    if (begin == end) then
        return
    else:
        mid = begin + (end - begin) / 2
        MergeSort(A, begin, mid, tempArray)
        MergeSort(A, mid + 1, end, tempArray)
        Merge(A[begin..mid], A[mid+1..end], tempArray)
        Copy(tempArray, A, begin, end)
```

- **Explanation:**  
  - The `tempArray` is used as a workspace during the merge.  
  - The `Merge` procedure copies elements from the two sorted subarrays into `tempArray` in sorted order.  
  - After merging, the sorted elements in `tempArray` are copied back into the original array `A` in the range `[begin, end]`.  
  - This approach uses $O(n)$ extra space overall since only one temporary array of size $n$ is allocated.

**Iteration 2: Alternating Temporary Arrays**

To further optimize and avoid the extra copying of data back and forth, the algorithm can alternate the roles of two arrays at every step.

**Pseudocode:**

```pseudo
MergeSort(A, B, begin, end)
    if (begin == end) then
        return
    else:
        mid = begin + (end - begin) / 2
        MergeSort(B, A, begin, mid)
        MergeSort(B, A, mid + 1, end)
        Merge(A, B, begin, mid, end)
        // Optionally, copy from B back to A if needed
```

- **Explanation:**  
  - Two arrays, `A` and `B`, are used alternately.  
  - Initially, the input is in `A`, and `B` is used as auxiliary space.  
  - The recursive calls swap the roles of `A` and `B`, so that after merging, the sorted subarray is written into `B`.  
  - This alternating approach minimizes unnecessary copying because the merge results are directly stored in the alternate array.  
  - Ultimately, the sorted output can be in either `A` or `B`, depending on the number of merge steps, and a final copy can be done if a specific output array is required.
  - This technique maintains an overall space complexity of $O(n)$.

**Key Takeaways on Space Optimizations for MergeSort:**

- The standard recursive MergeSort may use $O(n \log n)$ space if temporary arrays are allocated at each level.
- Using a single temporary array for all merge operations reduces the extra space to $O(n)$.
- Alternating the roles of two arrays can further optimize the process by reducing redundant copying, while still maintaining an $O(n)$ auxiliary space requirement.

#### **In-Place MergeSort Approaches**

While classic MergeSort uses additional space, several in-place variants exist that attempt to reduce or eliminate the extra space required. These methods, however, often come at the cost of increased algorithmic complexity or higher constant factors.

#### **Hybrid Sorting Techniques**

In practice, hybrid algorithms that combine MergeSort with other algorithms (such as Insertion Sort) are used. For example, many implementations switch to Insertion Sort when the subarray size becomes small, taking advantage of its efficiency on small or nearly sorted arrays.

#### **Comparison with QuickSort on Space Efficiency**

- **MergeSort:** Typically uses $O(n)$ extra space (or $O(n \log n)$ if each merge allocates new arrays).
- **QuickSort:** Is in-place, using only $O(\log n)$ extra space due to recursion depth.
- This makes QuickSort more appealing when memory usage is a critical concern.

---

## Part 2: Intro to QuickSort

### 1. Introduction to QuickSort

QuickSort is another Divide-and-Conquer sorting algorithm that is widely used due to its excellent average-case performance and in-place sorting capabilities.

- **Key Advantages:**
  - **In-Place Sorting:** QuickSort does not require additional arrays for sorting.
  - **Cache Efficiency:** It exhibits good cache locality, which contributes to its fast execution on modern hardware.

### 2. QuickSort Algorithm Breakdown

#### **Algorithm Steps:**

1. **Divide:**  
   Choose a pivot element and partition the array such that:
   - All elements less than the pivot are on its left.
   - All elements greater than the pivot are on its right.

2. **Conquer:**  
   Recursively apply QuickSort to the subarrays formed by the partitioning.

3. **Combine:**  
   No additional work is needed since the array is partitioned in place.

#### **Pseudocode:**

```pseudo
QuickSort(A, begin, end)
    if begin < end then:
        pivotIndex ← Partition(A, begin, end)
        QuickSort(A, begin, pivotIndex - 1)
        QuickSort(A, pivotIndex + 1, end)
```

#### **Partitioning Strategies in QuickSort**

Partitioning is a critical step in QuickSort, as it rearranges the array so that all elements less than the pivot come before it, and all elements greater than the pivot come after it. There are several strategies to implement partitioning. Below, we analyze one specific implementation from the lecture and then mention alternative approaches (Lomuto and Hoare) as comparisons.

**Lecture Partitioning Implementation:**

This implementation assumes:
- The array is indexed from 1 to n.
- There are no duplicate elements.
- n > 1.
- A sentinel value is defined such that $A[n+1] = \infty$ (or a value greater than any element in A).

**Pseudocode:**

```pseudo
partition(A[1..n], n, pIndex)
    // pIndex is the index of the chosen pivot
    pivot ← A[pIndex]
    swap(A[1], A[pIndex])   // Place pivot at A[1]
    low ← 2                // Start scanning from A[2]
    high ← n + 1           // Sentinel: A[n+1] is set to ∞
    
    while low < high do:
        while (A[low] < pivot) and (low < high) do low++;
        while (A[high] > pivot) and (low < high) do high--;
        if low < high then:
            swap(A[low], A[high])
    
    swap(A[1], A[low - 1])  // Place pivot into its final position
    return low - 1          // Return the pivot's final index
```

**Explanation:**

1. **Pivot Selection and Initialization:**
   - The pivot is chosen at index `pIndex` and then swapped with the first element (A[1]) so that the pivot is stored at the beginning of the array.
   - Two pointers are initialized:  
     - `low` starts at index 2 (just after the pivot).  
     - `high` is initialized to `n + 1`, where A[n+1] acts as a sentinel (set to a value greater than any element in A).

2. **Partitioning Loop:**
   - **Advancing `low`:**  
     The inner loop increments `low` while `A[low]` is less than or equal to the pivot and `low` remains less than `high`. This finds the first element from the left that is greater than the pivot.
   - **Decrementing `high`:**  
     Similarly, the inner loop decrements `high` while `A[high]` is greater than the pivot and `low` is still less than `high`. This finds the first element from the right that is less than or equal to the pivot.
   - **Swapping:**  
     If `low` is still less than `high`, the elements at `A[low]` and `A[high]` are swapped, correcting their positions relative to the pivot.

3. **Final Swap and Pivot Placement:**
   - Once the `low` and `high` pointers meet or cross, the pivot (currently at A[1]) is swapped with the element at `A[low - 1]`. This places the pivot in its correct sorted position.
   - The function then returns `low - 1`, which is the final index of the pivot.

### 3. Algorithm Invariant Analysis

The partition algorithm is designed to rearrange the array around a chosen pivot so that, by the end of the procedure, all elements less than the pivot are to its left and all elements greater than the pivot are to its right. Two key invariants are maintained during the loop execution:

**Invariant 1:** For all indices $i \geq \text{high}$, we have $A[i] > \text{pivot}$.

**Invariant 2:** For all indices $j$ such that $1 < j < \text{low}$, we have $A[j] < \text{pivot}$.

### 1. **Proof and Explanation:**

I. **Initialization:**

The algorithm begins by setting `high = n + 1` and, by assumption, $A[n+1] = \infty$.  
This ensures that the invariant $A[i] > \text{pivot}$ for all $i \geq \text{high}$ holds true at the start.

II. **The Inner Loop:**
   - **Incrementing `low`:**  
     The algorithm increments `low` while:
     ```pseudo
     while (A[low] < pivot) and (low < high) do low++;
     ```
      - **Outcome:** When this loop exits, it guarantees that $A[\text{low}] > \text{pivot}$.  
      - If $low$ equals $high$, the while condition ensures that the invariant still holds.

   - **Decrementing `high`:**  
     Similarly, the algorithm decrements `high` while:
     ```pseudo
     while (A[high] > pivot) and (low < high) do high--;
     ```
     - **Outcome:** When this loop exits, either $A[\text{high}] \leq \text{pivot}$ or $low$ equals $high$.  
     - If $high$ equals $low$, by our previous adjustment during low’s increment, we maintain $A[\text{high}] > \text{pivot}$.
     
III. **The Swap Operation:**

When the inner loops finish their execution, the algorithm has established that:
- **At index `low`:** The condition fails because $A[\text{low}] > \text{pivot}$ (otherwise, the first loop would have continued).
- **At index `high`:** The condition fails because $A[\text{high}] \leq \text{pivot}$ (otherwise, the second loop would have continued).

Thus, before the swap:
- $A[\text{low}] > \text{pivot}$, meaning this element belongs on the right side of the pivot.
- $A[\text{high}] \leq \text{pivot}$, meaning this element belongs on the left side of the pivot.

**During the Swap:**
- The algorithm swaps $A[\text{low}]$ and $A[\text{high}]$.  
- As a result, the element originally at `high` (which is $\leq$ pivot) moves to index `low`, and the element originally at `low` (which is $>$ pivot) moves to index `high`.

**Effect on Invariants:**
- After the swap, the subarray positions remain partitioned as intended:
  - All indices before `low` (from index 2 up to `low - 1`) still contain elements less than or equal to the pivot.
  - All indices from `high` onward contain elements greater than the pivot.
- The swap corrects the misplacement by ensuring that an element that should be on the left side of the pivot moves left, and an element that should be on the right side moves right.
- The algorithm then continues the while loop, further adjusting the `low` and `high` pointers, and maintaining the invariants:
  - For all $i \geq \text{high}$, $A[i] > \text{pivot}$.
  - For all $1 < j < \text{low}$, $A[j] < \text{pivot}$.

This careful exchange of elements guarantees that when the loop terminates, the array is correctly partitioned around the pivot.


### 2. **Invariant Maintenance at the End of Each Iteration:**

   - **For all $i \geq \text{high}, A[i] \gt \text{pivot}$:**  
     After each loop iteration, the algorithm ensures that every element in the range $[\text{high}, n+1]$ satisfies $A[i] > \text{pivot}$.
   
   - **For all $j$ where $1 < j < \text{low}, A[j] \lt pivot$:**  
     Simultaneously, all elements in the range $[2, \text{low}-1]$ satisfy $A[j] < \text{pivot}$.

### 3. **Conclusion of Partitioning:**

   - Once the loop terminates (i.e., when $low \geq high$), the invariants guarantee that:
     - Every element to the right of the final partition index (from $\text{high}$ onward) is greater than the pivot.
     - Every element before the index where the pivot will finally be placed is less than the pivot.
   - The pivot is then swapped into its final correct position (at index $low - 1$), ensuring that the entire array is partitioned around the pivot.

**Summary:**  
The invariant analysis confirms that at every iteration of the partitioning loop:
- All elements from index $\text{high}$ to the end of the array are greater than the pivot.
- All elements from index 2 to $\text{low}-1$ are less than the pivot.

This strict maintenance of invariants ensures that, when the loop concludes, the array is correctly partitioned around the pivot, which is then placed in its proper sorted position.

### 4. Handling Duplicates in QuickSort

When an array contains many duplicate elements, standard partitioning schemes (like Lomuto or Hoare) can perform inefficiently. This inefficiency occurs because these partition methods do not explicitly handle elements equal to the pivot, and they may end up placing almost all elements into one partition. In the worst-case scenario—such as when the array consists entirely of duplicates—this behavior can cause QuickSort to degrade to $O(n^2)$ time.

#### **The Problem with Duplicates**

- **Standard Partitioning Issue:**  
  Consider an array where every element is the same (e.g., `[5, 5, 5, 5, 5]`).  
  - When a pivot is chosen, every other element is equal to the pivot.
  - A standard partition algorithm may end up scanning through the entire array without effectively dividing it into smaller subarrays.
  - This leads to unbalanced partitions where one recursive call processes almost the entire array repeatedly, resulting in $O(n^2)$ performance.

- **Illustrative Example:**  
  For an array of $n$ identical elements, a naive partition might always return a partition index at one end (say index 1), leaving the remaining $n-1$ elements to be processed in the next recursive call. Thus, the recurrence becomes:
  
  $$
  T(n) = T(n-1) + O(n),
  $$
  
  which solves to $T(n) = O(n^2)$.

#### **Three-Way Partitioning**

To mitigate the issues caused by duplicates, a common solution is **three-way partitioning**, which divides the array into three distinct parts:
- **Elements less than the pivot.**
- **Elements equal to the pivot.**
- **Elements greater than the pivot.**

By doing so, all duplicate elements are grouped together, and the recursive calls only need to sort the subarrays with elements strictly less than or greater than the pivot, avoiding the worst-case behavior when many duplicates are present.

There are two common approaches to three-way partitioning:

- **Option 1: Two-Pass Partitioning**
  1. **Regular Partition:** First, partition the array using a standard method.
  2. **Pack Duplicates:** Then, perform an additional pass to group all elements equal to the pivot together.
  
- **Option 2: One-Pass Partitioning**
  - A more sophisticated approach that maintains four regions in the array:
    1. Elements less than the pivot.
    2. Elements equal to the pivot.
    3. Unclassified elements.
    4. Elements greater than the pivot.
  - This method processes the array in a single pass, simultaneously arranging elements into the appropriate regions.
  - While more complex to implement, it is efficient and minimizes the number of passes through the array.

#### **Three-Way Partitioning Pseudocode**

A common pseudocode for three-way partitioning (Option 2, one-pass) is as follows:

```pseudo
ThreeWayQuickSort(A, begin, end)
    if begin >= end then return
    pivot ← A[begin]
    lt ← begin    // A[begin ... lt-1] < pivot
    gt ← end      // A[gt+1 ... end] > pivot
    i ← begin + 1 // A[lt ... i-1] == pivot
    while i ≤ gt do:
        if A[i] < pivot then:
            swap(A[lt], A[i])
            lt ← lt + 1
            i ← i + 1
        else if A[i] > pivot then:
            swap(A[i], A[gt])
            gt ← gt - 1
        else:
            i ← i + 1
    ThreeWayQuickSort(A, begin, lt - 1)
    ThreeWayQuickSort(A, gt + 1, end)
```

#### **Key Takeaways**

- **Efficiency Improvement:**  
  Grouping duplicate elements together ensures that the recursive calls are made only on subarrays with elements that are strictly less than or greater than the pivot, which significantly improves performance on arrays with many duplicates.

- **Reduced Work:**  
  With three-way partitioning, if the array is composed mostly of duplicates, the recursive calls become trivial because the “equal” section does not need further sorting.

- **Implementation Choices:**  
  - **Two-Pass Partitioning** is simpler but requires an extra pass to group duplicates.
  - **One-Pass Partitioning** is more complex but more efficient since it minimizes the number of passes through the array.

By handling duplicates explicitly with three-way partitioning, QuickSort avoids the pitfall of degrading to $O(n^2)$ time on arrays with many equal elements, thereby preserving its average-case efficiency of $O(n \log n)$.

### 5. QuickSort Performance Analysis

#### **Time Complexity:**

- **Best Case:** $O(n \log n)$  
  Occurs when the pivot divides the array into two nearly equal halves.

- **Average Case:** $O(n \log n)$  
  With good pivot selection (often via randomization), QuickSort tends to run in $O(n \log n)$ time on average.

- **Worst Case:** $O(n^2)$  
  Occurs when the pivot results in highly unbalanced partitions (e.g., when the array is already sorted and the first or last element is chosen as the pivot).

#### **Space Complexity:**

- **In-Place Sorting:**  
  QuickSort is implemented in place, so its extra space is primarily due to the recursion stack, which is $O(\log n)$ on average.  
  In the worst-case scenario (highly unbalanced partitions), the recursion depth can become $O(n)$.

#### **Stability:**

- **Stability:**  
  QuickSort is generally **not stable**.  
  - **Explanation:**  
    The partitioning process typically involves swapping elements that are equal to the pivot, which can change their relative order in the original array.  
  - **Implications:**  
    In applications where preserving the original order of equal elements is important, additional modifications are required to make QuickSort stable, or an alternative stable sorting algorithm (such as MergeSort) should be used.

### 6. Comparison: QuickSort vs. MergeSort

| **Aspect**          | **MergeSort**                         | **QuickSort**                                |
|---------------------|---------------------------------------|----------------------------------------------|
| **Time Complexity** | $O(n \log n)$ in all cases           | Average: $O(n \log n)$; Worst: $O(n^2)$        |
| **Space Complexity**| $O(n)$ extra space                   | In-place, $O(\log n)$ stack space            |
| **Stability**       | Stable                                | Not stable (unless carefully implemented)    |
| **Practical Performance** | Predictable; often used for linked lists and stable sorting requirements | Typically faster in practice due to better cache locality and in-place sorting |
| **Pivot/Partitioning** | N/A (uses merging)                  | Pivot selection and partitioning are critical for performance |

#### **When to Use QuickSort vs. MergeSort**

- **MergeSort** is preferred when:
  - A stable sort is required.
  - Sorting linked lists.
  - Predictable $O(n \log n)$ performance is needed regardless of input.

- **QuickSort** is preferred when:
  - In-place sorting is a priority.
  - Average-case performance is more important than worst-case performance.
  - Cache performance is critical, as QuickSort often has better locality.

### 7. Modern Optimizations & Real-World Implementations

- **Hybrid Sorting Algorithms**:  
  Many real-world libraries (e.g., Timsort in Python, introsort in C++) combine multiple sorting algorithms. For instance, Insertion Sort may be used for small subarrays within QuickSort or MergeSort to improve performance.

- **Dual-Pivot QuickSort**:  
  An optimized version of QuickSort (used in Java's standard library) that uses two pivots for partitioning, often achieving about a 10% performance improvement over traditional QuickSort.

- **Algorithmic Improvements**:  
  Research by Bentley & McIlroy (1993) and Yaroslavskiy (2009) has led to practical enhancements that optimize sorting for modern hardware.

---

## Summary and Key Takeaways

1. **MergeSort & QuickSort Overview**:
   - **MergeSort** employs a divide-and-conquer approach with a predictable $O(n \log n)$ running time but uses extra space.
   - **QuickSort** is an in-place, generally faster algorithm in practice, with an average time complexity of $O(n \log n)$, although its worst-case is $O(n^2)$.

2. **Key Trade-offs**:
   - **Space vs. Time**: MergeSort's extra space requirement versus QuickSort's in-place efficiency.
   - **Stability**: MergeSort is stable, while QuickSort is typically not unless specifically implemented to be.
   - **Input Characteristics**: QuickSort's performance can be significantly affected by pivot selection and input order, whereas MergeSort's performance is consistent.

3. **Optimizations**:
   - Hybrid methods that combine the strengths of different algorithms can lead to better performance.
   - Randomization in QuickSort helps mitigate worst-case scenarios.
   - Advanced partitioning techniques, such as three-way partitioning, improve QuickSort's efficiency in the presence of duplicates.

4. **Practical Applications**:
   - **MergeSort** is ideal for applications requiring stable sort and predictable performance.
   - **QuickSort** is widely used in systems where in-place sorting and average-case performance are prioritized.

These lecture notes provide a comprehensive understanding of both MergeSort and QuickSort, covering their algorithms, analyses, and practical considerations. By grasping these concepts, you can make informed decisions about which sorting algorithm to use based on specific application needs and dataset characteristics.

---

## Appendix
### 'Merge' Function for Iterative Mergesort

```pseudo
// Pseudocode for Merge Procedure (Single Temporary Array Version)
// Used in Iterative MergeSort

procedure Merge(A, left, mid, right, temp)
    i ← left            // Pointer for the left subarray (A[left..mid])
    j ← mid + 1         // Pointer for the right subarray (A[mid+1..right])
    k ← left            // Pointer for the temporary array

    // Merge the two subarrays into temp
    while i ≤ mid and j ≤ right do:
        if A[i] ≤ A[j] then:
            temp[k] ← A[i]
            i ← i + 1
        else:
            temp[k] ← A[j]
            j ← j + 1
        k ← k + 1

    // Copy any remaining elements from the left subarray
    while i ≤ mid do:
        temp[k] ← A[i]
        i ← i + 1
        k ← k + 1

    // Copy any remaining elements from the right subarray
    while j ≤ right do:
        temp[k] ← A[j]
        j ← j + 1
        k ← k + 1

    // Copy the merged elements back into the original array A
    for l ← left to right do:
        A[l] ← temp[l]
```
**Explanation:**

In the **Single Temporary Array Version**, a temporary array `temp` of size at least `n` is allocated once. The `Merge` procedure uses this array to merge two sorted subarrays (from indices `left` to `mid` and `mid+1` to `right`) back into the original array `A`. After merging, the relevant portion of `temp` is copied back into `A`.

### 'Merge' Function for Two-Array Alternating Implementation

```pseudo
// Pseudocode for Merge Procedure (Two-Array Alternating Implementation)
// In this approach, we alternate the roles of A and B to avoid extra copying

procedure Merge(A, B, left, mid, right)
    i ← left            // Pointer for the left subarray in A (A[left..mid])
    j ← mid + 1         // Pointer for the right subarray in A (A[mid+1..right])
    k ← left            // Pointer for the merged output in B

    // Merge elements from A into B
    while i ≤ mid and j ≤ right do:
        if A[i] ≤ A[j] then:
            B[k] ← A[i]
            i ← i + 1
        else:
            B[k] ← A[j]
            j ← j + 1
        k ← k + 1

    // Copy any remaining elements from the left subarray
    while i ≤ mid do:
        B[k] ← A[i]
        i ← i + 1
        k ← k + 1

    // Copy any remaining elements from the right subarray
    while j ≤ right do:
        B[k] ← A[j]
        j ← j + 1
        k ← k + 1
```

**Explanation:**

In the **Two-Array Alternating Implementation**, two arrays `A` and `B` are used. In each merge step, the sorted output is written into `B` by merging segments from `A`. The roles of `A` and `B` are alternated in recursive calls to avoid the overhead of copying merged results back into the original array. This approach minimizes redundant copying and still requires only $O(n)$ additional space.

These implementations can be included in the appendix of your lecture notes to illustrate different techniques for optimizing MergeSort's space usage.

### **Quicksort: Alternative Partitioning Strategies**

- **Lomuto Partition Scheme:**
  - **Idea:** Uses a single pointer to track the "boundary" between elements less than or equal to the pivot and those greater.
  - **Differences:** Simpler code but can be less efficient in certain cases due to more swaps.
  
  ```pseudo
  Partition_Lomuto(A, begin, end)
      pivot ← A[end]
      i ← begin - 1
      for j ← begin to end - 1 do:
          if A[j] ≤ pivot then:
              i ← i + 1
              swap(A[i], A[j])
      swap(A[i + 1], A[end])
      return i + 1
  ```

- **Hoare Partition Scheme (given in lecture):**
  - **Idea:** Uses two pointers that start at the beginning and end of the array and move toward each other, swapping elements when necessary.
  - **Differences:** Generally performs fewer swaps than Lomuto’s scheme and can be more efficient, but it returns a different pivot index that may require slight modifications in the recursive calls.
  
  ```pseudo
  Partition_Hoare(A, begin, end)
      pivot ← A[begin] // Chooses first element as pivot for simplicity 
      i ← begin - 1
      j ← end + 1
      while true do:
          repeat i ← i + 1 until A[i] ≥ pivot
          repeat j ← j - 1 until A[j] ≤ pivot
          if i ≥ j then:
              return j
          swap(A[i], A[j])
  ```

**Summary of Differences:**
- The lecture’s partitioning implementation uses explicit pointers (`low` and `high`) along with a sentinel, providing clear boundaries and is tailored for arrays indexed from 1.  
- Lomuto and Hoare partition schemes are popular alternatives; each has its own trade-offs in terms of simplicity, the number of swaps, and efficiency.
- The choice of partitioning strategy can significantly impact QuickSort’s performance, particularly in terms of the balance of the partitions and the handling of duplicates.

This detailed analysis of partitioning strategies, including the lecture-specific implementation and alternative methods, forms the backbone of understanding QuickSort’s performance and behavior.

