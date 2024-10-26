---
tags:
  - Original
---

# Linked List

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
  - Since the new nodes are created in-place and the space usage is mainly for pointers, the space complexity is __`O(1)`__, constant space complexity.

In summary, the code efficiently merges two sorted singly-linked lists into a single sorted list __with a time complexity of `O(m + n)` and a space complexity of `O(1)`.__

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
