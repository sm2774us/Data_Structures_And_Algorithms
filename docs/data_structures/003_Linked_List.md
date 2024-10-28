---
tags:
  - Original
---

# Linked List

## Design a Linked List

### Problem Description: [LeetCode - Problem 707 - Design a Linked List](https://leetcode.com/problems/design-linked-list/description/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

This solution defines a doubly linked list using a `MyLinkedList` class, designed to perform efficient node access, insertion, and deletion operations. Let's go through the design, each method, a walkthrough, and a complexity analysis.

### Solution Design

The `MyLinkedList` class uses a **doubly linked list** structure with two sentinel nodes (`__head` and `__tail`) to represent the start and end of the list, simplifying insertion and deletion operations near the list boundaries. The list also keeps track of its current size (`__size`), which helps in validating indices for operations.

### Methods and Their Explanations

#### 1. `__init__`: Constructor
Initializes the linked list:
- `__head` and `__tail` are sentinel nodes with no values (`-1`).
- `__head.next` points to `__tail`, and `__tail.prev` points to `__head`.
- `__size` is initialized to `0` since the list starts empty.

#### 2. `get`: Get the Value of a Node by Index
- If the index is within valid bounds, the function traverses from either the head or the tail, whichever is closer to the target index.
  - **Left half** (`0 <= index <= __size // 2`): Uses `__forward` traversal starting from the `__head`.
  - **Right half** (`__size // 2 < index < __size`): Uses `__backward` traversal starting from the `__tail`.
- Returns the node's value at the target index or `-1` if the index is invalid.

#### 3. `addAtHead`: Insert Node at the Head
- Calls `__add`, passing `__head` as the previous node to insert a new node after `__head`.

#### 4. `addAtTail`: Insert Node at the Tail
- Calls `__add`, passing `__tail.prev` as the previous node to insert a new node before `__tail`.

#### 5. `addAtIndex`: Insert Node at a Specific Index
- Checks if the index is within bounds (`0 <= index <= __size`). If valid:
  - If closer to the **head**, it calls `__forward` to find the insertion point.
  - If closer to the **tail**, it calls `__backward` to find the insertion point.
- Inserts the new node at the specified index.

#### 6. `deleteAtIndex`: Delete Node at a Specific Index
- Checks if the index is valid. If so:
  - Calls `__forward` or `__backward` to locate the node to delete.
  - Calls `__remove` to unlink the node from the list.

#### 7. `__add`: Helper for Node Insertion
- Creates a new `Node` with the given value, inserts it after `preNode`, and updates links to include the new node.

#### 8. `__remove`: Helper for Node Deletion
- Unlinks the specified node from the list by adjusting the `next` pointer of the node’s `prev` and the `prev` pointer of the node’s `next`.

#### 9. `__forward` and `__backward`: Helpers for Traversing the List
- `__forward`: Starts from a given node, traversing forward to reach a specified index.
- `__backward`: Starts from a given node, traversing backward to reach a specified index.

### Example Walkthrough

```python
linked_list = MyLinkedList()
linked_list.addAtHead(1)    # List: 1
linked_list.addAtTail(2)    # List: 1 -> 2
linked_list.addAtIndex(1, 3)  # List: 1 -> 3 -> 2
print(linked_list.get(1))   # Output: 3
linked_list.deleteAtIndex(1)  # List: 1 -> 2
print(linked_list.get(1))   # Output: 2
```

#### Explanation of Steps:
1. `addAtHead(1)`: Creates and inserts a node with value `1` at the head.
2. `addAtTail(2)`: Creates and appends a node with value `2` at the tail.
3. `addAtIndex(1, 3)`: Inserts a node with value `3` at index `1`, between nodes `1` and `2`.
4. `get(1)`: Retrieves the value of the node at index `1`, which is `3`.
5. `deleteAtIndex(1)`: Deletes the node at index `1` (value `3`), leaving the list as `1 -> 2`.
6. `get(1)`: Retrieves the value of the node at index `1`, which is now `2`.

### Complexity Analysis

#### Time Complexity
- **`get(index)`**: __`O(n)`__, where `n` is the size of the list.
  - Traverses either forward or backward depending on index proximity to `head` or `tail`, requiring up to __`O(n)`__ operations in the worst case.
- **`addAtHead(val)`** and **`addAtTail(val)`**: __`O(1)`__.
  - Insertion at the head or tail is done in constant time, involving only link adjustments.
- **`addAtIndex(index, val)`**: __`O(n)`__.
  - Requires traversal to the specified index (up to __`O(n)`__ operations) before inserting the node.
- **`deleteAtIndex(index)`**: __`O(n)`__.
  - Requires traversal to locate the node to delete (up to __`O(n)`__ operations) before unlinking it.

#### Space Complexity
- **Overall**: __`O(n)`__ for storing `n` nodes in the list, each requiring space for `val`, `next`, and `prev`.
- **Auxiliary Space**: __`O(1)`__ as all helper methods operate in-place without additional data structures.

This design leverages doubly linked list advantages for efficient node removal and insertion but incurs __`O(n)`__ time complexity for direct access due to traversal requirements.

---

### Solutions

=== "Python"

    ```python
    # Time:  O(n)
    # Space: O(n)

    from typing import Optional

    class Node:
        def __init__(self, value: int) -> None:
            self.val: int = value
            self.next: Optional[Node] = None
            self.prev: Optional[Node] = None


    class MyLinkedList:
        def __init__(self) -> None:
            """
            Initialize your data structure here.
            """
            self.__head: Node = Node(-1)
            self.__tail: Node = Node(-1)
            self.__head.next = self.__tail
            self.__tail.prev = self.__head
            self.__size: int = 0

        def get(self, index: int) -> int:
            """
            Get the value of the index-th node in the linked list. If the index is invalid, return -1.
            """
            if 0 <= index <= self.__size // 2:
                return self.__forward(0, index, self.__head.next).val
            elif self.__size // 2 < index < self.__size:
                return self.__backward(self.__size, index, self.__tail).val
            return -1

        def addAtHead(self, val: int) -> None:
            """
            Add a node of value val before the first element of the linked list.
            After the insertion, the new node will be the first node of the linked list.
            """
            self.__add(self.__head, val)

        def addAtTail(self, val: int) -> None:
            """
            Append a node of value val to the last element of the linked list.
            """
            self.__add(self.__tail.prev, val)

        def addAtIndex(self, index: int, val: int) -> None:
            """
            Add a node of value val before the index-th node in the linked list.
            If index equals to the length of linked list, the node will be appended to the end of linked list.
            If index is greater than the length, the node will not be inserted.
            """
            if 0 <= index <= self.__size // 2:
                self.__add(self.__forward(0, index, self.__head.next).prev, val)
            elif self.__size // 2 < index <= self.__size:
                self.__add(self.__backward(self.__size, index, self.__tail).prev, val)

        def deleteAtIndex(self, index: int) -> None:
            """
            Delete the index-th node in the linked list, if the index is valid.
            """
            if 0 <= index <= self.__size // 2:
                self.__remove(self.__forward(0, index, self.__head.next))
            elif self.__size // 2 < index < self.__size:
                self.__remove(self.__backward(self.__size, index, self.__tail))

        def __add(self, preNode: Node, val: int) -> None:
            node = Node(val)
            node.prev = preNode
            node.next = preNode.next
            node.prev.next = node.next.prev = node
            self.__size += 1

        def __remove(self, node: Node) -> None:
            node.prev.next = node.next
            node.next.prev = node.prev
            self.__size -= 1

        def __forward(self, start: int, end: int, curr: Node) -> Node:
            while start != end:
                start += 1
                curr = curr.next
            return curr

        def __backward(self, start: int, end: int, curr: Node) -> Node:
            while start != end:
                start -= 1
                curr = curr.prev
            return curr
    ```

=== "C++"

    ```cpp
    #include <iostream>

    class Node {
    public:
        int val;
        Node* next;
        Node* prev;
        Node(int value) : val(value), next(nullptr), prev(nullptr) {}
    };

    class MyLinkedList {
    public:
        MyLinkedList() {
            head = new Node(-1);
            tail = new Node(-1);
            head->next = tail;
            tail->prev = head;
            size = 0;
        }

        int get(int index) {
            if (index < 0 || index >= size) return -1;
            Node* curr = (index < size / 2) ? forward(0, index, head->next) : backward(size, index, tail);
            return curr->val;
        }

        void addAtHead(int val) { add(head, val); }

        void addAtTail(int val) { add(tail->prev, val); }

        void addAtIndex(int index, int val) {
            if (index < 0 || index > size) return;
            Node* preNode = (index <= size / 2) ? forward(0, index, head->next)->prev : backward(size, index, tail)->prev;
            add(preNode, val);
        }

        void deleteAtIndex(int index) {
            if (index < 0 || index >= size) return;
            Node* node = (index < size / 2) ? forward(0, index, head->next) : backward(size, index, tail);
            remove(node);
        }

    private:
        Node* head;
        Node* tail;
        int size;

        void add(Node* preNode, int val) {
            Node* node = new Node(val);
            node->prev = preNode;
            node->next = preNode->next;
            preNode->next->prev = node;
            preNode->next = node;
            ++size;
        }

        void remove(Node* node) {
            node->prev->next = node->next;
            node->next->prev = node->prev;
            delete node;
            --size;
        }

        Node* forward(int start, int end, Node* curr) {
            while (start++ != end) curr = curr->next;
            return curr;
        }

        Node* backward(int start, int end, Node* curr) {
            while (start-- != end) curr = curr->prev;
            return curr;
        }
    };

    int main() {
        MyLinkedList list;
        list.addAtHead(1);
        list.addAtTail(2);
        list.addAtIndex(1, 3);
        std::cout << "Value at index 1: " << list.get(1) << std::endl; // Output: 3
        list.deleteAtIndex(1);
        std::cout << "Value at index 1 after deletion: " << list.get(1) << std::endl; // Output: 2
        return 0;
    }
    ```

