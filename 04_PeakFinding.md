# Lecture 4: Peak Finding

## 1. Introduction

In optimization and search problems, identifying optimal points within data structures is crucial. **Peak Finding** is a fundamental concept in computer science that focuses on locating elements in arrays that are greater than or equal to their neighbors. This lecture explores both **One-Dimensional (1D)** and **Two-Dimensional (2D)** peak finding, examining algorithms that employ strategies similar to binary search to achieve efficient results.

### **Global vs. Local Maximums**

- **Global Maximum**:
  - **Definition**: The highest value in the entire dataset.
  - **Applications**:
    - Finding the most profitable investment.
    - Identifying the highest-scoring student in a class.
    - Determining the best scenic viewpoint.

- **Local Maximum (Peak)**:
  - **Definition**: An element that is greater than or equal to its immediate neighbors.
    $$
    A[i-1] \leq A[i] \geq A[i+1]
    $$
    *(Assuming arrays are indexed from 1, and $A[0] = A[n+1] = -\infty$)*
  - **Advantages**:
    - **Efficiency**: Finding a local maximum can be faster than finding a global maximum.
    - **Sufficiency**: Often, a local maximum is "good enough" for practical purposes.
    - **Proximity**: Local maxima are typically close to the global maximum, providing near-optimal solutions quickly.

### **Why Local Maximums?**

- **Good Enough Solution**: In many real-world scenarios, finding a local maximum provides a satisfactory solution without the computational overhead of identifying the global maximum.
- **Efficiency**: Algorithms designed to find local maxima often have lower time complexities, making them suitable for large datasets.
- **Practicality**: Local maxima can be sufficient in applications like resource allocation, scheduling, and optimization tasks where a globally optimal solution is unnecessary or impractical.

---

## 2. 1-Dimensional (1D) Peak Finding

### 2.1. Definitions

#### **Global Maximum**

- **Input**: Array $A[1..n]$
- **Output**: The global maximum element in $A$.
  
##### **Pseudocode: FindMax**

```pseudo
int FindMax(A, n)
    max = A[1]
    for i = 2 to n do:
        if A[i] > max then
            max = A[i]
    return max
```

- **Time Complexity**: $O(n)$
- **Limitation**: Linear time can be too slow for large datasets, motivating the search for more efficient algorithms.

#### **Local Maximum (Peak)**

- **Definition**: An element $A[i]$ is a **local maximum** if it satisfies:
  $$
  A[i-1] \leq A[i] \geq A[i+1]
  $$
  *(With $A[0] = A[n+1] = -\infty$ to handle boundary conditions)*

- **Assumption**: The array is indexed from 1 to $n$.

### 2.2. Peak Finding Algorithms

#### **Algorithm 1: Simple Linear Scan**

- **Approach**:
  - Start from the first element.
  - Examine each element sequentially.
  - Stop when a peak is found.

```pseudo
int FindPeakLinear(A, n)
    for i = 1 to n do:
        if A[i-1] <= A[i] and A[i] >= A[i+1] then
            return i
    return -1 // No peak found
```

- **Running Time**: $O(n)$
- **Explanation**:
  - The algorithm checks each element to determine if it's a peak.
  - It guarantees finding at least one peak due to the boundary conditions.

#### **Algorithm 2: Reduce-and-Conquer Strategy**

To achieve a more efficient peak finding in a one-dimensional array, we employ the **Reduce-and-Conquer** strategy, which is akin to the binary search method. This approach systematically reduces the search space by half in each recursive step, ensuring that a peak is found in logarithmic time.

```pseudo
int FindPeak(A, begin, end)
    if begin == end then
        return begin
    mid = begin + (end - begin) / 2
    if A[mid] > A[mid - 1] and A[mid] > A[mid + 1] then
        return mid
    else if A[mid + 1] > A[mid] then
        return FindPeak(A, mid + 1, end)
    else // A[mid - 1] > A[mid]
        return FindPeak(A, begin, mid - 1)
```

##### **How the Algorithm Works**

1. **Initialization**:
   - **Input**: Array `A[1..n]`, `begin = 1`, `end = n`.
   - The algorithm starts by examining the middle element of the array.

