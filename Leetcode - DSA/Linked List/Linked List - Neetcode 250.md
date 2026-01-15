#linked-list #dsa #leetcode 
### [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
```Java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null) return head;

        ListNode prev = null;
        ListNode curr = head;
        ListNode next = head.next;

        while(curr != null){
            curr.next = prev;

            prev = curr;
            curr = next;
            if(next != null) next = next.next;
        }

        return prev;
    }
}
```

Maintain 3 pointers - prev, curr and next
curr stays at head, prev is 1 step behind and next is 1 step ahead
At each node, break the link from curr → next and make the link from curr → prev. 
This done using line 10 - ```curr.next = prev;```
Now update the pointers - move them ahead
Finally prev points to the last node and return that

### [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
```Java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode head1 = list1;
        ListNode head2 = list2;

        ListNode head = new ListNode(-101);
        ListNode newHead = head;

        while(head1!=null && head2!=null){
            if(head1.val <= head2.val){
                head.next = new ListNode(head1.val);
                head1 = head1.next;
            }
            else{
                head.next = new ListNode(head2.val);;
                head2 = head2.next;
            }
            head = head.next;
        }

        while(head1!=null){
            head.next = new ListNode(head1.val);
            head1 = head1.next;
            head = head.next;
        }

        while(head2!=null){
            head.next = new ListNode(head2.val);
            head2 = head2.next;
            head = head.next;
        }

        return newHead.next;
    }
}
```

Merge Sort Algorithm is used here
A dummy head is created first - you could have manually determined the actual head but this is straight forward 

### [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
#slow-fast-pointer
```Java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        
        while(fast!=null && fast.next!=null){
            if(fast.next!=null){
                fast = fast.next.next;          
            }
            slow = slow.next;            
            if(slow==fast){
                return true;
            }
        }
        return false;
    }
}
```

The fast pointer moves 2 steps at a time and the slow 1 step at a time
If there is a cycle, there will come a time when both reach on the same node. If there is no cycle, they will exit the while loop to return false

### [143. Reorder List](https://leetcode.com/problems/reorder-list/)
```Java
class Solution {
    public void reorderList(ListNode head) {
        ListNode curr = head;

        //reach mid first
        int size = getSize(head);
        int mid = size/2;
        if(size%2!=0) mid++;

        while(--mid!=0) curr = curr.next;
       
        ListNode midNode = curr;        
        Stack<ListNode> st = new Stack<>();

        while(curr.next!=null){  //push mid+1 to end nodes in stack
            curr = curr.next;
            st.push(curr);
        }

        midNode.next = null;    //later make sure mid is our last node, to avoid cycle

        //now back to starting nodes
        ListNode prev = head;
        curr = prev.next;

        while(!st.isEmpty()){
            ListNode node = st.pop();
            node.next = null;
            node.next = curr;
            prev.next = node;

            prev = curr;
            if(curr!=null) curr = curr.next;
        }
    }

    int getSize(ListNode head){
        ListNode fast = head;
        int size = 0;
        while(fast!=null && fast.next!=null){
            fast = fast.next.next;
            size+=2;
        }
        if(fast!=null) size++;
        return size;
    }
}
```

We want to maintain the first half of the list as it is and insert the second half in reverse order between them
So, we can use a Stack to get the nodes in reverse order
Hence, we first reach the middle of the linked list and push all the nodes from mid+1 to end in our stack
Then again from start, we pop each node insert them in between 2 nodes and continue till our stack is empty
 ! Remember, the mid node becomes our last node so we need to mark it's next to null to make it our last node

### [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
```Java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        int size = getSize(head);
        if(size<2 || n==size) return head.next;
        int count = 1;
        ListNode temp = head;
        ListNode prev = head;
        while(count!=(size-n+1)){
            prev = temp;
            temp = temp.next;
            count++;
        }
        prev.next = temp.next;
        return head;
    }
    int getSize(ListNode head){
        ListNode fast = head;
        int size = 0;
        while(fast!=null && fast.next!=null){
            fast = fast.next.next;
            size+=2;
        }
        if(fast!=null) size++;
        return size;
    }
}
```

Get the size of the linked list and reach the nth node from end
Maintain a prev pointer to remove the specified node

### [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