=== "Rust"

    ```rust
    use std::rc::Rc;
    use std::cell::RefCell;

    struct Node {
        val: i32,
        next: Option<Rc<RefCell<Node>>>,
        prev: Option<Rc<RefCell<Node>>>,
    }

    impl Node {
        fn new(value: i32) -> Rc<RefCell<Node>> {
            Rc::new(RefCell::new(Node {
                val: value,
                next: None,
                prev: None,
            }))
        }
    }

    struct MyLinkedList {
        head: Rc<RefCell<Node>>,
        tail: Rc<RefCell<Node>>,
        size: usize,
    }

    impl MyLinkedList {
        fn new() -> Self {
            let head = Node::new(-1);
            let tail = Node::new(-1);
            head.borrow_mut().next = Some(Rc::clone(&tail));
            tail.borrow_mut().prev = Some(Rc::clone(&head));
            MyLinkedList { head, tail, size: 0 }
        }

        fn get(&self, index: usize) -> i32 {
            if index >= self.size { return -1; }
            let mut node = if index < self.size / 2 {
                self.head.borrow().next.clone()
            } else {
                self.tail.borrow().prev.clone()
            };
            for _ in 0..index {
                node = node.unwrap().borrow().next.clone();
            }
            node.unwrap().borrow().val
        }

        fn add_at_head(&mut self, val: i32) {
            self.add(&self.head, val);
        }

        fn add_at_tail(&mut self, val: i32) {
            self.add(self.tail.borrow().prev.as_ref().unwrap(), val);
        }

        fn add_at_index(&mut self, index: usize, val: i32) {
            if index > self.size { return; }
            let pre_node = if index <= self.size / 2 {
                self.head.clone()
            } else {
                self.tail.borrow().prev.clone().unwrap()
            };
            self.add(&pre_node, val);
        }

        fn delete_at_index(&mut self, index: usize) {
            if index >= self.size { return; }
            let node = if index < self.size / 2 {
                self.head.borrow().next.clone()
            } else {
                self.tail.borrow().prev.clone()
            };
            self.remove(&node.unwrap());
        }

        fn add(&mut self, pre_node: &Rc<RefCell<Node>>, val: i32) {
            let node = Node::new(val);
            node.borrow_mut().prev = Some(Rc::clone(pre_node));
            pre_node.borrow_mut().next = Some(Rc::clone(&node));
            self.size += 1;
        }

        fn remove(&mut self, node: &Rc<RefCell<Node>>) {
            let prev = node.borrow().prev.clone();
            let next = node.borrow().next.clone();
            prev.unwrap().borrow_mut().next = next.clone();
            next.unwrap().borrow_mut().prev = prev.clone();
            self.size -= 1;
        }
    }

    fn main() {
        let mut list = MyLinkedList::new();
        list.add_at_head(1);
        list.add_at_tail(2);
        list.add_at_index(1, 3);
        println!("Value at index 1: {}", list.get(1)); // Output: 3
        list.delete_at_index(1);
        println!("Value at index 1 after deletion: {}", list.get(1)); // Output: 2
    }
    ```

=== "C#"

    ```csharp
    using System;

    public class Node {
        public int val;
        public Node next, prev;
        public Node(int value) {
            val = value;
            next = prev = null;
        }
    }

    public class MyLinkedList {
        private Node head, tail;
        private int size;

        public MyLinkedList() {
            head = new Node(-1);
            tail = new Node(-1);
            head.next = tail;
            tail.prev = head;
            size = 0;
        }

        public int Get(int index) {
            if (index < 0 || index >= size) return -1;
            Node curr = (index < size / 2) ? Forward(0, index, head.next) : Backward(size, index, tail);
            return curr.val;
        }

        public void AddAtHead(int val) { Add(head, val); }

        public void AddAtTail(int val) { Add(tail.prev, val); }

        public void AddAtIndex(int index, int val) {
            if (index < 0 || index > size) return;
            Node preNode = (index <= size / 2) ? Forward(0, index, head.next).prev : Backward(size, index, tail).prev;
            Add(preNode, val);
        }

        public void DeleteAtIndex(int index) {
            if (index < 0 || index >= size) return;
            Node node = (index < size / 2) ? Forward(0, index, head.next) : Backward(size, index, tail);
            Remove(node);
        }

        private void Add(Node preNode, int val) {
            Node node = new Node(val);
            node.prev = preNode;
            node.next = preNode.next;
            preNode.next.prev = node;
            preNode.next = node;
            size++;
        }

        private void Remove(Node node) {
            node.prev.next = node.next;
            node.next.prev = node.prev;
            size--;
        }

        private Node Forward(int start, int end, Node curr) {
            while (start++ != end) curr = curr.next;
            return curr;
        }

        private Node Backward(int start, int end, Node curr) {
            while (start-- != end) curr = curr.prev;
            return curr;
        }
    }

    public class Program {
        public static void Main() {
            MyLinkedList list = new MyLinkedList();
            list.AddAtHead(1);
            list.AddAtTail(2);
            list.AddAtIndex(1, 3);
            Console.WriteLine("Value at index 1: " + list.Get(1)); // Output: 3
            list.DeleteAtIndex(1);
            Console.WriteLine("Value at index 1 after deletion: " + list.Get(1)); // Output: 2
        }
    }
    ```

=== "Java"

    ```java
    class Node {
        int val;
        Node next, prev;

        Node(int value) {
            this.val = value;
        }
    }

    public class MyLinkedList {
        private Node head, tail;
        private int size;

        public MyLinkedList() {
            head = new Node(-1);
            tail = new Node(-1);
            head.next = tail;
            tail.prev = head;
            size = 0;
        }

        public int get(int index) {
            if (index < 0 || index >= size) return -1;
            Node curr = (index < size / 2) ? forward(0, index, head.next) : backward(size, index, tail);
            return curr.val;
        }

        public void addAtHead(int val) { add(head, val); }

        public void addAtTail(int val) { add(tail.prev, val); }

        public void addAtIndex(int index, int val) {
            if (index < 0 || index > size) return;
            Node preNode = (index <= size / 2) ? forward(0, index, head.next).prev : backward(size, index, tail).prev;
            add(preNode, val);
        }

        public void deleteAtIndex(int index) {
            if (index < 0 || index >= size) return;
            Node node = (index < size / 2) ? forward(0, index, head.next) : backward(size, index, tail);
            remove(node);
        }

        private void add(Node preNode, int val) {
            Node node = new Node(val);
            node.prev = preNode;
            node.next = preNode.next;
            preNode.next.prev = node;
            preNode.next = node;
            size++;
        }

        private void remove(Node node) {
            node.prev.next = node.next;
            node.next.prev = node.prev;
            size--;
        }

        private Node forward(int start, int end, Node curr) {
            while (start++ != end) curr = curr.next;
            return curr;
        }

        private Node backward(int start, int end, Node curr) {
            while (start-- != end) curr = curr.prev;
            return curr;
        }

        public static void main(String[] args) {
            MyLinkedList list = new MyLinkedList();
            list.addAtHead(1);
            list.addAtTail(2);
            list.addAtIndex(1, 3);
            System.out.println("Value at index 1: " + list.get(1)); // Output: 3
            list.deleteAtIndex(1);
            System.out.println("Value at index 1 after deletion: " + list.get(1)); // Output: 2
        }
    }
    ```

=== "Scala"

    ```scala
    class Node(var val: Int, var next: Node = null, var prev: Node = null)

    class MyLinkedList {
        private val head = new Node(-1)
        private val tail = new Node(-1)
        private var size = 0
        head.next = tail
        tail.prev = head

        def get(index: Int): Int = {
            if (index < 0 || index >= size) -1
            else (if (index < size / 2) forward(0, index, head.next) else backward(size, index, tail)).val
        }

        def addAtHead(value: Int): Unit = add(head, value)

        def addAtTail(value: Int): Unit = add(tail.prev, value)

        def addAtIndex(index: Int, value: Int): Unit = {
            if (index >= 0 && index <= size) {
            val preNode = if (index <= size / 2) forward(0, index, head.next).prev else backward(size, index, tail).prev
            add(preNode, value)
            }
        }

        def deleteAtIndex(index: Int): Unit = {
            if (index >= 0 && index < size) {
            val node = if (index < size / 2) forward(0, index, head.next) else backward(size, index, tail)
            remove(node)
            }
        }

        private def add(preNode: Node, value: Int): Unit = {
            val node = new Node(value)
            node.prev = preNode
            node.next = preNode.next
            preNode.next.prev = node
            preNode.next = node
            size += 1
        }

        private def remove(node: Node): Unit = {
            node.prev.next = node.next
            node.next.prev = node.prev
            size -= 1
        }

        private def forward(start: Int, end: Int, curr: Node): Node = {
            var cur = curr
            for (_ <- start until end) cur = cur.next
            cur
        }

        private def backward(start: Int, end: Int, curr: Node): Node = {
            var cur = curr
            for (_ <- end until start) cur = cur.prev
            cur
        }
    }

    object Main {
        def main(args: Array[String]): Unit = {
            val list = new MyLinkedList
            list.addAtHead(1)
            list.addAtTail(2)
            list.addAtIndex(1, 3)
            println(s"Value at index 1: ${list.get(1)}") // Output: 3
            list.deleteAtIndex(1)
            println(s"Value at index 1 after deletion: ${list.get(1)}") // Output: 2
        }
    }
    ```

=== "Kotlin"

    ```kotlin
    class Node(val value: Int) {
        var next: Node? = null
        var prev: Node? = null
    }

    class MyLinkedList {
        private val head = Node(-1)
        private val tail = Node(-1)
        private var size = 0

        init {
            head.next = tail
            tail.prev = head
        }

        fun get(index: Int): Int {
            if (index < 0 || index >= size) return -1
            val node = if (index < size / 2) forward(0, index, head.next) else backward(size, index, tail)
            return node?.value ?: -1
        }

        fun addAtHead(value: Int) = add(head, value)

        fun addAtTail(value: Int) = add(tail.prev!!, value)

        fun addAtIndex(index: Int, value: Int) {
            if (index < 0 || index > size) return
            val preNode = if (index <= size / 2) forward(0, index, head.next)?.prev else backward(size, index, tail)?.prev
            add(preNode!!, value)
        }

        fun deleteAtIndex(index: Int) {
            if (index < 0 || index >= size) return
            val node = if (index < size / 2) forward(0, index, head.next) else backward(size, index, tail)
            node?.let { remove(it) }
        }

        private fun add(preNode: Node, value: Int) {
            val node = Node(value)
            node.prev = preNode
            node.next = preNode.next
            preNode.next?.prev = node
            preNode.next = node
            size++
        }

        private fun remove(node: Node) {
            node.prev?.next = node.next
            node.next?.prev = node.prev
            size--
        }

        private fun forward(start: Int, end: Int, curr: Node?): Node? {
            var cur = curr
            repeat(end - start) { cur = cur?.next }
            return cur
        }

        private fun backward(start: Int, end: Int, curr: Node?): Node? {
            var cur = curr
            repeat(start - end) { cur = cur?.prev }
            return cur
        }
    }

    fun main() {
        val list = MyLinkedList()
        list.addAtHead(1)
        list.addAtTail(2)
        list.addAtIndex(1, 3)
        println("Value at index 1: ${list.get(1)}") // Output: 3
        list.deleteAtIndex(1)
        println("Value at index 1 after deletion: ${list.get(1)}") // Output: 2
    }
    ```