2. **Middle Element Evaluation**:
   - **Calculate `mid`**: `mid = begin + (end - begin) / 2`.
   - **Check if `A[mid]` is a Peak**:
     - If `A[mid]` is greater than both its immediate neighbors (`A[mid - 1]` and `A[mid + 1]`), it is a peak, and the algorithm returns `mid`.

3. **Deciding the Search Direction**:
   - **If `A[mid + 1] > A[mid]`**:
     - There must be a peak in the **right half** of the array (`mid + 1` to `end`).
     - **Recurse** on the right half: `FindPeak(A, mid + 1, end)`.
   - **Else**:
     - There must be a peak in the **left half** of the array (`begin` to `mid - 1`).
     - **Recurse** on the left half: `FindPeak(A, begin, mid - 1)`.

4. **Termination**:
   - The recursion continues, halving the search space each time, until a peak is found.
   - **Base Case**: When `begin == end`, the single element is a peak by default.

##### **Key Properties and Invariants**

1. **Existence of a Peak in the Search Range**:
   - **Invariant**: At any recursive step, there exists at least one peak within the current search range `[begin, end]`.
   - **Justification**:
     - If `A[mid + 1] > A[mid]`, a peak must exist in the right half.
     - If `A[mid - 1] > A[mid]`, a peak must exist in the left half.
     - If neither is true, `A[mid]` itself is a peak.

2. **Loop Invariant**:
   - The algorithm maintains that there is always a peak within the current search boundaries.
   - This invariant is preserved through each recursive call, ensuring the algorithm progresses towards a peak.

3. **Correctness through Induction**:
   - **Base Case**: An array of size 1 (`begin == end`) trivially contains a peak.
   - **Inductive Step**: Assume the algorithm correctly finds a peak for arrays smaller than size `n`. For an array of size `n`, the algorithm reduces the problem to a subarray where a peak must exist, thereby maintaining correctness.

##### **Explanation of Efficiency**

- **Time Complexity Analysis**:
  
  The algorithm divides the search space by half in each recursive call, similar to binary search.

  $$
  T(n) = T\left(\frac{n}{2}\right) + \Theta(1)
  $$

  - **Unrolling the Recurrence**:

    $
    T(n) = T\left(\frac{n}{2}\right) + \Theta(1) \\
    = T\left(\frac{n}{4}\right) + \Theta(1) + \Theta(1) \\
    = T\left(\frac{n}{8}\right) + \Theta(1) + \Theta(1) + \Theta(1) \\
    \vdots \\
    = T(1) + \Theta(\log n) \\
    = \Theta(\log n)
    $

  - **Conclusion**: The running time of the Reduce-and-Conquer Peak Finding algorithm is $O(\log n)$, making it significantly more efficient than the linear scan method.

- **Space Complexity**:
  
  - **Iterative Implementation**: $O(1)$, as it uses a constant amount of additional space.
  - **Recursive Implementation**: $O(\log n)$, due to the recursion stack.

##### **Illustrative Example**

Consider the array indexed from 1 to 12:

$$
A = [2, 4, 9, 2, 11, 12, 13, 14, 15, 18, 17, 5]
$$

1. **First Call**: `FindPeak(A, 1, 12)`
   - `mid = 1 + (12 - 1) / 2 = 6`
   - `A[6] = 12`
   - Compare with neighbors: `A[5] = 11` and `A[7] = 13`
   - Since `A[7] > A[6]`, recurse on the right half: `FindPeak(A, 7, 12)`

2. **Second Call**: `FindPeak(A, 7, 12)`
   - `mid = 7 + (12 - 7) / 2 = 9`
   - `A[9] = 15`
   - Compare with neighbors: `A[8] = 14` and `A[10] = 18`
   - Since `A[10] > A[9]`, recurse on the right half: `FindPeak(A, 10, 12)`

3. **Third Call**: `FindPeak(A, 10, 12)`
   - `mid = 10 + (12 - 10) / 2 = 11`
   - `A[11] = 17`
   - Compare with neighbors: `A[10] = 18` and `A[12] = 5`
   - Since `A[10] > A[11]`, recurse on the left half: `FindPeak(A, 10, 10)`

4. **Fourth Call**: `FindPeak(A, 10, 10)`
   - `begin = end = 10`
   - `A[10] = 18` is a peak (since `A[9] = 15` and `A[11] = 17`)
   - **Return**: `10`

