#### **Greedy Algorithms**

**Greedy algorithms** are a class of algorithms that make the **locally optimal choice** at each step with the intention of finding a **global optimum**. They are characterized by their simplicity and efficiency, often providing optimal solutions for specific types of problems where the greedy choice property holds.

##### **Key Characteristics**
- **Local Optimization**: At every step, the algorithm selects the best possible option available without considering the broader context.
- **No Backtracking**: Once a choice is made, it is never reconsidered, which makes greedy algorithms straightforward and fast.
- **Optimal for Specific Problems**: Greedy algorithms do not always yield the optimal solution for all problems, but they work exceptionally well for problems that satisfy the greedy choice property and exhibit optimal substructure.

##### **Example: Activity Selection Problem**

**Problem Statement**:
Given a set of activities, each with a start time and a finish time, select the maximum number of non-overlapping activities that can be performed by a single person.

**Greedy Choice**:
Always select the activity that finishes earliest. This choice leaves the maximum remaining time for other activities.

**Pseudocode**:
```pseudo
function ActivitySelection(activities):
    sort activities by finish time
    selected = []
    last_finish_time = -infinity

    for activity in activities:
        if activity.start >= last_finish_time:
            selected.append(activity)
            last_finish_time = activity.finish

    return selected
```

**Explanation**:
1. **Sort** the activities based on their finish times in ascending order.
2. **Initialize** an empty list `selected` to store the chosen activities and set `last_finish_time` to negative infinity.
3. **Iterate** through the sorted activities:
   - **Select** an activity if its start time is greater than or equal to `last_finish_time`.
   - **Update** `last_finish_time` to the finish time of the selected activity.
4. **Return** the list of selected activities, which represents the maximum number of non-overlapping activities.

**Insight**:
The greedy approach works here because selecting the activity that finishes earliest maximizes the remaining time for subsequent activities, ensuring that as many activities as possible can be accommodated without overlaps.

##### **When to Use Greedy Algorithms**
- **Optimal Substructure**: The problem can be broken down into smaller subproblems which can be solved optimally and combined to form an optimal solution for the original problem.
- **Greedy Choice Property**: A global optimum can be reached by making a locally optimal choice at each step.
  
**Common Problems Solved by Greedy Algorithms**:
- **Huffman Coding**: For data compression by building an optimal prefix-free binary tree.
- **Dijkstra's Algorithm**: For finding the shortest path in a graph.
- **Prim's and Kruskal's Algorithms**: For finding the minimum spanning tree in a graph.
- **Coin Change Problem**: When the coin denominations allow a greedy approach to find the minimum number of coins.

**Caution**:
While greedy algorithms are powerful and efficient, they are not universally applicable. It's crucial to verify that the problem satisfies the greedy choice property and has an optimal substructure before applying a greedy strategy.

---
```

---

### **Notes on Greedy Algorithms**

1. **Understanding the Greedy Choice Property**:
   - Ensure that making a local optimal choice leads to a global optimum.
   - Verify through problem analysis or proofs that the greedy approach is valid.

2. **Optimal Substructure**:
   - Confirm that optimal solutions to subproblems contribute to the optimal solution of the overall problem.

3. **Examples of Greedy Failures**:
   - **Fractional Knapsack Problem**: Greedy algorithms work if fractions of items can be taken, but not for the 0/1 Knapsack Problem where items must be taken whole.
   - **Graph Coloring**: Greedy algorithms may not always provide the optimal number of colors.

4. **Combining Greedy with Other Techniques**:
   - In some cases, greedy algorithms can be part of a larger algorithmic strategy, such as dynamic programming or divide-and-conquer.

---

Feel free to integrate this section into your **Lecture 3b: Binary Search** notes where it best fits, such as under an "Additional Concepts" or "Algorithmic Strategies" subsection. This will provide a broader perspective on algorithm design techniques beyond binary search.