=== "Go"

    ```go
    package main
    import "fmt"

    type Node struct {
        val  int
        next *Node
        prev *Node
    }

    type MyLinkedList struct {
        head *Node
        tail *Node
        size int
    }

    func Constructor() MyLinkedList {
        head := &Node{-1, nil, nil}
        tail := &Node{-1, nil, nil}
        head.next = tail
        tail.prev = head
        return MyLinkedList{head: head, tail: tail, size: 0}
    }

    func (this *MyLinkedList) Get(index int) int {
        if index < 0 || index >= this.size {
            return -1
        }
        var node *Node
        if index < this.size/2 {
            node = this.forward(0, index, this.head.next)
        } else {
            node = this.backward(this.size, index, this.tail)
        }
        return node.val
    }

    func (this *MyLinkedList) AddAtHead(val int) {
        this.add(this.head, val)
    }

    func (this *MyLinkedList) AddAtTail(val int) {
        this.add(this.tail.prev, val)
    }

    func (this *MyLinkedList) AddAtIndex(index, val int) {
        if index < 0 || index > this.size {
            return
        }
        var preNode *Node
        if index <= this.size/2 {
            preNode = this.forward(0, index, this.head.next).prev
        } else {
            preNode = this.backward(this.size, index, this.tail).prev
        }
        this.add(preNode, val)
    }

    func (this *MyLinkedList) DeleteAtIndex(index int) {
        if index < 0 || index >= this.size {
            return
        }
        var node *Node
        if index < this.size/2 {
            node = this.forward(0, index, this.head.next)
        } else {
            node = this.backward(this.size, index, this.tail)
        }
        this.remove(node)
    }

    func (this *MyLinkedList) add(preNode *Node, val int) {
        node := &Node{val: val}
        node.prev = preNode
        node.next = preNode.next
        preNode.next.prev = node
        preNode.next = node
        this.size++
    }

    func (this *MyLinkedList) remove(node *Node) {
        node.prev.next = node.next
        node.next.prev = node.prev
        this.size--
    }

    func (this *MyLinkedList) forward(start, end int, curr *Node) *Node {
        for start < end {
            curr = curr.next
            start++
        }
        return curr
    }

    func (this *MyLinkedList) backward(start, end int, curr *Node) *Node {
        for start > end {
            curr = curr.prev
            start--
        }
        return curr
    }

    func main() {
        list := Constructor()
        list.AddAtHead(1)
        list.AddAtTail(2)
        list.AddAtIndex(1, 3)
        fmt.Println("Value at index 1:", list.Get(1)) // Output: 3
        list.DeleteAtIndex(1)
        fmt.Println("Value at index 1 after deletion:", list.Get(1)) // Output: 2
    }
    ```

=== "TypeScript"

    ```typescript
    class Node {
        val: number;
        next: Node | null = null;
        prev: Node | null = null;

        constructor(value: number) {
            this.val = value;
        }
    }

    class MyLinkedList {
        private head: Node;
        private tail: Node;
        private size: number = 0;

        constructor() {
            this.head = new Node(-1);
            this.tail = new Node(-1);
            this.head.next = this.tail;
            this.tail.prev = this.head;
        }

        get(index: number): number {
            if (index < 0 || index >= this.size) return -1;
            const node = (index < this.size / 2) ? this.forward(0, index, this.head.next) : this.backward(this.size, index, this.tail);
            return node ? node.val : -1;
        }

        addAtHead(value: number): void {
            this.add(this.head, value);
        }

        addAtTail(value: number): void {
            this.add(this.tail.prev as Node, value);
        }

        addAtIndex(index: number, value: number): void {
            if (index < 0 || index > this.size) return;
            const preNode = (index <= this.size / 2) ? this.forward(0, index, this.head.next)?.prev : this.backward(this.size, index, this.tail)?.prev;
            if (preNode) this.add(preNode, value);
        }

        deleteAtIndex(index: number): void {
            if (index < 0 || index >= this.size) return;
            const node = (index < this.size / 2) ? this.forward(0, index, this.head.next) : this.backward(this.size, index, this.tail);
            if (node) this.remove(node);
        }

        private add(preNode: Node, value: number): void {
            const node = new Node(value);
            node.prev = preNode;
            node.next = preNode.next;
            preNode.next!.prev = node;
            preNode.next = node;
            this.size++;
        }

        private remove(node: Node): void {
            node.prev!.next = node.next;
            node.next!.prev = node.prev;
            this.size--;
        }

        private forward(start: number, end: number, curr: Node | null): Node | null {
            while (start++ !== end) curr = curr!.next;
            return curr;
        }

        private backward(start: number, end: number, curr: Node | null): Node | null {
            while (start-- !== end) curr = curr!.prev;
            return curr;
        }
    }

    // Testing
    const list = new MyLinkedList();
    list.addAtHead(1);
    list.addAtTail(2);
    list.addAtIndex(1, 3);
    console.log("Value at index 1:", list.get(1)); // Output: 3
    list.deleteAtIndex(1);
    console.log("Value at index 1 after deletion:", list.get(1)); // Output: 2
    ```

=== "R"

    ```r
    Node <- setRefClass("Node",
                        fields = list(
                        val = "numeric",
                        next = "Node",
                        prev = "Node"
                        ))

    MyLinkedList <- setRefClass("MyLinkedList",
                                fields = list(
                                head = "Node",
                                tail = "Node",
                                size = "numeric"
                                ),
                                methods = list(
                                initialize = function() {
                                    head <<- Node$new(val = -1)
                                    tail <<- Node$new(val = -1)
                                    head$next <- tail
                                    tail$prev <- head
                                    size <<- 0
                                },
                                get = function(index) {
                                    if (index < 0 || index >= size) return(-1)
                                    node <- if (index < size / 2) .self$forward(0, index, head$next) else .self$backward(size, index, tail)
                                    node$val
                                },
                                addAtHead = function(val) {
                                    add(head, val)
                                },
                                addAtTail = function(val) {
                                    add(tail$prev, val)
                                },
                                addAtIndex = function(index, val) {
                                    if (index < 0 || index > size) return(NULL)
                                    preNode <- if (index <= size / 2) .self$forward(0, index, head$next)$prev else .self$backward(size, index, tail)$prev
                                    add(preNode, val)
                                },
                                deleteAtIndex = function(index) {
                                    if (index < 0 || index >= size) return(NULL)
                                    node <- if (index < size / 2) .self$forward(0, index, head$next) else .self$backward(size, index, tail)
                                    remove(node)
                                },
                                add = function(preNode, val) {
                                    node <- Node$new(val = val)
                                    node$prev <- preNode
                                    node$next <- preNode$next
                                    preNode$next$prev <- node
                                    preNode$next <- node
                                    size <<- size + 1
                                },
                                remove = function(node) {
                                    node$prev$next <- node$next
                                    node$next$prev <- node$prev
                                    size <<- size - 1
                                },
                                forward = function(start, end, curr) {
                                    while (start < end) {
                                    start <- start + 1
                                    curr <- curr$next
                                    }
                                    curr
                                },
                                backward = function(start, end, curr) {
                                    while (start > end) {
                                    start <- start - 1
                                    curr <- curr$prev
                                    }
                                    curr
                                }
                                ))

    # Testing
    list <- MyLinkedList$new()
    list$addAtHead(1)
    list$addAtTail(2)
    list$addAtIndex(1, 3)
    print(paste("Value at index 1:", list$get(1))) # Output: 3
    list$deleteAtIndex(1)
    print(paste("Value at index 1 after deletion:", list$get(1))) # Output: 2
    ```

=== "Julia"

    ```julia
    module MyLinkedListModule

    mutable struct Node
        val::Int
        next::Union{Node, Nothing}
        prev::Union{Node, Nothing}

        Node(val::Int) = new(val, nothing, nothing)
    end

    mutable struct MyLinkedList
        head::Node
        tail::Node
        size::Int

        MyLinkedList() = begin
            head = Node(-1)
            tail = Node(-1)
            head.next = tail
            tail.prev = head
            new(head, tail, 0)
        end
    end

    function get(list::MyLinkedList, index::Int)::Int
        if index < 0 || index >= list.size
            return -1
        end
        node = index < div(list.size, 2) ? forward(0, index, list.head.next) : backward(list.size, index, list.tail)
        return node.val
    end

    function add_at_head!(list::MyLinkedList, val::Int)
        add!(list, list.head, val)
    end

    function add_at_tail!(list::MyLinkedList, val::Int)
        add!(list, list.tail.prev, val)
    end

    function add_at_index!(list::MyLinkedList, index::Int, val::Int)
        if index < 0 || index > list.size
            return
        end
        preNode = index <= div(list.size, 2) ? forward(0, index, list.head.next).prev : backward(list.size, index, list.tail).prev
        add!(list, preNode, val)
    end

    function delete_at_index!(list::MyLinkedList, index::Int)
        if index < 0 || index >= list.size
            return
        end
        node = index < div(list.size, 2) ? forward(0, index, list.head.next) : backward(list.size, index, list.tail)
        remove!(list, node)
    end

    function add!(list::MyLinkedList, preNode::Node, val::Int)
        node = Node(val)
        node.prev = preNode
        node.next = preNode.next
        preNode.next.prev = node
        preNode.next = node
        list.size += 1
    end

    function remove!(list::MyLinkedList, node::Node)
        node.prev.next = node.next
        node.next.prev = node.prev
        list.size -= 1
    end

    function forward(start::Int, end::Int, curr::Node)::Node
        while start < end
            curr = curr.next
            start += 1
        end
        return curr
    end

    function backward(start::Int, end::Int, curr::Node)::Node
        while start > end
            curr = curr.prev
            start -= 1
        end
        return curr
    end

    # Testing
    list = MyLinkedList()
    add_at_head!(list, 1)
    add_at_tail!(list, 2)
    add_at_index!(list, 1, 3)
    println("Value at index 1:", get(list, 1)) # Output: 3
    delete_at_index!(list, 1)
    println("Value at index 1 after deletion:", get(list, 1)) # Output: 2

    end # module
    ```

## Reverse Linked List

### Problem Description: [LeetCode - Problem 206 - Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

The problem is to reverse a singly linked list. Given a linked list, the goal is to reverse the links between the nodes such that the last node becomes the first node and the first node becomes the last.

The code provided has two solutions:
1. **Iterative approach (`reverseIter`)**
2. **Recursive approach (`reverseRecur`)**

#### **1. Iterative Approach (`reverseIter`)**

The iterative approach reverses the linked list by changing the `next` pointer of each node to point to the previous node.

