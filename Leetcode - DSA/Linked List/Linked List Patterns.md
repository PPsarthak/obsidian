#leetcode #linked-list 

###### Basic Pattern
```
Techniques to solve these problems include using dummy head/tail nodes to simplify insertion/deletion logic, employing slow and fast pointers (Floyd’s cycle detection) for loop-related tasks

1. Dummy Node Technique
	- Use when: Inserting/deleting at head, merging lists.
	- Idea: Create a fake head → do ops → return dummy.next.
	- Benefit: No separate handling for head.
2. Fast & Slow Pointers (Floyd’s Algorithm)
	- Use when: Cycle detection, middle node
	- Idea: Fast moves 2 steps, Slow moves 1 step
	- Benefit: Quick traversing
3. Prev–Curr–Next Pointer Manipulation
	- Use when: Reversing/deleting/inserting nodes, checking front and back
	- Idea: Move them together and use them when you want to acess prev & next nodes
```
---
#### 1. Addition / Arithmetic on Linked Lists
- 2. Add Two Numbers – Add two numbers represented by linked lists, digits stored in reverse order.
#### 2. Node Deletion
- 19. Remove Nth Node From End of List – Delete the nth node from the end of a linked list in one pass using two pointers. 
- 82. Remove Duplicates from Sorted List II – Remove all nodes with duplicate values from a sorted list, keeping only distinct values.
- 83. Remove Duplicates from Sorted List – Remove extra nodes with the same value from a sorted list, leaving one occurrence.
- 84. Remove Linked List Elements – Delete all nodes with a given value from the linked list.
- 85. Delete Node in a Linked List – Delete a given node without access to the head pointer by overwriting its value and adjusting links.
- 86. Remove Zero Sum Consecutive Nodes from Linked List – Remove contiguous sequences of nodes whose sum is zero until none remain.
- 87. Delete the Middle Node of a Linked List – Remove the middle node from the linked list.
- 88. Remove Nodes From Linked List – Remove all nodes that have a greater value node appearing later in the list.

#### 3. Merging / Combining Lists
- 21. Merge Two Sorted Lists – Merge two sorted linked lists into a single sorted list.
- 23. Merge k Sorted Lists – Merge k sorted linked lists into one sorted list efficiently.
- 24. Merge In Between Linked Lists – Splice one linked list into another between given positions.
#### 4. Swapping / Rearranging Nodes
- 24. Swap Nodes in Pairs – Swap every two adjacent nodes in a linked list.
- 143. Reorder List – Rearrange nodes in alternating first–last order.
- 144. Odd Even Linked List – Group all odd-indexed nodes followed by even-indexed nodes.
- 145. Swapping Nodes in a Linked List – Swap nodes at two given positions in the list
#### 5. Reversing Lists
- 25. Reverse Nodes in k-Group – Reverse nodes in groups of k in the linked list.
- 92. Reverse Linked List II – Reverse a portion of the linked list between two given positions.
- 93. Reverse Linked List – Reverse the entire linked list.
- 94. Reverse Nodes in Even Length Groups – Reverse nodes in groups whose length is even.
#### 6. Cloning / Complex References
- 138. Copy List with Random Pointer – Clone a linked list where each node has an additional random pointer.
#### 7. Cycle Detection / Handling
- 141. Linked List Cycle – Detect if the linked list contains a cycle.
- 142. Linked List Cycle II – Find the starting node of the cycle in the linked list.
#### 8. Order-Based Rearrangement
- 146. LRU Cache – Design a cache that evicts the least recently used item.
- 460. LFU Cache – Design a cache that evicts the least frequently used item.
- 461. Linked List Random Node – Return a random node’s value from the linked list.
#### 9. Sorting Linked Lists
- 147. Insertion Sort List – Sort the linked list using insertion sort.
- 148. Sort List – Sort the linked list in O(n log n) time.
#### 10. Intersection / Meeting Point Problems
- 160. Intersection of Two Linked Lists – Find the node where two linked lists intersect.
#### 11. Multi-Level / Nested List Handling
- 430. Flatten a Multilevel Doubly Linked List – Flatten a multilevel doubly linked list into a single-level list.
#### 12. Splitting Lists
- 725. Split Linked List in Parts – Split the linked list into k equal or nearly equal parts.
#### 13. Middle / Position Finding
- 876. Middle of the Linked List – Find the middle node of the linked list.

###### Something Extra using ChatGPT:
![[Linked List Problem Patterns - ChatGPT.png]]