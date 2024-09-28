# Design-Circular-Deque
coding question


Design your implementation of the circular double-ended queue (deque).

Implement the MyCircularDeque class:

MyCircularDeque(int k) Initializes the deque with a maximum size of k.
boolean insertFront() Adds an item at the front of Deque. Returns true if the operation is successful, or false otherwise.
boolean insertLast() Adds an item at the rear of Deque. Returns true if the operation is successful, or false otherwise.
boolean deleteFront() Deletes an item from the front of Deque. Returns true if the operation is successful, or false otherwise.
boolean deleteLast() Deletes an item from the rear of Deque. Returns true if the operation is successful, or false otherwise.
int getFront() Returns the front item from the Deque. Returns -1 if the deque is empty.
int getRear() Returns the last item from Deque. Returns -1 if the deque is empty.
boolean isEmpty() Returns true if the deque is empty, or false otherwise.
boolean isFull() Returns true if the deque is full, or false otherwise.
 

Example 1:

Input
["MyCircularDeque", "insertLast", "insertLast", "insertFront", "insertFront", "getRear", "isFull", "deleteLast", "insertFront", "getFront"]
[[3], [1], [2], [3], [4], [], [], [], [4], []]
Output
[null, true, true, true, false, 2, true, true, true, 4]

Explanation
MyCircularDeque myCircularDeque = new MyCircularDeque(3);
myCircularDeque.insertLast(1);  // return True
myCircularDeque.insertLast(2);  // return True
myCircularDeque.insertFront(3); // return True
myCircularDeque.insertFront(4); // return False, the queue is full.
myCircularDeque.getRear();      // return 2
myCircularDeque.isFull();       // return True
myCircularDeque.deleteLast();   // return True
myCircularDeque.insertFront(4); // return True
myCircularDeque.getFront();     // return 4
 

Constraints:

1 <= k <= 1000

0 <= value <= 1000

At most 2000 calls will be made to insertFront, insertLast, deleteFront, deleteLast, getFront, getRear, isEmpty, isFull.

# SOLUTION

Intuition
Understanding a Deque: A deque (double-ended queue) allows insertion and deletion of elements at both the front and rear ends. It is useful for scenarios where operations need to be performed at both ends in constant time, O(1).
Circular Nature: Since this is a circular deque, it means that after reaching the end of the queue, it wraps around to the beginning. This is useful for efficient utilization of space.
Doubly Linked List: Using a doubly linked list is ideal for a deque because it allows direct access to both previous and next nodes, facilitating easy insertion and deletion at both ends.
Approach
Node Structure:

Create a Node class (or structure) representing each element in the deque. Each node will have:
A value (val).
A pointer to the previous node (prev).
A pointer to the next node (next).
Deque Class Initialization:

The MyCircularDeque class is initialized with a fixed size (k), representing the maximum number of elements it can hold.
Initialize count to track the current number of elements.
Use two pointers, front and rear, to represent the first and last nodes of the deque.
Insertions:

Insert at Front:
If the deque is not full, create a new node.
If the deque is empty, set both front and rear to this new node.
If not empty, adjust pointers:
The new node's next points to the current front.
The current front's prev points to the new node.
Update front to the new node.
Increment count after successful insertion.
Insert at Rear:
If the deque is not full, create a new node.
If the deque is empty, set both front and rear to this new node.
If not empty, adjust pointers:
The new node's prev points to the current rear.
The current rear's next points to the new node.
Update rear to the new node.
Increment count after successful insertion.
Deletions:

Delete from Front:
If the deque is empty, return false.
If the deque has only one element, set both front and rear to null.
Otherwise, update the front pointer to front.next and adjust its prev pointer.
Decrement count after successful deletion.
Delete from Rear:
If the deque is empty, return false.
If the deque has only one element, set both front and rear to null.
Otherwise, update the rear pointer to rear.prev and adjust its next pointer.
Decrement count after successful deletion.
Access Operations:

Get Front:
If the deque is empty, return -1.
Otherwise, return the value of the front node.
Get Rear:
If the deque is empty, return -1.
Otherwise, return the value of the rear node.
Helper Functions:

isEmpty():
Return true if count is 0; otherwise, false.
isFull():
Return true if count equals the maximum capacity n; otherwise, false.

Complexity

Time complexity:O(1)

Space complexity:O(k)

# JAVA CODE

    class MyCircularDeque {
    
    private class Node {
    
        int val;
        
        Node prev, next;
        
        Node(int value) {
        
            this.val = value;
            
        }
        
    }
    
    private int count;
    
    private int n;
    
    private Node front;
    
    private Node rear;

    public MyCircularDeque(int k) {
    
        this.count = 0;
        
        this.n = k;
        
        this.front = null;
        
        this.rear = null;
        
    }
    
    public boolean insertFront(int value) {
    
        if (isFull()) {
        
            return false;
            
        }
        
        Node newNode = new Node(value);
        
        if (front == null && rear == null) {
        
            front = rear = newNode;
            
        } 
        
        else {
        
            newNode.next = front;
            
            front.prev = newNode;
            
            front = newNode;
            
        }
        
        count++;
        
        return true;
    }
    
    public boolean insertLast(int value) {
    
        if (isFull()) {
        
            return false;
            
        }
        Node newNode = new Node(value);
        
        if (front == null && rear == null) {
        
            front = rear = newNode;
            
        } 
        else {
        
            rear.next = newNode;
            
            newNode.prev = rear;
            
            rear = newNode;
            
        }
        
        count++;
        
        return true;
    }
    
    public boolean deleteFront() {
    
        if (isEmpty()) {
        
            return false;
        }
        
        if (front == rear) {
        
            front = rear = null;
            
        } 
        
        else {
        
            front = front.next;
            
            front.prev = null;
            
        }
        
        count--;
        
        return true;
    }
    
    public boolean deleteLast() {
    
        if (isEmpty()) {
        
            return false;
            
        }
        
        if (front == rear) {
        
            front = rear = null;
            
        } 
        
        else {
        
            rear = rear.prev;
            
            rear.next = null;
            
        }
        
        count--;
        
        return true;
        
    }
    
    public int getFront() {
    
        if (isEmpty()) {
        
            return -1;
            
        }
        return front.val;
    }
    
    public int getRear() {
    
        if (isEmpty()) {
        
            return -1;
            
        }
        
        return rear.val;
        
    }
    
    public boolean isEmpty() {
    
        return count == 0;
        
    }
    
    public boolean isFull() {
    
        return count == n;
        
    }
    
}

/**
 * Your MyCircularDeque object will be instantiated and called as such:
 * MyCircularDeque obj = new MyCircularDeque(k);
 * boolean param_1 = obj.insertFront(value);
 * boolean param_2 = obj.insertLast(value);
 * boolean param_3 = obj.deleteFront();
 * boolean param_4 = obj.deleteLast();
 * int param_5 = obj.getFront();
 * int param_6 = obj.getRear();
 * boolean param_7 = obj.isEmpty();
 * boolean param_8 = obj.isFull();
 */
