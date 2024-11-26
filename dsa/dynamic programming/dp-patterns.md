### **Recognizing and Formulating Dynamic Programming Problems**

Now that you understand how to implement Dynamic Programming (DP) using Memoization and Tabulation, the next step is learning how to recognize when a problem can be solved using DP and how to systematically formulate a DP solution.

---

### **How to Recognize DP Problems**

Dynamic Programming is applicable when a problem exhibits the following characteristics:

1. **Optimal Substructure**
    - The solution to a larger problem can be constructed from the solutions to smaller subproblems.
    - Example: The Fibonacci sequence is built using smaller Fibonacci numbers \( F(n) = F(n-1) + F(n-2) \).

2. **Overlapping Subproblems**
    - The same subproblems are solved multiple times in a naive approach.
    - Example: In a naive recursive Fibonacci implementation, \( F(4) \) is computed multiple times while solving \( F(6) \).

---

### **Steps to Formulate a DP Solution**

1. **Define the Problem State**
    - Identify the variables that represent the problem's state at any given point.
    - Example: In the **Knapsack Problem**, the state could be represented as \( dp[i][w] \), where \( i \) is the index of the current item, and \( w \) is the remaining capacity of the knapsack.

2. **Define the Recurrence Relation**
    - Establish how the solution to a problem depends on the solutions to its subproblems.
    - Example: In the **Knapsack Problem**, the recurrence relation is:  
      \[
      dp[i][w] = \max(dp[i-1][w], dp[i-1][w-\text{weight}[i]] + \text{value}[i])
      \]

3. **Identify Base Cases**
    - Determine the simplest scenarios where the solution is known without computation.
    - Example: In the **Knapsack Problem**, \( dp[0][w] = 0 \), because no items can fit into the knapsack.

4. **Iterate (Tabulation) or Recurse (Memoization)**
    - Use the recurrence relation to compute the values of subproblems either iteratively or recursively, depending on the approach.

5. **Extract the Final Answer**
    - The final answer will typically be stored in a specific entry in the table or memoization structure.
    - Example: In the **Knapsack Problem**, the solution is \( dp[n][W] \), where \( n \) is the total number of items, and \( W \) is the knapsack capacity.

---

### **Example Problem: 0/1 Knapsack Problem**

#### Problem Statement:
Given \( n \) items, each with a weight and value, determine the maximum value you can obtain by selecting a subset of the items, such that their total weight does not exceed \( W \) (the capacity of the knapsack). Each item can be included **at most once**.

---

#### **Formulating the DP Solution**

1. **State Representation**  
   Let \( dp[i][w] \) represent the maximum value obtainable using the first \( i \) items with a knapsack capacity \( w \).

2. **Recurrence Relation**
    - If we don't include the \( i \)-th item: \( dp[i][w] = dp[i-1][w] \)
    - If we include the \( i \)-th item (if its weight allows):  
      \( dp[i][w] = dp[i-1][w-\text{weight}[i]] + \text{value}[i] \)
    - Combine the two cases:  
      \[
      dp[i][w] = \max(dp[i-1][w], dp[i-1][w-\text{weight}[i]] + \text{value}[i])
      \]

3. **Base Cases**
    - \( dp[0][w] = 0 \): No items means no value, regardless of \( w \).
    - \( dp[i][0] = 0 \): A knapsack with capacity 0 cannot hold any items.

4. **Final Answer**  
   The solution is stored in \( dp[n][W] \), where \( n \) is the total number of items and \( W \) is the knapsack capacity.

---

#### **Implementation**

**Python Code:**
```python
def knapsack(weights, values, W):
    n = len(weights)
    dp = [[0] * (W + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        for w in range(W + 1):
            if weights[i - 1] <= w:
                dp[i][w] = max(dp[i - 1][w], dp[i - 1][w - weights[i - 1]] + values[i - 1])
            else:
                dp[i][w] = dp[i - 1][w]

    return dp[n][W]

# Example usage
weights = [1, 2, 3]
values = [6, 10, 12]
W = 5
print(knapsack(weights, values, W))  # Output: 22
```