- **Result**: The algorithm correctly identifies `A[10] = 18` as a peak.

##### **Handling Potential Issues**

- **Missing Else Condition**:
  
  Initially, the algorithm appeared to miss an `else` condition. However, by carefully structuring the conditional checks, the algorithm implicitly handles the scenario where neither neighbor is greater, ensuring that the current element is a peak.

- **Edge Cases**:
  
  - **All Elements Equal**:
    - Example: `[7, 7, 7, 7, 7]`
    - **Behavior**: The algorithm will identify the middle element as a peak since it is not less than its neighbors.
  
  - **Strictly Increasing or Decreasing Arrays**:
    - Example (Increasing): `[1, 2, 3, 4, 5]`
      - **Peak**: The last element (`5`) is a peak.
    - Example (Decreasing): `[5, 4, 3, 2, 1]`
      - **Peak**: The first element (`5`) is a peak.

##### **Formal Correctness Proof**

1. **Base Case**:
   - For an array of size 1 (`begin == end`), the single element is trivially a peak.
   
2. **Inductive Step**:
   - Assume the algorithm correctly finds a peak for all arrays of size less than `n`.
   - For an array of size `n`, the algorithm examines the middle element.
     - If the middle element is a peak, it returns immediately.
     - If the right neighbor is greater, by the **Invariant**, a peak must exist in the right half.
     - If the left neighbor is greater, a peak must exist in the left half.
   - By the inductive hypothesis, the recursive call on the selected half correctly identifies a peak.
   
3. **Conclusion**:
   - By induction, the algorithm correctly finds a peak for any array of size `n`.

##### **Running Time Analysis**

- **Recurrence Relation**:
  $$
  T(n) = T\left(\frac{n}{2}\right) + \Theta(1)
  $$
  - **Explanation**:
    - **$T\left(\frac{n}{2}\right)$**: Time for the recursive call on half the array.
    - **$\Theta(1)$**: Constant time for comparing the middle element with its neighbors.

- **Solving the Recurrence**:
  - **Unrolling**:

    $
    T(n) = T\left(\frac{n}{2}\right) + \Theta(1) \\
    = T\left(\frac{n}{4}\right) + \Theta(1) + \Theta(1) \\
    = T\left(\frac{n}{8}\right) + \Theta(1) + \Theta(1) + \Theta(1) \\
    \vdots \\
    = T(1) + \Theta(\log n)
    $
  
  - **Conclusion**: 
    $$
    T(n) = O(\log n)
    $$
  
  - The algorithm efficiently reduces the search space logarithmically, ensuring a fast peak finding process even for large arrays.

##### **Illustrative Example**

Consider the array indexed from 1 to 12:

$$
A = [2, 4, 9, 2, 11, 12, 13, 14, 15, 18, 17, 5]
$$

1. **First Call**: `FindPeak(A, 1, 12)`
   - `mid = 1 + (12 - 1) / 2 = 6`
   - `A[6] = 12`
   - Compare with neighbors: `A[5] = 11` and `A[7] = 13`
   - Since `A[7] > A[6]`, recurse on the right half: `FindPeak(A, 7, 12)`

2. **Second Call**: `FindPeak(A, 7, 12)`
   - `mid = 7 + (12 - 7) / 2 = 9`
   - `A[9] = 15`
   - Compare with neighbors: `A[8] = 14` and `A[10] = 18`
   - Since `A[10] > A[9]`, recurse on the right half: `FindPeak(A, 10, 12)`

3. **Third Call**: `FindPeak(A, 10, 12)`
   - `mid = 10 + (12 - 10) / 2 = 11`
   - `A[11] = 17`
   - Compare with neighbors: `A[10] = 18` and `A[12] = 5`
   - Since `A[10] > A[11]`, recurse on the left half: `FindPeak(A, 10, 10)`

4. **Fourth Call**: `FindPeak(A, 10, 10)`
   - `begin = end = 10`
   - `A[10] = 18` is a peak (since `A[9] = 15` and `A[11] = 17`)
   - **Return**: `10`

- **Result**: The algorithm correctly identifies `A[10] = 18` as a peak.

##### **Handling Potential Issues**

- **Missing Else Condition**:
  
  Initially, the algorithm appeared to miss an `else` condition. However, by structuring the conditional checks appropriately, the algorithm ensures that if neither neighbor is greater, the current middle element is a peak. This implicit handling negates the need for an explicit `else` clause.