##### Steps:
- Initialize three pointers: `prev` (which will track the previous node), `curr` (which points to the current node being processed), and `newHead` (which tracks the reversed list's head).
- Loop through the linked list:
  1. Store the next node in a temporary variable `curr.next`.
  2. Reverse the link by pointing the current node (`curr.next`) to the previous node (`prev`).
  3. Move the `prev` and `curr` pointers forward (to the current node and the next node, respectively).
- Continue this until you reach the end of the list.
- Finally, return the `newHead` as the new head of the reversed list.

#### **2. Recursive Approach (`reverseRecur`)**

In the recursive approach, you break down the list into two parts: the head and the rest of the list. You recursively reverse the rest of the list and then link the current node (`head`) to the reversed list.

##### Steps:
1. If the list is empty or contains only one node, return the head (base case).
2. Otherwise, call the recursive function for the rest of the list (`head.next`).
3. After the recursive call, reverse the link between `head` and `head.next`.
4. Finally, return the new head of the reversed list (the return value of the recursive call).

### Example Run for Iterative Solution (`reverseIter`)

#### Input:
```
1 -> 2 -> 3 -> 4 -> 5 -> NULL
```

#### Execution:

- Initially: 
  - `prev = None`, `curr = 1`, `newHead = None`
- First iteration:
  - Reverse link: `curr.next = prev` => `1.next = None`
  - Move forward: `prev = 1`, `curr = 2`, `newHead = 1`
- Second iteration:
  - Reverse link: `2.next = 1`
  - Move forward: `prev = 2`, `curr = 3`, `newHead = 2`
- And so on until:
  
#### Output:
```
5 -> 4 -> 3 -> 2 -> 1 -> NULL
```

### Example Run for Recursive Solution (`reverseRecur`)

#### Input:
```
1 -> 2 -> 3 -> 4 -> 5 -> NULL
```

#### Execution:

- Break the list recursively:
  - Call for `2 -> 3 -> 4 -> 5 -> NULL`
  - Call for `3 -> 4 -> 5 -> NULL`
  - Call for `4 -> 5 -> NULL`
  - Call for `5 -> NULL` (base case, return `5`)
  
- On returning, reverse the links:
  - Set `4.next.next = 4` and `4.next = NULL`
  - Set `3.next.next = 3` and `3.next = NULL`
  - And so on.

#### Output:
```
5 -> 4 -> 3 -> 2 -> 1 -> NULL
```

Both approaches correctly reverse the linked list, but the iterative solution is more space-efficient than the recursive one.

### Complexity Analysis

#### **1. Iterative Approach (`reverseIter`)**

- **Time Complexity**: 
  - The iterative approach processes each node exactly once in a single pass through the list. Therefore, the time complexity is **O(n)**, where `n` is the number of nodes in the linked list.

- **Space Complexity**: 
  - The iterative approach uses only a constant amount of extra space, primarily for the three pointers (`prev`, `curr`, and `newHead`). Hence, the space complexity is **O(1)**.

#### **2. Recursive Approach (`reverseRecur`)**

- **Time Complexity**: 
  - Each recursive call processes one node, and since there are `n` nodes in the list, the time complexity is **O(n)**.

- **Space Complexity**:
  - The recursive approach has a space complexity of **O(n)** because each recursive call adds a new frame to the call stack, and there are `n` recursive calls for a list of `n` nodes.

### Solutions

=== "Python"

    ```python
    from typing import Optional

    # Definition for singly-linked list.
    class ListNode:
        def __init__(self, val=0, next=None) -> None:
            self.val = val
            self.next = next

    def push(head: Optional[ListNode], data: int) -> Optional[ListNode]:
        new_node = ListNode(data)
        if head is None:
            return new_node
        else:
            curr = head
            while curr.next is not None:
                curr = curr.next
            curr.next = new_node
            return head

    def print_list(head: Optional[ListNode]) -> None:
        while head:
            print(f"{head.data}-->", end="")
            head = head.next
        print("NULL")

    class Solution:
        def reverseIter(self, head: Optional[ListNode]) -> Optional[ListNode]:
            if not head or not head.next:
                return head
            newHead = None
            prev = None
            curr = head

            while curr:
                prev = curr
                curr = curr.next
                prev.next = newHead
                newHead = prev
            
            return prev

        def reverseRecur(self, head: Optional[ListNode]) -> Optional[ListNode]:
            if not head or not head.next:
                return head
            
            rest = self.reverseRecur(head.next)
            head.next.next = head
            head.next = None
            
            return rest

    if __name__ == "__main__":
        head: Optional[ListNode] = None
        head = push(head, 1)
        head = push(head, 2)
        head = push(head, 3)
        head = push(head, 4)
        head = push(head, 5)
        
        print("Before Reversing LinkedList (Iterative):\n")
        print_list(head)
        head = Solution().reverseIter(head)    
        print("After Reversing LinkedList (Iterative):\n")
        print_list(head)

        head = None
        head = push(head, 1)
        head = push(head, 2)
        head = push(head, 3)
        head = push(head, 4)
        head = push(head, 5)

        print("Before Reversing LinkedList (Recursive):\n")
        print_list(head)
        head = Solution().reverseRecur(head)    
        print("After Reversing LinkedList (Recursive):\n")
        print_list(head)
    ```

=== "C++"

    ```cpp
    #include <iostream>
    #include <optional>

    struct ListNode {
        int val;
        ListNode *next;
        ListNode() : val(0), next(nullptr) {}
        ListNode(int x) : val(x), next(nullptr) {}
        ListNode(int x, ListNode *next) : val(x), next(next) {}
    };

    ListNode* push(ListNode* head, int data) {
        ListNode* new_node = new ListNode(data);
        if (head == nullptr) {
            return new_node;
        } else {
            ListNode* curr = head;
            while (curr->next.has_value()) {
                curr = curr->next.value();
            }
            curr->next = new_node;
            return head;
        }
    }

    void print_list(ListNode* head) {
        while (head != nullptr) {
            std::cout << head->data << "--> ";
            head = head->next.value_or(nullptr);
        }
        std::cout << "NULL" << std::endl;
    }

    class Solution {
    public:
        ListNode* reverseIter(ListNode* head) {
            if (!head || !head->next) {
                return head;
            }
            ListNode* newHead = nullptr;
            ListNode* prev = nullptr;
            ListNode* curr = head;

            while (curr) {
                prev = curr;
                curr = curr->next.value_or(nullptr);
                prev->next = newHead;
                newHead = prev;
            }

            return prev;
        }

        ListNode* reverseRecur(ListNode* head) {
            if (!head || !head->next) {
                return head;
            }

            ListNode* rest = reverseRecur(head->next.value());
            head->next.value()->next = head;
            head->next = nullptr;

            return rest;
        }
    };

    int main() {
        ListNode* head = nullptr;
        head = push(head, 1);
        head = push(head, 2);
        head = push(head, 3);
        head = push(head, 4);
        head = push(head, 5);

        std::cout << "Before Reversing LinkedList (Iterative):\n";
        print_list(head);
        head = Solution().reverseIter(head);
        std::cout << "After Reversing LinkedList (Iterative):\n";
        print_list(head);

        head = nullptr;
        head = push(head, 1);
        head = push(head, 2);
        head = push(head, 3);
        head = push(head, 4);
        head = push(head, 5);

        std::cout << "Before Reversing LinkedList (Recursive):\n";
        print_list(head);
        head = Solution().reverseRecur(head);
        std::cout << "After Reversing LinkedList (Recursive):\n";
        print_list(head);

        return 0;
    }
    ```

=== "Rust"

    ```rust
    #[derive(Debug)]
    struct ListNode {
        val: i32,
        next: Option<Box<ListNode>>,
    }

    impl ListNode {
        fn new(val: i32) -> Self {
            ListNode { val, next: None }
        }
    }

    fn push(head: Option<Box<ListNode>>, data: i32) -> Option<Box<ListNode>> {
        let mut new_node = Box::new(ListNode::new(data));
        match head {
            None => Some(new_node),
            Some(mut node) => {
                let mut current = &mut node;
                while let Some(ref mut next) = current.next {
                    current = next;
                }
                current.next = Some(new_node);
                Some(node)
            }
        }
    }

    fn print_list(head: Option<Box<ListNode>>) {
        let mut current = &head;
        while let Some(node) = current {
            print!("{} --> ", node.val);
            current = &node.next;
        }
        println!("NULL");
    }

    struct Solution;

    impl Solution {
        fn reverse_iter(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
            let mut prev = None;
            let mut curr = head;
            while let Some(mut node) = curr {
                let next = node.next.take();
                node.next = prev;
                prev = Some(node);
                curr = next;
            }
            prev
        }

        fn reverse_recur(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
            fn helper(node: Option<Box<ListNode>>, prev: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
                match node {
                    None => prev,
                    Some(mut n) => {
                        let next = n.next.take();
                        n.next = prev;
                        helper(next, Some(n))
                    }
                }
            }
            helper(head, None)
        }
    }

    fn main() {
        let mut head = None;
        head = push(head, 1);
        head = push(head, 2);
        head = push(head, 3);
        head = push(head, 4);
        head = push(head, 5);

        println!("Before Reversing LinkedList (Iterative):");
        print_list(head.clone());
        head = Solution::reverse_iter(head);
        println!("After Reversing LinkedList (Iterative):");
        print_list(head.clone());

        head = None;
        head = push(head, 1);
        head = push(head, 2);
        head = push(head, 3);
        head = push(head, 4);
        head = push(head, 5);

        println!("Before Reversing LinkedList (Recursive):");
        print_list(head.clone());
        head = Solution::reverse_recur(head);
        println!("After Reversing LinkedList (Recursive):");
        print_list(head);
    }
    ```

=== "C#"

    ```csharp
    using System;

    public class ListNode {
        public int val;
        public ListNode next;
        public ListNode(int x) { val = x; }
    }

    public class Solution {
        public static ListNode Push(ListNode head, int data) {
            var newNode = new ListNode(data);
            if (head == null) return newNode;
            
            var current = head;
            while (current.next != null) current = current.next;
            current.next = newNode;
            return head;
        }

        public static void PrintList(ListNode head) {
            while (head != null) {
                Console.Write($"{head.val} --> ");
                head = head.next;
            }
            Console.WriteLine("NULL");
        }

        public ListNode ReverseIter(ListNode head) {
            ListNode prev = null;
            while (head != null) {
                var nextNode = head.next;
                head.next = prev;
                prev = head;
                head = nextNode;
            }
            return prev;
        }

        public ListNode ReverseRecur(ListNode head) {
            if (head == null || head.next == null) return head;
            var newHead = ReverseRecur(head.next);
            head.next.next = head;
            head.next = null;
            return newHead;
        }
    }

    public class Program {
        public static void Main() {
            ListNode head = null;
            head = Solution.Push(head, 1);
            head = Solution.Push(head, 2);
            head = Solution.Push(head, 3);
            head = Solution.Push(head, 4);
            head = Solution.Push(head, 5);

            Console.WriteLine("Before Reversing LinkedList (Iterative):");
            Solution.PrintList(head);
            head = new Solution().ReverseIter(head);
            Console.WriteLine("After Reversing LinkedList (Iterative):");
            Solution.PrintList(head);

            head = null;
            head = Solution.Push(head, 1);
            head = Solution.Push(head, 2);
            head = Solution.Push(head, 3);
            head = Solution.Push(head, 4);
            head = Solution.Push(head, 5);

            Console.WriteLine("Before Reversing LinkedList (Recursive):");
            Solution.PrintList(head);
            head = new Solution().ReverseRecur(head);
            Console.WriteLine("After Reversing LinkedList (Recursive):");
            Solution.PrintList(head);
        }
    }
    ```

=== "Java"

    ```java
    class ListNode {
        int val;
        ListNode next;
        ListNode(int val) { this.val = val; }
    }

    class Solution {
        public static ListNode push(ListNode head, int data) {
            ListNode newNode = new ListNode(data);
            if (head == null) return newNode;
            ListNode curr = head;
            while (curr.next != null) curr = curr.next;
            curr.next = newNode;
            return head;
        }

        public static void printList(ListNode head) {
            while (head != null) {
                System.out.print(head.val + " --> ");
                head = head.next;
            }
            System.out.println("NULL");
        }

        public ListNode reverseIter(ListNode head) {
            ListNode prev = null;
            while (head != null) {
                ListNode nextNode = head.next;
                head.next = prev;
                prev = head;
                head = nextNode;
            }
            return prev;
        }

        public ListNode reverseRecur(ListNode head) {
            if (head == null || head.next == null) return head;
            ListNode rest = reverseRecur(head.next);
            head.next.next = head;
            head.next = null;
            return rest;
        }

        public static void main(String[] args) {
            ListNode head = null;
            head = Solution.push(head, 1);
            head = Solution.push(head, 2);
            head = Solution.push(head, 3);
            head = Solution.push(head, 4);
            head = Solution.push(head, 5);

            System.out.println("Before Reversing LinkedList (Iterative):");
            Solution.printList(head);
            head = new Solution().reverseIter(head);
            System.out.println("After Reversing LinkedList (Iterative):");
            Solution.printList(head);

            head = null;
            head = Solution.push(head, 1);
            head = Solution.push(head, 2);
            head = Solution.push(head, 3);
            head = Solution.push(head, 4);
            head = Solution.push(head, 5);

            System.out.println("Before Reversing LinkedList (Recursive):");
            Solution.printList(head);
            head = new Solution().reverseRecur(head);
            System.out.println("After Reversing LinkedList (Recursive):");
            Solution.printList(head);
        }
    }
    ```

=== "Scala"

    ```scala
    class ListNode(var value: Int) {
        var next: Option[ListNode] = None
    }

    object Solution {
        def push(head: Option[ListNode], data: Int): Option[ListNode] = {
            val newNode = Some(new ListNode(data))
            head match {
                case None => newNode
                case Some(node) =>
                    var current = node
                    while (current.next.isDefined) current = current.next.get
                    current.next = newNode
                    head
            }
        }

        def printList(head: Option[ListNode]): Unit = {
            var current = head
            while (current.isDefined) {
                print(s"${current.get.value} --> ")
                current = current.get.next
            }
            println("NULL")
        }

        def reverseIter(head: Option[ListNode]): Option[ListNode] = {
            var prev: Option[ListNode] = None
            var curr = head
            while (curr.isDefined) {
                val next = curr.get.next
                curr.get.next = prev
                prev = curr
                curr = next
            }
            prev
        }

        def reverseRecur(head: Option[ListNode]): Option[ListNode] = {
            if (head.isEmpty || head.get.next.isEmpty) head
            else {
                val rest = reverseRecur(head.get.next)
                head.get.next.get.next = head.get
                head.get.next = None
                rest
            }
        }
    }

    object Main extends App {
        var head: Option[ListNode] = None
        head = Solution.push(head, 1)
        head = Solution.push(head, 2)
        head = Solution.push(head, 3)
        head = Solution.push(head, 4)
        head = Solution.push(head, 5)

        println("Before Reversing LinkedList (Iterative):")
        Solution.printList(head)
        head = Solution.reverseIter(head)
        println("After Reversing LinkedList (Iterative):")
        Solution.printList(head)

        head = None
        head = Solution.push(head, 1)
        head = Solution.push(head, 2)
        head = Solution.push(head, 3)
        head = Solution.push(head, 4)
        head = Solution.push(head, 5)

        println("Before Reversing LinkedList (Recursive):")
        Solution.printList(head)
        head = Solution.reverseRecur(head)
        println("After Reversing LinkedList (Recursive):")
        Solution.printList(head)
    }
    ```

=== "Kotlin"

    ```kotlin
    class ListNode(var value: Int) {
        var next: ListNode? = null
    }

    object Solution {
        fun push(head: ListNode?, data: Int): ListNode? {
            val newNode = ListNode(data)
            if (head == null) return newNode
            var current = head
            while (current.next != null) {
                current = current.next!!
            }
            current.next = newNode
            return head
        }

        fun printList(head: ListNode?) {
            var current = head
            while (current != null) {
                print("${current.value} --> ")
                current = current.next
            }
            println("NULL")
        }

        fun reverseIter(head: ListNode?): ListNode? {
            var prev: ListNode? = null
            var curr = head
            while (curr != null) {
                val next = curr.next
                curr.next = prev
                prev = curr
                curr = next
            }
            return prev
        }

        fun reverseRecur(head: ListNode?): ListNode? {
            if (head?.next == null) return head
            val rest = reverseRecur(head.next)
            head.next!!.next = head
            head.next = null
            return rest
        }
    }

    fun main() {
        var head: ListNode? = null
        head = Solution.push(head, 1)
        head = Solution.push(head, 2)
        head = Solution.push(head, 3)
        head = Solution.push(head, 4)
        head = Solution.push(head, 5)

        println("Before Reversing LinkedList (Iterative):")
        Solution.printList(head)
        head = Solution.reverseIter(head)
        println("After Reversing LinkedList (Iterative):")
        Solution.printList(head)

        head = null
        head = Solution.push(head, 1)
        head = Solution.push(head, 2)
        head = Solution.push(head, 3)
        head = Solution.push(head, 4)
        head = Solution.push(head, 5)

        println("Before Reversing LinkedList (Recursive):")
        Solution.printList(head)
        head = Solution.reverseRecur(head)
        println("After Reversing LinkedList (Recursive):")
        Solution.printList(head)
    }
    ```

=== "Go"

    ```go
    package main

    import "fmt"

    type ListNode struct {
        Val  int
        Next *ListNode
    }

    func push(head *ListNode, data int) *ListNode {
        newNode := &ListNode{Val: data}
        if head == nil {
            return newNode
        }
        current := head
        for current.Next != nil {
            current = current.Next
        }
        current.Next = newNode
        return head
    }

    func printList(head *ListNode) {
        for head != nil {
            fmt.Printf("%d --> ", head.Val)
            head = head.Next
        }
        fmt.Println("NULL")
    }

    func reverseIter(head *ListNode) *ListNode {
        var prev *ListNode
        current := head
        for current != nil {
            next := current.Next
            current.Next = prev
            prev = current
            current = next
        }
        return prev
    }

    func reverseRecur(head *ListNode) *ListNode {
        if head == nil || head.Next == nil {
            return head
        }
        rest := reverseRecur(head.Next)
        head.Next.Next = head
        head.Next = nil
        return rest
    }

    func main() {
        var head *ListNode
        head = push(head, 1)
        head = push(head, 2)
        head = push(head, 3)
        head = push(head, 4)
        head = push(head, 5)

        fmt.Println("Before Reversing LinkedList (Iterative):")
        printList(head)
        head = reverseIter(head)
        fmt.Println("After Reversing LinkedList (Iterative):")
        printList(head)

        head = nil
        head = push(head, 1)
        head = push(head, 2)
        head = push(head, 3)
        head = push(head, 4)
        head = push(head, 5)

        fmt.Println("Before Reversing LinkedList (Recursive):")
        printList(head)
        head = reverseRecur(head)
        fmt.Println("After Reversing LinkedList (Recursive):")
        printList(head)
    }
    ```

=== "TypeScript"

    ```typescript
    class ListNode {
        val: number;
        next: ListNode | null = null;

        constructor(val: number) {
            this.val = val;
        }
    }

    function push(head: ListNode | null, data: number): ListNode | null {
        const newNode = new ListNode(data);
        if (head === null) return newNode;

        let current = head;
        while (current.next !== null) current = current.next;
        current.next = newNode;
        return head;
    }

    function printList(head: ListNode | null): void {
        while (head !== null) {
            process.stdout.write(`${head.val} --> `);
            head = head.next;
        }
        console.log("NULL");
    }

    function reverseIter(head: ListNode | null): ListNode | null {
        let prev: ListNode | null = null;
        let curr = head;
        while (curr !== null) {
            const nextNode = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextNode;
        }
        return prev;
    }

    function reverseRecur(head: ListNode | null): ListNode | null {
        if (head === null || head.next === null) return head;
        const rest = reverseRecur(head.next);
        head.next.next = head;
        head.next = null;
        return rest;
    }

    let head: ListNode | null = null;
    head = push(head, 1);
    head = push(head, 2);
    head = push(head, 3);
    head = push(head, 4);
    head = push(head, 5);

    console.log("Before Reversing LinkedList (Iterative):");
    printList(head);
    head = reverseIter(head);
    console.log("After Reversing LinkedList (Iterative):");
    printList(head);

    head = null;
    head = push(head, 1);
    head = push(head, 2);
    head = push(head, 3);
    head = push(head, 4);
    head = push(head, 5);

    console.log("Before Reversing LinkedList (Recursive):");
    printList(head);
    head = reverseRecur(head);
    console.log("After Reversing LinkedList (Recursive):");
    printList(head);
    ```

=== "R"

    ```r
    # Define a ListNode class
    ListNode <- setRefClass("ListNode",
        fields = list(
            val = "numeric",
            next = "ListNode"
        )
    )

    # Function to push a new node at the end
    push <- function(head, data) {
        newNode <- ListNode$new(val = data)
        if (is.null(head)) {
            return(newNode)
        }
        
        current <- head
        while (!is.null(current$next)) {
            current <- current$next
        }
        current$next <- newNode
        return(head)
    }

    # Function to print the list
    printList <- function(head) {
        current <- head
        while (!is.null(current)) {
            cat(current$val, "--> ")
            current <- current$next
        }
        cat("NULL\n")
    }

    # Iterative reversal function
    reverseIter <- function(head) {
        prev <- NULL
        current <- head
        while (!is.null(current)) {
            nextNode <- current$next
            current$next <- prev
            prev <- current
            current <- nextNode
        }
        return(prev)
    }

    # Recursive reversal function
    reverseRecur <- function(head) {
        if (is.null(head) || is.null(head$next)) {
            return(head)
        }
        
        rest <- reverseRecur(head$next)
        head$next$next <- head
        head$next <- NULL
        return(rest)
    }

    # Test the functions
    head <- NULL
    head <- push(head, 1)
    head <- push(head, 2)
    head <- push(head, 3)
    head <- push(head, 4)
    head <- push(head, 5)

    cat("Before Reversing LinkedList (Iterative):\n")
    printList(head)
    head <- reverseIter(head)
    cat("After Reversing LinkedList (Iterative):\n")
    printList(head)

    # Reset the list
    head <- NULL
    head <- push(head, 1)
    head <- push(head, 2)
    head <- push(head, 3)
    head <- push(head, 4)
    head <- push(head, 5)

    cat("Before Reversing LinkedList (Recursive):\n")
    printList(head)
    head <- reverseRecur(head)
    cat("After Reversing LinkedList (Recursive):\n")
    printList(head)
    ```

=== "Julia"

    ```julia
    struct ListNode
        val::Int
        next::Union{ListNode, Nothing}
        ListNode(val) = new(val, nothing)
    end

    function push(head::Union{ListNode, Nothing}, data::Int)
        new_node = ListNode(data)
        if head === nothing
            return new_node
        end
        current = head
        while current.next !== nothing
            current = current.next
        end
        current.next = new_node
        return head
    end

    function print_list(head::Union{ListNode, Nothing})
        while head !== nothing
            print("$(head.val) --> ")
            head = head.next
        end
        println("NULL")
    end

    function reverse_iter(head::Union{ListNode, Nothing})
        prev = nothing
        current = head
        while current !== nothing
            next_node = current.next
            current.next = prev
            prev = current
            current = next_node
        end
        return prev
    end

    function reverse_recur(head::Union{ListNode, Nothing})
        if head === nothing || head.next === nothing
            return head
        end
        rest = reverse_recur(head.next)
        head.next.next = head
        head.next = nothing
        return rest
    end

    # Test code
    head = nothing
    head = push(head, 1)
    head = push(head, 2)
    head = push(head, 3)
    head = push(head, 4)
    head = push(head, 5)

    println("Before Reversing LinkedList (Iterative):")
    print_list(head)
    head = reverse_iter(head)
    println("After Reversing LinkedList (Iterative):")
    print_list(head)

    head = nothing
    head = push(head, 1)
    head = push(head, 2)
    head = push(head, 3)
    head = push(head, 4)
    head = push(head, 5)

    println("Before Reversing LinkedList (Recursive):")
    print_list(head)
    head = reverse_recur(head)
    println("After Reversing LinkedList (Recursive):")
    print_list(head)
    ```

## Merge Two Sorted Linked Lists

### Problem Description: [LeetCode - Problem 21 - Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/description/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

- For simplicity, we create a dummy node to which we attach nodes from lists.
- We iterate over lists using two-pointers
- And, then build up a resulting list so that values are monotonically increased.

#### Detailed Explanation
- The `Solution` class contains a method `mergeTwoLists` that merges two sorted singly-linked lists, `list1` and `list2`, into a single sorted list. It uses a pointer `cur` to iterate over the lists and create the merged list.
  
- The method iterates through both lists, comparing the values of nodes at the same position and linking them in sorted order to build the merged list. Once one of the lists has been fully merged, any remaining elements from the other list are appended to the merged list.

- Finally, the method returns the head of the merged list.

### Complexity Analysis
- Let m be the number of nodes in `list1` and n be the number of nodes in `list2`.
  
- __Time Complexity:__
  - The merging process iterates through both lists once in a single pass until either `list1` or `list2` is exhausted.
  - Therefore, the time complexity is __`O(m + n)`__ where m and n are the lengths of `list1` and `list2` respectively.

- __Space Complexity:__
  - Additional memory is only used for the `cur` pointer and the new merged linked list.
  - Since the new nodes are created in-place and the space usage is mainly for pointers, the space complexity is ____`O(1)`____, constant space complexity.

In summary, the code efficiently merges two sorted singly-linked lists into a single sorted list __with a time complexity of `O(m + n)` and a space complexity of __`O(1)`__.__

### Solutions

=== "Python"

    ```python
    from typing import Optional

    # Definition for singly-linked list.
    class ListNode:
        def __init__(self, val=0, next=None) -> None:
            self.val = val
            self.next = next


    class Solution:
        def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
            cur = dummy = ListNode()
            while list1 and list2:               
                if list1.val < list2.val:
                    cur.next = list1
                    list1, cur = list1.next, list1
                else:
                    cur.next = list2
                    list2, cur = list2.next, list2
                    
            if list1 or list2:
                cur.next = list1 if list1 else list2
                
            return dummy.next
    ```

=== "C++"

    ```cpp
    #include <iostream>
    #include <optional>

    struct ListNode {
        int val;
        ListNode *next;
        ListNode() : val(0), next(nullptr) {}
        ListNode(int x) : val(x), next(nullptr) {}
        ListNode(int x, ListNode *next) : val(x), next(next) {}
    };

    class Solution {
    public:
        ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
            ListNode* cur = new ListNode();
            ListNode* dummy = cur;

            while (list1 && list2) {
                if (list1->val < list2->val) {
                    cur->next = list1;
                    list1 = list1->next;
                } else {
                    cur->next = list2;
                    list2 = list2->next;
                }
                cur = cur->next;
            }

            cur->next = list1 ? list1 : list2;

            return dummy->next;
        }
    };

    int main() {
        ListNode* list1 = new ListNode(1);
        list1->next = new ListNode(3);
        list1->next->next = new ListNode(5);

        ListNode* list2 = new ListNode(2);
        list2->next = new ListNode(4);
        list2->next->next = new ListNode(6);

        Solution solution;
        ListNode* mergedList = solution.mergeTwoLists(list1, list2);

        while (mergedList != nullptr) {
            std::cout << mergedList->val << " ";
            mergedList = mergedList->next;
        }
        std::cout << std::endl;

        return 0;
    }
    ```

=== "Rust"

    ```rust
    #[derive(PartialEq, Eq, Clone, Debug)]
    pub struct ListNode {
        pub val: i32,
        pub next: Option<Box<ListNode>>,
    }

    impl ListNode {
        fn new(val: i32) -> Self {
            ListNode { val, next: None }
        }
    }

    pub struct Solution;

    impl Solution {
        pub fn merge_two_lists(
            list1: Option<Box<ListNode>>,
            list2: Option<Box<ListNode>>,
        ) -> Option<Box<ListNode>> {
            let mut dummy = Box::new(ListNode::new(0));
            let mut current = &mut dummy;

            let (mut l1, mut l2) = (list1, list2);
            while let (Some(mut node1), Some(mut node2)) = (l1, l2) {
                if node1.val < node2.val {
                    l1 = node1.next.take();
                    current.next = Some(node1);
                } else {
                    l2 = node2.next.take();
                    current.next = Some(node2);
                }
                current = current.next.as_mut().unwrap();
            }
            current.next = l1.or(l2);
            dummy.next
        }
    }

    fn main() {
        let list1 = Some(Box::new(ListNode { val: 1, next: Some(Box::new(ListNode::new(3))) }));
        let list2 = Some(Box::new(ListNode { val: 2, next: Some(Box::new(ListNode::new(4))) }));
        
        let merged = Solution::merge_two_lists(list1, list2);
        println!("{:?}", merged);
    }
    ```

=== "C#"

    ```csharp
    public class ListNode {
        public int val;
        public ListNode next;
        public ListNode(int x) { val = x; }
    }

    public class Solution {
        public ListNode MergeTwoLists(ListNode list1, ListNode list2) {
            ListNode dummy = new ListNode(0);
            ListNode current = dummy;

            while (list1 != null && list2 != null) {
                if (list1.val < list2.val) {
                    current.next = list1;
                    list1 = list1.next;
                } else {
                    current.next = list2;
                    list2 = list2.next;
                }
                current = current.next;
            }
            current.next = list1 ?? list2;
            return dummy.next;
        }
    }

    class Program {
        static void Main(string[] args) {
            ListNode list1 = new ListNode(1) { next = new ListNode(3) };
            ListNode list2 = new ListNode(2) { next = new ListNode(4) };

            Solution solution = new Solution();
            ListNode merged = solution.MergeTwoLists(list1, list2);

            PrintList(merged);
        }

        static void PrintList(ListNode head) {
            while (head != null) {
                Console.Write(head.val + " -> ");
                head = head.next;
            }
            Console.WriteLine("null");
        }
    }
    ```

=== "Java"

    ```java
    class ListNode {
        int val;
        ListNode next;
        ListNode(int x) { val = x; }
    }

    class Solution {
        public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
            ListNode dummy = new ListNode(0);
            ListNode current = dummy;

            while (list1 != null && list2 != null) {
                if (list1.val < list2.val) {
                    current.next = list1;
                    list1 = list1.next;
                } else {
                    current.next = list2;
                    list2 = list2.next;
                }
                current = current.next;
            }
            current.next = (list1 != null) ? list1 : list2;
            return dummy.next;
        }
    }

    public class Main {
        public static void main(String[] args) {
            ListNode list1 = new ListNode(1);
            list1.next = new ListNode(3);
            ListNode list2 = new ListNode(2);
            list2.next = new ListNode(4);

            Solution solution = new Solution();
            ListNode merged = solution.mergeTwoLists(list1, list2);

            printList(merged);
        }

        public static void printList(ListNode head) {
            while (head != null) {
                System.out.print(head.val + " -> ");
                head = head.next;
            }
            System.out.println("null");
        }
    }
    ```

=== "Scala"

    ```scala
    class ListNode(var x: Int = 0, var next: ListNode = null)

    object Solution {
        def mergeTwoLists(list1: ListNode, list2: ListNode): ListNode = {
            val dummy = new ListNode(0)
            var cur = dummy
            var l1 = list1
            var l2 = list2

            while (l1 != null && l2 != null) {
                if (l1.x < l2.x) {
                    cur.next = l1
                    l1 = l1.next
                } else {
                    cur.next = l2
                    l2 = l2.next
                }
                cur = cur.next
            }
            cur.next = if (l1 != null) l1 else l2
            dummy.next
        }
    }

    object Main extends App {
        def printList(head: ListNode): Unit = {
            var current = head
            while (current != null) {
                print(s"${current.x} -> ")
                current = current.next
            }
            println("null")
        }

        val list1 = new ListNode(1, new ListNode(3))
        val list2 = new ListNode(2, new ListNode(4))

        val merged = Solution.mergeTwoLists(list1, list2)
        printList(merged)
    }
    ```

=== "Kotlin"

    ```kotlin
    class ListNode(var `val`: Int, var next: ListNode? = null)

    class Solution {
        fun mergeTwoLists(list1: ListNode?, list2: ListNode?): ListNode? {
            val dummy = ListNode(0)
            var cur = dummy
            var l1 = list1
            var l2 = list2

            while (l1 != null && l2 != null) {
                if (l1.`val` < l2.`val`) {
                    cur.next = l1
                    l1 = l1.next
                } else {
                    cur.next = l2
                    l2 = l2.next
                }
                cur = cur.next!!
            }
            cur.next = l1 ?: l2
            return dummy.next
        }
    }

    fun printList(head: ListNode?) {
        var current = head
        while (current != null) {
            print("${current.`val`} -> ")
            current = current.next
        }
        println("null")
    }

    fun main() {
        val list1 = ListNode(1, ListNode(3))
        val list2 = ListNode(2, ListNode(4))

        val solution = Solution()
        val merged = solution.mergeTwoLists(list1, list2)

        printList(merged)
    }
    ```

=== "Go"

    ```go
    package main

    import "fmt"

    type ListNode struct {
        Val  int
        Next *ListNode
    }

    func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
        dummy := &ListNode{}
        current := dummy

        for list1 != nil && list2 != nil {
            if list1.Val < list2.Val {
                current.Next = list1
                list1 = list1.Next
            } else {
                current.Next = list2
                list2 = list2.Next
            }
            current = current.Next
        }
        if list1 != nil {
            current.Next = list1
        } else {
            current.Next = list2
        }

        return dummy.Next
    }

    func printList(head *ListNode) {
        for head != nil {
            fmt.Printf("%d -> ", head.Val)
            head = head.Next
        }
        fmt.Println("nil")
    }

    func main() {
        list1 := &ListNode{Val: 1, Next: &ListNode{Val: 3}}
        list2 := &ListNode{Val: 2, Next: &ListNode{Val: 4}}

        fmt.Println("Merged Linked List:")
        merged := mergeTwoLists(list1, list2)
        printList(merged)
    }
    ```

=== "TypeScript"

    ```typescript
    class ListNode {
        val: number;
        next: ListNode | null;

        constructor(val: number = 0, next: ListNode | null = null) {
            this.val = val;
            this.next = next;
        }
    }

    class Solution {
        mergeTwoLists(list1: ListNode | null, list2: ListNode | null): ListNode | null {
            const dummy = new ListNode();
            let current = dummy;

            while (list1 && list2) {
                if (list1.val < list2.val) {
                    current.next = list1;
                    list1 = list1.next;
                } else {
                    current.next = list2;
                    list2 = list2.next;
                }
                current = current.next;
            }
            current.next = list1 || list2;
            return dummy.next;
        }
    }

    function printList(head: ListNode | null): void {
        while (head) {
            process.stdout.write(head.val + " -> ");
            head = head.next;
        }
        console.log("null");
    }

    const list1 = new ListNode(1, new ListNode(3));
    const list2 = new ListNode(2, new ListNode(4));

    const solution = new Solution();
    const merged = solution.mergeTwoLists(list1, list2);

    console.log("Merged Linked List:");
    printList(merged);
    ```

=== "R"

    ```r
    ListNode <- setRefClass("ListNode",
                            fields = list(val = "numeric", next = "ANY"))

    mergeTwoLists <- function(list1, list2) {
        dummy <- ListNode$new(val = 0)
        current <- dummy

        while (!is.null(list1) && !is.null(list2)) {
            if (list1$val < list2$val) {
                current$next <- list1
                list1 <- list1$next
            } else {
                current$next <- list2
                list2 <- list2$next
            }
            current <- current$next
        }

        if (!is.null(list1)) {
            current$next <- list1
        } else {
            current$next <- list2
        }

        return(dummy$next)
    }

    printList <- function(head) {
        while (!is.null(head)) {
            cat(head$val, "-> ")
            head <- head$next
        }
        cat("NULL\n")
    }

    # Example usage
    list1 <- ListNode$new(val = 1, next = ListNode$new(val = 3))
    list2 <- ListNode$new(val = 2, next = ListNode$new(val = 4))

    cat("Merged Linked List:\n")
    merged <- mergeTwoLists(list1, list2)
    printList(merged)
    ```

=== "Julia"

    ```julia
    mutable struct ListNode
        val::Int
        next::Union{ListNode, Nothing}
        ListNode(val::Int, next::Union{ListNode, Nothing}=nothing) = new(val, next)
    end

    function mergeTwoLists(list1::Union{ListNode, Nothing}, list2::Union{ListNode, Nothing})
        dummy = ListNode(0)
        current = dummy

        while list1 !== nothing && list2 !== nothing
            if list1.val < list2.val
                current.next = list1
                list1 = list1.next
            else
                current.next = list2
                list2 = list2.next
            end
            current = current.next
        end

        current.next = if list1 !== nothing list1 else list2 end
        return dummy.next
    end

    function printList(head::Union{ListNode, Nothing})
        while head !== nothing
            print(head.val, " -> ")
            head = head.next
        end
        println("nil")
    end

    # Example usage
    list1 = ListNode(1, ListNode(3))
    list2 = ListNode(2, ListNode(4))

    println("Merged Linked List:")
    merged = mergeTwoLists(list1, list2)
    printList(merged)
    ```

## Linked List Cycle

### Problem Description: [LeetCode - Problem 21 - Linked List Cycle](https://leetcode.com/problems/merge-two-sorted-lists/description/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

### Complexity Analysis

#### Time Complexity

#### Space Complexity

---

### Solutions

=== "Python"

    ```python
    # TC = O(N), SC = O(1)

    from typing import Optional

    class Node:
        def __init__(self, data: int) -> None:
            self.data: int = data
            self.next: Optional['Node'] = None

    def push(head: Optional[Node], data: int) -> Node:
        new_node = Node(data)
        if head is None:
            return new_node
        else:
            curr = head
            while curr.next is not None:
                curr = curr.next
            curr.next = new_node
            return head

    #def print_list(head: Optional[Node]) -> None:
    #    while head:
    #        print(f"{head.data}-->", end="")
    #        head = head.next
    #    print("NULL")

    def print_list(head: Optional[Node]) -> None:
        visited = set()
        while head:
            if head in visited:
                print(f"({head.data})-->", end="")
                print("Cycle detected here...")
                return
            print(f"{head.data}-->", end="")
            visited.add(head)
            head = head.next
        print("NULL")

    class Solution:
        def hasCycle(self, head: Optional[Node]) -> bool:
            if not head or not head.next:
                return False
            
            slow = head
            fast = head.next
            
            while slow != fast:
                if not fast or not fast.next:
                    return False
                slow = slow.next
                fast = fast.next.next
            
            return True

    if __name__ == "__main__":
        head: Optional[Node] = None
        head = push(head, 1)
        head = push(head, 2)
        head = push(head, 3)
        head = push(head, 4)
        head = push(head, 5)

        print("Does the following LinkedList: [ ", end="")
        print_list(head)
        print(f" ] have a cycle? {Solution().hasCycle(head)}")
    ```

=== "C++"

    ```cpp
    #include <iostream>
    #include <unordered_set>

    class Node {
    public:
        int data;
        Node* next;

        Node(int data) : data(data), next(nullptr) {}
    };

    Node* push(Node* head, int data) {
        Node* new_node = new Node(data);
        if (!head) {
            return new_node;
        }
        Node* curr = head;
        while (curr->next) {
            curr = curr->next;
        }
        curr->next = new_node;
        return head;
    }

    void print_list(Node* head) {
        std::unordered_set<Node*> visited;
        while (head) {
            if (visited.count(head)) {
                std::cout << "(" << head->data << ")-->";
                std::cout << "Cycle detected here..." << std::endl;
                return;
            }
            std::cout << head->data << "-->";
            visited.insert(head);
            head = head->next;
        }
        std::cout << "NULL" << std::endl;
    }

    class Solution {
    public:
        bool hasCycle(Node* head) {
            if (!head || !head->next) return false;

            Node* slow = head;
            Node* fast = head->next;

            while (slow != fast) {
                if (!fast || !fast->next) return false;
                slow = slow->next;
                fast = fast->next->next;
            }
            return true;
        }
    };

    int main() {
        Node* head = nullptr;
        head = push(head, 1);
        head = push(head, 2);
        head = push(head, 3);
        head = push(head, 4);
        head = push(head, 5);

        std::cout << "Does the following LinkedList: [ ";
        print_list(head);
        std::cout << "] have a cycle? " << std::boolalpha << Solution().hasCycle(head) << std::endl;

        return 0;
    }
    ```

=== "Rust"

    ```rust
    use std::collections::HashSet;

    #[derive(Debug)]
    struct Node {
        data: i32,
        next: Option<Box<Node>>,
    }

    impl Node {
        fn new(data: i32) -> Self {
            Node { data, next: None }
        }
    }

    fn push(head: Option<Box<Node>>, data: i32) -> Option<Box<Node>> {
        let mut new_node = Box::new(Node::new(data));
        if let Some(mut curr) = head {
            while let Some(next) = curr.next.take() {
                curr = next;
            }
            curr.next = Some(new_node);
            Some(head)
        } else {
            Some(new_node)
        }
    }

    fn print_list(head: &Option<Box<Node>>) {
        let mut visited = HashSet::new();
        let mut current = head;
        while let Some(node) = current {
            if visited.contains(&node.as_ref()) {
                println!("({})--> Cycle detected here...", node.data);
                return;
            }
            print!("{}-->", node.data);
            visited.insert(node.as_ref());
            current = &node.next;
        }
        println!("NULL");
    }

    struct Solution;

    impl Solution {
        fn has_cycle(head: &Option<Box<Node>>) -> bool {
            let mut slow = head.as_ref();
            let mut fast = head.as_ref();

            while let Some(s) = slow {
                slow = s.next.as_ref();
                fast = fast.and_then(|f| f.next.as_ref()).and_then(|f| f.next.as_ref());
                if slow == fast {
                    return true;
                }
            }
            false
        }
    }

    fn main() {
        let mut head = None;
        head = push(head, 1);
        head = push(head, 2);
        head = push(head, 3);
        head = push(head, 4);
        head = push(head, 5);

        print!("Does the following LinkedList: [ ");
        print_list(&head);
        println!("] have a cycle? {}", Solution::has_cycle(&head));
    }
    ```

=== "C#"

    ```csharp
    using System;
    using System.Collections.Generic;

    public class Node {
        public int data;
        public Node next;

        public Node(int data) {
            this.data = data;
            this.next = null;
        }
    }

    public class Solution {
        public bool HasCycle(Node head) {
            if (head == null || head.next == null) return false;

            Node slow = head;
            Node fast = head.next;

            while (slow != fast) {
                if (fast == null || fast.next == null) return false;
                slow = slow.next;
                fast = fast.next.next;
            }
            return true;
        }
    }

    class Program {
        static Node Push(Node head, int data) {
            Node newNode = new Node(data);
            if (head == null) {
                return newNode;
            } else {
                Node curr = head;
                while (curr.next != null) {
                    curr = curr.next;
                }
                curr.next = newNode;
                return head;
            }
        }

        static void PrintList(Node head) {
            HashSet<Node> visited = new HashSet<Node>();
            while (head != null) {
                if (visited.Contains(head)) {
                    Console.WriteLine($"({head.data})--> Cycle detected here...");
                    return;
                }
                Console.Write($"{head.data}-->");
                visited.Add(head);
                head = head.next;
            }
            Console.WriteLine("NULL");
        }

        static void Main() {
            Node head = null;
            head = Push(head, 1);
            head = Push(head, 2);
            head = Push(head, 3);
            head = Push(head, 4);
            head = Push(head, 5);

            Console.Write("Does the following LinkedList: [ ");
            PrintList(head);
            Console.WriteLine($"] have a cycle? {new Solution().HasCycle(head)}");
        }
    }
    ```

=== "Java"

    ```java
    import java.util.HashSet;

    class Node {
        int data;
        Node next;

        Node(int data) {
            this.data = data;
            this.next = null;
        }
    }

    class Solution {
        public boolean hasCycle(Node head) {
            if (head == null || head.next == null) return false;

            Node slow = head;
            Node fast = head.next;

            while (slow != fast) {
                if (fast == null || fast.next == null) return false;
                slow = slow.next;
                fast = fast.next.next;
            }
            return true;
        }
    }

    public class LinkedListCycle {
        static Node push(Node head, int data) {
            Node newNode = new Node(data);
            if (head == null) {
                return newNode;
            } else {
                Node curr = head;
                while (curr.next != null) {
                    curr = curr.next;
                }
                curr.next = newNode;
                return head;
            }
        }

        static void printList(Node head) {
            HashSet<Node> visited = new HashSet<>();
            while (head != null) {
                if (visited.contains(head)) {
                    System.out.println("(" + head.data + ")--> Cycle detected here...");
                    return;
                }
                System.out.print(head.data + "-->");
                visited.add(head);
                head = head.next;
            }
            System.out.println("NULL");
        }

        public static void main(String[] args) {
            Node head = null;
            head = push(head, 1);
            head = push(head, 2);
            head = push(head, 3);
            head = push(head, 4);
            head = push(head, 5);

            System.out.print("Does the following LinkedList: [ ");
            printList(head);
            System.out.println("] have a cycle? " + new Solution().hasCycle(head));
        }
    }
    ```

=== "Scala"

    ```scala
    import scala.collection.mutable

    class Node(var data: Int) {
        var next: Option[Node] = None
    }

    object LinkedListCycle {
        def push(head: Option[Node], data: Int): Option[Node] = {
            val newNode = new Node(data)
            head match {
            case None => Some(newNode)
            case Some(curr) =>
                var temp = curr
                while (temp.next.isDefined) {
                    temp = temp.next.get
                }
                temp.next = Some(newNode)
                head
            }
        }

        def printList(head: Option[Node]): Unit = {
            val visited = mutable.HashSet[Node]()
            var current = head
            while (current.isDefined) {
                current match {
                    case Some(node) =>
                        if (visited.contains(node)) {
                            println(s"(${node.data})--> Cycle detected here...")
                            return
                        }
                        print(s"${node.data}-->")
                        visited.add(node)
                        current = node.next
                    case None => 
                }
            }
            println("NULL")
        }

        class Solution {
            def hasCycle(head: Option[Node]): Boolean = {
            if (head.isEmpty || head.get.next.isEmpty) return false

            var slow = head
            var fast = head.flatMap(_.next)

            while (slow != fast) {
                if (fast.isEmpty || fast.get.next.isEmpty) return false
                slow = slow.flatMap(_.next)
                fast = fast.flatMap(_.next).flatMap(_.next)
            }
            true
            }
        }

        def main(args: Array[String]): Unit = {
            var head: Option[Node] = None
            head = push(head, 1)
            head = push(head, 2)
            head = push(head, 3)
            head = push(head, 4)
            head = push(head, 5)

            print("Does the following LinkedList: [ ")
            printList(head)
            println(s"] have a cycle? ${new Solution().hasCycle(head)}")
        }
    }
    ```

=== "Kotlin"

    ```kotlin
    class Node(var data: Int) {
        var next: Node? = null
    }

    class Solution {
        fun hasCycle(head: Node?): Boolean {
            if (head == null || head.next == null) return false

            var slow: Node? = head
            var fast: Node? = head.next

            while (slow != fast) {
                if (fast == null || fast.next == null) return false
                slow = slow?.next
                fast = fast.next?.next
            }
            return true
        }
    }

    fun push(head: Node?, data: Int): Node {
        val newNode = Node(data)
        return if (head == null) {
            newNode
        } else {
            var curr = head
            while (curr.next != null) {
                curr = curr.next!!
            }
            curr.next = newNode
            head
        }
    }

    fun printList(head: Node?) {
        val visited = mutableSetOf<Node>()
        var current: Node? = head
        while (current != null) {
            if (current in visited) {
                println("(${current.data})--> Cycle detected here...")
                return
            }
            print("${current.data}-->")
            visited.add(current)
            current = current.next
        }
        println("NULL")
    }

    fun main() {
        var head: Node? = null
        head = push(head, 1)
        head = push(head, 2)
        head = push(head, 3)
        head = push(head, 4)
        head = push(head, 5)

        print("Does the following LinkedList: [ ")
        printList(head)
        println("] have a cycle? ${Solution().hasCycle(head)}")
    }
    ```

=== "Go"

    ```go
    package main

    import (
        "fmt"
    )

    type Node struct {
        data int
        next *Node
    }

    func push(head *Node, data int) *Node {
        newNode := &Node{data: data}
        if head == nil {
            return newNode
        }
        curr := head
        for curr.next != nil {
            curr = curr.next
        }
        curr.next = newNode
        return head
    }

    func printList(head *Node) {
        visited := make(map[*Node]bool)
        for head != nil {
            if visited[head] {
                fmt.Printf("(%d)--> Cycle detected here...\n", head.data)
                return
            }
            fmt.Printf("%d-->", head.data)
            visited[head] = true
            head = head.next
        }
        fmt.Println("NULL")
    }

    type Solution struct{}

    func (s *Solution) hasCycle(head *Node) bool {
        if head == nil || head.next == nil {
            return false
        }

        slow, fast := head, head.next

        for slow != fast {
            if fast == nil || fast.next == nil {
                return false
            }
            slow = slow.next
            fast = fast.next.next
        }
        return true
    }

    func main() {
        var head *Node
        head = push(head, 1)
        head = push(head, 2)
        head = push(head, 3)
        head = push(head, 4)
        head = push(head, 5)

        fmt.Print("Does the following LinkedList: [ ")
        printList(head)
        fmt.Printf("] have a cycle? %v\n", (&Solution{}).hasCycle(head))
    }
    ```

=== "TypeScript"

    ```typescript
    class Node {
        data: number;
        next: Node | null;

        constructor(data: number) {
            this.data = data;
            this.next = null;
        }
    }

    function push(head: Node | null, data: number): Node {
        const newNode = new Node(data);
        if (head === null) {
            return newNode;
        } else {
            let curr = head;
            while (curr.next !== null) {
                curr = curr.next;
            }
            curr.next = newNode;
            return head;
        }
    }

    function printList(head: Node | null): void {
        const visited = new Set<Node>();
        while (head !== null) {
            if (visited.has(head)) {
                console.log(`(${head.data})--> Cycle detected here...`);
                return;
            }
            process.stdout.write(`${head.data}-->`);
            visited.add(head);
            head = head.next;
        }
        console.log("NULL");
    }

    class Solution {
        hasCycle(head: Node | null): boolean {
            if (head === null || head.next === null) return false;

            let slow: Node | null = head;
            let fast: Node | null = head.next;

            while (slow !== fast) {
                if (fast === null || fast.next === null) return false;
                slow = slow.next;
                fast = fast.next.next;
            }
            return true;
        }
    }

    function main(): void {
        let head: Node | null = null;
        head = push(head, 1);
        head = push(head, 2);
        head = push(head, 3);
        head = push(head, 4);
        head = push(head, 5);

        process.stdout.write("Does the following LinkedList: [ ");
        printList(head);
        console.log(`] have a cycle? ${new Solution().hasCycle(head)}`);
    }

    main();
    ```