**Java Code:**
```java
public class Knapsack {
    public static int knapsack(int[] weights, int[] values, int W) {
        int n = weights.length;
        int[][] dp = new int[n + 1][W + 1];

        for (int i = 1; i <= n; i++) {
            for (int w = 0; w <= W; w++) {
                if (weights[i - 1] <= w) {
                    dp[i][w] = Math.max(dp[i - 1][w], dp[i - 1][w - weights[i - 1]] + values[i - 1]);
                } else {
                    dp[i][w] = dp[i - 1][w];
                }
            }
        }

        return dp[n][W];
    }

    public static void main(String[] args) {
        int[] weights = {1, 2, 3};
        int[] values = {6, 10, 12};
        int W = 5;
        System.out.println(knapsack(weights, values, W)); // Output: 22
    }
}
```

---

### **Optimized Space Complexity**

Since the current state in the 0/1 Knapsack problem only depends on the previous state, we can reduce space complexity to \( O(W) \) by using a 1D array.

**Python (Space-Optimized):**
```python
def knapsack_optimized(weights, values, W):
    n = len(weights)
    dp = [0] * (W + 1)

    for i in range(n):
        for w in range(W, weights[i] - 1, -1):  # Traverse backwards
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])

    return dp[W]

# Example usage
weights = [1, 2, 3]
values = [6, 10, 12]
W = 5
print(knapsack_optimized(weights, values, W))  # Output: 22
```

**Java (Space-Optimized):**
```java
public class KnapsackOptimized {
    public static int knapsack(int[] weights, int[] values, int W) {
        int n = weights.length;
        int[] dp = new int[W + 1];

        for (int i = 0; i < n; i++) {
            for (int w = W; w >= weights[i]; w--) { // Traverse backwards
                dp[w] = Math.max(dp[w], dp[w - weights[i]] + values[i]);
            }
        }

        return dp[W];
    }

    public static void main(String[] args) {
        int[] weights = {1, 2, 3};
        int[] values = {6, 10, 12};
        int W = 5;
        System.out.println(knapsack(weights, values, W)); // Output: 22
    }
}
```

---

### **Advanced Dynamic Programming Patterns**

Once you’re comfortable with the basics of DP, it’s time to explore more complex and widely applicable patterns. These include multi-dimensional DP problems, problems with sequences, and problems involving multiple decisions at each step. Here are some advanced DP topics:

---

### **1. Longest Common Subsequence (LCS)**

#### **Problem Statement**
Given two strings, find the length of their longest subsequence that appears in both strings. A subsequence is a sequence that appears in the same order but not necessarily consecutively.

#### **Formulation**

1. **State Representation**  
   Let \( dp[i][j] \) represent the length of the LCS of the first \( i \) characters of string \( A \) and the first \( j \) characters of string \( B \).

2. **Recurrence Relation**
    - If the characters match:  
      \[
      dp[i][j] = dp[i-1][j-1] + 1
      \]
    - If the characters don’t match:  
      \[
      dp[i][j] = \max(dp[i-1][j], dp[i][j-1])
      \]

3. **Base Cases**
    - \( dp[0][j] = 0 \): LCS of an empty string and any other string is 0.
    - \( dp[i][0] = 0 \): LCS of any string and an empty string is 0.

4. **Final Answer**  
   \( dp[m][n] \), where \( m \) and \( n \) are the lengths of the strings.

---

#### **Implementation**

**Python Code:**
```python
def longest_common_subsequence(A, B):
    m, n = len(A), len(B)
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if A[i - 1] == B[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

    return dp[m][n]

# Example usage
print(longest_common_subsequence("ABCBDAB", "BDCAB"))  # Output: 4
```

**Java Code:**
```java
public class LCS {
    public static int longestCommonSubsequence(String A, String B) {
        int m = A.length(), n = B.length();
        int[][] dp = new int[m + 1][n + 1];

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (A.charAt(i - 1) == B.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[m][n];
    }

    public static void main(String[] args) {
        System.out.println(longestCommonSubsequence("ABCBDAB", "BDCAB")); // Output: 4
    }
}
```

---

### **2. Matrix Chain Multiplication**

#### **Problem Statement**
Given dimensions of matrices, determine the minimum number of scalar multiplications required to multiply them in an optimal order.

#### **Formulation**

1. **State Representation**  
   Let \( dp[i][j] \) represent the minimum cost to multiply matrices from \( i \) to \( j \).

