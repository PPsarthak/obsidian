#bubble-sort #insertion-sort #merge-sort #quick-sort #sorting #leetcode #dsa
## Bubble Sort

> Info

**Bubble sort** is a sorting algorithm that compares two adjacent elements and swaps them until they are in the intended order.

> Code

```Java
void bubbleSort(int array[]) {
    int size = array.length;
    for (int i = 0; i < size - 1; i++){
		boolean swapped = false;
	    for (int j = 0; j < size - i - 1; j++){ //compare 2 adj elem
		    if (array[j] > array[j + 1]) {          // swapping
	          int temp = array[j];
	          array[j] = array[j + 1];
	          array[j + 1] = temp;
	          swapped = true;
	        }
		}
		if(!swapped) break;
	}
}
```

> Complexities
- **Worst Case Complexity:** `O(n2)`  - Array reverse sorted
- **Best Case Complexity:** `O(n)`   Array already sorted
- **Average Case Complexity:** `O(n2)`  

Space complexity is `O(1)`

---
## Insertion Sort

> Info

Insertion sort is a sorting algorithm that places an unsorted element at its suitable place in each iteration.

Insertion sort works similarly as we sort cards in our hand in a card game.

> Code

```Java
void insertionSort(int array[]) {
    int size = array.length;

    for (int step = 1; step < size; step++) {
		int key = array[step];
	    int j = step - 1;

      // Compare key with each element on the left
		while (j >= 0 && key < array[j]) {
	        array[j + 1] = array[j];
	        --j;
		}

      // Place key at after the element just smaller than it.
		array[j + 1] = key;
	}
}
```

> Complexities

**Worst Case Complexity:** `O(n2)`  - Array reverse sorted
**Best Case Complexity:**`O(n)` - Array already sorted
**Average Case Complexity:**`O(n2)`

---
## Merge Sort

> Info

The problem is divided into smaller sub-array and each sub-array is solved individually

> Code

```Java
void mergeSort(int[] arr, int first, int last){  
    if(first<last){  //keep dividing till first = last is not true
        int mid = first + (last-first)/2;  
        mergeSort(arr, first, mid);  
        mergeSort(arr, mid+1, last);  
        merge(arr, first, mid, last);  
    }
}

void merge(int[] arr, int first, int mid, int last){  
    int[] arr1 = new int[mid-first+1];  
    int[] arr2 = new int[last-mid];  
  
    for (int i = 0; i < arr1.length; i++) arr1[i] = arr[first+i];  //store first-mid
    for (int i = 0; i < arr2.length; i++) arr2[i] = arr[mid+1+i];  //store mid+1-last
    
    int i = 0, j = 0, k = first;  
    
    while(i < arr1.length && j < arr2.length){  
        if(arr1[i]<arr2[j]) arr[k++] = arr1[i++];  
        else arr[k++] = arr2[j++];  
    }  
    
    while(i < arr1.length){  
        arr[k++] = arr1[i++];  
    }    
    
    while(j < arr2.length){  
        arr[k++] = arr2[j++];  
    }
}
```

> Complexities

In any case (whether the array is sorted/reverse sorted), the array divides itself into smaller sub-arrays. So, time complexity is always `O(nlogn)`.

Space Complexity = `O(n)`

---
## Quick Sort

> Info

An array is divided such that, the pivot element (element selected randomly) should be positioned in such a way that elements less than pivot are kept on the left side and elements greater than pivot are on the right side of the pivot.

> Code

```Java
void quickSort(int[] arr, int first, int last){  
    if(first<last){  
        int down = first, up = last;  
        int pivot = first;  
        while(down<up){  
            while(arr[pivot]>arr[down] && down<last) down++;  
            while(arr[pivot]<=arr[up] && up>first) up--;  
            if(down<up) swap(arr, up, down);  
        }        
        swap(arr, pivot, up);  
        quickSort(arr, first, up-1);  
        quickSort(arr, up+1, last);  
    }
}
```

> Complexities

**Worst Complexity**`O(n2)` - When pivot element picked is either the greatest or the smallest element.
**Best Complexity**`O(nlogn)` - When the pivot element is always the middle element
**Average Complexity**`O(nlogn)`

Space complexity - `O(logn)`

#linear-search #binary-search #interpolation-search  #quick-select