- **Edge Cases**:
  
  - **All Elements Equal**:
    - Example: `[7, 7, 7, 7, 7]`
    - **Behavior**: The algorithm will identify the middle element as a peak since it is not less than its neighbors.
  
  - **Strictly Increasing or Decreasing Arrays**:
    - Example (Increasing): `[1, 2, 3, 4, 5]`
      - **Peak**: The last element (`5`) is a peak.
    - Example (Decreasing): `[5, 4, 3, 2, 1]`
      - **Peak**: The first element (`5`) is a peak.

##### **Formal Correctness Proof**

1. **Base Case**:
   - For an array of size 1 (`begin == end`), the single element is trivially a peak.
   
2. **Inductive Step**:
   - Assume the algorithm correctly finds a peak for arrays of size less than `n`.
   - For an array of size `n`, the algorithm examines the middle element.
     - If the middle element is a peak, it returns immediately.
     - If the right neighbor is greater, a peak must exist in the right half.
     - If the left neighbor is greater, a peak must exist in the left half.
   - By the inductive hypothesis, the recursive call on the selected half correctly identifies a peak.
   
3. **Conclusion**:
   - By induction, the algorithm correctly finds a peak for any array of size `n`.

##### **Running Time Analysis**

- **Recurrence Relation**:
  $$
  T(n) = T\left(\frac{n}{2}\right) + \Theta(1)
  $$
  - **Explanation**:
    - **$T\left(\frac{n}{2}\right)$**: Time for the recursive call on half the array.
    - **$\Theta(1)$**: Constant time for comparing the middle element with its neighbors.

- **Solving the Recurrence**:
  - **Unrolling**:

    $
    T(n) = T\left(\frac{n}{2}\right) + \Theta(1) \\
    = T\left(\frac{n}{4}\right) + \Theta(1) + \Theta(1) \\
    = T\left(\frac{n}{8}\right) + \Theta(1) + \Theta(1) + \Theta(1) \\
    \vdots \\
    = T(1) + \Theta(\log n)
    $
  
  - **Conclusion**: 
    $$
    T(n) = O(\log n)
    $$
  
  - The algorithm efficiently reduces the search space logarithmically, ensuring a fast peak finding process even for large arrays.

---

### 2.3. Steep Peaks

- **Definition**: A **steep peak** is a local maximum where:
  $$
  A[i-1] < A[i] > A[i+1]
  $$

- **Challenge**:
  - Finding a steep peak may require examining more elements, potentially increasing the time complexity.
  - Example Problematic Array:
    ```
    7 7 7 7 7 7 7 7 7 8 7 7
    ```
    - Multiple potential peak positions, making it difficult to locate a unique steep peak efficiently.

- **Implications**:
  - The reduce-and-conquer approach may not efficiently find steep peaks, as the algorithm could still require linear time in the worst case.

#### **Problematic Example**

Consider the array:
```
7 7 7 7 7 7 7 7 7 8 7 7
```
*(Indexed from 1 to 12)*

- **Observation**:
  - The element `8` at index `10` is a steep peak since:
    $$
    A[9] = 7 < A[10] = 8 > A[11] = 7
    $$
  - However, multiple positions have the same value (`7`), complicating the search for a unique steep peak.

- **Issue**:
  - The reduce-and-conquer algorithm may not efficiently identify steep peaks in such arrays, as it relies on the presence of a single peak to guide the search direction.

#### **Algorithmic Implications**

- **Recurrence Relation for Steep Peak Finding**:
  $$
  T(n) = 2T\left(\frac{n}{2}\right) + O(1)
  $$
  
- **Unrolling the Recurrence**:

  $
  T(n) = 2T\left(\frac{n}{2}\right) + 1 \\
  = 2\left(2T\left(\frac{n}{4}\right) + 1\right) + 1 \\
  = 4T\left(\frac{n}{4}\right) + 2 + 1 \\
  = \dots \\
  = nT(1) + n/2 + n/4 + \dots + 1 \\
  = O(n)
  $
  
**Conclusion**: Finding steep peaks using a modified reduce-and-conquer approach results in a linear time complexity, negating the efficiency gains of the original binary search-inspired method.

---

## 3. 2-Dimensional (2D) Peak Finding

