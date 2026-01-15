#linear-search #binary-search #interpolation-search #searching #dsa #leetcode 
## Linear Search

> Info

**Linear search** is a simple search algorithm that checks every element in the list sequentially until the desired element is found or the list ends. It works on both sorted and unsorted lists.

> Code

```Java
int linearSearch(int[] array, int target) {
    int size = array.length;
    for (int i = 0; i < size; i++) {
        if (array[i] == target) {
            return i; // Return the index of the target element
        }
    }
    return -1; // Return -1 if the target is not found
}
```

> Complexities
- **Worst Case Complexity:** `O(n)` - When the target element is at the end of the array or not present.
- **Best Case Complexity:** `O(1)` - When the target element is the first element.
- **Average Case Complexity:** `O(n)`.

Space complexity is `O(1)`.

---
## Binary Search

> Info

**Binary search** is a more efficient search algorithm that works on sorted arrays. It repeatedly divides the search interval in half, checking the middle element and deciding whether to continue searching in the left or right half.

> Code

```Java
int binarySearch(int[] array, int target) {
    int left = 0;
    int right = array.length - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2; // Calculate mid index

        if (array[mid] == target) {
            return mid; // Return the index of the target element
        } 
        else if (array[mid] < target) {
            left = mid + 1; // Search in the right half
        } 
        else {
            right = mid - 1; // Search in the left half
        }
    }
    return -1; // Return -1 if the target is not found
}
```

> Complexities
- **Worst Case Complexity:** `O(log n)` - When the element is not present or is at the end of the search space.
- **Best Case Complexity:** `O(1)` - When the target element is the middle element.
- **Average Case Complexity:** `O(log n)`.

Space complexity is `O(1)`.

---
## Interpolation Search

> Info

**Interpolation search** is an algorithm used to find the position of a target value within a sorted array. It improves upon the binary search algorithm by using the formula to estimate the position of the target value based on the values of the key being searched. It is particularly efficient for ***uniformly distributed data.***

> Code

```Java
int interpolationSearch(int[] array, int target) {
    int low = 0;
    int high = array.length - 1;

    while (low <= high && target >= array[low] && target <= array[high]) {
        // Estimate the position of the target value
        int pos = low + ((target - array[low]) * (high - low)) / (array[high] - array[low]);
        
        // Check if the target is found at the estimated position
        if (array[pos] == target) {
            return pos; // Return the index of the target element
        }

        // If the target is larger, move the lower bound up
        if (array[pos] < target) {
            low = pos + 1;
        }
        // If the target is smaller, move the upper bound down
        else {
            high = pos - 1;
        }
    }
    return -1; // Return -1 if the target is not found
}
```

> Complexities
- **Worst Case Complexity:** `O(n)` - When the elements are uniformly distributed and the target is not present, leading to linear scans.
- **Best Case Complexity:** `O(1)` - When the target element is at the estimated position on the first attempt.
- **Average Case Complexity:** `O(log log n)` - Works well with uniformly distributed data.

Space complexity is `O(1)`.