=== "R"

    ```r
    Node <- setRefClass("Node", fields = list(data = "numeric", next = "Node"))

    push <- function(head, data) {
        new_node <- Node$new(data)
        if (is.null(head)) {
            return(new_node)
        } else {
            curr <- head
            while (!is.null(curr$next)) {
                curr <- curr$next
            }
            curr$next <- new_node
            return(head)
        }
    }

    print_list <- function(head) {
        visited <- list()
        while (!is.null(head)) {
            if (head %in% visited) {
                cat(sprintf("(%d)--> Cycle detected here...\n", head$data))
                return()
            }
            cat(sprintf("%d-->", head$data))
            visited <- append(visited, list(head))
            head <- head$next
        }
        cat("NULL\n")
    }

    has_cycle <- function(head) {
        if (is.null(head) || is.null(head$next)) return(FALSE)

        slow <- head
        fast <- head$next

        while (slow != fast) {
            if (is.null(fast) || is.null(fast$next)) return(FALSE)
            slow <- slow$next
            fast <- fast$next$next
        }
        return(TRUE)
    }

    main <- function() {
        head <- NULL
        head <- push(head, 1)
        head <- push(head, 2)
        head <- push(head, 3)
        head <- push(head, 4)
        head <- push(head, 5)

        cat("Does the following LinkedList: [ ")
        print_list(head)
        cat(sprintf("] have a cycle? %s\n", has_cycle(head)))
    }

    main()
    ```