### 3.1. Definitions

#### **2D Peak**

- **Definition**: An element $A[i][j]$ in a 2D array $A[1..n, 1..m]$ is a **peak** if it is not smaller than its (up to) four neighbors:
  $$
  A[i][j] \geq A[i-1][j], \quad A[i][j] \geq A[i+1][j], \quad A[i][j] \geq A[i][j-1], \quad A[i][j] \geq A[i][j+1]
  $$
  *(Assuming boundaries are treated with sentinel values like $-\infty$)*

### 3.2. Peak Finding Algorithms

#### **Algorithm 1: Column-Wise Global Maximum**

- **Approach**:
  1. **Find the Global Maximum** in each column.
  2. **Apply 1D Peak Finding** on the array of column maxima to identify a peak.

- **Pseudocode: FindPeak2D_GlobalMax**

```pseudo
int FindPeak2D_GlobalMax(A, n, m)
    array MaxPerColumn[1..m]
    for j = 1 to m do:
        MaxPerColumn[j] = FindMaxColumn(A, n, j)
    return FindPeak(MaxPerColumn, m)
```

- **Function: FindMaxColumn**

```pseudo
int FindMaxColumn(A, n, j)
    max = A[1][j]
    for i = 2 to n do:
        if A[i][j] > max then
            max = A[i][j]
    return max
```

- **Correctness**:
  - The global maxima of columns guarantee that the selected peak from `MaxPerColumn` is a peak in the 2D array.
  
- **Time Complexity**:
  - Finding the global maximum in each column: $O(mn)$
  - Applying 1D peak finding on `MaxPerColumn`: $O(\log m)$
  - **Total**: $O(mn + \log m)$
  
- **Limitation**:
  - Inefficient for large matrices due to the initial $O(mn)$ step of finding global maxima.

#### **Algorithm 2: Column-Wise Local Peaks**

- **Approach**:
  1. **Find a Local Peak** in each column.
  2. **Apply 1D Peak Finding** on the array of local peaks.

- **Pseudocode: FindPeak2D_LocalPeak**

```pseudo
int FindPeak2D_LocalPeak(A, n, m)
    array LocalPeaks[1..m]
    for j = 1 to m do:
        LocalPeaks[j] = FindLocalPeakColumn(A, n, j)
    return FindPeak(LocalPeaks, m)
```

- **Function: FindLocalPeakColumn**

```pseudo
int FindLocalPeakColumn(A, n, j)
    for i = 1 to n do:
        if A[i][j] >= A[i-1][j] and A[i][j] >= A[i+1][j] then
            return A[i][j]
    return -1 // No peak found
```

- **Correctness**:
  - **Incorrect**. A counter-example demonstrates that selecting a local peak from each column does not guarantee a peak in the entire 2D array.

##### **Counter-Example**

Consider the 2D array:

|   | 1 | 2 | 3 | 4 |
|---|---|---|---|---|
| 1 | 2 | 4 | 9 | 2 |
| 2 | 11|12|13|14|
| 3 |15|18|17|5 |

- **Local Peaks in Each Column**:
  - **Column 1**: 11 (since 11 > 2 and 11 > 15)
  - **Column 2**: 12 (since 12 > 4 and 12 > 18)
  - **Column 3**: 13 (since 13 > 9 and 13 > 17)
  - **Column 4**: 14 (since 14 > 2 and 14 > 5)

- **Array of Local Peaks**: `[11, 12, 13, 14]`

- **Applying 1D Peak Finding**:
  - The peak found is `13`.

- **Verification in Original Array**:
  - `A[2][3] = 13` is not a peak in the original array since:
    $$
    A[2][3] = 13 < A[3][3] = 17
    $$
  - **Conclusion**: The algorithm incorrectly identifies `13` as a peak, whereas `17` at `A[3][3]` is a valid peak.

#### **Algorithm 3: Optimized 2D Peak Finding**

- **Approach**:
  1. **Use 1D Peak Finding** to select specific columns.
  2. **Find the Maximum Element** in each selected column.
  3. **Apply 1D Peak Finding** on the array of these maxima to identify a valid 2D peak.

- **Pseudocode: FindPeak2D_Optimized**

