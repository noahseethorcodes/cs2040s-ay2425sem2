# Applying Lecture Concepts to the Problem Set

## **1. Problem: Finding the Maximum in a Unimodal Array**
**Lecture Connection:** [Peak Finding](#)

### **Concepts Applied**
- **Local vs. Global Maximums:** 
  - The problem involves an array that increases and then decreases. A **global maximum** exists and can be found efficiently.
- **Reduce-and-Conquer Strategy:**
  - Instead of a linear scan (O(n)), we use **binary search** to find the peak in **O(log n)** time.
- **Correctness via Invariants:**
  - The global maximum is always in the direction of the higher neighboring element.

### **Solution Approach**
- Use a **binary search-inspired peak-finding algorithm**:
  - If `A[mid] > A[mid+1]`, search **left**.
  - Otherwise, search **right**.
- Time Complexity: **O(log n)**.

---

## **2. Problem: Finding the Maximum in a More Complex Unimodal Array**
**Lecture Connection:** [Peak Finding - Generalizing to Multiple Peaks](#)

### **Concepts Applied**
- **Handling Multiple Local Maximums:**
  - Unlike simple unimodal arrays, the problem allows both an **increasing-decreasing** pattern and a **decreasing-increasing** pattern.
- **Greedy Choice Property:**
  - The true **global maximum** must exist in one of the peak regions.

### **Solution Approach**
- **Recursive Peak Finding with Directional Choice**:
  - Check `A[mid]` vs. `A[mid-1]` and `A[mid+1]` to determine the search region.
  - Handle **two peak cases** by recursively searching both directions if necessary.
- Time Complexity: **O(log n)**.

---

## **3. Problem: Load Balancing with Processors**
**Lecture Connection:** [Binary Search for Optimization Problems](#)

### **Concepts Applied**
- **Binary Search on Answer (Monotonic Function Property):**
  - The problem asks for the **minimum possible maximum load** across all processors.
  - The function **isFeasibleLoad(queryLoad, p)** is **monotonic**:
    - If `queryLoad` is **too low**, it’s **impossible** to distribute jobs within `p` processors.
    - If `queryLoad` is **high enough**, a valid assignment **exists**.

### **Solution Approach**
1. **Binary search on the load range**:
   - **Lower Bound** = `max(jobSizes)`, since no processor can have a load smaller than the largest job.
   - **Upper Bound** = `sum(jobSizes)`, where one processor handles all jobs.
2. **Use `isFeasibleLoad(queryLoad, p)` to guide binary search**:
   - If feasible, search **left** (try lower loads).
   - If not feasible, search **right** (increase the allowed max load).
3. **Time Complexity**: **O(n log (nM))**.

---

## **4. Problem: Approximation Algorithms for Load Balancing**
**Lecture Connection:** [Greedy Approximation and Lower Bound Proofs](#)

### **Concepts Applied**
- **Lower Bound Proof for the Optimal Solution:**
  - Any valid assignment **must** have a max load of at least:
    - **max(jobSizes)** (biggest single job).
    - **sum(jobSizes) / p** (average distribution).
- **Greedy Contiguous Chunking (2-Approximation):**
  - Any greedy assignment must satisfy `maxLoad ≤ 2 × OPT`.

### **Solution Approach**
- **Greedy Chunking Algorithm**:
  - Iterate through the array, assigning jobs **contiguously** until reaching an estimated "average" load.
  - Start a **new processor** when adding another job **exceeds the estimated target**.
  - Guarantees a solution within **at most 2× the optimal max load**.
- **Time Complexity**: **O(n)**.

---

## **5. Problem: Feasibility Check for Given Load**
**Lecture Connection:** [Binary Search Invariants and Loop Termination](#)

### **Concepts Applied**
- **Invariant for Feasibility Testing**:
  - While distributing jobs, **no processor should exceed `queryLoad`**.
  - Each new processor starts **only when required** (i.e., when the current processor's load exceeds `queryLoad`).
- **Greedy Strategy to Partition Jobs**:
  - Iterate through the list **once** and allocate jobs contiguously to the current processor.
  - If a job **cannot fit**, start a new processor.
  - If processors exceed `p`, return **false** (not feasible).

### **Solution Approach**
1. **Single-Pass Feasibility Check (`O(n)`)**:
   - Traverse the list once and simulate job allocation to `p` processors.
   - If more than `p` processors are required, return **false**.
2. **Use in Binary Search (`O(n log (nM))`)**:
   - Wrap the feasibility check inside a **binary search on the possible max load**.
- **Overall Complexity**: **O(n log (nM))**.

---

## **6. Problem: Handling Non-Unique Jobs in Peak Finding**
**Lecture Connection:** [Handling Plateaus in Peak Finding](#)

### **Concepts Applied**
- **Challenges of Duplicate Elements:**
  - If a region consists of **all the same value**, a **binary search approach may degrade to linear time**.
- **Worst-Case Scenario for Peak Finding:**
  - Consider an array where all values are equal except for **one peak**.
  - A naive binary search might need to scan **linearly** through duplicate values.

### **Solution Approach**
- **Modify Peak Finding Algorithm:**
  - If `A[mid] == A[mid+1]`, **search both sides**, leading to **O(n) worst case**.
  - Optimize by **jumping across plateau regions** whenever possible.
- **Time Complexity**:
  - **Best Case:** **O(log n)** (binary search works normally).
  - **Worst Case:** **O(n)** (entire array must be scanned).

---

## **Summary of Key Takeaways**
| **Concept**         | **Applied In**                                      | **Time Complexity** |
|---------------------|-----------------------------------------------------|----------------------|
| **Binary Search**  | Load balancing, peak finding                        | O(log n)            |
| **Reduce-and-Conquer** | Unimodal array max, peak finding                    | O(log n)            |
| **Binary Search on Answer** | Minimizing max load, tutorial allocation         | O(n log (nM))       |
| **Greedy Approximation** | Load balancing (2-approximation)                 | O(n)                |
| **Plateaus in Peak Finding** | Handling duplicate values in peak search       | O(n) (worst case)   |

These problems illustrate how **binary search**, **greedy algorithms**, and **approximation techniques** work together to solve **real-world optimization challenges efficiently**.