=== "Julia"

    ```julia
    mutable struct Node
        data::Int
        next::Union{Node, Nothing}
    end

    function push(head::Union{Node, Nothing}, data::Int)
        new_node = Node(data, nothing)
        if head === nothing
            return new_node
        else
            curr = head
            while curr.next !== nothing
                curr = curr.next
            end
            curr.next = new_node
            return head
        end
    end

    function print_list(head::Union{Node, Nothing})
        visited = Set{Node}()
        while head !== nothing
            if head in visited
                println("($head.data)--> Cycle detected here...")
                return
            end
            print("$head.data-->")
            push!(visited, head)
            head = head.next
        end
        println("NULL")
    end

    struct Solution end

    function has_cycle(s::Solution, head::Union{Node, Nothing})
        if head === nothing || head.next === nothing
            return false
        end

        slow = head
        fast = head.next

        while slow != fast
            if fast === nothing || fast.next === nothing
                return false
            end
            slow = slow.next
            fast = fast.next.next
        end
        return true
    end

    function main()
        head = nothing
        head = push(head, 1)
        head = push(head, 2)
        head = push(head, 3)
        head = push(head, 4)
        head = push(head, 5)

        print("Does the following LinkedList: [ ")
        print_list(head)
        println("] have a cycle? ", has_cycle(Solution(), head))
    end

    main()
    ```
