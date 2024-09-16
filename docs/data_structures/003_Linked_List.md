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
    ```

=== "C#"

    ```csharp
    ```

=== "Java"

    ```java
    ```

=== "Scala"

    ```scala
    ```

=== "Kotlin"

    ```kotlin
    ```

=== "Go"

    ```go
    ```

=== "TypeScript"

    ```typescript
    ```

=== "R"

    ```r
    ```

=== "Julia"

    ```julia
    ```

## Merge Two Sorted Lists

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
    ```

=== "C#"

    ```csharp
    ```

=== "Java"

    ```java
    ```

=== "Scala"

    ```scala
    ```

=== "Kotlin"

    ```kotlin
    ```

=== "Go"

    ```go
    ```

=== "TypeScript"

    ```typescript
    ```

=== "R"

    ```r
    ```

=== "Julia"

    ```julia
    ```
