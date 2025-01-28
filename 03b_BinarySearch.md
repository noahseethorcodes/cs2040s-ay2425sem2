# Lecture 3b: Binary Search

# Table of Contents
- [1. Introduction to Binary Search](#1-introduction-to-binary-search)
  - [Reduce-and-Conquer Strategy](#reduce-and-conquer-strategy)
- [2. Iterative Refinement of Binary Search](#2-iterative-refinement-of-binary-search)
  - [Version 1: Initial Implementation](#version-1-initial-implementation)
  - [Version 2: Addressing Initial Bugs](#version-2-addressing-initial-bugs)
  - [Final Version: Correct Implementation](#final-version-correct-implementation)
- [3. Terminology Related to Algorithms](#3-terminology-related-to-algorithms)
  - [3.1 Specification](#31-specification)
  - [3.2 Preconditions](#32-preconditions)
  - [3.3 Postconditions](#33-postconditions)
  - [3.4 Invariant](#34-invariant)
  - [3.5 Loop Invariant](#35-loop-invariant)
- [4. Correctness and Performance](#4-correctness-and-performance)
  - [4.1 Correctness](#41-correctness)
  - [4.2 Performance](#42-performance)
- [5. Correct Implementation of Binary Search](#5-correct-implementation-of-binary-search)
  - [5.1 Final Pseudocode](#51-final-pseudocode)
  - [5.2 Explanation of Pseudocode](#52-explanation-of-pseudocode)
- [6. Extending Binary Search Beyond Sorted Arrays](#6-extending-binary-search-beyond-sorted-arrays)
  - [6.1 Generalized Binary Search Use-Cases](#61-generalized-binary-search-use-cases)
  - [6.2 Example Scenario: Tutorial Allocation](#62-example-scenario-tutorial-allocation)
      - [Problem Description](#problem-description)
      - [Motivation for Using Binary Search](#motivation-for-using-binary-search)
      - [Defining the Monotonic Function](#defining-the-monotonic-function)
      - [Applying Binary Search to the Problem](#applying-binary-search-to-the-problem)
      - [Pseudocode Implementation](#pseudocode-implementation)
      - [Explanation of the Pseudocode](#explanation-of-the-pseudocode)
      - [Conclusion](#conclusion)
  - [6.3 Insights and Best Practices](#63-insights-and-best-practices)
- [7. Summary](#7-summary)
  - [Key Concepts](#key-concepts)
  - [Practical Takeaways](#practical-takeaways)
  - [Final Thoughts](#final-thoughts)

---
## 1. Introduction to Binary Search

**Binary Search** is an efficient algorithm for finding an item from a sorted list of items. It employs the **Reduce-and-Conquer** strategy, which involves breaking the problem into smaller subproblems, solving each subproblem independently, and combining their solutions to solve the original problem.

### Reduce-and-Conquer Strategy
- **Start with** $n$ elements to search.
- **Eliminate half** of them.
- **End with** $\frac{n}{2}$ elements to search.
- **Repeat** the process until the target element is found or the search space is exhausted.

---
    
### 2. Iterative Refinement of Binary Search

To illustrate careful programming practices, we examine different iterations of the binary search algorithm. By identifying and fixing bugs in each version, we refine the algorithm to ensure correctness and efficiency.

#### **Version 1: Initial Implementation**

```pseudo
int Search(A, key, n)
    begin = 0
    end = n
    while begin != end do:
        if key < A[(begin + end) / 2] then
            end = (begin + end) / 2 - 1
        else
            begin = (begin + end) / 2
    return A[end]
```

##### **Identified Bugs and Explanations**

1. **Bug 1: Array Out of Bounds**
   - **Issue**: The variable `end` is initialized to `n`, which is **out of bounds** since array indices range from `0` to `n-1`.
   - **Consequence**: Accessing `A[n]` will result in an error as it exceeds the array's valid index range.
   
2. **Bug 2: Potential Non-Termination**
   - **Issue**: The loop condition `while begin != end` combined with integer division rounding down can lead to scenarios where the loop **never terminates**.
   - **Example**:
     - **Initial State**: `begin = 0`, `end = 1`
     - **First Iteration**:
       - `mid = (0 + 1) / 2 = 0`
       - If `key >= A[mid]`, then `begin = mid = 0`
     - **Result**: `begin` remains `0`, `end` becomes `0 - 1 = -1`, which may cause incorrect behavior or termination issues.
   
3. **Bug 3: Incorrect Return Value**
   - **Issue**: The algorithm returns `A[end]`, which **may not** be the correct index of the `key`.
   - **Consequence**: Even if the `key` exists in the array, returning `A[end]` does not provide its index and may return an incorrect element.

---

#### **Version 2: Addressing Initial Bugs**

```pseudo
int Search(A, key, n)
    begin = 0
    end = n - 1
    while begin < end do:
        if key <= A[(begin + end) / 2] then
            end = (begin + end) / 2
        else
            begin = 1 + (begin + end) / 2
    return (A[begin] == key) ? begin : -1
```

##### **Identified Bug and Explanation**

4. **Bug 4: Potential Integer Overflow**
   - **Issue**: Calculating `mid` as `(begin + end) / 2` can lead to **integer overflow** when `begin` and `end` are large.
   - **Consequence**: The sum `begin + end` may exceed the maximum value an integer can hold, causing incorrect `mid` calculation and potentially undefined behavior.

---

#### **Final Version: Correct Implementation**

```pseudo
int Search(A, key, n)
    begin = 0
    end = n - 1
    while begin < end do:
        mid = begin + (end - begin) / 2
        if key <= A[mid] then
            end = mid
        else
            begin = mid + 1
    return (A[begin] == key) ? begin : -1
```

##### **Explanation of Fixes**

1. **Fixing Bug 1: Correcting Array Bounds**
   - **Change**: Initialize `end` to `n - 1` instead of `n`.
   - **Result**: Ensures that `end` references the **last valid index** of the array, preventing out-of-bounds access.

2. **Fixing Bug 2: Ensuring Loop Termination**
   - **Change**: Modify the loop condition to `while begin < end` and adjust the update rules.
   - **Result**: Prevents the loop from getting stuck by ensuring that `begin` and `end` converge correctly.

3. **Fixing Bug 3: Correct Return Value**
   - **Change**: Return the index `begin` if `A[begin]` equals the `key`, otherwise return `-1`.
   - **Result**: Provides the **correct index** of the `key` if it exists, or `-1` to indicate absence.

4. **Fixing Bug 4: Preventing Integer Overflow**
   - **Change**: Calculate `mid` as `begin + (end - begin) / 2` instead of `(begin + end) / 2`.
   - **Result**: Avoids the risk of integer overflow by rearranging the calculation, ensuring accurate `mid` determination even with large indices.

##### **Additional Enhancements**

- **Use of Conditional Operator**:
  - The final return statement utilizes a **ternary operator** to succinctly check if the `key` exists at the `begin` index and return the appropriate value.
  
- **Clarity and Readability**:
  - Improved variable naming and structured conditional statements enhance the **readability** and **maintainability** of the pseudocode.

##### **Final Pseudocode Overview**

The final version of the binary search algorithm ensures:
- **Correctness**: Accurately finds the `key` if it exists.
- **Efficiency**: Maintains a time complexity of $O(\log n)$ by halving the search space in each iteration.
- **Robustness**: Prevents common bugs such as out-of-bounds access and integer overflow.

---

## 3. Terminology Related to Algorithms

Understanding the following terms is crucial for analyzing and designing algorithms effectively.

### 3.1 Specification

- **Standard Specification**:
  - **Returns** the element if it is in the array.
  - **Returns** `null` if it is not in the array.
  
- **Alternate Specification**:
  - **Returns** the index if the element is in the array.
  - **Returns** `-1` if it is not in the array.

### 3.2 Preconditions

- **Definition**: A condition that must be true before the execution of an algorithm or function.
- **Good Practice**: **Validate preconditions** when possible to ensure the algorithm operates under correct assumptions.
  
- **Preconditions for Binary Search**:
  - **Array is of size** $n$.
  - **Array is sorted**.

### 3.3 Postconditions

- **Definition**: A condition that must be true after the execution of an algorithm or function.
  
- **Postconditions for Binary Search**:
  - **If the element is in the array**: `A[begin] = key`.

### 3.4 Invariant

- **Definition**: A relationship between variables that remains true throughout the execution of an algorithm.

### 3.5 Loop Invariant

- **Definition**: A relationship between variables that is true at the beginning (or end) of each iteration of a loop.
  
- **Loop Invariant for Binary Search**:
  - $A[\text{begin}] \leq \text{key} \leq A[\text{end}]$
  
- **Interpretation**:
  - The `key` is within the range of the subarray `A[begin:end+1]`, where `begin` and `end` denote the current range of the subarray being searched.

---

## 4. Correctness and Performance

### 4.1 Correctness

- **Loop Invariant**: Ensures that the algorithm maintains the condition $A[\text{begin}] \leq \text{key} \leq A[\text{end}]$ throughout its execution.
  
- **Termination**: The algorithm terminates when the search space is reduced to zero or one element, ensuring that the `key` is either found or correctly reported as not present.

### 4.2 Performance

- **Reduction of Search Space**:
  - Each iteration reduces the search space by half.
  
- **Analysis**:
  $$
  (\text{end} - \text{begin}) \leq \frac{n}{2^k} \quad \text{in iteration} \ k.
  $$
  
  - **Iteration Breakdown**:
    - **Iteration 1**: $(\text{end} - \text{begin}) = n$
    - **Iteration 2**: $(\text{end} - \text{begin}) = \frac{n}{2}$
    - **Iteration 3**: $(\text{end} - \text{begin}) = \frac{n}{4}$
    - $\vdots$
    - **Iteration k**: $(\text{end} - \text{begin}) \leq \frac{n}{2^k}$
  
  - **Termination Condition**:
    $$
    \frac{n}{2^k} = 1 \implies k = \log(n)
    $$
  
- **Time Complexity**:
  $$
  O(\log n)
  $$
  
- **Space Complexity**:
  - **Iterative Implementation**: $O(1)$ (constant space).
  - **Recursive Implementation**: $O(\log n)$ (due to recursion stack).

---

## 5. Correct Implementation of Binary Search

### 5.1 Final Pseudocode

```pseudo
int search(A, key, n)
    begin = 0
    end = n - 1
    while begin < end do:
        mid = begin + (end - begin) / 2
        if key <= A[mid] then
            end = mid
        else
            begin = mid + 1
    return (A[begin] == key) ? begin : -1
```

### 5.2 Explanation of Pseudocode

1. **Initialization**:
   - `begin` is set to the start of the array.
   - `end` is set to the end of the array.

2. **Loop Condition**:
   - The loop continues as long as `begin` is less than or equal to `end`.

3. **Mid Calculation**:
   - `mid` is calculated using `begin + (end - begin) / 2` to prevent potential integer overflow.

4. **Comparison and Pointer Update**:
   - If `key` is less or equal to `A[mid]`, the search continues in the left subarray by updating `end`.
   - If `key` is greater than `A[mid]`, the search continues in the right subarray by updating `begin`.

5. **Termination**:
   - Once `begin` is more than or equal to `end`, because of the loop invariant (which states that the key must be in the range of `begin` and `end`), the loop terminates and the value at `begin` is evaluated
   - If the `key` is not found, `-1` is returned to indicate its absence.

---

## 6. Extending Binary Search Beyond Sorted Arrays

Binary search is a versatile algorithm that extends beyond simply searching for elements in sorted arrays. It can be applied to a variety of optimization problems, especially those that exhibit **monotonicity**—where a function consistently increases or decreases as its input grows. This section explores an example that demonstrates how binary search can be leveraged to solve such problems efficiently.

### 6.1 Generalized Binary Search Use-Cases

- **Assume a Complicated Function**:
  ```java
  int complicatedFunction(int s);
  ```
  
- **Properties**:
  - The function is **always increasing**:
    $$
    \text{complicatedFunction}(i) < \text{complicatedFunction}(i + 1)
    $$
  
- **Objective**:
  - **Find the minimum value** $j$ such that:
    $$
    \text{complicatedFunction}(j) > 100
    $$

### 6.2 Example Scenario: Tutorial Allocation

Consider a typical scenario in a university where students need to be allocated to tutorial slots based on their preferences. Each student has indicated their preferred tutorial slot, and the goal is to assign all students to these slots while adhering to certain constraints.

##### **Problem Description**

- **Objective**: 
  - Allocate all students to tutorial slots.
  - **Constraints**:
    - Each tutorial slot can accommodate a maximum of **18 students**.
    - **Minimize** the maximum number of students in any single tutorial slot to ensure balanced allocation.

- **Challenge**:
  - Some tutorial slots may receive more than 18 students based on preferences, leading to overcapacity.
  - The task is to determine the **minimum number of tutorial slots** required to ensure that no slot exceeds the capacity of 18 students.

##### **Motivation for Using Binary Search**

The problem exhibits a **monotonic relationship** between the number of tutorial slots offered and the maximum number of students per slot:

- **Observation**: 
  - As the **number of tutorial slots increases**, the **maximum number of students** in any slot **decreases**.
  - This forms a **monotonic function**, making it a suitable candidate for binary search optimization.

##### **Defining the Monotonic Function**

To apply binary search, we define a function that captures the relationship between the number of tutorial slots and the allocation:

- **Function Definition**:
  $$
  \text{MaxStudents}(x) = \text{Number of students in the most crowded tutorial slot when offering } x \text{ tutorials}
  $$
  
- **Properties**:
  - **Monotonicity**: $\text{MaxStudents}(x)$ **decreases** as $x$ **increases**.
  - **Objective**: Find the **smallest** $x$ such that $\text{MaxStudents}(x) \leq 18$.

##### **Applying Binary Search to the Problem**

Given the monotonic nature of $\text{MaxStudents}(x)$, binary search can efficiently determine the minimum number of tutorial slots required.

- **Approach**:
  1. **Search Space**: Define the range of possible values for $x$ (number of tutorials). 
     - **Lower Bound**: 1 tutorial slot.
     - **Upper Bound**: Total number of students (each student in a separate slot).
  
  2. **Binary Search Procedure**:
     - **Mid Calculation**: Determine the midpoint of the current search space.
     - **Function Evaluation**: Compute $\text{MaxStudents}(\text{mid})$.
     - **Decision Making**:
       - If $\text{MaxStudents}(\text{mid}) \leq 18$, search the **left half** to find a potentially smaller $x$.
       - Otherwise, search the **right half** to increase $x$.
  
  3. **Termination**: The search concludes when the smallest $x$ satisfying the condition is found.

##### **Pseudocode Implementation**

Below is the pseudocode for applying binary search to determine the minimum number of tutorial slots required:

```pseudo
int findMinTutorials(int totalStudents, int maxCapacity)
    begin = 1
    end = totalStudents
    while begin < end do:
        mid = begin + (end - begin) / 2
        if MaxStudents(mid) <= maxCapacity then
            end = mid
        else
            begin = mid + 1
    return begin
```

##### **Explanation of the Pseudocode**

1. **Initialization**:
   - `begin` is set to **1**, representing the minimum number of tutorial slots.
   - `end` is set to **totalStudents**, representing the scenario where each student has their own slot.

2. **Binary Search Loop**:
   - The loop continues until `begin` is no longer less than `end`.
   - `mid` is calculated using `begin + (end - begin) / 2` to prevent integer overflow.
   
3. **Function Evaluation and Decision Making**:
   - If the **maximum number of students** in any tutorial slot with `mid` slots is **less than or equal to** the **maximum capacity** (`maxCapacity`), the algorithm searches the **left half** by setting `end = mid`.
   - Otherwise, it searches the **right half** by setting `begin = mid + 1`.
   
4. **Result**:
   - Once the loop terminates, `begin` holds the **minimum number of tutorial slots** required to ensure that no slot exceeds the capacity of 18 students.

##### **Conclusion**

By recognizing the **monotonic relationship** between the number of tutorial slots and the maximum number of students per slot, binary search can be effectively applied to optimize resource allocation in scenarios beyond simple array searches. This approach not only ensures efficiency but also provides a structured method to solve complex allocation and optimization problems in computer science and real-world applications.

### 6.3 Insights and Best Practices

- **Applicability**:
  - Binary search can be applied to **any monotonic function** (always increasing or always decreasing).
  - It's useful for optimization problems where you're searching for a threshold or boundary condition.
  
- **Advantages**:
  - **Efficiency**: Maintains the $O(\log n)$ time complexity by halving the search space in each iteration.
  - **Simplicity**: The core concept remains straightforward, making it adaptable to various problems.
  
- **Considerations**:
  - **Function Behavior**: Ensure the function is strictly increasing or decreasing to guarantee the correctness of binary search.
  - **Edge Cases**: Handle scenarios where the target condition might not be met within the given range.
  
- **Implementation Tips**:
  - **Define Clear Boundaries**: Clearly define the search space boundaries (`begin` and `end`).
  - **Avoid Overflow**: Use `begin + (end - begin) / 2` instead of `(begin + end) / 2` to prevent integer overflow.
  - **Termination Conditions**: Clearly define when the search should terminate to avoid infinite loops.

---

## 7. Summary

In this lecture, we delved deep into the **Binary Search** algorithm, exploring its foundational principles, iterative refinement, and versatile applications beyond simple array searches. Here's a consolidated overview of the key concepts covered:

### **Key Concepts**

1. **Binary Search Fundamentals**
   - **Reduce-and-Conquer Strategy**: Binary search efficiently narrows down the search space by repeatedly halving the number of elements to consider, achieving a time complexity of $O(\log n)$.
   - **Loop Invariant**: Maintaining the condition $A[\text{begin}] \leq \text{key} \leq A[\text{end}]$ ensures the correctness of each iteration.

2. **Iterative Refinement and Bug Fixing**
   - **Version 1** introduced fundamental bugs such as array out-of-bounds access, potential non-termination, and incorrect return values.
   - **Version 2** addressed some issues but introduced a new bug related to integer overflow.
   - **Final Version** rectified all identified bugs by:
     - Correctly initializing `end` to $n - 1$.
     - Ensuring proper loop termination conditions.
     - Returning the correct index or `-1` appropriately.
     - Preventing integer overflow by adjusting the mid-point calculation.

3. **Terminology in Algorithm Design**
   - **Specification**: Defines the expected output behavior of the algorithm, whether returning the element, its index, or a sentinel value like `-1`.
   - **Preconditions**: Conditions that must hold true before the algorithm begins (e.g., the array must be sorted).
   - **Postconditions**: Conditions that must hold true after the algorithm completes (e.g., the key is found at the returned index).
   - **Invariants**: Relationships between variables that remain true throughout the algorithm's execution, crucial for proving correctness.

4. **Extending Binary Search Beyond Sorted Arrays**
   - **Application Example**: Allocating students to tutorial slots by minimizing the maximum number of students per slot.
   - **Monotonic Functions**: Leveraging the property that certain functions consistently increase or decrease allows binary search to be applied to optimization problems, not just search tasks.
   - **Binary Search on Answer**: A technique where binary search is used to find the optimal value (e.g., minimum number of tutorials) that satisfies a given condition.

5. **Insights and Best Practices**
   - **Validation of Preconditions**: Always ensure that preconditions are met to maintain the algorithm's correctness.
   - **Avoid Common Pitfalls**: Such as integer overflow and improper loop conditions, which can lead to incorrect results or infinite loops.
   - **Testing with Edge Cases**: Essential for verifying the robustness of the algorithm across various scenarios.
   - **Understanding Invariants**: Critical for proving the algorithm's correctness and ensuring each iteration progresses towards the desired outcome.

### **Practical Takeaways**

- **Efficiency and Robustness**: The final implementation of binary search not only achieves optimal time complexity but also ensures robustness against common programming errors.
- **Versatility**: Binary search's applicability extends beyond searching in arrays to solving a wide range of optimization problems, provided the underlying conditions (like monotonicity) are satisfied.
- **Algorithmic Thinking**: Iteratively refining algorithms by identifying and fixing bugs fosters meticulous programming practices and a deeper understanding of algorithm design.

### **Final Thoughts**

Mastering binary search equips you with a powerful tool in the algorithmic toolkit, enabling you to tackle both traditional search problems and more complex optimization challenges with confidence and precision. By adhering to best practices and understanding the underlying principles, you can ensure the development of efficient, correct, and scalable algorithms in various computational contexts.