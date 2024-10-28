---
tags:
  - Original
---

# Design

## Design a LRU Cache

### Problem Description: [LeetCode - Problem 146 - Design a LRU Cache](https://leetcode.com/problems/lru-cache/description/)

### Problem Description:
Design a data structure that follows the constraints of a [__Least Recently Used (LRU) cache__](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU).

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

The provided Python code implements a Least Recently Used (LRU) Cache using two different approaches: one based on `OrderedDict` and another using a custom linked list with a hash map for fast access. Below is a detailed explanation of both implementations, followed by a complexity analysis for time and space.

### Explanation of the Implementations

#### 1. **LRUCache (OrderedDict-Based)**

- **Class Definition**:
  - The class `LRUCache` is initialized with a specified `capacity`.
  - It uses `collections.OrderedDict` to maintain the order of the keys.

- **Methods**:
  - `__init__(self, capacity: int)`: Initializes the cache with a given capacity and an empty `OrderedDict`.
  
  - `get(self, key: int) -> int`: 
    - Checks if the key exists in the cache.
    - If it exists, it retrieves the value, updates the order of the key (moving it to the end), and returns the value.
    - If it doesn't exist, returns `-1`.

  - `put(self, key: int, val: int) -> None`: 
    - If the key does not exist and the cache has reached its capacity, it evicts the least recently used (the first item in the `OrderedDict`).
    - Calls `__update` to update the value of the key.
  
  - `__update(self, key: int, val: int) -> None`: 
    - Removes the key if it exists (to update its value).
    - Adds the key and value to the cache, automatically moving it to the end (most recently used).

#### 2. **LRUCache2 (Linked List-Based)**

- **Class Definitions**:
  - `ListNode`: Represents each node in the linked list, containing the `key`, `value`, and pointers to the previous and next nodes.
  - `LinkedList`: A custom linked list class that maintains a head and tail pointer.
    - `insert(self, node: ListNode)`: Inserts a new node at the end of the linked list.
    - `delete(self, node: ListNode)`: Removes a node from the linked list.

- **Methods in `LRUCache2`**:
  - `__init__(self, capacity: int)`: Initializes the linked list and a hash map (`dict`) to store key-node pairs.
  
  - `get(self, key: int) -> int`: 
    - Similar to the previous implementation, retrieves the value if the key exists, updates its position, and returns the value. If it doesn't exist, returns `-1`.

  - `put(self, key: int, val: int) -> None`: 
    - If the key does not exist and the cache is full, it evicts the least recently used node (the head of the linked list).
    - Calls `__update` to insert or update the value of the key.

  - `__update(self, key: int, val: int) -> None`: 
    - Deletes the existing node if the key exists.
    - Creates a new `ListNode`, inserts it at the end of the linked list, and updates the hash map.

### Complexity Analysis

#### Time Complexity