2. **Recurrence Relation**
    - To multiply matrices from \( i \) to \( j \), try splitting at \( k \):  
      \[
      dp[i][j] = \min_{i \leq k < j}(dp[i][k] + dp[k+1][j] + \text{cost of multiplication at } k)
      \]
    - Cost of multiplication at \( k \):  
      \[
      \text{cost} = \text{dimensions}[i-1] \times \text{dimensions}[k] \times \text{dimensions}[j]
      \]

3. **Base Cases**  
   \( dp[i][i] = 0 \): A single matrix requires no multiplication.

4. **Final Answer**  
   \( dp[1][n-1] \), where \( n \) is the number of matrices.

---

#### **Implementation**

**Python Code:**
```python
def matrix_chain_multiplication(dimensions):
    n = len(dimensions)
    dp = [[0] * n for _ in range(n)]

    for length in range(2, n):
        for i in range(1, n - length + 1):
            j = i + length - 1
            dp[i][j] = float('inf')
            for k in range(i, j):
                cost = dp[i][k] + dp[k + 1][j] + dimensions[i - 1] * dimensions[k] * dimensions[j]
                dp[i][j] = min(dp[i][j], cost)

    return dp[1][n - 1]

# Example usage
dimensions = [10, 20, 30, 40, 30]
print(matrix_chain_multiplication(dimensions))  # Output: 30000
```

**Java Code:**
```java
public class MatrixChainMultiplication {
    public static int matrixChainMultiplication(int[] dimensions) {
        int n = dimensions.length;
        int[][] dp = new int[n][n];

        for (int length = 2; length < n; length++) {
            for (int i = 1; i < n - length + 1; i++) {
                int j = i + length - 1;
                dp[i][j] = Integer.MAX_VALUE;
                for (int k = i; k < j; k++) {
                    int cost = dp[i][k] + dp[k + 1][j] + dimensions[i - 1] * dimensions[k] * dimensions[j];
                    dp[i][j] = Math.min(dp[i][j], cost);
                }
            }
        }

        return dp[1][n - 1];
    }

    public static void main(String[] args) {
        int[] dimensions = {10, 20, 30, 40, 30};
        System.out.println(matrixChainMultiplication(dimensions)); // Output: 30000
    }
}
```

---

### **3. Subset Sum Problem**

#### **Problem Statement**
Given a set of integers and a target sum, determine if there is a subset whose sum equals the target.

#### **Formulation**

1. **State Representation**  
   Let \( dp[i][j] \) represent whether it’s possible to achieve sum \( j \) using the first \( i \) elements.

2. **Recurrence Relation**
    - If the current element is not included:  
      \( dp[i][j] = dp[i-1][j] \)
    - If the current element is included:  
      \( dp[i][j] = dp[i-1][j-\text{nums}[i-1]] \)

3. **Base Cases**
    - \( dp[0][0] = \text{True} \): A sum of 0 is always possible with no elements.
    - \( dp[i][0] = \text{True} \): Sum 0 is possible with any set of elements.

4. **Final Answer**  
   \( dp[n][\text{target}] \), where \( n \) is the size of the set.

---

#### **Implementation**

**Python Code:**
```python
def subset_sum(nums, target):
    n = len(nums)
    dp = [[False] * (target + 1) for _ in range(n + 1)]
    for i in range(n + 1):
        dp[i][0] = True

    for i in range(1, n + 1):
        for j in range(1, target + 1):
            if nums[i - 1] > j:
                dp[i][j] = dp[i - 1][j]
            else:
                dp[i][j] = dp[i - 1][j] or dp[i - 1][j - nums[i - 1]]

    return dp[n][target]

# Example usage
nums = [3, 34, 4, 12, 5, 2]
target = 9
print(subset_sum(nums, target))  # Output: True
```

**Java Code:**
```java
public class SubsetSum {
    public static boolean subsetSum(int[] nums, int target) {
        int n = nums.length;
        boolean[][] dp = new boolean[n + 1][target + 1];
        for (int i = 0; i <= n; i++) {
            dp[i][0] = true;
        }

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= target; j++) {
                if (nums[i - 1] > j) {
                    dp[i][j] = dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j

] || dp[i - 1][j - nums[i - 1]];
                }
            }
        }

        return dp[n][target];
    }

    public static void main(String[] args) {
        int[] nums = {3, 34, 4, 12, 5, 2};
        int target = 9;
        System.out.println(subsetSum(nums, target)); // Output: true
    }
}
```