### [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)
```Java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode a = l1;
        ListNode b = l2;
        boolean overflow = false;
        ListNode temp = new ListNode(-1);
        ListNode newHead = temp;
        while(a!=null && b!=null){
            int sum = a.val + b.val;
            if(overflow){
                overflow = false;
                sum+=1;
            }
            if(sum>9){
                overflow = true;
                sum = sum-10;
            }
            temp.next = new ListNode(sum);
            temp = temp.next;
            a = a.next;
            b = b.next;
        }
        while(a!=null){
            int sum = a.val;
            if(overflow){
                overflow = false;
                sum += 1;
            }
            if(sum>9){
                overflow = true;
                sum = sum-10;
            }
            temp.next = new ListNode(sum);
            temp = temp.next;
            a = a.next;
        }
        while(b!=null){
            int sum = b.val;
            if(overflow){
                overflow = false;
                sum += 1;
            }
            if(sum>9){
                overflow = true;
                sum = sum-10;
            }
            temp.next = new ListNode(sum);
            temp = temp.next;
            b = b.next;
        }
        if(overflow){
            temp.next = new ListNode(1);
        }
        return newHead.next;
    }
}
```

Straight forward - Maintain a flag overflow to use "carry-forward"
Add the 2 nodes and store in "sum". 
If overflow flag is already set, add 1 to sum and unset the overflow flag
Now if sum goes above 9, set the overflow flag, subtract 10 from the sum and create a node 

### [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)
```Java
class Solution {
    public int findDuplicate(int[] nums) {
        while(true){
            int i = nums[0];
            if(nums[i] != i) swap(nums, 0, i);
            else return i;
        }
    }
    void swap(int[] nums, int i, int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

### [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)
```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if(head == null || head.next == null) return head;
        if(left == right) return head;

        //get to the right index node
        ListNode end = head;
        int rcount = 1;
        while(rcount<right && end!=null){
            end = end.next;
            rcount++;
        }
        ListNode next = end.next;

        //get to the left index node
        ListNode start = head;
        ListNode prev = null;
        int lcount = 1;
        while(lcount<left && start!=null){
            prev = start;
            start = start.next;
            lcount++;
        }

        //add integers from left to right in reverse
        List<Integer> myVec = new ArrayList<>();
        while(start!=end){
            myVec.add(0,start.val);
            start = start.next;
        }
        myVec.add(0,start.val);

        //create new LL from vector for the range
        if(prev!=null){
            prev.next = null;
            int i = 0;
            while(i<myVec.size()){
                prev.next = new ListNode(myVec.get(i));
                prev = prev.next;
                i++;
            }
        }
        else if(prev == null){
            int i = 0;
            prev = new ListNode(myVec.get(i));
            head = prev;
            i++;
            while(i<myVec.size()){
                prev.next = new ListNode(myVec.get(i));
                prev = prev.next;
                i++;
            }
        }
        //append the LL beyond right index
        while(next!=null){
            prev.next = next;
            next = next.next;
            prev = prev.next;
        }

        return head;
    }
}
```


### [622. Design Circular Queue](https://leetcode.com/problems/design-circular-queue/)
```Java
class MyCircularQueue {
    int[] myArr;
    int front;
    int rear;
    public MyCircularQueue(int k) {
        myArr = new int[k];
        rear = -1;
        front = -1;
    }
    
    public boolean enQueue(int value) {
        if(this.isFull()) return false;
        if(front == rear && rear == -1){
            front = 0;
            rear = 0;
            myArr[rear] = value;
            return true;
        }
        rear = (rear+1)%(myArr.length);
        myArr[rear] = value;
        return true;
    }
    
    public boolean deQueue() {
        if(this.isEmpty()) return false;
        if(front == rear){
            front = -1;
            rear = -1;
            return true;
        }
        front = (front+1)%(myArr.length);
        return true;
    }
    
    public int Front() {
        if(this.isEmpty()) return -1;
        return myArr[front];
    }
    
    public int Rear() {
        if(isEmpty()) return -1;
        return myArr[rear];
    }
    
    public boolean isEmpty() {
        return (front == rear && front == -1);
    }
    
    public boolean isFull() {
        return ((rear+1)%(myArr.length) == front);
    }
}
```