```pseudo
int FindPeak2D_Optimized(A, n, m)
    // Step 1: Use 1D Peak Finder to examine log m columns
    int peakColumn = FindPeak(SelectedColumns, log m)
    
    // Step 2: Find the maximum element in the peak column
    int peakRow = FindMaxRow(A, n, peakColumn)
    
    // Step 3: Verify if the found element is a peak in 2D
    if A[peakRow][peakColumn] >= A[peakRow-1][peakColumn] and
       A[peakRow][peakColumn] >= A[peakRow+1][peakColumn] and
       A[peakRow][peakColumn] >= A[peakRow][peakColumn-1] and
       A[peakRow][peakColumn] >= A[peakRow][peakColumn+1] then
        return A[peakRow][peakColumn]
    else
        // Recurse on the side with the greater neighbor
        if A[peakRow][peakColumn-1] > A[peakRow][peakColumn] then
            return FindPeak2D_Optimized(A, n, peakColumn - 1)
        else
            return FindPeak2D_Optimized(A, n, peakColumn + 1)
```

- **Function: FindMaxRow**

```pseudo
int FindMaxRow(A, n, j)
    max = A[1][j]
    for i = 2 to n do:
        if A[i][j] > max then
            max = A[i][j]
    return i // Return the row index of the maximum element
```

- **Correctness**:
  - Ensures that the identified peak is valid within the 2D context by leveraging the properties of 1D peak finding.
  
- **Time Complexity**:
  - **Finding peaks in log m columns**: $O(n \log m)$
  - **Finding the maximum in each selected column**: $O(n)$ per column
  - **Total**: $O(n \log m)$

---

### 3.3. Insights and Best Practices

- **Invariant Maintenance**:
  - Maintaining **loop invariants** is crucial for proving algorithm correctness and ensuring that each iteration progresses towards finding a peak.
  
- **Algorithm Refinement**:
  - Iteratively identifying and fixing bugs enhances both the **correctness** and **efficiency** of algorithms.
  
- **Handling Edge Cases**:
  - Always consider edge cases, such as arrays with uniform values or minimal sizes, to ensure algorithm robustness.
  
- **Monotonic Functions and Binary Search**:
  - Binary search principles can be extended to solve optimization problems beyond simple searches, particularly those involving **monotonic relationships**.
  
- **Greedy Algorithms**:
  - Combining binary search with **greedy strategies** can lead to efficient solutions in allocation and optimization tasks, as seen in the tutorial allocation example.

---

## 4. Conclusion

### **Summary of Key Points**

1. **Global vs. Local Maximums**:
   - **Global Maximum**: The absolute highest value in an array.
   - **Local Maximum (Peak)**: An element that is greater than or equal to its immediate neighbors.
   - **Importance**: While global maxima provide the best possible solution, local maxima offer efficient and often sufficient alternatives.

2. **1D Peak Finding**:
   - **Linear Scan (Algorithm 1)**:
     - Simple and guarantees finding a peak.
     - Time Complexity: $O(n)$
   - **Reduce-and-Conquer (Algorithm 2)**:
     - Utilizes a binary search-like approach to find a peak.
     - Time Complexity: $O(\log n)$
     - **Steep Peaks**: Finding strict peaks can degrade performance to $O(n)$.

3. **2D Peak Finding**:
   - **Algorithm 1: Column-Wise Global Maximum**:
     - Correct but inefficient with Time Complexity: $O(mn + \log m)$
   - **Algorithm 2: Column-Wise Local Peaks**:
     - Incorrect due to potential invalid peak identification.
     - Demonstrated through a counter-example.
   - **Algorithm 3: Optimized 2D Peak Finding**:
     - Efficient and correct with Time Complexity: $O(n \log m)$

### **Practical Applications**

- **Resource Allocation**: Efficiently distributing resources (e.g., tutorial slots) to minimize maximum load.
- **Optimization Problems**: Locating optimal points in data structures for tasks like peak detection in signal processing.
- **Scheduling**: Assigning tasks or jobs to time slots or workers while balancing workload.

### **Final Thoughts**

Mastering peak finding algorithms, both in 1D and 2D, equips you with essential tools for tackling a wide range of optimization and search problems in computer science. By understanding the underlying principles, such as reduce-and-conquer strategies and loop invariants, and by adhering to best practices in algorithm design and analysis, you can develop efficient, correct, and scalable solutions to complex computational challenges.