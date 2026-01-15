---
topics: ALGORITHM
---
#Leetcode #DSA #Boyer-Moore-Majority-Voting-Algorithm

- <mark style="background: #33EE99;">The Boyer-Moore Majority Vote Algorithm identifies an element that appears more than n/2 times in a sequence of n elements with a time complexity of O(n) and a constant space complexity of O(1).</mark> 
- It works in two passes: first, it finds a candidate for the majority element by incrementing a counter for matching elements and decrementing it for non-matching elements, resetting the candidate if the count reaches zero. 
- Second, it verifies if this candidate is indeed the majority element by recounting its occurrences in the entire sequence.

### How the Algorithm Works (Two Passes)
**Pass 1: Candidate Identification**
- *Initialization*: Set a candidate variable to None and a count to 0. 
- *Iteration*: Traverse the input array once. 
	- If the count is 0, set the current element as the new candidate and set count to 1. 
	- If the current element is the same as the candidate, increment the count. 
	- If the current element is different from the candidate, decrement the count. 
- *Result*: After the first pass, the candidate variable will hold the potential majority element. 

**Pass 2: Candidate Verification**
- *Initialization*: Reset the count to 0. 
- *Iteration*: Traverse the input array a second time. 
	- Count the total number of times the candidate from the first pass appears in the array. 
- *Result*: If the final count is greater than n/2 (where n is the size of the array), then the candidate is the true majority element. Otherwise, no majority element exists. 

###### Key Characteristics
- Time Complexity: O(n) because the algorithm makes at most two passes through the input array. 
- Space Complexity: O(1) because it only uses a couple of extra variables (candidate and count), not requiring any additional data structures. 
- Greedy Algorithm: It's a greedy approach, making locally optimal choices (adjusting the candidate and count) at each step, leading to a global solution.

##### Code
```java
public int majorityElement(int[] nums) {
	int candidate = -1;
	int count = 0;
	for(int i: nums){
		if(count==0){
			candidate = i;
			// count++;
		}
		
		if(candidate == i) count++;
		else count--;
	}
	
	if(count==0) return -1;
	else return candidate;
}
```