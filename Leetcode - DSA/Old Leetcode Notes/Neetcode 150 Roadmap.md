#leetcode #dsa #placement-preparation 
# Greedy

---
## [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) 

#### Kadane's Algorithm

This algorithm is used to find the maximum sum in contigous sub-array. 
* Here we need two variables, currentSum and maxSum.
* Now we traverse the array 
* we add curr ele to the our currentSum variable
* then check if currentSum has gone beyond maxSum
* if yes, we update maxSum with currentSum
* now at the end of iteration is a very important step
* we see if our currentSum is negative or not
* if it is negative, we update the currentSum to zero

> Here's why?
> Let's say we have currentSum < 0 as of now
> Now there are 2 cases for the next element: it is either >0 or <=0
> Let's say if it <=0, then we know that adding it to our currentSum will make it more negative (or equal if it is 0)
> Better is, we make our currentSum = 0, and then automatically, if next element is *less* negative then our maxSum will take it as ans; if not then it will not affect our ans
> Let's say, next ele was positive, now if we add it to currentSum it may or may not become*entirely*  positive
> Better is, we make our currentSum = 0, this way that entire number will be checked if it exceeds our max or not

```Java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = Integer.MIN_VALUE, currentSum = 0;
        for(int i=0; i<nums.length; i++){
            currentSum += nums[i];
            if(currentSum>maxSum){
                maxSum = currentSum;
            }
            if(currentSum<0){
                currentSum = 0;
            }
        }
        return maxSum;
    }
}
```

---
## [Jump Game](https://leetcode.com/problems/jump-game/description/)

#### Dynamic Programming Approach

We start at index = 0. The value that is present at nums[index] represents the size of jump we can take from that index. **We can also take a smaller jump than that value**

So we try taking all jumps from 1 to nums[index].
Since this involves backtracking and we want to avoid unnecessary checks, we can use dp

Here we have an integer array as dp. 
0 = not visited
1 = true / it is possible to jump and reach end
-1 = false / it is not possible 


```Java
class Solution {
    int[] dp;
    int n;
    public boolean canJump(int[] nums) {
        n = nums.length;
        dp = new int[n];

        return recursive(0, nums);
    }

    boolean recursive(int index, int[] nums){
        if(index >= n-1){
            return true;
        }

        if(dp[index] == 1) return true;
        if(dp[index] == -1) return false;

        int curr = nums[index];
        int max_jump = index + curr;

        for(int i=index+1; i<=max_jump; i++){
            if(dp[i] == 0){
                boolean flag = recursive(i, nums);
                if(flag){
                    dp[i] = 1;
                    return true;
                }
                else{
                    dp[i] = -1;
                }
            }
        }

        dp[index] = -1;
        return false;
    }
}
```

#### Better Approach (from Solutions)

Here we iterate the array. A variable (reachable) keeps the track of maximum possible jump so far.
The goal is to ensure reachable is always ahead of i i.e., it is always possible to go even further if we want.
The moment our i goes ahead of this variable we return false

```Java
class Solution {
    public boolean canJump(int[] nums) {
       int reachable = 0;
       for(int i = 0; i < nums.length; i ++) {
           if(i > reachable) return false;
           reachable = Math.max(reachable, i + nums[i]);
       } 
       return true;
    }
}
```

---
## [Gas Station](https://leetcode.com/problems/gas-station/)

##### Brute Force - TLE

The brute force approach is pretty straight forward. We try each index as starting point and see if we can reach that index again.
> Important:
> It is very important to always take all the gas that we have. So at each index, we take all the gas[i] and reduce by cost[i] So the goal is to have maximum possible gas in the tank. Even if you have enough gas in the tank to go to next station, still take the all the current gas

```Java
class Solution {
    int n;
    public int canCompleteCircuit(int[] gas, int[] cost) {
        n = gas.length;

        if (Arrays.stream(gas).sum() < Arrays.stream(cost).sum()) {
            return -1;
        }
        
        for(int i=0; i<n; i++){    
            int curr = gas[i] - cost[i];
            if(curr < 0) continue;
            int idx = nextIdx(i);
            while(idx != i){
                curr += (gas[idx]-cost[idx]);
                if(curr < 0) break;
                idx = nextIdx(idx);
            }

            if(idx == i) return i;
        }

        return -1;
    }
    int nextIdx(int i){
        return (i+1)%n;
    }
}
```

#### Optimized - Kadane's Algorithm (from solutions):

```Java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        if(Arrays.stream(gas).sum() < Arrays.stream(cost).sum()) {
            return -1;
        }
        int curr = 0;
        int n = gas.length;
        int s = 0;

        for(int i=0; i<n; i++){
            curr += gas[i]-cost[i];

            if(curr < 0){
                curr = 0;
                s = i+1;
            }

        }
        return s;
    }
}
```

Take a good look at the solution. The first condition is very important for the solution. This condition ensures that a solution exists before going forward. 
Now the use of Kadane.....
Since we now know a solution does exist, the problem left is to find the index from where the solution exists.
Observation:
The index from where we start (our answer) will be such that the gas in the tank will always be greater than cost required. So the "curr" will always be >= 0. 
So if you use kadane's algorithm, you eliminate those indexes from where curr < 0

---