---

### **Specialized Optimizations in Dynamic Programming**

As we advance, some problems require highly optimized approaches to manage constraints such as memory usage, time complexity, or complex problem structures like graphs and combinatorics. This section explores **Bitmask DP**, **Digit DP**, and **Graph-based DP** techniques.

---

### **1. Bitmask DP**

#### **Problem Statement**
You are given \( n \) tasks, each requiring one unique worker from \( n \) workers. Each worker has a specific cost for performing a task. Find the minimum cost to assign all tasks such that every task is assigned to a unique worker.

---

#### **Formulation**

1. **State Representation**  
   Use a bitmask to represent which tasks have been assigned. If \( dp[mask] \) is the minimum cost to complete tasks represented by the bitmask \( mask \), the state depends on the tasks remaining.

2. **Recurrence Relation**  
   For each worker \( i \), iterate through all tasks \( j \):
    - If task \( j \) is not assigned in \( mask \), calculate the cost by assigning \( j \) to \( i \):  
      \[
      dp[mask] = \min(dp[mask], dp[mask \, | \, (1 << j)] + \text{cost}[i][j])
      \]

3. **Base Case**  
   \( dp[0] = 0 \): No cost if no tasks are assigned.

4. **Final Answer**  
   \( dp[(1 << n) - 1] \): The bitmask where all \( n \) tasks are assigned.

---

#### **Implementation**

**Python Code:**
```python
def min_cost_assignment(cost):
    n = len(cost)
    dp = [float('inf')] * (1 << n)
    dp[0] = 0

    for mask in range(1 << n):
        num_assigned = bin(mask).count('1')
        for task in range(n):
            if not (mask & (1 << task)):  # If task is not assigned
                dp[mask | (1 << task)] = min(dp[mask | (1 << task)],
                                             dp[mask] + cost[num_assigned][task])

    return dp[(1 << n) - 1]

# Example usage
cost = [
    [9, 2, 7],
    [6, 4, 3],
    [5, 8, 1]
]
print(min_cost_assignment(cost))  # Output: 13
```

**Java Code:**
```java
public class BitmaskDP {
    public static int minCostAssignment(int[][] cost) {
        int n = cost.length;
        int[] dp = new int[1 << n];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;

        for (int mask = 0; mask < (1 << n); mask++) {
            int numAssigned = Integer.bitCount(mask);
            for (int task = 0; task < n; task++) {
                if ((mask & (1 << task)) == 0) { // If task is not assigned
                    dp[mask | (1 << task)] = Math.min(dp[mask | (1 << task)],
                                                      dp[mask] + cost[numAssigned][task]);
                }
            }
        }

        return dp[(1 << n) - 1];
    }

    public static void main(String[] args) {
        int[][] cost = {
            {9, 2, 7},
            {6, 4, 3},
            {5, 8, 1}
        };
        System.out.println(minCostAssignment(cost)); // Output: 13
    }
}
```

---

### **2. Digit DP**

#### **Problem Statement**
Count numbers between \( L \) and \( R \) that satisfy a specific property (e.g., digits sum up to a given value, no repeating digits, divisible by \( K \)).

---

#### **Formulation**

1. **State Representation**  
   \( dp[pos][tight][state] \): The number of valid numbers formed considering up to the \( pos \)-th digit, whether the number is still "tight" (matches the prefix of \( R \)), and the state variable (e.g., digit sum or used digits).

2. **Recurrence Relation**
    - For each digit \( d \):
        - If \( tight = 1 \), \( d \) is constrained by the current digit of \( R \).
        - Update the state and recurse for the next digit.

3. **Base Case**  
   If \( pos == \text{length of the number} \), return whether the state satisfies the condition.

4. **Final Answer**  
   Subtract results from \( R \) and \( L-1 \).

---

#### **Implementation**

