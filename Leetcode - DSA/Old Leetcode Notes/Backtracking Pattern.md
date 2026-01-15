#leetcode 

### Theory/Notes

- Think in terms of a decision tree  - you have 2 decisions to take: pick or not-pick
- Always define the end  - add current path to result when at the end decision point
- Backtrack after exploring one choice but handle duplicates carefully (sorting + skip rule)

Most backtracking problems follow the below template - 
```Java
void backtrack(State state) {
    if (goal reached) {
        save solution
        return;
    }

    for (choice in possibleChoices) {
        make(choice);        // try this step
        backtrack(state);    // explore further
        undo(choice);        // backtrack (clean up)
    }
}
```

#### 1. Subsets (Power Set) Pattern
- **Goal**: Explore all combinations of including/excluding elements.
- **Typical Problems**:
	-  [[#78. Subsets 🟡]] 
	-  [[#90. Subsets II 🟡]] 
	- [[#77. Combinations 🟡]]
- **Approach**:
	- At each index: either take it or skip it.
	- Recursively advance index → till you reach end.
- **Key Difference**: You usually pass an `index` to avoid reusing earlier elements.
---
#### 2. Permutations Pattern
- **Goal**: Explore all possible orderings of items.
- **Typical Problems**:
	- [[#46. Permutations 🟡]]
	- [[#47. Permutations II 🟡]]
	- 526. Beautiful Arrangement 
	- 996. Number of Squareful Arrays 
	- 1239. Max Length Unique Characters 
- **Approach**:
	- At each step, pick one unused element
	- Use a boolean[] array to track choices
- **Key difference**: No start index - any unused element is valid

#### 3. Combination Sum / Combinatorial Search
- **Goal**: Build combinations that satisfy constraints (like sum = target)
- **Typical Problems**:
	- 39. Combination Sum
	- 40. Combination Sum II
	- 216. Combination Sum III
- **Approach**:
	- Similar to subsets, but add pruning (stop when sum > target, or when path length > k).
- **Key difference**: Strong use of constraints to cut branches early

#### 4. Constraint Placement (Board Problems)
- **Goal**: Place objects with rules (queens, knights, numbers…)
- **Typical Problems**:
	- N-Queens (51)
	- Sudoku Solver (37)
- **Approach**:
	- Row-by-row (or cell-by-cell) placement
	- At each cell: try all valid placements - backtrack if it leads to conflict
- **Key difference**: Heavy validity checks before making a move

#### 5. Path-Finding / Grid Exploration
- **Goal**: Explore possible paths in a matrix/grid
- **Typical Problems**:
	- Word Search (79)
	- Rat in a Maze (classic)
	- Unique Paths with obstacles (with constraints)
	- 22. Generate Parentheses (22, construct valid parenthesis string)
	- 93. Restore IP Addresses (93, cut into 4 valid segments)
	- 842. Split Array into Fibonacci (842, split string into valid sequence)
	- 784. Letter Case Permutation (784, branch with upper/lower at each char)
- **Approach**:
	- From current cell, try moving in 4 directions.
	- Mark visited cells; undo after exploring.
- **Key difference**: Grid-based state (row, col, visited)

#### 6. Partitioning Problems
- **Goal**: Break input into valid groups
- **Typical Problems**:
	- 131. Palindrome Partitioning
	- Word Break (backtracking version)
- **Approach**:
	- At each index, try splitting the string in different places
	- Only continue if the piece is valid (e.g., palindrome)
- **Key difference**: You’re recursively cutting the string at various points

| *Pattern*            | *Choice Source*                | *State Tracking*       | *Special Thing*   |
| ---------------------- | -------------------------------- | ------------------------ | ------------------- |
| *Subsets*              | Include/Exclude each element     | Index + path             | Simple branching    |
| *Permutations*         | Any unused element               | Used\[] array            | Order matters       |
| *Combination Sum*      | Pick next element (with pruning) | Start index + path + sum | Constraint-driven   |
| *Constraint Placement* | Board cells                      | Current row/col + board  | Validity checks     |
| *Path-Finding*         | Directions in grid               | (r, c) + visited         | Mark/unmark visited |
| *Partitioning*         | Cut string at different places   | Index + path             | Valid sub-strings   |

---

### Problems

##### 78. Subsets 🟡
[Pronlem](https://leetcode.com/problems/subsets): Given an integer array of *unique elements* (Subsets II has duplicates), the task is to generate all possible subsets, also known as the power set, without including any duplicate subsets in the solution; the function should return the solution in any order, covering all combinations from empty to the full set.
###### Approach:
- Pick the current element and move forward
- Then backtrack, un-pick that element and again move forward
- Note - using a HashSet for `ans` is optional here because our array contains unique elements. So the same subset will not get repeated here.
```Java
Set<List<Integer>> ans = new HashSet<>();
public List<List<Integer>> subsets(int[] nums) {
	backtrack(nums, 0, new ArrayList<>());
	return new ArrayList<>(ans);
}
void backtrack(int[] nums, int index, List<Integer> list){
	if(index == nums.length){
		ans.add(new ArrayList<>(list));
		return;
	}

	//pick
	list.add(nums[index]);
	backtrack(nums, index+1, list);

	//not pick
	list.remove(list.size()-1);
	backtrack(nums, index+1, list);
}
```
---
##### 90. Subsets II 🟡
[Problem](https://leetcode.com/problems/subsets-ii/description/): Given an integer array that *may contain duplicates* (Subsets I contains unique), the task is to return all possible subsets (the power set) without any duplicate subsets in the solution. The order of the solution does not matter. 
###### Approach:
- The judge expects the subsets to sorted, so for test case - `[2,2,2,1]`, `[2,2,1]` is not even expected. Instead `[1,2,2]` is expected. Hence, we sort the array before hand
- Then apply the same logic as Subsets
- Note - using a HashSet for `ans` is important here, to avoid duplicates. You can skip using HashSet in Subsets I but can't skip it here.
```Java
Set<List<Integer>> ans = new HashSet<>();
public List<List<Integer>> subsetsWithDup(int[] nums) {
	Arrays.sort(nums);
	backtrack(nums, 0, new ArrayList<>());
	return new ArrayList<>(ans);
}
void backtrack(int[] nums, int index, List<Integer> list){
	if(index == nums.length){
		ans.add(new ArrayList<>(list));
		return;
	}

	//pick
	list.add(nums[index]);
	backtrack(nums, index+1, list);

	//not pick
	list.remove(list.size()-1);
	backtrack(nums, index+1, list);
}
```
---
##### 77. Combinations 🟡
[Problem](https://leetcode.com/problems/combinations/): Generate all possible combinations of k numbers chosen from the range [1, n].  The combinations are unordered, meaning [1,2] and [2,1] are considered the same. The function should return a list of these combinations, and the order of the combinations in the output does not matter.

###### Approach 1:
You can go with our usual pick/not-pick style with the 2 base cases :
	- Either we have selected our 'k' elements so we add it our answer and backtrack
	- Or we have gone too far out of our given range of [1,n] so we backtrack
```Java
List<List<Integer>> ans = new ArrayList<>();
public List<List<Integer>> combine(int n, int k) {
	get(1, n, k, new ArrayList<>());
	return ans;
}
void get(int index, int n, int k, List<Integer> temp){
	if(k == 0){     //'k' elem are chosen
		ans.add(new ArrayList<>(temp));
		return;
	}

	//'k' elem are not chosen, but you have gone out of range now
	if(index > n) return; 

	//pick
	temp.add(index);
	get(index+1, n, k-1, temp);

	//not pick
	temp.remove(temp.size()-1);
	get(index+1, n, k, temp);
}
```

Here is the recursion tree of this code - 
```mathematica
Start: index=1, k=2, temp=[]
 ├─ Pick 1 → temp=[1], k=1
 │   ├─ Pick 2 → temp=[1,2], k=0 ✅ [1,2]
 │   ├─ Not pick 2
 │   │   ├─ Pick 3 → temp=[1,3], k=0 ✅ [1,3]
 │   │   ├─ Not pick 3
 │   │   │   ├─ Pick 4 → temp=[1,4], k=0 ✅ [1,4]
 │   │   │   └─ Not pick 4 → nothing
 │
 ├─ Not pick 1
 │   ├─ Pick 2 → temp=[2], k=1
 │   │   ├─ Pick 3 → temp=[2,3], k=0 ✅ [2,3]
 │   │   ├─ Not pick 3
 │   │   │   ├─ Pick 4 → temp=[2,4], k=0 ✅ [2,4]
 │   │   │   └─ Not pick 4 → nothing
 │   ├─ Not pick 2
 │   │   ├─ Pick 3 → temp=[3], k=1
 │   │   │   ├─ Pick 4 → temp=[3,4], k=0 ✅ [3,4]
 │   │   │   └─ Not pick 4 → nothing
 │   │   └─ Not pick 3 → eventually nothing
```

###### Approach 2:
Another approach is using a for-loop:
```Java
List<List<Integer>> ans = new ArrayList<>();
public List<List<Integer>> combine(int n, int k) {
	get(1, n, k, new ArrayList<>());
	return ans;
}
void get(int index, int n, int k, List<Integer> temp){
	if(k == 0){
		ans.add(new ArrayList<>(temp));
		return;
	}
	for(int i=index; i<=n; i++){
		temp.add(i);
		get(i+1, n, k-1, temp);
		temp.remove(temp.size()-1);
	}
}
```

Here is the recursion tree:
```mathematica
Start: index=1, k=2, temp=[]
 ├─ i=1 → temp=[1]
 │   ├─ recurse with index=2, k=1
 │   │   ├─ i=2 → temp=[1,2], k=0 ✅ [1,2]
 │   │   ├─ i=3 → temp=[1,3], k=0 ✅ [1,3]
 │   │   ├─ i=4 → temp=[1,4], k=0 ✅ [1,4]
 │
 ├─ i=2 → temp=[2]
 │   ├─ recurse with index=3, k=1
 │   │   ├─ i=3 → temp=[2,3], k=0 ✅ [2,3]
 │   │   ├─ i=4 → temp=[2,4], k=0 ✅ [2,4]
 │
 ├─ i=3 → temp=[3]
 │   ├─ recurse with index=4, k=1
 │   │   ├─ i=4 → temp=[3,4], k=0 ✅ [3,4]
 │
 ├─ i=4 → temp=[4]
 │   ├─ recurse with index=5, k=1 → stops (nothing)

```

**Big picture:**
- *Pick/Not Pick:* explores all binary decisions, but includes branches that eventually fail (e.g., not picking too many).
- *For-loop:* skips those redundant branches, directly iterates only over valid future candidates.
---
##### 46. Permutations 🟡
[Problem](https://leetcode.com/problems/permutations/description/): Given an array of distinct integer, generate all possible permutations
###### Approach:
- Use the `.contains()` to check if the element is already added in the list. Alternatively, you can also create a HashSet that remains consistent with the list. So, just check if the set contains current element, if not add it and then check the possible paths. If it is already there, skip it
- It is not possible to have only HashSet (and no List<Integer) because a HashSet doesn't preserve insertion order. So when you add the set into `ans`, you WILL get WA because of this. However, you can use Linked HashSet. 
```Java
List<List<Integer>> ans = new ArrayList<>();
public List<List<Integer>> permute(int[] nums) {
	backtrack(nums, new ArrayList<>());
	return ans;
}
void backtrack(int[] nums, List<Integer> list){
	if(list.size() == nums.length){
		ans.add(new ArrayList<>(list));
		return;
	}

	for(int i : nums){
		if(!list.contains(i)){
			list.add(i);
			backtrack(nums, list);
			list.remove(list.size()-1);
		}
	}
}
```
---
##### 47. Permutations II 🟡 
[Problem](https://leetcode.com/problems/permutations-ii/): Generate all unique permutations of an array that may contain duplicate numbers.
###### Approach:
- Here we are given that `nums[i] ∈ [-10,10]` so, when I pick the element, I decrement it by 100 and while backtracking I again increment it by 100
```Java
Set<List<Integer>> ans = new HashSet<>();
public List<List<Integer>> permuteUnique(int[] nums) {
	backtrack(nums, new ArrayList<>());
	return new ArrayList<>(ans);
}
void backtrack(int[] nums, List<Integer> list){
	if(list.size() == nums.length){
		ans.add(new ArrayList<>(list));
		return;
	}

	for(int i=0; i<nums.length; i++){
		if(nums[i] > -11){
			list.add(nums[i]);
			nums[i] -= 100;
			backtrack(nums, list);
			nums[i] += 100;
			list.remove(list.size()-1);
		}
	}
}
```

##### 39. Combination Sum
Find all unique combinations of numbers from a given array that sum up to a specified target value. The same number can be used multiple times in a combination, and the order of combinations does not matter.

##### 784. Letter Case Permutation
The task is to generate all possible strings from a given input string s by transforming each letter to either lowercase or uppercase. The function should return a list containing all possible strings, and the order of the strings in the output list does not matter, where the input string s consists of lowercase English letters, uppercase English letters, and digits. (Digits in the string remain unchanged)


----
```
401 Binary Watch (Easy) – Generates constrained combinations (time).
526 Beautiful Arrangement (Medium) – Count permutations with divisibility rules.
473 Matchsticks to Square (Medium) – Partition into 4 equal subsets → subset sum + pruning.
698 Partition to K Equal Sum Subsets (Medium) – Generalized version of above.
1239 Maximum Length of a Concatenated String with Unique Characters (Medium) – Subset generation with char-uniqueness constraints.
282 Expression Add Operators (Hard) – Insert operators to hit target → string + numeric recursion.
679 24 Game (Hard) – Recursive search with arithmetic operators.
996 Number of Squareful Arrays (Hard) – Permutations with extra constraint (adjacent sums must be perfect squares).
93 Restore IP Addresses (Medium) – Generate valid IP splits → string partitioning + validation.
842 Split Array into Fibonacci Sequence (Medium) – String partition + numeric constraints.
```

#### 1. Permutations
- Permutations
- Permutations II
- Beautiful Arrangement ✅
- Number of Square-ful Arrays ✅
#### 2. Combinations / Subsets
- Combination Sum
- Combination Sum II
- Combinations
- Subsets
- Subsets II
- Combination Sum III
- Max Length Unique Characters ✅

#### 3. Partitioning
- Palindrome Partitioning
- Sudoku Solver
- Partition to K Equal Sum Subsets ✅
- Matchsticks to Square ✅

#### 4. Constraint Satisfaction
- N-Queens
- Binary Watch ✅
- Letter Case Permutation

#### 5. String Construction
- Generate Parentheses
- Restore IP Addresses ✅
- Split Array into Fibonacci ✅
- Expression Add Operators ✅

#### 6. Search / Arithmetic Backtracking
- 24 Game ✅
- Word Search
- Squareful Arrays (also here as constraint search)