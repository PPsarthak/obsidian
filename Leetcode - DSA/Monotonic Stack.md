#new-notes

> Ref: https://leetcode.com/discuss/post/2347639/a-comprehensive-guide-and-template-for-m-irii/

Monotonic stacks are generally used for solving questions of the type - next greater element, next smaller element, previous greater element and previous smaller element. We are going to create a template for each of the format.

#### What is monotonic stack?
There could be four types of monotonic stacks. Please read them carefully, we'll refer to these types at multiple places in the sections below.
- **Strictly increasing** - every element of the stack is strictly greater than the previous element. Example - `[1, 4, 5, 8, 9]`
- **Non-decreasing** - every element of the stack is greater than or equal to the previous element. Example - `[1, 4, 5, 5, 8, 9, 9]`
- **Strictly decreasing** - every element of the stack is strictly smaller than the previous element - `[9, 8, 5, 4, 1]`
- **Non-increasing** - every element of the stack is smaller than or equal to the previous element. - `[9, 9, 8, 5, 5, 4, 1]`

#### General Template
```javascript
function buildMonoStack(arr) {
  // initialize an empty stack
  stack = [];
  
  // iterate through all the elements in the array
  for (i = 0 to arr.length - 1)) {
    while (stack is not empty && element represented by stack top `OPERATOR` arr[i]) {
      // if the previous condition is satisfied, we pop the top element
      let stackTop = stack.pop();
  
      // do something with stackTop here e.g.
      // nextGreater[stackTop] = i
    }
  
    if (stack.length) {
      // if stack has some elements left
      // do something with stack top here e.g.
      // previousGreater[i] = stack.at(-1)
    }

    // at the ened, we push the current index into the stack
     stack.push(i);
  }
  
  // At all points in time, the stack maintains its monotonic property
}
```

The `OPERATOR` could be any of the four : 	`>, >=, <, <=`
....deciding what type of monotonic stack are we creating

#### 💡 Core Idea
When looking for next elements → we traverse from right to left
When looking for previous elements → we traverse from left to right

#### Next Greater
> Go `n-1 to 0` and use `<= or <` operator
```java
int[] nextGreater(int[] arr) {
	int n = arr.length;
	int[] ans = new int[n];
	Stack<Integer> st = new Stack<>(); // stores elements (or indices)
	
	for (int i=n-1; i>=0; i--) {
		//while stack top is SMALLER than the current element
		while (!st.isEmpty() && st.peek() <= arr[i]){
			 st.pop(); // pop smaller elements
		 }
		ans[i] = st.isEmpty() ? -1 : st.peek(); // next greater found or -1
		st.push(arr[i]);
	}
	return ans;
}
```

#### Previous Greater 
> Go `0 to n-1` and use  `<= or <` operator
```java
int[] prevGreater(int[] arr) {
	int n = arr.length;
	int[] ans = new int[n];
	Stack<Integer> st = new Stack<>();
	
	for (int i=0; i<n; i++) {
		while (!st.isEmpty() && st.peek() <= arr[i]){
			st.pop();
		}
		ans[i] = st.isEmpty() ? -1 : st.peek();
		st.push(arr[i]);
	}
	return ans;
}
```

#### Next Smaller 
> Go `n-1 to 0` and use  `>= or >` operator
```java
int[] prevGreater(int[] arr) {
	int n = arr.length;
	int[] ans = new int[n];
	Stack<Integer> st = new Stack<>();
	
	for (int i=n-1; i>=0; i--) {
		while (!st.isEmpty() && st.peek() >= arr[i]){
			st.pop();
		}
		ans[i] = st.isEmpty() ? -1 : st.peek();
		st.push(arr[i]);
	}
	return ans;
}
```

#### Previous Smaller 
> Go `0 to n-1` and use  `>= or >` operator
```java
int[] prevGreater(int[] arr) {
	int n = arr.length;
	int[] ans = new int[n];
	Stack<Integer> st = new Stack<>();
	
	for (int i=0; i<n; i++) {
		while (!st.isEmpty() && st.peek() >= arr[i]){
			st.pop();
		}
		ans[i] = st.isEmpty() ? -1 : st.peek();
		st.push(arr[i]);
	}
	return ans;
}
```

#### ⚙️ Quick Summary Table

| Type                                                               | Direction    | Stack Maintains | Pop Condition | Result Meaning                  |
| ------------------------------------------------------------------ | ------------ | --------------- | ------------- | ------------------------------- |
| <span style="color:rgb(240, 81, 105)">Next Greater</span>          | Right → Left | Decreasing      | `<= arr[i]`   | First greater on right          |
| <span style="color:rgb(240, 81, 105)">Next Greater or Equal</span> | Right → Left | Decreasing      | `< arr[i]`    | First greater or equal on right |
| <span style="color:rgb(255, 192, 0)">Prev Greater</span>           | Left → Right | Decreasing      | `<= arr[i]`   | First greater on left           |
| <span style="color:rgb(255, 192, 0)">Prev Greater or Equal</span>  | Left → Right | Decreasing      | `< arr[i]`    | First greater or equal on left  |
| <span style="color:rgb(0, 176, 240)">Next Smaller</span>           | Right → Left | Increasing      | `>= arr[i]`   | First smaller on right          |
| <span style="color:rgb(0, 176, 240)">Next Smaller or Equal</span>  | Right → Left | Increasing      | `> arr[i]`    | First smaller or equal on right |
| <span style="color:rgb(0, 176, 80)">Prev Smaller</span>            | Left → Right | Increasing      | `>= arr[i]`   | First smaller on left           |
| <span style="color:rgb(0, 176, 80)">Prev Smaller or Equal</span>   | Left → Right | Increasing      | `> arr[i]`    | First smaller or equal on left  |

#### 🧠 When to Decide Which to Use <span style="color:rgb(240, 81, 105)">(Read it carefully)</span>
> Use <= or >=  for non-strict comparisons; for strict comparisons (ignore equals - < or >)
