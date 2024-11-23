### **1. Top-Down Approach**

**Overview:**

- Constructs the heap incrementally by adding one element at a time to an existing heap.
- Uses the **bubble-up** (or sift-up) operation to ensure the heap property is maintained after each insertion.

**Steps:**

1. Start with an empty heap (or a trivially valid heap with one element).
2. Add elements one by one to the heap.
3. Perform a bubble-up operation for the newly added element to maintain the heap property.

**Code Summary:**

- Starts at the second element (index `1`) and considers every subsequent element as a new addition to the heap.
- Compares the new element with its parent and bubbles it up if it violates the heap property.

**Time Complexity:**

- **Per Element:** O(log⁡k), where k is the index of the element being inserted.
- **Overall:** O(nlog⁡n), because each element is processed individually.

**Pros:**

- **Suitable for dynamically growing heaps.**
- Simple and intuitive to implement.

**Cons:**

- Relatively slower for building a static heap from a full array due to the O(nlog⁡n) time complexity.

### **2. Bottom-Up Approach**

**Overview:**

- Constructs the heap from the bottom-most non-leaf nodes upward.
- Uses the **sift-down** (or bubble-down) operation to heapify sub-trees.

**Steps:**

1. Start with the last non-leaf node in the array (`(n-1)/2`).
2. Move backward toward the root.
3. For each node, treat it as the root of a subtree and heapify it by comparing it with its children and swapping as necessary.

**Code Summary:**

- Processes nodes in reverse level order (bottom-up) and ensures the heap property at each subtree.
- Works directly on the array, reducing redundant work.

**Time Complexity:**

- **Per Subtree:** O(h), where h is the height of the subtree.
- **Overall:** O(n), because the number of nodes decreases exponentially with depth.

**Pros:**

- **Much faster than the top-down approach for building a static heap.**
- Minimizes redundant work since lower-level nodes are processed before upper-level nodes.

**Cons:**

- Less intuitive than the top-down approach.

**Note - Heapify approach is the same as the bottom-up construction method. It has the same time complexity, O(n)**