- **OrderedDict-Based LRUCache**:
  - **get**: 
    - __`O(1)`__ average case for retrieving a value using the `OrderedDict`'s hash map.
    - __`O(n)`__ in the worst case for moving the accessed item to the end of the `OrderedDict` due to the underlying structure (but this operation is generally fast due to the order being maintained).
  
  - **put**: 
    - __`O(1)`__ for checking existence and inserting into the `OrderedDict`. The eviction is O(1)`__ since it removes the first item (least recently used).

- **LinkedList-Based LRUCache2**:
  - **get**: 
    - __`O(1)`__ for retrieving the node from the hash map.
    - __`O(1)`__ for deleting and reinserting the node in the linked list (updating position).
  
  - **put**: 
    - __`O(1)`__ for checking existence and updating or inserting the node in the hash map.
    - __`O(1)`__ for deleting the least recently used node (head of the linked list) if the cache is full.

#### Space Complexity

- Both implementations maintain two data structures:
  - A hash map to store key-value pairs for __`O(1)`__ access.
  - An additional structure (either `OrderedDict` or a linked list) to maintain the order of keys.
  
- **Space Complexity**:
  - __`O(capacity)`__  for both implementations since they can only store up to `capacity` key-value pairs.

### Summary

- The `OrderedDict`-based implementation is simpler and leverages Python's built-in features, making it easy to read and understand.
- The linked list implementation is more manual but provides a good understanding of how LRU caches work at a lower level, managing nodes and pointers.
- Both implementations efficiently support the required operations while adhering to __`O(1)`__ average time complexity for `get` and `put` operations and __`O(capacity)`__  space complexity.

### Solutions

---

=== "Python"

	```python
    from typing import Optional, Dict
    import collections


    # Using OrderedDict
    class LRUCache:
        def __init__(self, capacity: int):
            self.cache: collections.OrderedDict[int, int] = collections.OrderedDict()
            self.capacity: int = capacity

        def get(self, key: int) -> int:
            if key not in self.cache:
                return -1
            val = self.cache[key]
            self.__update(key, val)
            return val

        def put(self, key: int, val: int) -> None:
            if key not in self.cache and len(self.cache) == self.capacity:
                self.cache.popitem(last=False)
            self.__update(key, val)

        def __update(self, key: int, val: int) -> None:
            if key in self.cache:
                del self.cache[key]
            self.cache[key] = val


    # Linked List implementation for LRU cache
    class ListNode:
        def __init__(self, key: int, val: int):
            self.val: int = val
            self.key: int = key
            self.next: Optional['ListNode'] = None
            self.prev: Optional['ListNode'] = None


    class LinkedList:
        def __init__(self):
            self.head: Optional[ListNode] = None
            self.tail: Optional[ListNode] = None

        def insert(self, node: ListNode) -> None:
            node.next, node.prev = None, None  # avoid dirty node
            if self.head is None:
                self.head = node
            else:
                assert self.tail is not None  # for type checking
                self.tail.next = node
                node.prev = self.tail
            self.tail = node

        def delete(self, node: ListNode) -> None:
            if node.prev:
                node.prev.next = node.next
            else:
                self.head = node.next
            if node.next:
                node.next.prev = node.prev
            else:
                self.tail = node.prev
            node.next, node.prev = None, None  # make node clean


    class LRUCache2:
        def __init__(self, capacity: int):
            self.list = LinkedList()
            self.dict: Dict[int, ListNode] = {}
            self.capacity: int = capacity

        def get(self, key: int) -> int:
            if key not in self.dict:
                return -1
            val = self.dict[key].val
            self.__update(key, val)
            return val

        def put(self, key: int, val: int) -> None:
            if key not in self.dict and len(self.dict) == self.capacity:
                assert self.list.head is not None  # for type checking
                del self.dict[self.list.head.key]
                self.list.delete(self.list.head)
            self.__update(key, val)

        def __update(self, key: int, val: int) -> None:
            if key in self.dict:
                self.list.delete(self.dict[key])
            node = ListNode(key, val)
            self.list.insert(node)
            self.dict[key] = node


    def main() -> None:
        # Demonstrating LRUCache (OrderedDict-based)
        print("Using LRUCache (OrderedDict-based):")
        lru_cache = LRUCache(capacity=2)
        lru_cache.put(1, 1)
        lru_cache.put(2, 2)
        print(lru_cache.get(1))  # returns 1
        lru_cache.put(3, 3)      # evicts key 2
        print(lru_cache.get(2))  # returns -1 (not found)
        lru_cache.put(4, 4)      # evicts key 1
        print(lru_cache.get(1))  # returns -1 (not found)
        print(lru_cache.get(3))  # returns 3
        print(lru_cache.get(4))  # returns 4

        print("\nUsing LRUCache2 (LinkedList-based):")
        # Demonstrating LRUCache2 (LinkedList-based)
        lru_cache2 = LRUCache2(capacity=2)
        lru_cache2.put(1, 1)
        lru_cache2.put(2, 2)
        print(lru_cache2.get(1))  # returns 1
        lru_cache2.put(3, 3)      # evicts key 2
        print(lru_cache2.get(2))  # returns -1 (not found)
        lru_cache2.put(4, 4)      # evicts key 1
        print(lru_cache2.get(1))  # returns -1 (not found)
        print(lru_cache2.get(3))  # returns 3
        print(lru_cache2.get(4))  # returns 4


    if __name__ == "__main__":
        main()
    ```

=== "C++"

	```cpp
    #include <iostream>
    #include <list>
    #include <unordered_map>

    // OrderedDict-based LRU Cache using std::list and std::unordered_map
    class LRUCacheOrdered {
        int capacity;
        std::list<std::pair<int, int>> cacheList;
        std::unordered_map<int, std::list<std::pair<int, int>>::iterator> cacheMap;

    public:
        LRUCacheOrdered(int cap) : capacity(cap) {}

        int get(int key) {
            if (cacheMap.find(key) == cacheMap.end()) return -1;
            cacheList.splice(cacheList.begin(), cacheList, cacheMap[key]);
            return cacheMap[key]->second;
        }

        void put(int key, int value) {
            if (cacheMap.find(key) != cacheMap.end()) {
                cacheList.erase(cacheMap[key]);
            } else if (cacheList.size() == capacity) {
                int lastKey = cacheList.back().first;
                cacheList.pop_back();
                cacheMap.erase(lastKey);
            }
            cacheList.emplace_front(key, value);
            cacheMap[key] = cacheList.begin();
        }
    };

    // LinkedList-based LRU Cache
    struct ListNode {
        int key, value;
        ListNode* next = nullptr;
        ListNode* prev = nullptr;
        ListNode(int k, int v) : key(k), value(v) {}
    };

    class LRUCacheLinkedList {
        int capacity;
        std::unordered_map<int, ListNode*> cacheMap;
        ListNode* head = nullptr;
        ListNode* tail = nullptr;

        void removeNode(ListNode* node) {
            if (node->prev) node->prev->next = node->next;
            if (node->next) node->next->prev = node->prev;
            if (node == head) head = node->next;
            if (node == tail) tail = node->prev;
        }

        void insertNodeAtEnd(ListNode* node) {
            node->next = nullptr;
            node->prev = tail;
            if (tail) tail->next = node;
            tail = node;
            if (!head) head = node;
        }

    public:
        LRUCacheLinkedList(int cap) : capacity(cap) {}

        int get(int key) {
            if (cacheMap.find(key) == cacheMap.end()) return -1;
            ListNode* node = cacheMap[key];
            removeNode(node);
            insertNodeAtEnd(node);
            return node->value;
        }

        void put(int key, int value) {
            if (cacheMap.find(key) != cacheMap.end()) {
                removeNode(cacheMap[key]);
                delete cacheMap[key];
            } else if (cacheMap.size() == capacity) {
                int removeKey = head->key;
                ListNode* temp = head;
                removeNode(head);
                delete temp;
                cacheMap.erase(removeKey);
            }
            ListNode* newNode = new ListNode(key, value);
            insertNodeAtEnd(newNode);
            cacheMap[key] = newNode;
        }

        ~LRUCacheLinkedList() {
            ListNode* curr = head;
            while (curr) {
                ListNode* temp = curr;
                curr = curr->next;
                delete temp;
            }
        }
    };

    int main() {
        // Testing OrderedDict-based LRU Cache
        std::cout << "Testing LRUCacheOrdered:\n";
        LRUCacheOrdered lruOrdered(2);
        lruOrdered.put(1, 1);
        lruOrdered.put(2, 2);
        std::cout << lruOrdered.get(1)`__ << std::endl;  // returns 1
        lruOrdered.put(3, 3);  // evicts key 2
        std::cout << lruOrdered.get(2) << std::endl;  // returns -1 (not found)
        lruOrdered.put(4, 4);  // evicts key 1
        std::cout << lruOrdered.get(1)`__ << std::endl;  // returns -1 (not found)
        std::cout << lruOrdered.get(3) << std::endl;  // returns 3
        std::cout << lruOrdered.get(4) << std::endl;  // returns 4

        // Testing LinkedList-based LRU Cache
        std::cout << "\nTesting LRUCacheLinkedList:\n";
        LRUCacheLinkedList lruLinked(2);
        lruLinked.put(1, 1);
        lruLinked.put(2, 2);
        std::cout << lruLinked.get(1)`__ << std::endl;  // returns 1
        lruLinked.put(3, 3);  // evicts key 2
        std::cout << lruLinked.get(2) << std::endl;  // returns -1 (not found)
        lruLinked.put(4, 4);  // evicts key 1
        std::cout << lruLinked.get(1)`__ << std::endl;  // returns -1 (not found)
        std::cout << lruLinked.get(3) << std::endl;  // returns 3
        std::cout << lruLinked.get(4) << std::endl;  // returns 4

        return 0;
    }
    ```

=== "Rust"

	```rust
    use std::collections::HashMap;

    // OrderedDict-based LRU Cache
    struct LRUCacheOrdered {
        capacity: usize,
        cache: HashMap<i32, i32>,
        order: Vec<i32>, // To maintain the order of keys
    }

    impl LRUCacheOrdered {
        fn new(capacity: usize) -> Self {
            LRUCacheOrdered {
                capacity,
                cache: HashMap::new(),
                order: Vec::new(),
            }
        }

        fn get(&mut self, key: i32) -> i32 {
            if let Some(&value) = self.cache.get(&key) {
                // Move the accessed key to the end of the order vector
                self.order.retain(|&k| k != key);
                self.order.push(key);
                return value;
            }
            -1 // Return -1 if key is not found
        }

        fn put(&mut self, key: i32, value: i32) {
            if self.cache.contains_key(&key) {
                // Key already exists, update value and move to end
                self.cache.insert(key, value);
                self.order.retain(|&k| k != key);
                self.order.push(key);
            } else {
                // If cache is full, remove the least recently used item
                if self.cache.len() == self.capacity {
                    let lru_key = self.order.remove(0); // Remove the first element
                    self.cache.remove(&lru_key);
                }
                self.cache.insert(key, value);
                self.order.push(key);
            }
        }
    }

    // LinkedList-based LRU Cache
    struct ListNode {
        key: i32,
        value: i32,
        prev: Option<Box<ListNode>>,
        next: Option<Box<ListNode>>,
    }

    struct LRUCacheLinkedList {
        capacity: usize,
        cache: HashMap<i32, Box<ListNode>>,
        head: Option<Box<ListNode>>,
        tail: Option<Box<ListNode>>,
    }

    impl LRUCacheLinkedList {
        fn new(capacity: usize) -> Self {
            let head = Box::new(ListNode { key: 0, value: 0, prev: None, next: None });
            let tail = Box::new(ListNode { key: 0, value: 0, prev: Some(head.clone()), next: None });

            LRUCacheLinkedList {
                capacity,
                cache: HashMap::new(),
                head: Some(head),
                tail: Some(tail),
            }
        }

        fn remove(&mut self, node: &Box<ListNode>) {
            if let Some(ref mut head) = self.head {
                if let Some(ref mut tail) = self.tail {
                    if let Some(ref mut prev) = node.prev {
                        prev.next = node.next.clone();
                    }
                    if let Some(ref mut next) = node.next {
                        next.prev = node.prev.clone();
                    }
                }
            }
            self.cache.remove(&node.key);
        }

        fn add_to_end(&mut self, node: Box<ListNode>) {
            if let Some(ref mut tail) = self.tail {
                let mut last = tail.prev.take().unwrap();
                last.next = Some(node.clone());
                last.next.as_mut().unwrap().prev = Some(last);
                tail.prev = Some(node);
            }
        }

        fn get(&mut self, key: i32) -> i32 {
            if let Some(node) = self.cache.get(&key) {
                self.remove(node);
                self.add_to_end(node.clone());
                return node.value;
            }
            -1 // Return -1 if key is not found
        }

        fn put(&mut self, key: i32, value: i32) {
            if let Some(node) = self.cache.get(&key) {
                self.remove(node);
            } else if self.cache.len() == self.capacity {
                // Evict the least recently used node
                if let Some(ref mut head) = self.head {
                    if let Some(ref mut first) = head.next {
                        self.remove(first);
                    }
                }
            }

            let new_node = Box::new(ListNode {
                key,
                value,
                prev: None,
                next: None,
            });
            self.cache.insert(key, new_node.clone());
            self.add_to_end(new_node);
        }
    }

    fn main() {
        // Testing OrderedDict-based LRUCache
        println!("Testing LRUCacheOrdered:");
        let mut lru_ordered = LRUCacheOrdered::new(2);
        lru_ordered.put(1, 1);
        lru_ordered.put(2, 2);
        println!("{}", lru_ordered.get(1)); // prints 1
        lru_ordered.put(3, 3); // evicts key 2
        println!("{}", lru_ordered.get(2)); // prints -1

        // Testing LinkedList-based LRUCache
        println!("\nTesting LRUCacheLinkedList:");
        let mut lru_linked = LRUCacheLinkedList::new(2);
        lru_linked.put(1, 1);
        lru_linked.put(2, 2);
        println!("{}", lru_linked.get(1)); // prints 1
        lru_linked.put(3, 3); // evicts key 2
        println!("{}", lru_linked.get(2)); // prints -1
    }
    ```

=== "C#"

	```csharp
    using System;
    using System.Collections.Generic;

    // OrderedDict-based LRU Cache
    public class LRUCacheOrdered {
        private int capacity;
        private LinkedList<(int key, int value)> cacheList = new LinkedList<(int, int)>();
        private Dictionary<int, LinkedListNode<(int key, int value)>> cacheMap = new Dictionary<int, LinkedListNode<(int, int)>>();

        public LRUCacheOrdered(int capacity) {
            this.capacity = capacity;
        }

        public int Get(int key) {
            if (!cacheMap.ContainsKey(key)) return -1;
            var node = cacheMap[key];
            cacheList.Remove(node);
            cacheList.AddFirst(node);
            return node.Value.value;
        }

        public void Put(int key, int value) {
            if (cacheMap.ContainsKey(key)) cacheList.Remove(cacheMap[key]);
            else if (cacheList.Count == capacity) {
                cacheMap.Remove(cacheList.Last.Value.key);
                cacheList.RemoveLast();
            }
            var newNode = new LinkedListNode<(int key, int value)>((key, value));
            cacheList.AddFirst(newNode);
            cacheMap[key] = newNode;
        }
    }

    // LinkedList-based LRU Cache
    public class ListNode {
        public int Key, Value;
        public ListNode Next, Prev;
        public ListNode(int key, int value) { Key = key; Value = value; }
    }

    public class LRUCacheLinkedList {
        private int capacity;
        private Dictionary<int, ListNode> cacheMap = new Dictionary<int, ListNode>();
        private ListNode head = null, tail = null;

        public LRUCacheLinkedList(int capacity) {
            this.capacity = capacity;
        }

        private void RemoveNode(ListNode node) {
            if (node.Prev != null) node.Prev.Next = node.Next;
            else head = node.Next;
            if (node.Next != null) node.Next.Prev = node.Prev;
            else tail = node.Prev;
        }

        private void InsertAtEnd(ListNode node) {
            if (tail != null) tail.Next = node;
            node.Prev = tail;
            node.Next = null;
            tail = node;
            if (head == null) head = node;
        }

        public int Get(int key) {
            if (!cacheMap.ContainsKey(key)) return -1;
            var node = cacheMap[key];
            RemoveNode(node);
            InsertAtEnd(node);
            return node.Value;
        }

        public void Put(int key, int value) {
            if (cacheMap.ContainsKey(key)) RemoveNode(cacheMap[key]);
            else if (cacheMap.Count == capacity) {
                cacheMap.Remove(head.Key);
                RemoveNode(head);
            }
            var newNode = new ListNode(key, value);
            InsertAtEnd(newNode);
            cacheMap[key] = newNode;
        }
    }

    public class Program {
        public static void Main() {
            Console.WriteLine("Testing LRUCacheOrdered:");
            var lruOrdered = new LRUCacheOrdered(2);
            lruOrdered.Put(1, 1);
            lruOrdered.Put(2, 2);
            Console.WriteLine(lruOrdered.Get(1));  // prints 1
            lruOrdered.Put(3, 3);                  // evicts key 2
            Console.WriteLine(lruOrdered.Get(2));  // prints -1 (not found)

            Console.WriteLine("\nTesting LRUCacheLinkedList:");
            var lruLinked = new LRUCacheLinkedList(2);
            lruLinked.Put(1, 1);
            lruLinked.Put(2, 2);
            Console.WriteLine(lruLinked.Get(1));   // prints 1
            lruLinked.Put(3, 3);                   // evicts key 2
            Console.WriteLine(lruLinked.Get(2));   // prints -1 (not found)
        }
    }
	```

=== "Java"

	```java
    import java.util.*;

    // OrderedDict-based LRU Cache
    class LRUCacheOrdered extends LinkedHashMap<Integer, Integer> {
        private int capacity;

        public LRUCacheOrdered(int capacity) {
            super(capacity, 0.75f, true);
            this.capacity = capacity;
        }

        @Override
        protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
            return size() > capacity;
        }

        public int get(int key) {
            return super.getOrDefault(key, -1);
        }

        public void put(int key, int value) {
            super.put(key, value);
        }
    }

    // LinkedList-based LRU Cache
    class ListNode {
        int key, value;
        ListNode prev, next;

        ListNode(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    class LRUCacheLinkedList {
        private int capacity;
        private Map<Integer, ListNode> cacheMap;
        private ListNode head, tail;

        public LRUCacheLinkedList(int capacity) {
            this.capacity = capacity;
            this.cacheMap = new HashMap<>();
        }

        private void removeNode(ListNode node) {
            if (node.prev != null) node.prev.next = node.next;
            else head = node.next;
            if (node.next != null) node.next.prev = node.prev;
            else tail = node.prev;
        }

        private void insertAtEnd(ListNode node) {
            if (tail != null) tail.next = node;
            node.prev = tail;
            tail = node;
            if (head == null) head = node;
        }

        public int get(int key) {
            if (!cacheMap.containsKey(key)) return -1;
            ListNode node = cacheMap.get(key);
            removeNode(node);
            insertAtEnd(node);
            return node.value;
        }

        public void put(int key, int value) {
            if (cacheMap.containsKey(key)) removeNode(cacheMap.get(key));
            else if (cacheMap.size() == capacity) {
                cacheMap.remove(head.key);
                removeNode(head);
            }
            ListNode newNode = new ListNode(key, value);
            insertAtEnd(newNode);
            cacheMap.put(key, newNode);
        }
    }

    public class Main {
        public static void main(String[] args) {
            System.out.println("Testing LRUCacheOrdered:");
            LRUCacheOrdered lruOrdered = new LRUCacheOrdered(2);
            lruOrdered.put(1, 1);
            lruOrdered.put(2, 2);
            System.out.println(lruOrdered.get(1));  // prints 1
            lruOrdered.put(3, 3);                  // evicts key 2
            System.out.println(lruOrdered.get(2));  // prints -1 (not found)

            System.out.println("\nTesting LRUCacheLinkedList:");
            LRUCacheLinkedList lruLinked = new LRUCacheLinkedList(2);
            lruLinked.put(1, 1);
            lruLinked.put(2, 2);
            System.out.println(lruLinked.get(1));   // prints 1
            lruLinked.put(3, 3);                   // evicts key 2
            System.out.println(lruLinked.get(2));   // prints -1 (not found)
        }
    }
	```

=== "Scala"

	```scala
    import scala.collection.mutable

    // OrderedDict-based LRU Cache using LinkedHashMap
    class LRUCacheOrdered(capacity: Int) extends mutable.LinkedHashMap[Int, Int] {
        override def removeEldestEntry(eldest: mutable.LinkedHashMap.Entry[Int, Int]): Boolean = {
            size > capacity
        }

        def get(key: Int): Int = {
            this.getOrElseUpdate(key, -1)
        }

        def put(key: Int, value: Int): Unit = {
            this.update(key, value)
        }
    }

    // LinkedList-based LRU Cache
    class ListNode(val key: Int, var value: Int) {
        var prev: ListNode = _
        var next: ListNode = _
    }

    class LRUCacheLinkedList(capacity: Int) {
        private val cacheMap = mutable.Map[Int, ListNode]()
        private val dummyHead = new ListNode(0, 0)
        private val dummyTail = new ListNode(0, 0)
        dummyHead.next = dummyTail
        dummyTail.prev = dummyHead

        private def removeNode(node: ListNode): Unit = {
            node.prev.next = node.next
            node.next.prev = node.prev
        }

        private def addNodeToTail(node: ListNode): Unit = {
            node.prev = dummyTail.prev
            node.next = dummyTail
            dummyTail.prev.next = node
            dummyTail.prev = node
        }

        def get(key: Int): Int = {
            cacheMap.get(key) match {
                case Some(node) =>
                    removeNode(node)
                    addNodeToTail(node)
                    node.value
                case None => -1
            }
        }

        def put(key: Int, value: Int): Unit = {
            cacheMap.get(key).foreach(removeNode)
            if (cacheMap.size == capacity) {
                val headNext = dummyHead.next
                cacheMap.remove(headNext.key)
                removeNode(headNext)
            }
            val newNode = new ListNode(key, value)
            cacheMap.put(key, newNode)
            addNodeToTail(newNode)
        }
    }

    object Main extends App {
        println("Testing LRUCacheOrdered:")
        val lruOrdered = new LRUCacheOrdered(2)
        lruOrdered.put(1, 1)
        lruOrdered.put(2, 2)
        println(lruOrdered.get(1))  // prints 1
        lruOrdered.put(3, 3)        // evicts key 2
        println(lruOrdered.get(2))  // prints -1 (not found)

        println("\nTesting LRUCacheLinkedList:")
        val lruLinked = new LRUCacheLinkedList(2)
        lruLinked.put(1, 1)
        lruLinked.put(2, 2)
        println(lruLinked.get(1))   // prints 1
        lruLinked.put(3, 3)         // evicts key 2
        println(lruLinked.get(2))   // prints -1 (not found)
    }
	```

=== "Kotlin"

	```kotlin
    // OrderedDict-based LRU Cache using LinkedHashMap
    class LRUCacheOrdered(private val capacity: Int) : LinkedHashMap<Int, Int>(capacity, 0.75f, true) {
        override fun removeEldestEntry(eldest: MutableMap.MutableEntry<Int, Int>): Boolean {
            return size > capacity
        }

        fun get(key: Int): Int {
            return super.getOrDefault(key, -1)
        }

        fun put(key: Int, value: Int) {
            super.put(key, value)
        }
    }

    // LinkedList-based LRU Cache
    class ListNode(val key: Int, var value: Int) {
        var prev: ListNode? = null
        var next: ListNode? = null
    }

    class LRUCacheLinkedList(private val capacity: Int) {
        private val cacheMap = mutableMapOf<Int, ListNode>()
        private val head = ListNode(0, 0)
        private val tail = ListNode(0, 0)

        init {
            head.next = tail
            tail.prev = head
        }

        private fun removeNode(node: ListNode) {
            node.prev?.next = node.next
            node.next?.prev = node.prev
        }

        private fun addNodeToTail(node: ListNode) {
            node.prev = tail.prev
            node.next = tail
            tail.prev?.next = node
            tail.prev = node
        }

        fun get(key: Int): Int {
            return cacheMap[key]?.let {
                removeNode(it)
                addNodeToTail(it)
                it.value
            } ?: -1
        }

        fun put(key: Int, value: Int) {
            cacheMap[key]?.let {
                removeNode(it)
            } ?: if (cacheMap.size == capacity) {
                head.next?.let {
                    cacheMap.remove(it.key)
                    removeNode(it)
                }
            }
            val newNode = ListNode(key, value)
            cacheMap[key] = newNode
            addNodeToTail(newNode)
        }
    }

    fun main() {
        println("Testing LRUCacheOrdered:")
        val lruOrdered = LRUCacheOrdered(2)
        lruOrdered.put(1, 1)
        lruOrdered.put(2, 2)
        println(lruOrdered.get(1))  // prints 1
        lruOrdered.put(3, 3)        // evicts key 2
        println(lruOrdered.get(2))  // prints -1 (not found)

        println("\nTesting LRUCacheLinkedList:")
        val lruLinked = LRUCacheLinkedList(2)
        lruLinked.put(1, 1)
        lruLinked.put(2, 2)
        println(lruLinked.get(1))   // prints 1
        lruLinked.put(3, 3)         // evicts key 2
        println(lruLinked.get(2))   // prints -1 (not found)
    }
	```

=== "Go"

	```go
    package main

    import (
        "container/list"
        "fmt"
    )

    // OrderedDict-based LRU Cache using container/list and map
    type LRUCacheOrdered struct {
        capacity int
        cache    map[int]*list.Element
        order    *list.List
    }

    type KeyValuePair struct {
        key, value int
    }

    func NewLRUCacheOrdered(capacity int) *LRUCacheOrdered {
        return &LRUCacheOrdered{
            capacity: capacity,
            cache:    make(map[int]*list.Element),
            order:    list.New(),
        }
    }

    func (lru *LRUCacheOrdered) Get(key int) int {
        if elem, found := lru.cache[key]; found {
            lru.order.MoveToFront(elem)
            return elem.Value.(*KeyValuePair).value
        }
        return -1
    }

    func (lru *LRUCacheOrdered) Put(key, value int) {
        if elem, found := lru.cache[key]; found {
            lru.order.MoveToFront(elem)
            elem.Value.(*KeyValuePair).value = value
        } else {
            if lru.order.Len() == lru.capacity {
                backElem := lru.order.Back()
                delete(lru.cache, backElem.Value.(*KeyValuePair).key)
                lru.order.Remove(backElem)
            }
            elem := lru.order.PushFront(&KeyValuePair{key, value})
            lru.cache[key] = elem
        }
    }

    // LinkedList-based LRU Cache
    type ListNode struct {
        key, value int
        prev, next *ListNode
    }

    type LRUCacheLinkedList struct {
        capacity int
        cache    map[int]*ListNode
        head, tail *ListNode
    }

    func NewLRUCacheLinkedList(capacity int) *LRUCacheLinkedList {
        head := &ListNode{}
        tail := &ListNode{}
        head.next = tail
        tail.prev = head
        return &LRUCacheLinkedList{
            capacity: capacity,
            cache:    make(map[int]*ListNode),
            head:     head,
            tail:     tail,
        }
    }

    func (lru *LRUCacheLinkedList) Get(key int) int {
        if node, found := lru.cache[key]; found {
            lru.remove(node)
            lru.addToEnd(node)
            return node.value
        }
        return -1
    }

    func (lru *LRUCacheLinkedList) Put(key, value int) {
        if node, found := lru.cache[key]; found {
            lru.remove(node)
        } else if len(lru.cache) == lru.capacity {
            frontNode := lru.head.next
            lru.remove(frontNode)
            delete(lru.cache, frontNode.key)
        }
        newNode := &ListNode{key: key, value: value}
        lru.addToEnd(newNode)
        lru.cache[key] = newNode
    }

    func (lru *LRUCacheLinkedList) remove(node *ListNode) {
        node.prev.next = node.next
        node.next.prev = node.prev
    }

    func (lru *LRUCacheLinkedList) addToEnd(node *ListNode) {
        node.prev = lru.tail.prev
        node.next = lru.tail
        lru.tail.prev.next = node
        lru.tail.prev = node
    }

    func main() {
        fmt.Println("Testing LRUCacheOrdered:")
        lruOrdered := NewLRUCacheOrdered(2)
        lruOrdered.Put(1, 1)
        lruOrdered.Put(2, 2)
        fmt.Println(lruOrdered.Get(1)) // prints 1
        lruOrdered.Put(3, 3)           // evicts key 2
        fmt.Println(lruOrdered.Get(2))  // prints -1 (not found)

        fmt.Println("\nTesting LRUCacheLinkedList:")
        lruLinked := NewLRUCacheLinkedList(2)
        lruLinked.Put(1, 1)
        lruLinked.Put(2, 2)
        fmt.Println(lruLinked.Get(1)) // prints 1
        lruLinked.Put(3, 3)           // evicts key 2
        fmt.Println(lruLinked.Get(2))  // prints -1 (not found)
    }
	```

=== "TypeScript"

	```typescript
    // OrderedDict-based LRU Cache using Map
    class LRUCacheOrdered {
        private cache: Map<number, number>;
        private capacity: number;

        constructor(capacity: number) {
            this.capacity = capacity;
            this.cache = new Map();
        }

        get(key: number): number {
            if (!this.cache.has(key)) return -1;
            const value = this.cache.get(key)!;
            this.cache.delete(key);  // Remove and re-insert to refresh order
            this.cache.set(key, value);
            return value;
        }

        put(key: number, value: number): void {
            if (this.cache.has(key)) this.cache.delete(key);
            this.cache.set(key, value);
            if (this.cache.size > this.capacity) {
                const oldestKey = this.cache.keys().next().value;
                this.cache.delete(oldestKey);
            }
        }
    }

    // LinkedList-based LRU Cache
    class ListNode {
        key: number;
        value: number;
        prev: ListNode | null = null;
        next: ListNode | null = null;

        constructor(key: number, value: number) {
            this.key = key;
            this.value = value;
        }
    }

    class LRUCacheLinkedList {
        private capacity: number;
        private cache: Map<number, ListNode>;
        private head: ListNode;
        private tail: ListNode;

        constructor(capacity: number) {
            this.capacity = capacity;
            this.cache = new Map();
            this.head = new ListNode(0, 0);
            this.tail = new ListNode(0, 0);
            this.head.next = this.tail;
            this.tail.prev = this.head;
        }

        private remove(node: ListNode): void {
            node.prev!.next = node.next;
            node.next!.prev = node.prev;
        }

        private addToEnd(node: ListNode): void {
            node.prev = this.tail.prev;
            node.next = this.tail;
            this.tail.prev!.next = node;
            this.tail.prev = node;
        }

        get(key: number): number {
            const node = this.cache.get(key);
            if (!node) return -1;
            this.remove(node);
            this.addToEnd(node);
            return node.value;
        }

        put(key: number, value: number): void {
            if (this.cache.has(key)) this.remove(this.cache.get(key)!);
            else if (this.cache.size === this.capacity) {
                const lruNode = this.head.next!;
                this.cache.delete(lruNode.key);
                this.remove(lruNode);
            }
            const newNode = new ListNode(key, value);
            this.cache.set(key, newNode);
            this.addToEnd(newNode);
        }
    }

    // Testing
    console.log("Testing LRUCacheOrdered:");
    const lruOrdered = new LRUCacheOrdered(2);
    lruOrdered.put(1, 1);
    lruOrdered.put(2, 2);
    console.log(lruOrdered.get(1));  // 1
    lruOrdered.put(3, 3);           // evicts key 2
    console.log(lruOrdered.get(2));  // -1

    console.log("\nTesting LRUCacheLinkedList:");
    const lruLinked = new LRUCacheLinkedList(2);
    lruLinked.put(1, 1);
    lruLinked.put(2, 2);
    console.log(lruLinked.get(1));   // 1
    lruLinked.put(3, 3);             // evicts key 2
    console.log(lruLinked.get(2));   // -1
	```

=== "R"

	```r
    # OrderedDict-based LRU Cache using environment
    LRUCacheOrdered <- function(capacity) {
        cache <- new.env(hash = TRUE)
        keys <- character(0)

        get <- function(key) {
            if (exists(as.character(key), envir = cache)) {
            value <- get(as.character(key), envir = cache)
            keys <<- c(setdiff(keys, key), key)  # Move key to end
            return(value)
            }
            return(-1)
        }

        put <- function(key, value) {
            key <- as.character(key)
            if (exists(key, envir = cache)) {
            keys <<- setdiff(keys, key)
            }
            assign(key, value, envir = cache)
            keys <<- c(keys, key)
            if (length(keys) > capacity) {
            remove(list = keys[1], envir = cache)
            keys <<- keys[-1]
            }
        }

        list(get = get, put = put)
    }

    # LinkedList-based LRU Cache using lists
    LRUCacheLinkedList <- function(capacity) {
        cache <- list()
        head <- list(key = NULL, value = NULL, prev = NULL, next = NULL)
        tail <- list(key = NULL, value = NULL, prev = head, next = NULL)
        head$next <- tail

        get <- function(key) {
            if (!is.null(cache[[as.character(key)]])) {
            node <- cache[[as.character(key)]]
            removeNode(node)
            addToEnd(node)
            return(node$value)
            }
            return(-1)
        }

        put <- function(key, value) {
            if (!is.null(cache[[as.character(key)]])) {
            removeNode(cache[[as.character(key)]])
            } else if (length(cache) == capacity) {
            removeHead()
            }
            newNode <- list(key = key, value = value, prev = NULL, next = NULL)
            cache[[as.character(key)]] <<- newNode
            addToEnd(newNode)
        }

        removeNode <- function(node) {
            node$prev$next <- node$next
            node$next$prev <- node$prev
            cache[[as.character(node$key)]] <<- NULL
        }

        addToEnd <- function(node) {
            node$prev <- tail$prev
            node$next <- tail
            tail$prev$next <- node
            tail$prev <- node
        }

        removeHead <- function() {
            node <- head$next
            removeNode(node)
        }

        list(get = get, put = put)
    }

    # Testing
    cat("Testing LRUCacheOrdered:\n")
    lruOrdered <- LRUCacheOrdered(2)
    lruOrdered$put(1, 1)
    lruOrdered$put(2, 2)
    cat(lruOrdered$get(1), "\n")  # prints 1
    lruOrdered$put(3, 3)          # evicts key 2
    cat(lruOrdered$get(2), "\n")  # prints -1

    cat("\nTesting LRUCacheLinkedList:\n")
    lruLinked <- LRUCacheLinkedList(2)
    lruLinked$put(1, 1)
    lruLinked$put(2, 2)
    cat(lruLinked$get(1), "\n")   # prints 1
    lruLinked$put(3, 3)           # evicts key 2
    cat(lruLinked$get(2), "\n")   # prints -1
	```

=== "Julia"

	```julia
    using DataStructures

    # OrderedDict-based LRU Cache
    struct LRUCacheOrdered
        capacity::Int
        cache::OrderedDict{Int, Int}

        function LRUCacheOrdered(capacity::Int)
            new(capacity, OrderedDict{Int, Int}())
        end
    end

    function get(cache::LRUCacheOrdered, key::Int)
        if haskey(cache.cache, key)
            value = cache.cache[key]
            delete!(cache.cache, key)
            cache.cache[key] = value
            return value
        end
        return -1
    end

    function put(cache::LRUCacheOrdered, key::Int, value::Int)
        if haskey(cache.cache, key)
            delete!(cache.cache, key)
        end
        cache.cache[key] = value
        if length(cache.cache) > cache.capacity
            popfirst!(cache.cache)
        end
    end

    # Testing OrderedDict-based LRUCache
    println("Testing LRUCacheOrdered:")
    lruOrdered = LRUCacheOrdered(2)
    put(lruOrdered, 1, 1)
    put(lruOrdered, 2, 2)
    println(get(lruOrdered, 1))  # prints 1
    put(lruOrdered, 3, 3)        # evicts key 2
    println(get(lruOrdered, 2))  # prints -1

    # Julia does not natively support complex custom LinkedList structures as easily as other languages
    # For brevity, `OrderedDict` approach is typically preferred in Julia for LRUCache
	```

## Design Circular Queue

### Problem Description: [LeetCode - Problem 622 - Design Circular Queue](https://leetcode.com/problems/design-circular-queue/description/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Problem Description:
Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle, and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Implement the `MyCircularQueue` class:

- `MyCircularQueue(k)` Initializes the object with the size of the queue to be `k`.
- `int Front()` Gets the front item from the queue. If the queue is empty, return `-1`.
- `int Rear()` Gets the last item from the queue. If the queue is empty, return `-1`.
- `boolean enQueue(int value)` Inserts an element into the circular queue. Return `true` if the operation is successful.
- `boolean deQueue()` Deletes an element from the circular queue. Return `true` if the operation is successful.
- `boolean isEmpty()` Checks whether the circular queue is empty or not.
- `boolean isFull()` Checks whether the circular queue is full or not.

You must solve the problem without using the built-in queue data structure in your programming language. 

Bonus: Can you provide the in-place merge, improved space complexity solution.

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

Here's a detailed explanation of the provided circular queue solution, along with the time and space complexity analysis.

1. **Class Structure**:
   - The `MyCircularQueue` class implements a circular queue data structure using an array (list in Python). 
   - It maintains three key attributes:
     - `__start`: An integer that tracks the index of the front element in the queue.
     - `__size`: An integer that keeps track of the number of elements currently in the queue.
     - `__buffer`: A list of integers that serves as the underlying storage for the queue, initialized to the specified capacity `k`.

2. **Initialization (`__init__` method)**:
   - The constructor takes an integer `k` as an argument, representing the maximum size of the queue. 
   - It initializes the internal variables:
     - `__start` to 0 (indicating the front of the queue).
     - `__size` to 0 (indicating that the queue is initially empty).
     - `__buffer` to a list of zeros of size `k`.

3. **Enqueue (`enQueue` method)**:
   - This method inserts an element at the rear of the queue:
     - It first checks if the queue is full by calling `isFull()`. If the queue is full, it returns `False`.
     - If not full, it calculates the position to insert the new value using the formula `(__start + __size) % len(__buffer)`, which wraps around if necessary.
     - The element is added to the calculated position, and `__size` is incremented by 1.
     - Finally, it returns `True` to indicate that the operation was successful.

4. **Dequeue (`deQueue` method)**:
   - This method removes an element from the front of the queue:
     - It checks if the queue is empty by calling `isEmpty()`. If empty, it returns `False`.
     - If not empty, it updates `__start` to point to the next element in the queue using the formula `(__start + 1)`__ % len(__buffer)`, wrapping around if necessary.
     - `__size` is decremented by 1 to reflect the removal of the element.
     - It returns `True` to indicate that the operation was successful.

5. **Front (`Front` method)**:
   - This method returns the front element of the queue:
     - If the queue is empty (checked using `isEmpty()`), it returns -1.
     - Otherwise, it returns the element at the `__start` index in `__buffer`.

6. **Rear (`Rear` method)**:
   - This method returns the last element of the queue:
     - If the queue is empty, it returns -1.
     - Otherwise, it calculates the position of the last element using `(__start + __size - 1)`__ % len(__buffer)` and returns that element.

7. **isEmpty (`isEmpty` method)**:
   - This method checks if the queue is empty by comparing `__size` to 0. It returns `True` if empty, otherwise `False`.

8. **isFull (`isFull` method)**:
   - This method checks if the queue is full by comparing `__size` to the length of `__buffer`. It returns `True` if full, otherwise `False`.

### Complexity Analysis

1. **Time Complexity**:
   - **Initialization**: 
     - The `__init__` method runs in __`O(k)`__ time due to the creation of the buffer list of size `k`. 
   - **Enqueue (`enQueue`)**: 
     - The method runs in __`O(1)`__ time because it performs a constant number of operations: checking if full, calculating the position, and inserting the element.
   - **Dequeue (`deQueue`)**: 
     - The method also runs in __`O(1)`__ time for similar reasons: checking if empty, updating the start position, and decrementing the size.
   - **Front (`Front`)**: 
     - This method runs in __`O(1)`__ time as it accesses the element directly.
   - **Rear (`Rear`)**: 
     - This method runs in __`O(1)`__ time as it accesses the element directly.
   - **isEmpty**: 
     - This method runs in __`O(1)`__ time as it performs a simple comparison.
   - **isFull**: 
     - This method runs in __`O(1)`__ time as it performs a simple comparison.

2. **Space Complexity**:
   - The space complexity of the circular queue is __`O(k)`__ due to the storage in `__buffer`, where `k` is the capacity of the queue. 
   - Other variables (`__start`, `__size`) consume __`O(1)`__ space, but they do not scale with the input size.

### Summary
The overall complexity for the circular queue operations (enqueue, dequeue, and access) is very efficient, with constant time complexity __`O(1)`__ for each operation and linear space complexity __`O(k)`__ for the buffer, making this implementation well-suited for scenarios requiring a fixed-size queue with efficient insertions and deletions.

### Solutions

---

=== "Python"

	```python
    class MyCircularQueue:
        def __init__(self, k: int):
            """
            Initialize your data structure here. Set the size of the queue to be k.
            :param k: int
            """
            self.__start: int = 0
            self.__size: int = 0
            self.__buffer: list[int] = [0] * k

        def enQueue(self, value: int) -> bool:
            """
            Insert an element into the circular queue. Return true if the operation is successful.
            :param value: int
            :return: bool
            """
            if self.isFull():
                return False
            self.__buffer[(self.__start + self.__size) % len(self.__buffer)] = value
            self.__size += 1
            return True

        def deQueue(self) -> bool:
            """
            Delete an element from the circular queue. Return true if the operation is successful.
            :return: bool
            """
            if self.isEmpty():
                return False
            self.__start = (self.__start + 1)`__ % len(self.__buffer)
            self.__size -= 1
            return True

        def Front(self) -> int:
            """
            Get the front item from the queue.
            :return: int
            """
            return -1 if self.isEmpty() else self.__buffer[self.__start]

        def Rear(self) -> int:
            """
            Get the last item from the queue.
            :return: int
            """
            return -1 if self.isEmpty() else self.__buffer[(self.__start + self.__size - 1)`__ % len(self.__buffer)]

        def isEmpty(self) -> bool:
            """
            Checks whether the circular queue is empty or not.
            :return: bool
            """
            return self.__size == 0

        def isFull(self) -> bool:
            """
            Checks whether the circular queue is full or not.
            :return: bool
            """
            return self.__size == len(self.__buffer)

    def main() -> None:
        # Create a circular queue of capacity 3
        circular_queue = MyCircularQueue(3)
        
        print(circular_queue.enQueue(1))  # returns True
        print(circular_queue.enQueue(2))  # returns True
        print(circular_queue.enQueue(3))  # returns True
        print(circular_queue.enQueue(4))  # returns False, the queue is full

        print(circular_queue.Rear())       # returns 3
        print(circular_queue.isFull())     # returns True

        print(circular_queue.deQueue())    # returns True
        print(circular_queue.enQueue(4))   # returns True

        print(circular_queue.Rear())       # returns 4

        print(circular_queue.Front())      # returns 2

    if __name__ == "__main__":
        main()
    ```

=== "C++"

	```cpp
    #include <iostream>
    #include <vector>

    class MyCircularQueue {
    private:
        int start;
        int size;
        std::vector<int> buffer;

    public:
        MyCircularQueue(int k) : start(0), size(0), buffer(k, 0) {}

        bool enQueue(int value) {
            if (isFull()) return false;
            buffer[(start + size) % buffer.size()] = value;
            size++;
            return true;
        }

        bool deQueue() {
            if (isEmpty()) return false;
            start = (start + 1)`__ % buffer.size();
            size--;
            return true;
        }

        int Front() {
            return isEmpty() ? -1 : buffer[start];
        }

        int Rear() {
            return isEmpty() ? -1 : buffer[(start + size - 1)`__ % buffer.size()];
        }

        bool isEmpty() {
            return size == 0;
        }

        bool isFull() {
            return size == buffer.size();
        }
    };

    int main() {
        MyCircularQueue circularQueue(3);
        
        std::cout << std::boolalpha;
        std::cout << circularQueue.enQueue(1)`__ << std::endl;  // returns true
        std::cout << circularQueue.enQueue(2) << std::endl;  // returns true
        std::cout << circularQueue.enQueue(3) << std::endl;  // returns true
        std::cout << circularQueue.enQueue(4) << std::endl;  // returns false

        std::cout << circularQueue.Rear() << std::endl;       // returns 3
        std::cout << circularQueue.isFull() << std::endl;     // returns true

        std::cout << circularQueue.deQueue() << std::endl;    // returns true
        std::cout << circularQueue.enQueue(4) << std::endl;   // returns true

        std::cout << circularQueue.Rear() << std::endl;       // returns 4
        std::cout << circularQueue.Front() << std::endl;      // returns 2

        return 0;
    }
    ```

=== "Rust"

	```rust
    struct MyCircularQueue {
        start: usize,
        size: usize,
        buffer: Vec<i32>,
    }

    impl MyCircularQueue {
        fn new(k: i32) -> Self {
            Self {
                start: 0,
                size: 0,
                buffer: vec![0; k as usize],
            }
        }

        fn en_queue(&mut self, value: i32) -> bool {
            if self.is_full() {
                return false;
            }
            self.buffer[(self.start + self.size) % self.buffer.len()] = value;
            self.size += 1;
            true
        }

        fn de_queue(&mut self) -> bool {
            if self.is_empty() {
                return false;
            }
            self.start = (self.start + 1)`__ % self.buffer.len();
            self.size -= 1;
            true
        }

        fn front(&self) -> i32 {
            if self.is_empty() {
                -1
            } else {
                self.buffer[self.start]
            }
        }

        fn rear(&self) -> i32 {
            if self.is_empty() {
                -1
            } else {
                self.buffer[(self.start + self.size - 1)`__ % self.buffer.len()]
            }
        }

        fn is_empty(&self) -> bool {
            self.size == 0
        }

        fn is_full(&self) -> bool {
            self.size == self.buffer.len()
        }
    }

    fn main() {
        let mut circular_queue = MyCircularQueue::new(3);
        
        println!("{}", circular_queue.en_queue(1));  // returns true
        println!("{}", circular_queue.en_queue(2));  // returns true
        println!("{}", circular_queue.en_queue(3));  // returns true
        println!("{}", circular_queue.en_queue(4));  // returns false

        println!("{}", circular_queue.rear());        // returns 3
        println!("{}", circular_queue.is_full());     // returns true

        println!("{}", circular_queue.de_queue());     // returns true
        println!("{}", circular_queue.en_queue(4));   // returns true

        println!("{}", circular_queue.rear());        // returns 4
        println!("{}", circular_queue.front());       // returns 2
    }
    ```

=== "C#"

	```csharp
    using System;

    public class MyCircularQueue {
        private int start;
        private int size;
        private int[] buffer;

        public MyCircularQueue(int k) {
            start = 0;
            size = 0;
            buffer = new int[k];
        }

        public bool EnQueue(int value) {
            if (IsFull()) return false;
            buffer[(start + size) % buffer.Length] = value;
            size++;
            return true;
        }

        public bool DeQueue() {
            if (IsEmpty()) return false;
            start = (start + 1)`__ % buffer.Length;
            size--;
            return true;
        }

        public int Front() {
            return IsEmpty() ? -1 : buffer[start];
        }

        public int Rear() {
            return IsEmpty() ? -1 : buffer[(start + size - 1)`__ % buffer.Length];
        }

        public bool IsEmpty() {
            return size == 0;
        }

        public bool IsFull() {
            return size == buffer.Length;
        }
    }

    class Program {
        static void Main(string[] args) {
            MyCircularQueue circularQueue = new MyCircularQueue(3);
            
            Console.WriteLine(circularQueue.EnQueue(1));  // returns true
            Console.WriteLine(circularQueue.EnQueue(2));  // returns true
            Console.WriteLine(circularQueue.EnQueue(3));  // returns true
            Console.WriteLine(circularQueue.EnQueue(4));  // returns false

            Console.WriteLine(circularQueue.Rear());      // returns 3
            Console.WriteLine(circularQueue.IsFull());    // returns true

            Console.WriteLine(circularQueue.DeQueue());   // returns true
            Console.WriteLine(circularQueue.EnQueue(4));   // returns true

            Console.WriteLine(circularQueue.Rear());      // returns 4
            Console.WriteLine(circularQueue.Front());     // returns 2
        }
    }
    ```

=== "Java"

	```java
    class MyCircularQueue {
        private int start;
        private int size;
        private int[] buffer;

        public MyCircularQueue(int k) {
            start = 0;
            size = 0;
            buffer = new int[k];
        }

        public boolean enQueue(int value) {
            if (isFull()) return false;
            buffer[(start + size) % buffer.length] = value;
            size++;
            return true;
        }

        public boolean deQueue() {
            if (isEmpty()) return false;
            start = (start + 1)`__ % buffer.length;
            size--;
            return true;
        }

        public int Front() {
            return isEmpty() ? -1 : buffer[start];
        }

        public int Rear() {
            return isEmpty() ? -1 : buffer[(start + size - 1)`__ % buffer.length];
        }

        public boolean isEmpty() {
            return size == 0;
        }

        public boolean isFull() {
            return size == buffer.length;
        }
    }

    public class Main {
        public static void main(String[] args) {
            MyCircularQueue circularQueue = new MyCircularQueue(3);
            
            System.out.println(circularQueue.enQueue(1));  // returns true
            System.out.println(circularQueue.enQueue(2));  // returns true
            System.out.println(circularQueue.enQueue(3));  // returns true
            System.out.println(circularQueue.enQueue(4));  // returns false

            System.out.println(circularQueue.Rear());      // returns 3
            System.out.println(circularQueue.isFull());    // returns true

            System.out.println(circularQueue.deQueue());   // returns true
            System.out.println(circularQueue.enQueue(4));   // returns true

            System.out.println(circularQueue.Rear());      // returns 4
            System.out.println(circularQueue.Front());     // returns 2
        }
    }
    ```

=== "Scala"

	```scala
    class MyCircularQueue(k: Int) {
        private var start: Int = 0
        private var size: Int = 0
        private val buffer: Array[Int] = new Array[Int](k)

        def enQueue(value: Int): Boolean = {
            if (isFull()) return false
            buffer((start + size) % buffer.length) = value
            size += 1
            true
        }

        def deQueue(): Boolean = {
            if (isEmpty()) return false
            start = (start + 1)`__ % buffer.length
            size -= 1
            true
        }

        def Front(): Int = {
            if (isEmpty()) -1 else buffer(start)
        }

        def Rear(): Int = {
            if (isEmpty()) -1 else buffer((start + size - 1)`__ % buffer.length)
        }

        def isEmpty(): Boolean = size == 0

        def isFull(): Boolean = size == buffer.length
    }

    object Main extends App {
        val circularQueue = new MyCircularQueue(3)

        println(circularQueue.enQueue(1))  // returns true
        println(circularQueue.enQueue(2))  // returns true
        println(circularQueue.enQueue(3))  // returns true
        println(circularQueue.enQueue(4))  // returns false

        println(circularQueue.Rear())       // returns 3
        println(circularQueue.isFull())     // returns true

        println(circularQueue.deQueue())    // returns true
        println(circularQueue.enQueue(4))   // returns true

        println(circularQueue.Rear())       // returns 4
        println(circularQueue.Front())      // returns 2
    }
    ```

=== "Kotlin"

	```kotlin
    class MyCircularQueue(k: Int) {
        private var start = 0
        private var size = 0
        private val buffer: IntArray = IntArray(k)

        fun enQueue(value: Int): Boolean {
            if (isFull()) return false
            buffer[(start + size) % buffer.size] = value
            size++
            return true
        }

        fun deQueue(): Boolean {
            if (isEmpty()) return false
            start = (start + 1)`__ % buffer.size
            size--
            return true
        }

        fun Front(): Int {
            return if (isEmpty()) -1 else buffer[start]
        }

        fun Rear(): Int {
            return if (isEmpty()) -1 else buffer[(start + size - 1)`__ % buffer.size]
        }

        fun isEmpty(): Boolean {
            return size == 0
        }

        fun isFull(): Boolean {
            return size == buffer.size
        }
    }

    fun main() {
        val circularQueue = MyCircularQueue(3)

        println(circularQueue.enQueue(1))  // returns true
        println(circularQueue.enQueue(2))  // returns true
        println(circularQueue.enQueue(3))  // returns true
        println(circularQueue.enQueue(4))  // returns false

        println(circularQueue.Rear())       // returns 3
        println(circularQueue.isFull())     // returns true

        println(circularQueue.deQueue())    // returns true
        println(circularQueue.enQueue(4))   // returns true

        println(circularQueue.Rear())       // returns 4
        println(circularQueue.Front())      // returns 2
    }
    ```

=== "Go"

	```go
    package main

    import "fmt"

    type MyCircularQueue struct {
        start  int
        size   int
        buffer []int
    }

    func Constructor(k int) MyCircularQueue {
        return MyCircularQueue{
            start:  0,
            size:   0,
            buffer: make([]int, k),
        }
    }

    func (this *MyCircularQueue) EnQueue(value int) bool {
        if this.IsFull() {
            return false
        }
        this.buffer[(this.start+this.size)%len(this.buffer)] = value
        this.size++
        return true
    }

    func (this *MyCircularQueue) DeQueue() bool {
        if this.IsEmpty() {
            return false
        }
        this.start = (this.start + 1)`__ % len(this.buffer)
        this.size--
        return true
    }

    func (this *MyCircularQueue) Front() int {
        if this.IsEmpty() {
            return -1
        }
        return this.buffer[this.start]
    }

    func (this *MyCircularQueue) Rear() int {
        if this.IsEmpty() {
            return -1
        }
        return this.buffer[(this.start+this.size-1)%len(this.buffer)]
    }

    func (this *MyCircularQueue) IsEmpty() bool {
        return this.size == 0
    }

    func (this *MyCircularQueue) IsFull() bool {
        return this.size == len(this.buffer)
    }

    func main() {
        circularQueue := Constructor(3)

        fmt.Println(circularQueue.EnQueue(1))  // returns true
        fmt.Println(circularQueue.EnQueue(2))  // returns true
        fmt.Println(circularQueue.EnQueue(3))  // returns true
        fmt.Println(circularQueue.EnQueue(4))  // returns false

        fmt.Println(circularQueue.Rear())       // returns 3
        fmt.Println(circularQueue.IsFull())     // returns true

        fmt.Println(circularQueue.DeQueue())    // returns true
        fmt.Println(circularQueue.EnQueue(4))   // returns true

        fmt.Println(circularQueue.Rear())       // returns 4
        fmt.Println(circularQueue.Front())      // returns 2
    }
    ```

=== "TypeScript"

	```typescript
    class MyCircularQueue {
        private start: number;
        private size: number;
        private buffer: number[];

        constructor(k: number) {
            this.start = 0;
            this.size = 0;
            this.buffer = new Array(k).fill(0);
        }

        enQueue(value: number): boolean {
            if (this.isFull()) return false;
            this.buffer[(this.start + this.size) % this.buffer.length] = value;
            this.size++;
            return true;
        }

        deQueue(): boolean {
            if (this.isEmpty()) return false;
            this.start = (this.start + 1)`__ % this.buffer.length;
            this.size--;
            return true;
        }

        Front(): number {
            return this.isEmpty() ? -1 : this.buffer[this.start];
        }

        Rear(): number {
            return this.isEmpty() ? -1 : this.buffer[(this.start + this.size - 1)`__ % this.buffer.length];
        }

        isEmpty(): boolean {
            return this.size === 0;
        }

        isFull(): boolean {
            return this.size === this.buffer.length;
        }
    }

    const main = () => {
        const circularQueue = new MyCircularQueue(3);
        
        console.log(circularQueue.enQueue(1));  // returns true
        console.log(circularQueue.enQueue(2));  // returns true
        console.log(circularQueue.enQueue(3));  // returns true
        console.log(circularQueue.enQueue(4));  // returns false

        console.log(circularQueue.Rear());       // returns 3
        console.log(circularQueue.isFull());     // returns true

        console.log(circularQueue.deQueue());    // returns true
        console.log(circularQueue.enQueue(4));   // returns true

        console.log(circularQueue.Rear());       // returns 4
        console.log(circularQueue.Front());      // returns 2
    };

    main();
    ```

=== "R"

	```r
    MyCircularQueue <- setRefClass(
        "MyCircularQueue",
        fields = list(
            start = "numeric",
            size = "numeric",
            buffer = "numeric"
        ),
        methods = list(
            initialize = function(k) {
                start <<- 1
                size <<- 0
                buffer <<- numeric(k)
            },
            enQueue = function(value) {
                if (isFull()) return(FALSE)
                buffer[(start + size - 1)`__ %% length(buffer) + 1] <<- value
                size <<- size + 1
                return(TRUE)
            },
            deQueue = function() {
                if (isEmpty()) return(FALSE)
                start <<- (start %% length(buffer)) + 1
                size <<- size - 1
                return(TRUE)
            },
            Front = function() {
                if (isEmpty()) return(-1)
                return(buffer[start])
            },
            Rear = function() {
                if (isEmpty()) return(-1)
                return(buffer[(start + size - 2) %% length(buffer) + 1])
            },
            isEmpty = function() {
                return(size == 0)
            },
            isFull = function() {
                return(size == length(buffer))
            }
        )
    )

    main <- function() {
        circularQueue <- MyCircularQueue$new(3)
        
        print(circularQueue$enQueue(1))  # returns TRUE
        print(circularQueue$enQueue(2))  # returns TRUE
        print(circularQueue$enQueue(3))  # returns TRUE
        print(circularQueue$enQueue(4))  # returns FALSE

        print(circularQueue$Rear())       # returns 3
        print(circularQueue$isFull())     # returns TRUE

        print(circularQueue$deQueue())    # returns TRUE
        print(circularQueue$enQueue(4))   # returns TRUE

        print(circularQueue$Rear())       # returns 4
        print(circularQueue$Front())      # returns 2
    }

    main()
    ```

=== "Julia"

	```julia
    struct MyCircularQueue
        start::Int
        size::Int
        buffer::Vector{Int}
    end

    function MyCircularQueue(k::Int)
        return MyCircularQueue(1, 0, zeros(Int, k))
    end

    function enQueue(queue::MyCircularQueue, value::Int)
        if isFull(queue)
            return false
        end
        queue.buffer[(queue.start + queue.size - 1)`__ % length(queue.buffer) + 1] = value
        queue.size += 1
        return true
    end

    function deQueue(queue::MyCircularQueue)
        if isEmpty(queue)
            return false
        end
        queue.start = (queue.start % length(queue.buffer)) + 1
        queue.size -= 1
        return true
    end

    function Front(queue::MyCircularQueue)
        return isEmpty(queue) ? -1 : queue.buffer[queue.start]
    end

    function Rear(queue::MyCircularQueue)
        return isEmpty(queue) ? -1 : queue.buffer[(queue.start + queue.size - 2) % length(queue.buffer) + 1]
    end

    function isEmpty(queue::MyCircularQueue)
        return queue.size == 0
    end

    function isFull(queue::MyCircularQueue)
        return queue.size == length(queue.buffer)
    end

    function main()
        circularQueue = MyCircularQueue(3)

        println(enQueue(circularQueue, 1))  # returns true
        println(enQueue(circularQueue, 2))  # returns true
        println(enQueue(circularQueue, 3))  # returns true
        println(enQueue(circularQueue, 4))  # returns false

        println(Rear(circularQueue))         # returns 3
        println(isFull(circularQueue))       # returns true

        println(deQueue(circularQueue))      # returns true
        println(enQueue(circularQueue, 4))   # returns true

        println(Rear(circularQueue))         # returns 4
        println(Front(circularQueue))        # returns 2
    end

    main()
    ```
