# Lecture 3a: Algorithm Analysis

# Table of Contents

- [1. Introduction to Algorithm Analysis](#1-introduction-to-algorithm-analysis)
- [2. Big-O Notation (Upper Bound)](#2-big-o-notation-upper-bound)
  - [2.1 Formal Definition](#21-formal-definition)
  - [2.2 Why Big-O?](#22-why-big-o)
  - [2.3 Common Time Complexities](#23-common-time-complexities)
  - [2.4 Other Asymptotic Notations](#24-other-asymptotic-notations)
- [3. Big-Omega Notation (Lower Bound)](#3-big-omega-notation-lower-bound)
  - [3.1 Formal Definition](#31-formal-definition)
  - [3.2 Equivalence Between Big-O and Big-Omega](#32-equivalence-between-big-o-and-big-omega)
- [4. Big-Theta Notation (Tight Bound)](#4-big-theta-notation-tight-bound)
  - [4.1 Formal Definition](#41-formal-definition)
  - [4.2 Visualization](#42-visualization)
  - [4.3 Rules of Thumb](#43-rules-of-thumb)
  - [4.4 Additional Examples](#44-additional-examples)
- [5. Model of Computation](#5-model-of-computation)
  - [5.1 RAM Model](#51-ram-model)
  - [5.2 Counting Operations](#52-counting-operations)
- [6. Cost Analysis Notes](#6-cost-analysis-notes)
  - [Additional Insight on Nested Loops](#additional-insight-on-nested-loops)
  - [Relevant Code Examples](#relevant-code-examples)
  - [Incorporating Recursive Function Calls](#incorporating-recursive-function-calls)
- [7. Putting It All Together](#7-putting-it-all-together)
  - [Steps for Analysis](#steps-for-analysis)
- [8. Summary](#8-summary)

---
## 1. Introduction to Algorithm Analysis
Algorithm analysis is critical in computer science because it allows us to:
- Predict the performance of an algorithm.
- Compare different algorithms solving the same problem.
- Understand how the algorithm scales as input size grows.

We often use **asymptotic notations**—Big-O, Big-Theta, and Big-Omega—to capture the growth rate of an algorithm’s running time or space usage.

---

## 2. Big-O Notation (Upper Bound)

### 2.1 Formal Definition
**Big-O notation** provides an **upper bound** on the growth rate of a function. Formally, if $T(n) $ represents the running time of an algorithm, then

$$
T(n) = O\bigl(f(n)\bigr)\quad \text{if and only if} \quad 
\exists\, c > 0,\ \exists\, n_{0} > 0\ \text{such that}\ 
\forall\, n \ge n_{0},\ T(n) \le c \times f(n).
$$

In simpler terms:
- $T(n)$ grows *no faster* than $f(n)$.
- Beyond some input size $n_{0}$, $T(n)$ stays below (or on) a constant multiple $c$ of $f(n)$.

A common way to visualize this is with a diagram plotting $T(n)$ alongside $c \cdot f(n)$: for $n \ge n_0$, $T(n)$ never exceeds $c \cdot f(n)$.

> **Insight**: Big-O is an *upper bound*, which means it can sometimes over-approximate the growth of $T(n)$. It’s still useful for worst-case comparisons of different algorithms.

### 2.2 Why Big-O?
- It abstracts away **constant factors** and **lower-order terms**.
- It focuses on **dominant terms** as $n \to \infty$.
- It simplifies comparing algorithms (e.g., an $O(n)$ algorithm is generally more scalable than an $O(n^2)$ one for large $n$).

### 2.3 Common Time Complexities
- **Constant Time**: $O(1)$
- **Logarithmic Time**: $O(\log n)$
- **Linear Time**: $O(n)$
- **Linearithmic Time**: $O(n \log n)$
- **Quadratic Time**: $O(n^2)$
- **Cubic Time**: $O(n^3)$
- **Exponential Time**: $O(2^n)$
- **Factorial Time**: $O(n!)$

### 2.4 Other Asymptotic Notations
- **$Big-\Omega$** (Lower Bound)  
  $T(n) = \Omega\bigl(f(n)\bigr)$ if $T(n)$ grows at least as fast as $f(n)$, for sufficiently large $n$.
- **$Big-\Theta$** (Tight Bound)  
  $T(n) = \Theta\bigl(f(n)\bigr)$ if $T(n)$ is both $O\bigl(f(n)\bigr)$ and $\Omega\bigl(f(n)\bigr)$. This indicates $T(n)$ grows *exactly* at the same rate as $f(n)$ (up to constant factors).

---

## 3. Big-Omega Notation (Lower Bound)

### 3.1 Formal Definition
**$Big-\Omega$ notation** provides a **lower bound** on the growth rate of a function. Formally, if $T(n)$ represents the running time of an algorithm, then

$$
T(n) = \Omega\bigl(f(n)\bigr)\quad \text{if and only if} \quad 
\exists\, c > 0,\ \exists\, n_{0} > 0\ \text{such that}\ 
\forall\, n \ge n_{0},\ T(n) \ge c \times f(n).
$$

In simpler terms:
- $T(n)$ grows *no slower* than $f(n)$.
- Beyond some input size $n_{0}$, $T(n)$ is at least some constant multiple $c$ of $f(n)$.

This can be visualized with a diagram: past a certain point $n_0$, the function $T(n)$ is always above (or equal to) $c \cdot f(n)$.

### 3.2 Equivalence Between Big-O and Big-Omega
**Claim**: $f(n) = O(g(n))$ if and only if $g(n) = \Omega(f(n))$.

**Proof**:
1. **Forward Direction**:
   - Assume $f(n) = O(g(n))$.
   - By definition, there exist constants $c > 0$ and $n_0 > 0$ such that for all $n \ge n_0$, $f(n) \le c \cdot g(n)$.
   - This implies $g(n) \ge \frac{1}{c} \cdot f(n)$ for all $n \ge n_0$, satisfying the definition of $g(n) = \Omega(f(n))$.

2. **Backward Direction**:
   - Assume $g(n) = \Omega(f(n))$.
   - By definition, there exist constants $k > 0$ and $m_0 > 0$ such that for all $n \ge m_0$, $g(n) \ge k \cdot f(n)$.
   - This implies $f(n) \le \frac{1}{k} \cdot g(n)$ for all $n \ge m_0$, satisfying the definition of $f(n) = O(g(n))$.

Thus, $f(n) = O(g(n))$ if and only if $g(n) = \Omega(f(n))$.

---

## 4. Big-Theta Notation (Tight Bound)

### 4.1 Formal Definition
**Big-$\Theta$notation** provides a **tight bound** on the growth rate of a function. Formally, if$ T(n) $ represents the running time of an algorithm, then

$$
T(n) = \Theta\bigl(f(n)\bigr)\quad \text{if and only if} \quad 
T(n) = O\bigl(f(n)\bigr) \ \text{and} \ T(n) = \Omega\bigl(f(n)\bigr).
$$

In simpler terms:
- $T(n)$ grows *at the same rate* as $f(n)$, up to constant factors.
- It is both an upper and lower bound, providing a precise asymptotic behavior.

### 4.2 Visualization
Imagine plotting $T(n)$ and $f(n)$ on a graph:
- **Big-O** ensures $T(n)$ does not exceed $c \cdot f(n)$ for $n \ge n_0$.
- **$Big-\Omega$** ensures $T(n)$ is not below $k \cdot f(n)$ for $n \ge m_0$.
- **$Big-\Theta$** means $T(n)$ is sandwiched between $k \cdot f(n)$ and $c \cdot f(n)$ for sufficiently large $n$.

> **Diagram Description**:  
> A simple diagram would show two lines representing $T(n)$ and $f(n)$, with shaded regions indicating the bounds set by $c \cdot f(n)$ and $k \cdot f(n)$. $T(n)$ remains within these bounds for all $n \ge n_0$.

### 4.3 Rules of Thumb

1. **Polynomial Degree Rule**
   - **Rule**: If $T(n)$ is a polynomial of degree $k$, then:

     $T(n) = O(n^k)$
   - **Example**:

     $10n^5 + 50n^3 + 10n + 17 = O(n^5)$

2. **Addition Rule**
   - **Rule**: If $T(n) = O(f(n))$ and $S(n) = O(g(n))$, then:

     $T(n) + S(n) = O(f(n) + g(n))$
   - **Example**:
   - 
     $10n^2 = O(n^2)$
     
     $5n\log(n) = O(n\log(n))$
     
     $10n^2 + 5n\log(n) = O(n^2 + n\log(n)) = O(n^2)$

3. **Multiplication Rule**
   - **Rule**: If $T(n) = O(f(n))$ and $S(n) = O(g(n))$, then:

     $T(n) \times S(n) = O(f(n) \times g(n))$
   - **Example**:

     $10n^2 = O(n^2)$
     
     $5n = O(n)$
     
     $(10n^2)(5n) = 50n^3 = O(n^2 \times n) = O(n^3)$

### 4.4 Additional Examples

1. **Exponential Growth Example**
   - **Statement**:

     $22n + 2n + 2 = O(2^{2n})$
   - **Explanation**: Although the polynomial $22n + 2n + 2$ grows exponentially relative to $2^{2n}$, it is still bounded above by $O(2^{2n})$. However, it's more precise to classify it as $O(n)$ since exponential functions grow much faster.

2. **Logarithmic Factorial Example**
   - **Statement**:

     $\log(n!) = O(n \log n)$
   - **Explanation**: Using Stirling's approximation:

     $n! \approx n^n e^{-n} \sqrt{2\pi n}$
     
     Taking the logarithm:

     $\log(n!) \approx n \log n - n + \frac{1}{2} \log(2\pi n) = O(n \log n)$

---

## 5. Model of Computation

### 5.1 RAM Model
The **Random Access Machine (RAM) model** is a commonly assumed model for algorithm analysis:
1. Each **simple operation** (addition, multiplication, comparison, etc.) takes **constant time** $O(1)$.
2. Data can be accessed in **constant time**, given its address (RAM-like memory).
3. We measure overall time complexity by **counting the number** of these $O(1)$ operations.

**Why use the RAM model?**
- It’s a simplified abstraction that approximates how operations scale on typical modern CPUs.
- It generally works well for analyzing large inputs, even though in reality hardware details can affect actual runtimes.

### 5.2 Counting Operations
An alternative view is to explicitly **count the number of operations** (like comparisons, arithmetic operations, etc.). Each operation is assumed $O(1)$ in the RAM model, so if we have $T(n)$ total operations for input size $n$, the running time in Big-O is derived from that $T(n)$.

> **Insight**:  
> In specialized or advanced contexts (e.g., parallel algorithms, GPU computing, or large matrix operations), different computational models may be necessary (e.g., PRAM), but the RAM model is sufficient for most standard algorithm analysis.

---

## 6. Cost Analysis Notes

- **General Principles**:
  - **Not all costs are constant**.

- **Specific Rules**:
  1. **Loops**:
     - **Cost** = (Number of iterations) × (Maximum cost of one iteration)
     - **Example**:
       ```java
       for (int i = 0; i < n; i++) {
           // O(1) operation
       }
       ```
       - **Cost**: $O(n) \times O(1) = O(n)$

  2. **Nested Loops**:
     - **Cost** = (Number of iterations of Outer Loop) × (Number of iterations of Inner Loop) × (Maximum cost of one iteration)
     - **Example**:
       ```java
       for (int i = 0; i < n; i++) {
           for (int j = 0; j < m; j++) {
               // O(1) operation
           }
       }
       ```
       - **Cost**: $O(n) \times O(m) \times O(1) = O(n \times m)$

  3. **Sequential Statements**:
     - **Cost** = Cost of first statement + Cost of second statement
     - **Example**:
       ```java
       int a = computeA(); // O(n)
       int b = computeB(); // O(m)
       ```
       - **Time Complexity**: $O(n) + O(m) = O(n + m)$

  4. **If/Else Statements**:
     - **Cost** = Maximum(cost of first branch, cost of second branch)
     - Alternatively: ≤ (Cost of first branch) + (Cost of second branch)
     - **Example**:
       ```java
       if (condition) {
           // O(n) operation
       } else {
           // O(m) operation
       }
       ```
       - **Time Complexity**: $O(\max(n, m))$

---

### **Additional Insight on Nested Loops**

- **Impact on Time Complexity**:
  - **Single Loop**: Typically contributes $O(n)$ to the overall time complexity.
  - **Nested Loops**: Multiply their individual complexities, leading to $O(n \times m)$, which can escalate to $O(n^2)$ if both loops run $n$ times.

- **Space Complexity Considerations**:
  - Be mindful that nested loops can also impact space complexity, especially if they involve additional data structures within the inner loop.

- **Optimizing Nested Loops**:
  - Look for opportunities to reduce the number of iterations.
  - Consider whether the inner loop can be transformed into a different algorithmic approach (e.g., using hashing to reduce lookup times).

---

### **Relevant Code Examples**

1. **Single Loop Example**:
   ```java
   for (int i = 0; i < n; i++) {
       // O(1) operation
   }
   ```
   - **Time Complexity**: $O(n)$

2. **Nested Loop Example**:
   ```java
   for (int i = 0; i < n; i++) {
       for (int j = 0; j < m; j++) {
           // O(1) operation
       }
   }
   ```
   - **Time Complexity**: $O(n \times m)$

3. **Sequential Statements Example**:
   ```java
   int a = computeA(); // O(n)
   int b = computeB(); // O(m)
   ```
   - **Time Complexity**: $O(n) + O(m) = O(n + m)$

4. **If/Else Statement Example**:
   ```java
   if (condition) {
       // O(n) operation
   } else {
       // O(m) operation
   }
   ```
   - **Time Complexity**: $O(\max(n, m))$

---

### **Incorporating Recursive Function Calls**

- **Recursive Fibonacci Example**:
  ```java
  public int fibonacci(int n) {
      if (n <= 1) return n;
      return fibonacci(n - 1) + fibonacci(n - 2);
  }
  ```
  - **Running Time Analysis**:
    - Let $T(n)$ be the running time.
    - **Recurrence Relation**:
      $$T(n) = 1 + T(n - 1) + T(n - 2)$$
    - **Solution**:
      $$T(n) = O(2^n)$$

- **Explanation/Insight on Recurrence Relations**:
  - **Recurrence Relations** are equations that define a function in terms of its values on smaller inputs.
  - **Techniques to Solve Recurrences**:
    - **Substitution Method**: Guess the form of the solution and use mathematical induction to prove it.
    - **Recursion Tree Method**: Visualize the recurrence as a tree and sum the costs at each level.
    - **Master Theorem**: Provides a shortcut to solve recurrences of the form $T(n) = aT\left(\frac{n}{b}\right) + f(n)$
  - **Insight**:
    - The Fibonacci example showcases how exponential growth arises from multiple recursive calls.
    - Optimizing recursive algorithms (e.g., using memoization or dynamic programming) can significantly reduce time complexity by avoiding redundant computations.

---

## 7. Putting It All Together

When analyzing an algorithm, consider:
1. **Upper Bounds (Big-O)**: How fast can $T(n)$ grow?  
2. **Lower Bounds ($Big-\Omega$)**: How slow can $T(n)$ grow?  
3. **Tight Bounds ($Big-\Theta$)**: If both bounds match, we have a complete picture of $T(n)$.

### Steps for Analysis
1. **Identify the basic operations.**
2. **Count how many times** those operations occur as a function of $n$ (the input size).
3. **Express this count** in asymptotic notation (commonly Big-O).
4. **(Optionally) Provide $Big-\Theta$ or $Big-\Omega$** to indicate tighter or lower bounds if relevant.

> **Tips**:  
> Typical patterns to look for:
> - **Nested loops** often yield polynomial time ($O(n^2)$, $O(n^3)$, etc.).
> - **Divide-and-conquer** approaches often have complexities like $O(n \log n)$.
> - **Recurrence relations** can be solved using the Master Theorem or expansion methods to find time complexities.

---

## 8. Summary

- **Big-O**: Upper bound on growth.  
  $$T(n) = O\bigl(f(n)\bigr) \iff \exists\, c > 0, n_0 > 0 \text{ s.t. } T(n) \le c\cdot f(n) \text{ for all } n \ge n_0.$$

- **$Big-\Omega$**: Lower bound on growth.  
  $$T(n) = \Omega\bigl(f(n)\bigr) \iff \exists\, c > 0, n_0 > 0 \text{ s.t. } T(n) \ge c\cdot f(n) \text{ for all } n \ge n_0.$$

- **$Big-\Theta$**: Tight bound (both upper and lower).  
  $$T(n) = \Theta\bigl(f(n)\bigr) \iff T(n) = O(f(n)) \text{ and } T(n) = \Omega(f(n)).$$

- We use the **RAM model** for simplicity in counting operations.  
- **Rules of Thumb**:
  - Polynomial Degree: $T(n) = O(n^k)$.
  - Addition: $O(f(n)) + O(g(n)) = O(f(n) + g(n))$.
  - Multiplication: $O(f(n)) \times O(g(n)) = O(f(n)g(n))$.

- **Examples**:
  - $10n^5 + 50n^3 + 10n + 17 = O(n^5)$.
  - $10n^2 + 5n\log(n) = O(n^2)$.
  - $(10n^2)(5n) = O(n^3)$.
  - $22n + 2n + 2 = O(2^{2n})$ (more precise as $O(n)$ ).
  - $\log(n!) = O(n \log n)$ (using Stirling's approximation).

Understanding these definitions and notations lays the foundation for analyzing algorithmic efficiency throughout the course.