**Python Code:**
```python
def digit_dp(pos, tight, sum_, digits, dp):
    if pos == len(digits):
        return 1 if sum_ % 2 == 0 else 0  # Example condition: sum of digits is even
    
    if dp[pos][tight][sum_] != -1:
        return dp[pos][tight][sum_]
    
    limit = digits[pos] if tight else 9
    result = 0
    for digit in range(0, limit + 1):
        result += digit_dp(pos + 1, tight and (digit == limit), sum_ + digit, digits, dp)
    
    dp[pos][tight][sum_] = result
    return result

def count_numbers(L, R):
    def preprocess(num):
        return list(map(int, str(num)))
    
    def solve(num):
        digits = preprocess(num)
        dp = [[[-1] * 100 for _ in range(2)] for _ in range(len(digits))]
        return digit_dp(0, 1, 0, digits, dp)
    
    return solve(R) - solve(L - 1)

# Example usage
print(count_numbers(10, 20))  # Output: Numbers between 10 and 20 with even digit sums.
```
**Java Code:**

```public class MinCostAssignment {
    public static int minCostAssignment(int[][] cost) {
        int n = cost.length;
        int[] dp = new int[1 << n];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;

        for (int mask = 0; mask < (1 << n); mask++) {
            int numAssigned = Integer.bitCount(mask);
            for (int task = 0; task < n; task++) {
                if ((mask & (1 << task)) == 0) { // If task is not assigned
                    dp[mask | (1 << task)] = Math.min(dp[mask | (1 << task)],
                                                      dp[mask] + cost[numAssigned][task]);
                }
            }
        }

        return dp[(1 << n) - 1];
    }

    public static void main(String[] args) {
        int[][] cost = {
            {9, 2, 7},
            {6, 4, 3},
            {5, 8, 1}
        };
        System.out.println(minCostAssignment(cost)); // Output: 13
    }
}
```
---

### **3. Graph-based DP**

#### **Problem Statement**
Find the shortest path from a source to all vertices in a graph with weighted edges.

---

#### **Formulation**

1. **State Representation**  
   \( dp[u][k] \): The shortest path to vertex \( u \) using at most \( k \) edges.

2. **Recurrence Relation**  
   \[
   dp[u][k] = \min(dp[u][k-1], \min_{(v, w) \in \text{edges}} (dp[v][k-1] + w))
   \]

3. **Base Case**  
   \( dp[source][0] = 0 \), \( dp[u][0] = \infty \) for all \( u \neq \text{source} \).

4. **Final Answer**  
   \( \min_k dp[u][k] \) for the destination vertex.

---

#### **Implementation**

**Python Code (Bellman-Ford):**
```python
def bellman_ford(graph, source, V):
    dp = [float('inf')] * V
    dp[source] = 0

    for _ in range(V - 1):
        new_dp = dp[:]
        for u, v, w in graph:
            if dp[u] + w < new_dp[v]:
                new_dp[v] = dp[u] + w
        dp = new_dp

    return dp

# Example usage
graph = [
    (0, 1, 4),
    (0, 2, 2),
    (1, 2, 5),
    (1, 3, 10),
    (2, 3, 3)
]
V = 4
source = 0
print(bellman_ford(graph, source, V))  # Output: Shortest distances from source.
```
**Java Code:**
```java
import java.util.Arrays;

public class BellmanFord {
    public static int[] bellmanFord(int[][] edges, int V, int source) {
        int[] dp = new int[V];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[source] = 0;

        // Relax all edges up to V-1 times
        for (int i = 0; i < V - 1; i++) {
            int[] newDp = dp.clone();
            for (int[] edge : edges) {
                int u = edge[0], v = edge[1], weight = edge[2];
                if (dp[u] != Integer.MAX_VALUE && dp[u] + weight < newDp[v]) {
                    newDp[v] = dp[u] + weight;
                }
            }
            dp = newDp;
        }

        return dp;
    }

    public static void main(String[] args) {
        int[][] edges = {
            {0, 1, 4},
            {0, 2, 2},
            {1, 2, 5},
            {1, 3, 10},
            {2, 3, 3}
        };
        int V = 4, source = 0;

        int[] shortestDistances = bellmanFord(edges, V, source);
        System.out.println(Arrays.toString(shortestDistances)); // Output: [0, 4, 2, 5]
    }
}
```
---


