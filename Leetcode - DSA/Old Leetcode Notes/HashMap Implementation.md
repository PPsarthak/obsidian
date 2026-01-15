#hash-map #dsa #leetcode 

**Buckets**:
- A HashMap initializes with a default of 16 buckets (indexed from 0 to 15) in heap memory. Each bucket can hold a singly linked list of entries (nodes).
![[HashMap Internal Implementation.png|550]]
**Load Factor and Rehashing**:
- The **load factor** determines how full the HashMap can become before it increases its capacity and rehashes its contents. 
- The <span style="color:rgb(102, 207, 255)">default load factor is 0.75</span>, meaning the HashMap will resize when it exceeds 75% of its capacity.
- The threshold for resizing is calculated as the product of the current capacity and the load factor. <span style="color:rgb(102, 207, 255)">When the number of entries exceeds this threshold, the HashMap typically doubles its capacity and rehashes all entries.</span>
- Rehashing involves recalculating the hash codes of existing entries, which can lead to a change in the order of elements in the HashMap.

 **Hashing**:
- When adding a key-value pair using the `put` method, the HashMap computes the hash code of the key using the `hashCode()` method. This hash code determines the index of the bucket where the entry will be stored.
- The `hashCode()` method of the Object class returns an integer representing the memory reference of the object. This can be overridden to provide a custom implementation.

**Collision**: 
- This occurs when two keys hash to the same bucket. Since a HashMap cannot store two entries with the same key, a collision resolution strategy is needed.
- When a collision occurs, the `equals()` method is used to check if the key already exists in the bucket. If it does, the value is updated; if not, the new entry is added.
- HashMap do not allow duplicate keys, ensuring that each key is associated with only one value for efficient retrieval.

**Collision Resolution Strategies**:
    - **Chaining**: Each bucket contains a linked list (or another collection) of entries that hash to the same index.
    - **Open Addressing**: All entries are stored in the array itself. When a collision occurs, the algorithm searches for the next available slot using a probing sequence.
    
> The order of elements in a HashMap is not guaranteed to be consistent across different iterations. This is due to the internal implementation which does not preserve insertion order. The storage order may change due to resizing or rehashing.

### Changes in Java 8 Implementation

- Prior to Java 8, collisions were handled using linked lists, resulting in slower retrieval times (O(n)) as the number of collisions increased.
- In Java 8, when the number of nodes in a single bucket exceeds a threshold (known as the **Treeify Threshold**), the internal structure of that bucket is converted from a linked list to a balanced tree (TreeNode).
- This change reduces the time complexity for retrieval from O(n) to O(log(n)) for buckets with many collisions, improving performance.
- If the number of nodes in a bucket decreases below a certain threshold, the tree is converted back to a linked list to optimize memory usage.