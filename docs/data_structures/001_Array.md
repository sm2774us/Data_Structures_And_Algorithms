---
tags:
  - Original
---

# Array

## Merge Two Sorted Lists

### Problem Description:
Merge two sorted lists.

Bonus: Can you provide the in-place merge, improved space complexity solution.

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

#### Solution 1: Returning a New List

**Code Walkthrough:**
1. **Initialize Pointers and Result List:**  
   You create an empty list `result` to store the merged output and use two pointers `i` and `j` initialized to `0`. These pointers will track the current elements in `list1` and `list2`, respectively.

2. **Merge Two Lists Using Pointers:**
   - While both pointers haven't reached the end of their respective lists:
     - If the current element of `list1` (`list1[i]`) is smaller, it's appended to `result`, and the pointer `i` is incremented.
     - Otherwise, the current element from `list2` (`list2[j]`) is appended, and `j` is incremented.

3. **Appending Remaining Elements:**
   - After the loop, there may be leftover elements in `list1` or `list2`. These are added to `result` using the `extend` method.

4. **Return the Result List:**  
   The merged list is returned.

#### **Time Complexity:**
- Each element in both `list1` and `list2` is processed exactly once, so the time complexity is:
  - **O(n + m)** where `n` is the length of `list1` and `m` is the length of `list2`.

#### **Space Complexity:**
- Since you're creating a new list (`result`) to store the merged elements, the space complexity is:
  - **O(n + m)** because the new list will hold `n + m` elements.

---

#### Solution 2: In-Place Merge

**Code Walkthrough:**
1. **Extend `list1`:**  
   - You first extend `list1` to have enough space for the elements of `list2` by appending `n` zeros, where `n` is the length of `list2`.

2. **Initialize Indices for Reverse Traversal:**
   - You set up three pointers:
     - `i`: Points to the last element of the original `list1`.
     - `j`: Points to the last element of `list2`.
     - `k`: Points to the last position of the newly extended `list1`.

3. **Merge in Reverse Order:**
   - While both `i` and `j` are valid (i.e., not negative), compare the elements at `list1[i]` and `list2[j]`.
   - Place the larger element at position `k` in `list1` and move the corresponding pointer (`i` or `j`) and the pointer `k` backward.

4. **Handle Remaining Elements of `list2`:**
   - If there are any elements left in `list2` (i.e., if `j >= 0`), copy them into `list1`.

5. **No Need to Return `list1`:**  
   - Since the merge happens in-place, `list1` is modified directly and no new list is returned.

#### **Time Complexity:**
- The time complexity is again **O(n + m)** because each element from `list1` and `list2` is compared and placed in the appropriate position.

#### **Space Complexity:**
- The space complexity is **O(n)**, where `n` is the size of `list2`, because the only additional memory used is for extending `list1` to fit the elements from `list2`. There is no need for an extra list to hold the merged result.

---

### Summary of Time and Space Complexity:

| Solution | Time Complexity | Space Complexity      |
|----------|-----------------|-----------------------|
| Solution 1 (New List)       | **O(n + m)** | **O(n + m)** (new list) |
| Solution 2 (In-Place Merge) | **O(n + m)** | **O(n)** (in-place)     |

- Both solutions have the same time complexity of **O(n + m)**, but the space complexity differs. Solution 1 creates a new list for the merged result, while Solution 2 does the merge in-place, thus saving space.

### Solutions

=== "Python"

	```python
	# Solution-1 : Return New List
	# TC = O(n + m), SC = O(n + m)

	from typing import List

    class Solution:
		def merge_lists(list1: List[int], list2: List[int]) -> List[int]:
			result = []
			i, j = 0, 0
			
			while i < len(list1) and j < len(list2):
				if list1[i] < list2[j]:
					result.append(list1[i])
					i += 1
				else:
					result.append(list2[j])
					j += 1
			
			# Append remaining elements
			result.extend(list1[i:])
			result.extend(list2[j:])
			
			return result

		def merge_lists(list1: List[int], list2: List[int]) -> None:
			m, n = len(list1), len(list2)
			list1.extend([0] * n)  # Extend list1 to accommodate elements from list2

			i, j, k = m - 1, n - 1, m + n - 1

			# Merge in reverse order
			while i >= 0 and j >= 0:
				if list1[i] > list2[j]:
					list1[k] = list1[i]
					i -= 1
				else:
					list1[k] = list2[j]
					j -= 1
				k -= 1

			# Copy remaining elements of list2, if any
			while j >= 0:
				list1[k] = list2[j]
				j -= 1
				k -= 1


	def main() -> None:
		list1: List[int] = [1, 2, 4]
		list2: List[int] = [1, 3, 4]

		list: List[int] = merge_lists(list1, list2)

		print(f"Merged List: {list}")

		list1 = [1, 2, 4]
		list2 = [1, 3, 4]

		merge_lists(list1, list2)

		print(f"Merged List: {list1}")

	if __name__ == "__main__":
		main()

	## Output:
	## Merged List: [1, 1, 2, 3, 4, 4]
	```

=== "C++"

	```cpp
	#include <iostream>
	#include <vector>

	class Solution {
	public:
		// Solution 1: Return New List
		std::vector<int> mergeLists(const std::vector<int>& list1, const std::vector<int>& list2) {
			std::vector<int> result;
			size_t i = 0, j = 0;

			while (i < list1.size() && j < list2.size()) {
				if (list1[i] < list2[j]) {
					result.push_back(list1[i]);
					++i;
				} else {
					result.push_back(list2[j]);
					++j;
				}
			}
			result.insert(result.end(), list1.begin() + i, list1.end());
			result.insert(result.end(), list2.begin() + j, list2.end());

			return result;
		}

		// Solution 2: In-place Merge
		void mergeListsInPlace(std::vector<int>& list1, std::vector<int>& list2) {
			size_t m = list1.size();
			size_t n = list2.size();
			list1.resize(m + n);

			size_t i = m - 1, j = n - 1, k = m + n - 1;

			while (i != SIZE_MAX && j != SIZE_MAX) {
				if (list1[i] > list2[j]) {
					list1[k--] = list1[i--];
				} else {
					list1[k--] = list2[j--];
				}
			}
			while (j != SIZE_MAX) {
				list1[k--] = list2[j--];
			}
		}
	};

	int main() {
		Solution solution;

		// Example for Solution 1
		std::vector<int> list1 = {1, 2, 4};
		std::vector<int> list2 = {1, 3, 4};
		auto mergedList = solution.mergeLists(list1, list2);
		std::cout << "Merged List (New List): ";
		for (int num : mergedList) {
			std::cout << num << " ";
		}
		std::cout << std::endl;

		// Example for Solution 2
		list1 = {1, 2, 4};
		list2 = {1, 3, 4};
		solution.mergeListsInPlace(list1, list2);
		std::cout << "Merged List (In-place): ";
		for (int num : list1) {
			std::cout << num << " ";
		}
		std::cout << std::endl;

		return 0;
	}
	```

=== "Rust"

	```rust
	struct Solution;

	impl Solution {
		// Solution 1: Return New List
		pub fn merge_lists(list1: &[i32], list2: &[i32]) -> Vec<i32> {
			let mut result = Vec::new();
			let (mut i, mut j) = (0, 0);

			while i < list1.len() && j < list2.len() {
				if list1[i] < list2[j] {
					result.push(list1[i]);
					i += 1;
				} else {
					result.push(list2[j]);
					j += 1;
				}
			}

			result.extend_from_slice(&list1[i..]);
			result.extend_from_slice(&list2[j..]);

			result
		}

		// Solution 2: In-place Merge
		pub fn merge_lists_in_place(list1: &mut Vec<i32>, list2: &[i32]) {
			let m = list1.len();
			let n = list2.len();
			list1.extend(vec![0; n]);

			let (mut i, mut j, mut k) = (m as isize - 1, n as isize - 1, (m + n) as isize - 1);

			while i >= 0 && j >= 0 {
				if list1[i as usize] > list2[j as usize] {
					list1[k as usize] = list1[i as usize];
					i -= 1;
				} else {
					list1[k as usize] = list2[j as usize];
					j -= 1;
				}
				k -= 1;
			}

			while j >= 0 {
				list1[k as usize] = list2[j as usize];
				j -= 1;
				k -= 1;
			}
		}
	}

	fn main() {
		let solution = Solution;

		// Example for Solution 1
		let list1 = vec![1, 2, 4];
		let list2 = vec![1, 3, 4];
		let merged_list = solution.merge_lists(&list1, &list2);
		println!("Merged List (New List): {:?}", merged_list);

		// Example for Solution 2
		let mut list1 = vec![1, 2, 4];
		let list2 = vec![1, 3, 4];
		solution.merge_lists_in_place(&mut list1, &list2);
		println!("Merged List (In-place): {:?}", list1);
	}

	```

=== "C#"

	```csharp
	using System;
	using System.Collections.Generic;

	public class Solution {
		// Solution 1: Return New List
		public List<int> MergeLists(List<int> list1, List<int> list2) {
			List<int> result = new List<int>();
			int i = 0, j = 0;

			while (i < list1.Count && j < list2.Count) {
				if (list1[i] < list2[j]) {
					result.Add(list1[i]);
					i++;
				} else {
					result.Add(list2[j]);
					j++;
				}
			}

			result.AddRange(list1.GetRange(i, list1.Count - i));
			result.AddRange(list2.GetRange(j, list2.Count - j));

			return result;
		}

		// Solution 2: In-place Merge
		public void MergeListsInPlace(List<int> list1, List<int> list2) {
			int m = list1.Count;
			int n = list2.Count;
			list1.AddRange(new int[n]);

			int i = m - 1, j = n - 1, k = m + n - 1;

			while (i >= 0 && j >= 0) {
				if (list1[i] > list2[j]) {
					list1[k] = list1[i];
					i--;
				} else {
					list1[k] = list2[j];
					j--;
				}
				k--;
			}

			while (j >= 0) {
				list1[k] = list2[j];
				j--;
				k--;
			}
		}
	}

	public class Program {
		public static void Main() {
			var solution = new Solution();

			// Example for Solution 1
			var list1 = new List<int> { 1, 2, 4 };
			var list2 = new List<int> { 1, 3, 4 };
			var mergedList = solution.MergeLists(list1, list2);
			Console.WriteLine("Merged List (New List): " + string.Join(", ", mergedList));

			// Example for Solution 2
			list1 = new List<int> { 1, 2, 4 };
			list2 = new List<int> { 1, 3, 4 };
			solution.MergeListsInPlace(list1, list2);
			Console.WriteLine("Merged List (In-place): " + string.Join(", ", list1));
		}
	}
	```

=== "Java"

	```java
	import java.util.ArrayList;
	import java.util.List;

	class Solution {
		// Solution 1: Return New List
		public List<Integer> mergeLists(List<Integer> list1, List<Integer> list2) {
			List<Integer> result = new ArrayList<>();
			int i = 0, j = 0;

			while (i < list1.size() && j < list2.size()) {
				if (list1.get(i) < list2.get(j)) {
					result.add(list1.get(i));
					i++;
				} else {
					result.add(list2.get(j));
					j++;
				}
			}
			result.addAll(list1.subList(i, list1.size()));
			result.addAll(list2.subList(j, list2.size()));

			return result;
		}

		// Solution 2: In-place Merge
		public void mergeListsInPlace(List<Integer> list1, List<Integer> list2) {
			int m = list1.size();
			int n = list2.size();
			for (int k = 0; k < n; k++) {
				list1.add(0); // Extending list1 to accommodate list2
			}

			int i = m - 1, j = n - 1, k = m + n - 1;

			while (i >= 0 && j >= 0) {
				if (list1.get(i) > list2.get(j)) {
					list1.set(k--, list1.get(i--));
				} else {
					list1.set(k--, list2.get(j--));
				}
			}

			while (j >= 0) {
				list1.set(k--, list2.get(j--));
			}
		}
	}

	public class Main {
		public static void main(String[] args) {
			Solution solution = new Solution();

			// Example for Solution 1
			List<Integer> list1 = new ArrayList<>(List.of(1, 2, 4));
			List<Integer> list2 = new ArrayList<>(List.of(1, 3, 4));
			List<Integer> mergedList = solution.mergeLists(list1, list2);
			System.out.println("Merged List (New List): " + mergedList);

			// Example for Solution 2
			list1 = new ArrayList<>(List.of(1, 2, 4));
			list2 = new ArrayList<>(List.of(1, 3, 4));
			solution.mergeListsInPlace(list1, list2);
			System.out.println("Merged List (In-place): " + list1);
		}
	}
	```

=== "Scala"

	```scala
	object Solution {
	  // Solution 1: Return New List
	  def mergeLists(list1: List[Int], list2: List[Int]): List[Int] = {
		  (list1, list2) match {
		    case (Nil, _) => list2
		    case (_, Nil) => list1
		    case (h1 :: t1, h2 :: t2) =>
			    if (h1 < h2) h1 :: mergeLists(t1, list2)
			    else h2 :: mergeLists(list1, t2)
		  }
	  }

	  // Solution 2: In-place Merge
	  def mergeListsInPlace(list1: Array[Int], m: Int, list2: Array[Int], n: Int): Unit = {
		  var i = m - 1
		  var j = n - 1
		  var k = m + n - 1

		  while (i >= 0 && j >= 0) {
		    if (list1(i) > list2(j)) {
			    list1(k) = list1(i)
			    i -= 1
		    } else {
			    list1(k) = list2(j)
			    j -= 1
		    }
		    k -= 1
		  }

		  while (j >= 0) {
		    list1(k) = list2(j)
		    j -= 1
		    k -= 1
		  }
	  }

	  def main(args: Array[String]): Unit = {
		  // Example for Solution 1
		  val list1 = List(1, 2, 4)
		  val list2 = List(1, 3, 4)
		  val mergedList = mergeLists(list1, list2)
		  println(s"Merged List (New List): $mergedList")

		  // Example for Solution 2
		  val listArray1 = Array(1, 2, 4) ++ Array.fill(3)(0) // Allocating space for list2
		  val listArray2 = Array(1, 3, 4)
		  mergeListsInPlace(listArray1, 3, listArray2, 3)
		  println(s"Merged List (In-place): ${listArray1.mkString(", ")}")
	  }
	}
	```

=== "Kotlin"

	```kotlin
	class Solution {
		// Solution 1: Return New List
		fun mergeLists(list1: List<Int>, list2: List<Int>): List<Int> {
			val result = mutableListOf<Int>()
			var i = 0
			var j = 0

			while (i < list1.size && j < list2.size) {
				if (list1[i] < list2[j]) {
					result.add(list1[i])
					i++
				} else {
					result.add(list2[j])
					j++
				}
			}

			result.addAll(list1.subList(i, list1.size))
			result.addAll(list2.subList(j, list2.size))

			return result
		}

		// Solution 2: In-place Merge
		fun mergeListsInPlace(list1: MutableList<Int>, list2: List<Int>) {
			val m = list1.size
			val n = list2.size
			list1.addAll(MutableList(n) { 0 }) // Extend list1

			var i = m - 1
			var j = n - 1
			var k = m + n - 1

			while (i >= 0 && j >= 0) {
				if (list1[i] > list2[j]) {
					list1[k--] = list1[i--]
				} else {
					list1[k--] = list2[j--]
				}
			}

			while (j >= 0) {
				list1[k--] = list2[j--]
			}
		}
	}

	fun main() {
		val solution = Solution()

		// Example for Solution 1
		val list1 = listOf(1, 2, 4)
		val list2 = listOf(1, 3, 4)
		val mergedList = solution.mergeLists(list1, list2)
		println("Merged List (New List): $mergedList")

		// Example for Solution 2
		val mutableList1 = mutableListOf(1, 2, 4)
		val mutableList2 = listOf(1, 3, 4)
		solution.mergeListsInPlace(mutableList1, mutableList2)
		println("Merged List (In-place): $mutableList1")
	}
	```

=== "Go"

	```go
	package main

	import (
		"fmt"
	)

	type Solution struct{}

	// Solution 1: Return New List
	func (s *Solution) mergeLists(list1 []int, list2 []int) []int {
		result := []int{}
		i, j := 0, 0

		for i < len(list1) && j < len(list2) {
			if list1[i] < list2[j] {
				result = append(result, list1[i])
				i++
			} else {
				result = append(result, list2[j])
				j++
			}
		}

		result = append(result, list1[i:]...)
		result = append(result, list2[j:]...)

		return result
	}

	// Solution 2: In-place Merge
	func (s *Solution) mergeListsInPlace(list1 *[]int, list2 []int) {
		m := len(*list1)
		n := len(list2)
		*list1 = append(*list1, make([]int, n)...)

		i, j, k := m-1, n-1, m+n-1

		for i >= 0 && j >= 0 {
			if (*list1)[i] > list2[j] {
				(*list1)[k] = (*list1)[i]
				i--
			} else {
				(*list1)[k] = list2[j]
				j--
			}
			k--
		}

		for j >= 0 {
			(*list1)[k] = list2[j]
			j--
			k--
		}
	}

	func main() {
		solution := Solution{}

		// Example for Solution 1
		list1 := []int{1, 2, 4}
		list2 := []int{1, 3, 4}
		mergedList := solution.mergeLists(list1, list2)
		fmt.Println("Merged List (New List):", mergedList)

		// Example for Solution 2
		list1 = []int{1, 2, 4}
		list2 = []int{1, 3, 4}
		solution.mergeListsInPlace(&list1, list2)
		fmt.Println("Merged List (In-place):", list1)
	}
	```

=== "TypeScript"

	```typescript
	class Solution {
		// Solution 1: Return New List
		mergeLists(list1: number[], list2: number[]): number[] {
			const result: number[] = [];
			let i = 0, j = 0;

			while (i < list1.length && j < list2.length) {
				if (list1[i] < list2[j]) {
					result.push(list1[i]);
					i++;
				} else {
					result.push(list2[j]);
					j++;
				}
			}

			result.push(...list1.slice(i));
			result.push(...list2.slice(j));

			return result;
		}

		// Solution 2: In-place Merge
		mergeListsInPlace(list1: number[], list2: number[]): void {
			const m = list1.length;
			const n = list2.length;
			list1.length += n; // Extend list1

			let i = m - 1, j = n - 1, k = m + n - 1;

			while (i >= 0 && j >= 0) {
				if (list1[i] > list2[j]) {
					list1[k--] = list1[i--];
				} else {
					list1[k--] = list2[j--];
				}
			}

			while (j >= 0) {
				list1[k--] = list2[j--];
			}
		}
	}

	function main() {
		const solution = new Solution();

		// Example for Solution 1
		const list1 = [1, 2, 4];
		const list2 = [1, 3, 4];
		const mergedList = solution.mergeLists(list1, list2);
		console.log("Merged List (New List):", mergedList);

		// Example for Solution 2
		const listArray1 = [1, 2, 4];
		const listArray2 = [1, 3, 4];
		solution.mergeListsInPlace(listArray1, listArray2);
		console.log("Merged List (In-place):", listArray1);
	}

	main();
	```

=== "R"

	```r
    #library(R6)
    #Solution <- R6::R6Class("Solution",
	Solution <- setRefClass(
	  "Solution",
	  fields = list(),
	  
	  methods = list(
		# Solution 1: Return New List
		mergeLists = function(list1, list2) {
		  result <- integer(0)
		  i <- 1
		  j <- 1
		  
		  while (i <= length(list1) && j <= length(list2)) {
			if (list1[i] < list2[j]) {
			  result <- c(result, list1[i])
			  i <- i + 1
			} else {
			  result <- c(result, list2[j])
			  j <- j + 1
			}
		  }
		  
		  result <- c(result, list1[i:length(list1)], list2[j:length(list2)])
		  return(result)
		},
		
		# Solution 2: In-place Merge
		mergeListsInPlace = function(list1, list2) {
		  m <- length(list1)
		  n <- length(list2)
		  list1 <- c(list1, rep(0, n)) # Extend list1
		  
		  i <- m
		  j <- n
		  k <- m + n
		  
		  while (i > 0 && j > 0) {
			if (list1[i] > list2[j]) {
			  list1[k] <- list1[i]
			  i <- i - 1
			} else {
			  list1[k] <- list2[j]
			  j <- j - 1
			}
			k <- k - 1
		  }
		  
		  while (j > 0) {
			list1[k] <- list2[j]
			j <- j - 1
			k <- k - 1
		  }
		  
		  return(list1)
		}
	  )
	)

	main <- function() {
	  solution <- Solution$new()

	  # Example for Solution 1
	  list1 <- c(1, 2, 4)
	  list2 <- c(1, 3, 4)
	  mergedList <- solution$mergeLists(list1, list2)
	  print(paste("Merged List (New List):", toString(mergedList)))

	  # Example for Solution 2
	  list1 <- c(1, 2, 4, 0, 0, 0) # Allocating space for list2
	  list2 <- c(1, 3, 4)
	  mergedList <- solution$mergeListsInPlace(list1, list2)
	  print(paste("Merged List (In-place):", toString(mergedList)))
	}

	main()
	```

=== "Julia"

	```julia
	struct Solution
	end

	# Solution 1: Return New List
	function mergeLists(list1::Vector{Int}, list2::Vector{Int})
		result = Int[]
		i, j = 1, 1

		while i <= length(list1) && j <= length(list2)
			if list1[i] < list2[j]
				push!(result, list1[i])
				i += 1
			else
				push!(result, list2[j])
				j += 1
			end
		end

		append!(result, list1[i:end])
		append!(result, list2[j:end])
		return result
	end

	# Solution 2: In-place Merge
	function mergeListsInPlace!(list1::Vector{Int}, list2::Vector{Int})
		m = length(list1)
		n = length(list2)
		append!(list1, zeros(Int, n)) # Extend list1

		i, j, k = m, n, m + n

		while i > 0 && j > 0
			if list1[i] > list2[j]
				list1[k] = list1[i]
				i -= 1
			else
				list1[k] = list2[j]
				j -= 1
			end
			k -= 1
		end

		while j > 0
			list1[k] = list2[j]
			j -= 1
			k -= 1
		end
	end

	function main()
		solution = Solution()

		# Example for Solution 1
		list1 = [1, 2, 4]
		list2 = [1, 3, 4]
		mergedList = mergeLists(list1, list2)
		println("Merged List (New List): ", mergedList)

		# Example for Solution 2
		list1 = [1, 2, 4, 0, 0, 0] # Allocating space for list2
		list2 = [1, 3, 4]
		mergeListsInPlace!(list1, list2)
		println("Merged List (In-place): ", list1)
	end

	main()
	```

## Merge `k` Sorted Lists

### Problem Description:
Merge `k` Sorted Lists.

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

Sure! Let's break down the provided C++ solution for merging `k` sorted arrays, along with an in-place merge solution, and analyze their time and space complexities.

### Detailed Solution Explanation for the Priority Queue Method

#### Problem Statement
You are given `k` sorted arrays (or vectors) and need to merge them into a single sorted array.

#### Approach
The provided solution uses a **priority queue** (min-heap) to efficiently retrieve the smallest element across all `k` arrays. Here's a step-by-step breakdown of the approach:

1. **Data Structures**:
   - A **priority queue** is used to maintain the current smallest elements from each of the `k` arrays. Each element in the priority queue is a pair of iterators, pointing to the current position in each sorted array.
   - A **result vector** is used to store the merged output.

2. **Initialization**:
   - The priority queue is initialized with the beginning iterators of all non-empty arrays. The custom comparator ensures that the smallest element can be accessed efficiently.

3. **Merging Process**:
   - While the priority queue is not empty:
     - The top element (smallest) is extracted, and its value is added to the result vector.
     - The iterator for the extracted element is advanced. If the iterator hasn't reached the end of its corresponding array, the next element is pushed back into the priority queue.

4. **Result**:
   - Once all elements have been processed, the result vector contains the merged sorted elements.

#### Time Complexity
- **Pushing Elements**: Inserting elements into the priority queue takes $O(\log k)$ time.
- **Total Merges**: Each of the $N$ elements (where $N$ is the total number of elements across all arrays) is processed once. Therefore, the overall complexity is:
  
```math
O(N \log k)
```

#### Space Complexity
- **Priority Queue Storage**: The priority queue holds at most `k` elements at any time, leading to a space complexity of $O(k)$.
- **Result Vector**: The space used for the result vector is $O(N)$ since it stores all merged elements.

Overall, the space complexity is dominated by the result vector:

```math
O(N + k)
```

### In-Place Merge Solution

An alternative in-place merge approach modifies one of the original arrays to accommodate the merged result, thereby not requiring an additional array for storing the merged result. Here’s how you can implement this:

#### In-Place Merge Function

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

void mergeInPlace(vector<vector<char>>& chunks) {
    if (chunks.empty()) return;

    // Create a vector to store all elements
    vector<char> result;
    
    // Collect all elements in the result vector
    for (const auto& chunk : chunks) {
        result.insert(result.end(), chunk.begin(), chunk.end());
    }

    // Sort the result vector in-place
    sort(result.begin(), result.end());

    // Output the merged result
    cout << "Merged Array: ";
    for (char c : result) {
        cout << c << " ";
    }
    cout << endl;
}

int main() {
    vector<vector<char>> chunks = {
        {'a', 'c', 'e'},
        {'b', 'd', 'f'},
        {'g', 'h'}
    };

    mergeInPlace(chunks); // Merge the arrays in-place

    return 0;
}
```

### Explanation of In-Place Merge Solution

#### Steps
1. **Collection**: Iterate through each of the `k` arrays and append their elements to a single `result` vector.
2. **Sorting**: Use the standard library's `sort()` function to sort the entire `result` vector in place.

#### Time Complexity
- **Collecting Elements**: Appending elements from all `k` arrays takes $O(N)$.
- **Sorting**: The sorting operation takes $O(N \log N)$ in the worst case.
  
Overall, the time complexity for this approach is:

```math
O(N \log N)
```

#### Space Complexity
- **Result Vector**: We store all elements in a new vector, which requires $O(N)$ space.
  
Thus, the space complexity is:

```math
O(N)
```

### Summary of Complexity Analysis

- **Priority Queue Method**:
  - Time Complexity: $O(N \log k)$
  - Space Complexity: $O(N + k)$

- **In-Place Merge Method**:
  - Time Complexity: $O(N \log N)$
  - Space Complexity: $O(N)$

### Conclusion

The priority queue method is generally more efficient for merging `k` sorted arrays, especially when $k$ is much smaller than $N$$, as it takes advantage of the sorted nature of the input arrays. The in-place merge method, while simpler, incurs a higher time complexity due to the sorting step, making it less optimal for large datasets.


### Solutions

=== "Python"

	```python
	from typing import List

	class Solution:
		def merge(self, chunks: List[List[str]]) -> List[str]:
			from heapq import heappop, heappush
			
			# Define a pair type for iterators
			heap = []
			
			# Initialize the priority queue with the beginning and end of each vector
			for i, v in enumerate(chunks):
				if v:
					heappush(heap, (v[0], i, 0))  # (value, chunk_index, value_index)

			result = []
			while heap:
				value, chunk_index, value_index = heappop(heap)
				result.append(value)
				if value_index + 1 < len(chunks[chunk_index]):
					next_value = chunks[chunk_index][value_index + 1]
					heappush(heap, (next_value, chunk_index, value_index + 1))

			return result

		def merge_in_place(self, chunks: List[List[str]]) -> None:
			if not chunks:
				return

			result = []
			for chunk in chunks:
				result.extend(chunk)

			result.sort()
			print("Merged Array:", result)


	def main() -> None:
		solution = Solution()
		chunks = [['a', 'c', 'e'], ['b', 'd', 'f'], ['g', 'h']]
		merged = solution.merge(chunks)
		print("Merged Array:", merged)

		solution.merge_in_place(chunks)

	if __name__ == "__main__":
		main()
	```

=== "C++"

	```cpp
	#include <iostream>
	#include <vector>
	#include <queue>
	#include <utility> // For std::pair

	using namespace std;

	class Solution {
	public:
		vector<char> merge(vector<vector<char>>& chunks) {
			// Define a pair type for iterators
			typedef pair<vector<char>::iterator, vector<char>::iterator> it_pair;

			// Custom comparator for the priority queue
			auto comp = [](const it_pair& a, const it_pair& b) {
				return *a.first > *b.first; // Min-heap based on the value pointed to by iterators
			};

			// Priority queue (min-heap) to hold the iterators
			priority_queue<it_pair, vector<it_pair>, decltype(comp)> q(comp);

			// Initialize the priority queue with the beginning and end of each vector
			for (auto& v : chunks) {
				if (!v.empty()) { // Only push non-empty vectors
					q.push({v.begin(), v.end()});
				}
			}

			vector<char> res; // Result vector to store the merged result
			while (!q.empty()) {
				auto [it, end] = q.top(); // Get the smallest element iterator
				q.pop();
				res.push_back(*it); // Add the smallest element to the result
				if (++it != end) { // Move the iterator forward
					q.push({it, end}); // Push the next element of the same vector to the queue
				}
			}

			return res; // Return the merged result
		}
		
		void mergeInPlace(vector<vector<char>>& chunks) {
			if (chunks.empty()) return;

			// Create a vector to store all elements
			vector<char> result;
			
			// Collect all elements in the result vector
			for (const auto& chunk : chunks) {
				result.insert(result.end(), chunk.begin(), chunk.end());
			}

			// Sort the result vector in-place
			sort(result.begin(), result.end());

			// Output the merged result
			cout << "Merged Array: ";
			for (char c : result) {
				cout << c << " ";
			}
			cout << endl;
		}

	int main() {
		Solution solution;
		// Example input: k sorted arrays
		vector<vector<char>> chunks = {
			{'a', 'c', 'e'},
			{'b', 'd', 'f'},
			{'g', 'h'}
		};

		vector<char> merged = solution.merge(chunks); // Merge the arrays

		// Output the merged result
		cout << "Merged Array: ";
		for (char c : merged) {
			cout << c << " ";
		}
		cout << endl;
		
		chunks = {
			{'a', 'c', 'e'},
			{'b', 'd', 'f'},
			{'g', 'h'}
		};
		
		merged = solution.merge(chunks); // Merge the arrays

		// Output the merged result
		cout << "Merged Array: ";
		for (char c : merged) {
			cout << c << " ";
		}
		cout << endl;

		return 0; // Indicate successful completion
	}
	```

=== "Rust"

	```rust
	use std::cmp::Ordering;
	use std::collections::BinaryHeap;

	struct Solution;

	impl Solution {
		pub fn merge(chunks: Vec<Vec<char>>) -> Vec<char> {
			let mut heap = BinaryHeap::new();
			for (i, v) in chunks.iter().enumerate() {
				if !v.is_empty() {
					heap.push((v[0], i, 0)); // (value, chunk_index, value_index)
				}
			}

			let mut result = Vec::new();
			while let Some((value, chunk_index, value_index)) = heap.pop() {
				result.push(value);
				let next_index = value_index + 1;
				if next_index < chunks[chunk_index].len() {
					heap.push((chunks[chunk_index][next_index], chunk_index, next_index));
				}
			}

			result
		}

		pub fn merge_in_place(chunks: Vec<Vec<char>>) {
			let mut result: Vec<char> = chunks.iter().flat_map(|chunk| chunk.iter()).cloned().collect();
			result.sort();
			println!("Merged Array: {:?}", result);
		}
	}

	fn main() {
		let solution = Solution;
		let chunks = vec![vec!['a', 'c', 'e'], vec!['b', 'd', 'f'], vec!['g', 'h']];
		let merged = solution.merge(chunks.clone());
		println!("Merged Array: {:?}", merged);
		solution.merge_in_place(chunks);
	}
	```

=== "C#"

	```csharp
	using System;
	using System.Collections.Generic;

	public class Solution
	{
		public List<char> Merge(List<List<char>> chunks)
		{
			var heap = new SortedSet<(char value, int chunkIndex, int valueIndex)>();
			for (int i = 0; i < chunks.Count; i++)
			{
				if (chunks[i].Count > 0)
					heap.Add((chunks[i][0], i, 0));
			}

			var result = new List<char>();
			while (heap.Count > 0)
			{
				var (value, chunkIndex, valueIndex) = heap.Min;
				heap.Remove(heap.Min);
				result.Add(value);
				if (valueIndex + 1 < chunks[chunkIndex].Count)
				{
					heap.Add((chunks[chunkIndex][valueIndex + 1], chunkIndex, valueIndex + 1));
				}
			}

			return result;
		}

		public void MergeInPlace(List<List<char>> chunks)
		{
			if (chunks.Count == 0) return;

			var result = new List<char>();
			foreach (var chunk in chunks)
			{
				result.AddRange(chunk);
			}
			
			result.Sort();
			Console.WriteLine("Merged Array: " + string.Join(" ", result));
		}
	}

	public class Program
	{
		public static void Main()
		{
			var solution = new Solution();
			var chunks = new List<List<char>> {
				new List<char> {'a', 'c', 'e'},
				new List<char> {'b', 'd', 'f'},
				new List<char> {'g', 'h'}
			};
			
			var merged = solution.Merge(chunks);
			Console.WriteLine("Merged Array: " + string.Join(" ", merged));
			
			solution.MergeInPlace(chunks);
		}
	}
	```

=== "Java"

	```java
	import java.util.*;

	class Solution {
		public List<Character> merge(List<List<Character>> chunks) {
			PriorityQueue<Iterator<Character>> heap = new PriorityQueue<>((a, b) -> Character.compare(a.next(), b.next()));
			
			for (List<Character> chunk : chunks) {
				if (!chunk.isEmpty()) {
					heap.offer(chunk.iterator());
				}
			}

			List<Character> result = new ArrayList<>();
			while (!heap.isEmpty()) {
				Iterator<Character> it = heap.poll();
				if (it.hasNext()) {
					result.add(it.next());
					if (it.hasNext()) {
						heap.offer(it);
					}
				}
			}

			return result;
		}

		public void mergeInPlace(List<List<Character>> chunks) {
			if (chunks.isEmpty()) return;

			List<Character> result = new ArrayList<>();
			for (List<Character> chunk : chunks) {
				result.addAll(chunk);
			}
			Collections.sort(result);
			System.out.println("Merged Array: " + result);
		}

		public static void main(String[] args) {
			Solution solution = new Solution();
			List<List<Character>> chunks = Arrays.asList(
				Arrays.asList('a', 'c', 'e'),
				Arrays.asList('b', 'd', 'f'),
				Arrays.asList('g', 'h')
			);

			List<Character> merged = solution.merge(chunks);
			System.out.println("Merged Array: " + merged);

			solution.mergeInPlace(chunks);
		}
	}
	```

=== "Scala"

	```scala
	import scala.collection.mutable
	import scala.collection.mutable.ListBuffer

	class Solution {
	  def merge(chunks: List[List[Char]]): List[Char] = {
		val heap = mutable.PriorityQueue.empty[(Char, Int, Int)](Ordering.by[(Char, Int, Int), Char](_._1).reverse)

		for (i <- chunks.indices if chunks(i).nonEmpty) {
		  heap.enqueue((chunks(i)(0), i, 0))
		}

		val result = ListBuffer[Char]()
		while (heap.nonEmpty) {
		  val (value, chunkIndex, valueIndex) = heap.dequeue()
		  result += value
		  if (valueIndex + 1 < chunks(chunkIndex).length) {
			heap.enqueue((chunks(chunkIndex)(valueIndex + 1), chunkIndex, valueIndex + 1))
		  }
		}

		result.toList
	  }

	  def mergeInPlace(chunks: List[List[Char]]): Unit = {
		if (chunks.isEmpty) return

		val result = chunks.flatten.sorted
		println("Merged Array: " + result.mkString(" "))
	  }
	}

	object Main extends App {
	  val solution = new Solution
	  val chunks = List(
		List('a', 'c', 'e'),
		List('b', 'd', 'f'),
		List('g', 'h')
	  )

	  val merged = solution.merge(chunks)
	  println("Merged Array: " + merged.mkString(" "))
	  solution.mergeInPlace(chunks)
	}
	```

=== "Kotlin"

	```kotlin
	class Solution {
		fun merge(chunks: List<List<Char>>): List<Char> {
			val heap = PriorityQueue<Pair<Char, Pair<Int, Int>>> { a, b -> a.first.compareTo(b.first) }

			for (i in chunks.indices) {
				if (chunks[i].isNotEmpty()) {
					heap.add(Pair(chunks[i][0], Pair(i, 0)))
				}
			}

			val result = mutableListOf<Char>()
			while (heap.isNotEmpty()) {
				val (value, (chunkIndex, valueIndex)) = heap.poll()
				result.add(value)
				if (valueIndex + 1 < chunks[chunkIndex].size) {
					heap.add(Pair(chunks[chunkIndex][valueIndex + 1], Pair(chunkIndex, valueIndex + 1)))
				}
			}

			return result
		}

		fun mergeInPlace(chunks: List<List<Char>>) {
			if (chunks.isEmpty()) return

			val result = chunks.flatten().sorted()
			println("Merged Array: ${result.joinToString(" ")}")
		}
	}

	fun main() {
		val solution = Solution()
		val chunks = listOf(
			listOf('a', 'c', 'e'),
			listOf('b', 'd', 'f'),
			listOf('g', 'h')
		)

		val merged = solution.merge(chunks)
		println("Merged Array: ${merged.joinToString(" ")}")
		solution.mergeInPlace(chunks)
	}
	```

=== "Go"

	```go
	package main

	import (
		"container/heap"
		"fmt"
		"sort"
	)

	type Item struct {
		value      rune
		chunkIndex int
		valueIndex int
	}

	type MinHeap []Item

	func (h MinHeap) Len() int           { return len(h) }
	func (h MinHeap) Less(i, j int) bool { return h[i].value < h[j].value }
	func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

	func (h *MinHeap) Push(x interface{}) {
		*h = append(*h, x.(Item))
	}

	func (h *MinHeap) Pop() interface{} {
		old := *h
		n := len(old)
		x := old[n-1]
		*h = old[0 : n-1]
		return x
	}

	type Solution struct{}

	func (s *Solution) Merge(chunks [][]rune) []rune {
		h := &MinHeap{}
		heap.Init(h)

		for i, chunk := range chunks {
			if len(chunk) > 0 {
				heap.Push(h, Item{value: chunk[0], chunkIndex: i, valueIndex: 0})
			}
		}

		result := []rune{}
		for h.Len() > 0 {
			item := heap.Pop(h).(Item)
			result = append(result, item.value)
			if item.valueIndex+1 < len(chunks[item.chunkIndex]) {
				nextItem := Item{value: chunks[item.chunkIndex][item.valueIndex+1], chunkIndex: item.chunkIndex, valueIndex: item.valueIndex + 1}
				heap.Push(h, nextItem)
			}
		}

		return result
	}

	func (s *Solution) MergeInPlace(chunks [][]rune) {
		if len(chunks) == 0 {
			return
		}

		result := []rune{}
		for _, chunk := range chunks {
			result = append(result, chunk...)
		}
		sort.Slice(result, func(i, j int) bool {
			return result[i] < result[j]
		})

		fmt.Println("Merged Array:", string(result))
	}

	func main() {
		solution := Solution{}
		chunks := [][]rune{
			{'a', 'c', 'e'},
			{'b', 'd', 'f'},
			{'g', 'h'},
		}

		merged := solution.Merge(chunks)
		fmt.Println("Merged Array:", string(merged))
		solution.MergeInPlace(chunks)
	}
	```

=== "TypeScript"

	```typescript
	class Solution {
		merge(chunks: Array<Array<string>>): Array<string> {
			const heap: Array<[string, number, number]> = [];

			for (let i = 0; i < chunks.length; i++) {
				if (chunks[i].length > 0) {
					heap.push([chunks[i][0], i, 0]);
				}
			}

			heap.sort(([valueA], [valueB]) => valueA.localeCompare(valueB));

			const result: string[] = [];
			while (heap.length > 0) {
				const [value, chunkIndex, valueIndex] = heap.shift()!;
				result.push(value);
				if (valueIndex + 1 < chunks[chunkIndex].length) {
					heap.push([chunks[chunkIndex][valueIndex + 1], chunkIndex, valueIndex + 1]);
				}
				heap.sort(([valueA], [valueB]) => valueA.localeCompare(valueB));
			}

			return result;
		}

		mergeInPlace(chunks: Array<Array<string>>): void {
			if (chunks.length === 0) return;

			const result: string[] = [];
			for (const chunk of chunks) {
				result.push(...chunk);
			}

			result.sort();
			console.log("Merged Array:", result.join(" "));
		}
	}

	function main() {
		const solution = new Solution();
		const chunks = [
			['a', 'c', 'e'],
			['b', 'd', 'f'],
			['g', 'h']
		];

		const merged = solution.merge(chunks);
		console.log("Merged Array:", merged.join(" "));
		
		solution.mergeInPlace(chunks);
	}

	main();
	```

=== "R"

	```r
	merge <- function(chunks) {
	  heap <- list()
	  
	  for (i in seq_along(chunks)) {
		if (length(chunks[[i]]) > 0) {
		  heap[[length(heap) + 1]] <- list(value = chunks[[i]][1], chunkIndex = i, valueIndex = 1)
		}
	  }
	  
	  result <- character(0)
	  
	  while (length(heap) > 0) {
		minIndex <- which.min(sapply(heap, function(x) x$value))
		minItem <- heap[[minIndex]]
		result <- c(result, minItem$value)
		heap <- heap[-minIndex]
		
		nextValueIndex <- minItem$valueIndex + 1
		if (nextValueIndex <= length(chunks[[minItem$chunkIndex]])) {
		  heap[[length(heap) + 1]] <- list(value = chunks[[minItem$chunkIndex]][nextValueIndex], chunkIndex = minItem$chunkIndex, valueIndex = nextValueIndex)
		}
	  }
	  
	  return(result)
	}

	mergeInPlace <- function(chunks) {
	  if (length(chunks) == 0) return()
	  
	  result <- unlist(chunks)
	  result <- sort(result)
	  cat("Merged Array:", paste(result, collapse = " "), "\n")
	}

	main <- function() {
	  chunks <- list(
		c('a', 'c', 'e'),
		c('b', 'd', 'f'),
		c('g', 'h')
	  )
	  
	  merged <- merge(chunks)
	  cat("Merged Array:", paste(merged, collapse = " "), "\n")
	  
	  mergeInPlace(chunks)
	}

	main()
	```

=== "Julia"

	```julia
	module Solution

	export merge, merge_in_place

	function merge(chunks::Vector{Vector{Char}})::Vector{Char}
		heap = PriorityQueue{Tuple{Char, Int, Int}}()

		for (i, chunk) in enumerate(chunks)
			if !isempty(chunk)
				enqueue!(heap, (chunk[1], i, 1))
			end
		end

		result = Char[]
		while !isempty(heap)
			value, chunkIndex, valueIndex = dequeue!(heap)
			push!(result, value)
			if valueIndex < length(chunks[chunkIndex])
				enqueue!(heap, (chunks[chunkIndex][valueIndex + 1], chunkIndex, valueIndex + 1))
			end
		end

		return result
	end

	function merge_in_place(chunks::Vector{Vector{Char}})
		if isempty(chunks)
			return
		end

		result = reduce(vcat, chunks)
		sort!(result)
		println("Merged Array: ", result)
	end

	end

	using .Solution

	function main()
		chunks = [
			['a', 'c', 'e'],
			['b', 'd', 'f'],
			['g', 'h']
		]

		merged = Solution.merge(chunks)
		println("Merged Array: ", merged)

		Solution.merge_in_place(chunks)
	end

	main()
	```

## Remove Duplicates from Sorted Array

### Problem Description: [LeetCode - Problem 26 - Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Intuition

The __intuition__ is to use __two pointers, `i` and `j`__, to iterate through the array. The variable `j` is used to keep track of the current index where a unique element should be placed. The initial value of `j` is 1 since the first element in the array is always unique and doesn't need to be changed.

### Solution Explanation

The code starts iterating from `i = 1` because we need to compare each element with its previous element to check for duplicates.

The main logic is inside the for loop:

  - If the current element `nums[i]` is not equal to the previous element `nums[i - 1]`, it means we have encountered a new unique element.
  - In that case, we update `nums[j]` with the value of the unique element at `nums[i]`, and then increment `j` by `1` to mark the next position for a new unique element.
  - By doing this, we effectively overwrite any duplicates in the array and only keep the unique elements.

Once the loop finishes, the value of `j` represents the length of the resulting array with duplicates removed.

Finally, we return `j` as the desired result.

### Complexity Analysis

- __Time Complexity :__
  - Initialization: The variable assignment operation to initialize `j = 1` takes constant time `O(1)`.
  - Loop: The loop runs through the `nums` list once, comparing elements and updating the `j` index. This loop iterates through the list of length `n` with `n-1` comparisons.
    - Each iteration does constant work (comparing array elements and updating a counter), leading to a time complexity of `O(n)`.
  - __Therefore, the overall time complexity of the `removeDuplicates` method is `O(n)`, where `n` is the length of the input list `nums`.__

- __Space Complexity :__
  - The algorithm performs modifications in-place, updating the input list `nums` without using any extra data structures.
  - The extra space used is only for a constant number of integer variables (`j`, `i`) and does not depend on the input size.
  - __Hence, the algorithm has a space complexity of `O(1)`, indicating constant space usage regardless of the size of the input list `nums`.__

__In summary :__
- __`n` being the length of the input list `nums`.__
  - __Overall Time Complexity: `O(n)`__
  - __Overall Space Complexity: `O(1)`__

This solution effectively removes duplicates from the input list in linear time complexity while using only constant additional space, making it an efficient and space-saving approach for this problem.

### Solutions

=== "Python"

    ```python
    from typing import List

    class Solution:
        def removeDuplicates(self, nums: List[int]) -> int:
            j = 1
            for i in range(1, len(nums)):
                if nums[i] != nums[i - 1]:
                    nums[j] = nums[i]
                    j += 1
            return j
    ```

=== "C++"

    ```cpp
    #include <vector>

    class Solution {
    public:
        int removeDuplicates(vector<int>& nums) {
            int j = 1;
            for(int i = 1; i < nums.size(); i++){
                if(nums[i] != nums[i - 1]){
                    nums[j] = nums[i];
                    j++;
                }
            }
            return j;
        }
    };
    ```

=== "Rust"

    ```rust
    struct Solution;

    impl Solution {
        pub fn remove_duplicates(nums: &mut Vec<i32>) -> usize {
            let mut j = 1;
            let len = nums.len();
            for i in 1..len {
                if nums[i] != nums[i - 1] {
                    nums[j] = nums[i];
                    j += 1;
                }
            }
            j
        }
    }

    fn main() {
        let mut nums = vec![1, 1, 2];
        let new_length = Solution::remove_duplicates(&mut nums);
        println!("New length: {}", new_length);
        println!("Modified array: {:?}", &nums[..new_length]);
    }
    ```

=== "C#"

    ```csharp
    using System;

    public class Solution {
        public int RemoveDuplicates(int[] nums) {
            if (nums.Length == 0) return 0;

            int j = 1;
            for (int i = 1; i < nums.Length; i++) {
                if (nums[i] != nums[i - 1]) {
                    nums[j] = nums[i];
                    j++;
                }
            }
            return j;
        }
        
        public static void Main() {
            var nums = new int[] { 1, 1, 2 };
            var solution = new Solution();
            int newLength = solution.RemoveDuplicates(nums);
            Console.WriteLine("New length: " + newLength);
            Console.WriteLine("Modified array: " + string.Join(", ", nums[..newLength]));
        }
    }
    ```

=== "Java"

    ```java
    class Solution {
        public int removeDuplicates(int[] nums) {
            int j = 1;
            for (int i = 1; i < nums.length; i++) {
                if (nums[i] != nums[i - 1]) {
                    nums[j] = nums[i];
                    j++;
                }
            }
            return j;
        }
    }
    ```

=== "Scala"

    ```scala
    class Solution {
        def removeDuplicates(nums: Array[Int]): Int = {
            var j = 1
            for (i <- 1 until nums.length) {
                if (nums(i) != nums(i - 1)) {
                    nums(j) = nums(i)
                    j += 1
                }
            }
            j
        }
    }

    object Main extends App {
        val solution = new Solution
        val nums = Array(1, 1, 2)
        val newLength = solution.removeDuplicates(nums)
        println(s"New length: $newLength")
        println(s"Modified array: ${nums.take(newLength).mkString(", ")}")
    }
    ```

=== "Kotlin"

    ```kotlin
    class Solution {
        fun removeDuplicates(nums: Array<Int>): Int {
            var j = 1
            for (i in 1 until nums.size) {
                if (nums[i] != nums[i - 1]) {
                    nums[j] = nums[i]
                    j++
                }
            }
            return j
        }
    }

    fun main() {
        val solution = Solution()
        val nums = arrayOf(1, 1, 2, 2, 3, 4, 4, 5, 5)
        val resultSize = solution.removeDuplicates(nums)
        
        println("Modified Array Size: $resultSize")
        for (i in 0 until resultSize) {
            print("${nums[i]} ")
        }
    }
    ```

=== "Go"

    ```go
    package main

    import "fmt"

    type Solution struct{}

    func (s Solution) RemoveDuplicates(nums []int) int {
        if len(nums) == 0 {
            return 0
        }
        j := 1
        for i := 1; i < len(nums); i++ {
            if nums[i] != nums[i - 1] {
                nums[j] = nums[i]
                j++
            }
        }
        return j
    }

    func main() {
        s := Solution{}
        nums := []int{1, 1, 2}
        newLength := s.RemoveDuplicates(nums)
        fmt.Println("New length:", newLength)
        fmt.Println("Modified array:", nums[:newLength])
    }
    ```

=== "TypeScript"

    ```typescript
    class Solution {
        removeDuplicates(nums: number[]): number {
            let j = 1;
            for (let i = 1; i < nums.length; i++) {
                if (nums[i] !== nums[i - 1]) {
                    nums[j] = nums[i];
                    j++;
                }
            }
            return j;
        }
    }

    // Example usage
    const solution = new Solution();
    const nums = [1, 1, 2];
    const newLength = solution.removeDuplicates(nums);
    console.log("New length:", newLength);
    console.log("Modified array:", nums.slice(0, newLength));
    ```

=== "R"

    ```r
    #library(R6)
    #Solution <- R6::R6Class("Solution",
    Solution <- setRefClass("Solution",
        methods = list(
            removeDuplicates = function(nums) {
            if (length(nums) == 0) return(0)
            j <- 1
            for (i in 2:length(nums)) {
                if (nums[i] != nums[i - 1]) {
                nums[j + 1] <- nums[i]
                j <- j + 1
                }
            }
            return(j)
            }
        )
    )

    # Example usage
    solution <- Solution$new()
    nums <- c(1, 1, 2)
    newLength <- solution$removeDuplicates(nums)
    cat("New length:", newLength, "\n")
    cat("Modified array:", nums[1:newLength], "\n")
    ```

=== "Julia"

    ```julia
    struct Solution
    end

    function remove_duplicates(s::Solution, nums::Vector{Int})::Int
        if isempty(nums)
            return 0
        end
        j = 1
        for i in 2:length(nums)
            if nums[i] != nums[i - 1]
                nums[j + 1] = nums[i]
                j += 1
            end
        end
        return j
    end

    # Example usage
    s = Solution()
    nums = [1, 1, 2]
    newLength = remove_duplicates(s, nums)
    println("New length: ", newLength)
    println("Modified array: ", nums[1:newLength])
    ```

## Remove Element

### Problem Description: [LeetCode - Problem 27 - Remove Element](https://leetcode.com/problems/remove-element/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Intuition

The __intuition__ behind this solution is to __iterate through the array and keep track of two pointers: `index` and `i`__. The index pointer represents the position where the next non-target element should be placed, while the i pointer iterates through the array elements. By overwriting the target elements with non-target elements, the solution effectively removes all occurrences of the target value from the array.

### Solution Explanation

- Initialize __`index`__ to __`0`__, which represents the current position for the next non-target element.
- Iterate through each element of the input array using the __`i`__ pointer.
- For each element __`nums[i]`__, check if it is equal to the target value.
  - If __`nums[i]`__ is not equal to __`val`__, it means it is a non-target element.
  - Set __`nums[index]`__ to __`nums[i]`__ to store the non-target element at the current index position.
  - Increment __`index`__ by __`1`__ to move to the next position for the next non-target element.
- Continue this process until all elements in the array have been processed.
- __Finally, return the value of `index`, which represents the length of the modified array.__

### Complexity Analysis

- __Time Complexity :__
  - The algorithm iterates through the input list __`nums`__ once, where __the number of iterations is equal to the length of the list `len(nums) = n`.__
    - Each iteration involves constant time operations: comparing elements, updating the index __`index`__, and potentially updating array elements.
  - As each element in the array is processed once, the time complexity is __`O(n)`__, where __`n` is the length of the input list `nums`__.

- __Space Complexity :__
  - The additional space used in the algorithm is independent of the input size and primarily includes maintaining constant variables (__`index`, `i`, `val`__) to track the processing.
  - The space complexity is __`O(1)`__, indicating that the space usage remains constant regardless of the input size, as the modifications are made in-place without utilizing any extra data structures.

__In summary :__
- __`n` being the length of the input list `nums`.__
  - __Overall Time Complexity: `O(n)`__
  - __Overall Space Complexity: `O(1)`__

The solution efficiently removes the specified element from the input array with a linear time complexity and constant space complexity, making it an optimal approach for this problem.

### Solutions

=== "Python"

    ```python
    class Solution:
        def removeElement(self, nums: List[int], val: int) -> int:
            index = 0
            for i in range(len(nums)):
                if nums[i] != val:
                    nums[index] = nums[i]
                    index += 1
            return index
    ```

=== "C++"

    ```cpp
    #include <vector>

    class Solution {
    public:
        int removeElement(std::vector<int>& nums, int val) {
            int index = 0;
            for (int i = 0; i < nums.size(); i++) {
                if (nums[i] != val) {
                    nums[index] = nums[i];
                    index++;
                }
            }
            return index;
        }
    };
    ```

=== "Rust"

    ```rust
    struct Solution;

    impl Solution {
        pub fn remove_element(nums: &mut Vec<i32>, val: i32) -> i32 {
            let mut index = 0;
            for i in 0..nums.len() {
                if nums[i] != val {
                    nums[index] = nums[i];
                    index += 1;
                }
            }
            index as i32
        }
    }
    ```

=== "C#"

    ```csharp
    using System;
    using System.Collections.Generic;

    public class Solution {
        public int RemoveElement(List<int> nums, int val) {
            int index = 0;
            for (int i = 0; i < nums.Count; i++) {
                if (nums[i] != val) {
                    nums[index] = nums[i];
                    index++;
                }
            }
            return index;
        }
    }
    ```

=== "Java"

    ```java
    import java.util.List;

    public class Solution {
        public int removeElement(List<Integer> nums, int val) {
            int index = 0;
            for (int i = 0; i < nums.size(); i++) {
                if (nums.get(i) != val) {
                    nums.set(index, nums.get(i));
                    index++;
                }
            }
            return index;
        }
    }
    ```

=== "Scala"

    ```scala
    class Solution {
        def removeElement(nums: scala.collection.mutable.Seq[Int], value: Int): Int = {
            var index = 0
            for (i <- nums.indices) {
                if (nums(i) != value) {
                    nums(index) = nums(i)
                    index += 1
                }
            }
            index
        }
    }
    ```

=== "Kotlin"

    ```kotlin
    class Solution {
        fun removeElement(nums: MutableList<Int>, value: Int): Int {
            var index = 0
            for (i in nums.indices) {
                if (nums[i] != value) {
                    nums[index] = nums[i]
                    index++
                }
            }
            return index
        }
    }
    ```

=== "Go"

    ```go
    package main

    import "fmt"

    type Solution struct{}

    func (s Solution) removeElement(nums []int, val int) int {
        index := 0
        for i := 0; i < len(nums); i++ {
            if nums[i] != val {
                nums[index] = nums[i]
                index++
            }
        }
        return index
    }

    func main() {
        nums := []int{3, 2, 2, 3}
        sol := Solution{}
        result := sol.removeElement(nums, 3)
        fmt.Println(result)
    }
    ```

=== "TypeScript"

    ```typescript
    class Solution {
        removeElement(nums: number[], val: number): number {
            let index = 0;
            for (let i = 0; i < nums.length; i++) {
                if (nums[i] !== val) {
                    nums[index] = nums[i];
                    index++;
                }
            }
            return index;
        }
    }
    ```

=== "R"

    ```r
    #library(R6)
    #Solution <- R6::R6Class("Solution",
    Solution <- setRefClass("Solution", 
        methods = list(
            removeElement = function(nums, val) {
                index <- 0
                for (i in seq_along(nums)) {
                    if (nums[i] != val) {
                    nums[index + 1] <- nums[i]
                    index <- index + 1
                    }
                }
                return(index)
            }
        )
    )
    ```

=== "Julia"

    ```julia
    module SolutionModule

    mutable struct Solution
        function removeElement(nums::Vector{Int}, val::Int)::Int
            index = 0
            for i in 1:length(nums)
                if nums[i] != val
                    nums[index + 1] = nums[i]
                    index += 1
                end
            end
            return index
        end
    end

    end
    ```

## Next Permutation

### Problem Description: [LeetCode - Problem 31 - Next Permutation](https://leetcode.com/problems/next-permutation/description/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Intuition

The problem requires finding the __next lexicographical permutation__ of a given sequence of numbers. The next permutation is the next arrangement of numbers that's larger than the current sequence but in ascending order. If no such arrangement exists (i.e., the numbers are in descending order), the next permutation is simply the smallest possible arrangement (ascending order).

### Key Insights

- The __next permutation__ problem boils down to rearranging the elements to find the smallest possible permutation that is larger than the given one.
- This involves finding the right elements to swap and then arranging the rest of the sequence in ascending order to get the smallest possible arrangement.

### Solution Explanation

### Complexity Analysis

### Solutions

=== "Python"

    ```python
    from typing import List

    class Solution:
        def reverse(self, nums: List[int], i: int, j: int) -> None:
            """
            Helper function to reverse the array segment between indices i and j.
            """
            while i <= j:
                nums[i], nums[j] = nums[j], nums[i]
                i += 1
                j -= 1

        def nextPermutation(self, nums: List[int]) -> None:
            n = len(nums)  # Get the size of the array.
            i, j = n - 1, n - 1

            # Step 1: Find the first decreasing element when traversing from the end.
            while i > 0 and nums[i - 1] >= nums[i]:
                i -= 1
            i -= 1  # i is now the index of the first decreasing element (nums[i] < nums[i+1]).

            # Step 2: If such an element was found, find the first element just larger than nums[i].
            if i >= 0:
                while j >= 0 and nums[i] >= nums[j]:
                    j -= 1

                # Step 3: Swap nums[i] with nums[j], which is the smallest larger element.
                nums[i], nums[j] = nums[j], nums[i]

            # Step 4: Reverse the segment from i+1 to the end to get the smallest possible arrangement.
            self.reverse(nums, i + 1, n - 1)

    # Example Usage
    if __name__ == '__main__':
        solution = Solution()
        nums = [1, 2, 3]
        solution.nextPermutation(nums)
        print(nums)
    ```

=== "C++"

    ```cpp
    class Solution {
    public:
        // Helper function to reverse the array segment between indices i and j.
        void reverse(vector<int> &nums, int i, int j) {
            // Reverse the segment in place using two pointers.
            while(i <= j) {
                swap(nums[i++], nums[j--]);
            }
        }

        // Function to find the next lexicographical permutation of the given array.
        void nextPermutation(vector<int>& nums) {
            int n = nums.size(); // Get the size of the array.
            int i = n - 1, j = n - 1;

            // Step 1: Find the first decreasing element when traversing from the end.
            while(i > 0 && nums[i - 1] >= nums[i]) {
                --i;
            }
            --i; // Now, i is the index of the first decreasing element (nums[i] < nums[i+1]).

            // Step 2: If such an element was found, find the first element just larger than nums[i].
            if(i >= 0) {
                while(j >= 0 && nums[i] >= nums[j]) {
                    --j;
                }
                // Step 3: Swap nums[i] with nums[j], which is the smallest larger element.
                swap(nums[i], nums[j]);
            }

            // Step 4: Reverse the segment from i+1 to the end to get the smallest possible arrangement.
            reverse(nums, i + 1, n - 1);
        }
    };

    int main() {
        Solution solution;
        std::vector<int> nums = {1, 2, 3};
        solution.nextPermutation(nums);
        for (int num : nums) {
            std::cout << num << " ";
        }
        std::cout << std::endl;
        return 0;
    }
    ```

=== "Rust"

    ```rust
    struct Solution;

    impl Solution {
        fn reverse(nums: &mut Vec<i32>, mut i: usize, mut j: usize) {
            while i <= j {
                nums.swap(i, j);
                i += 1;
                j -= 1;
            }
        }

        fn next_permutation(&self, nums: &mut Vec<i32>) {
            let n = nums.len();
            let (mut i, mut j) = (n - 1, n - 1);

            while i > 0 && nums[i - 1] >= nums[i] {
                i -= 1;
            }
            if i > 0 {
                i -= 1;
                while j >= 0 && nums[i] >= nums[j] {
                    j -= 1;
                }
                nums.swap(i, j);
            }

            Solution::reverse(nums, i + 1, n - 1);
        }
    }

    fn main() {
        let mut nums = vec![1, 2, 3];
        let solution = Solution;
        solution.next_permutation(&mut nums);
        println!("{:?}", nums);
    }
    ```

=== "C#"

    ```csharp
    using System;
    using System.Collections.Generic;

    public class Solution {
        public void Reverse(List<int> nums, int i, int j) {
            while (i <= j) {
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++;
                j--;
            }
        }

        public void NextPermutation(List<int> nums) {
            int n = nums.Count;
            int i = n - 1, j = n - 1;

            while (i > 0 && nums[i - 1] >= nums[i]) {
                i--;
            }
            i--;

            if (i >= 0) {
                while (j >= 0 && nums[i] >= nums[j]) {
                    j--;
                }
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
            }

            Reverse(nums, i + 1, n - 1);
        }

        public static void Main(string[] args) {
            Solution solution = new Solution();
            List<int> nums = new List<int> { 1, 2, 3 };
            solution.NextPermutation(nums);
            Console.WriteLine(string.Join(" ", nums));
        }
    }
    ```

=== "Java"

    ```java
    import java.util.*;

    public class Solution {

        public void reverse(List<Integer> nums, int i, int j) {
            while (i <= j) {
                Collections.swap(nums, i, j);
                i++;
                j--;
            }
        }

        public void nextPermutation(List<Integer> nums) {
            int n = nums.size();
            int i = n - 1, j = n - 1;

            while (i > 0 && nums.get(i - 1) >= nums.get(i)) {
                i--;
            }
            i--;

            if (i >= 0) {
                while (j >= 0 && nums.get(i) >= nums.get(j)) {
                    j--;
                }
                Collections.swap(nums, i, j);
            }

            reverse(nums, i + 1, n - 1);
        }

        public static void main(String[] args) {
            Solution solution = new Solution();
            List<Integer> nums = new ArrayList<>(Arrays.asList(1, 2, 3));
            solution.nextPermutation(nums);
            System.out.println(nums);
        }
    }
    ```

=== "Scala"

    ```scala
    object Solution {
        def reverse(nums: Array[Int], i: Int, j: Int): Unit = {
            var l = i
            var r = j
            while (l <= r) {
                val temp = nums(l)
                nums(l) = nums(r)
                nums(r) = temp
                l += 1
                r -= 1
            }
        }

        def nextPermutation(nums: Array[Int]): Unit = {
            val n = nums.length
            var i = n - 1
            var j = n - 1

            while (i > 0 && nums(i - 1) >= nums(i)) i -= 1
            i -= 1

            if (i >= 0) {
                while (j >= 0 && nums(i) >= nums(j)) j -= 1
                val temp = nums(i)
                nums(i) = nums(j)
                nums(j) = temp
            }

            reverse(nums, i + 1, n - 1)
        }

        def main(args: Array[String]): Unit = {
            val nums = Array(1, 2, 3)
            nextPermutation(nums)
            println(nums.mkString(", "))
        }
    }
    ```

=== "Kotlin"

    ```kotlin
    class Solution {
        fun reverse(nums: MutableList<Int>, i: Int, j: Int) {
            var left = i
            var right = j
            while (left <= right) {
                nums[left] = nums[right].also { nums[right] = nums[left] }
                left++
                right--
            }
        }

        fun nextPermutation(nums: MutableList<Int>) {
            val n = nums.size
            var i = n - 1
            var j = n - 1

            while (i > 0 && nums[i - 1] >= nums[i]) i--
            i--

            if (i >= 0) {
                while (j >= 0 && nums[i] >= nums[j]) j--
                nums[i] = nums[j].also { nums[j] = nums[i] }
            }

            reverse(nums, i + 1, n - 1)
        }
    }

    fun main() {
        val solution = Solution()
        val nums = mutableListOf(1, 2, 3)
        solution.nextPermutation(nums)
        println(nums)
    }
    ```

=== "Go"

    ```go
    package main

    import (
        "fmt"
    )

    type Solution struct{}

    func (s Solution) reverse(nums []int, i int, j int) {
        for i <= j {
            nums[i], nums[j] = nums[j], nums[i]
            i++
            j--
        }
    }

    func (s Solution) nextPermutation(nums []int) {
        n := len(nums)
        i, j := n-1, n-1

        for i > 0 && nums[i-1] >= nums[i] {
            i--
        }
        i--

        if i >= 0 {
            for j >= 0 && nums[i] >= nums[j] {
                j--
            }
            nums[i], nums[j] = nums[j], nums[i]
        }

        s.reverse(nums, i+1, n-1)
    }

    func main() {
        solution := Solution{}
        nums := []int{1, 2, 3}
        solution.nextPermutation(nums)
        fmt.Println(nums)
    }
    ```

=== "TypeScript"

    ```typescript
    class Solution {
        reverse(nums: number[], i: number, j: number): void {
            while (i <= j) {
                [nums[i], nums[j]] = [nums[j], nums[i]];
                i++;
                j--;
            }
        }

        nextPermutation(nums: number[]): void {
            const n = nums.length;
            let i = n - 1;
            let j = n - 1;

            while (i > 0 && nums[i - 1] >= nums[i]) {
                i--;
            }
            i--;

            if (i >= 0) {
                while (j >= 0 && nums[i] >= nums[j]) {
                    j--;
                }
                [nums[i], nums[j]] = [nums[j], nums[i]];
            }

            this.reverse(nums, i + 1, n - 1);
        }
    }

    const solution = new Solution();
    const nums = [1, 2, 3];
    solution.nextPermutation(nums);
    console.log(nums);
    ```

=== "R"

    ```r
    #library(R6)
    #Solution <- R6::R6Class("Solution",
    Solution <- setRefClass("Solution", 
        methods = list(
            reverse = function(nums, i, j) {
                while (i <= j) {
                    tmp <- nums[i + 1]
                    nums[i + 1] <- nums[j + 1]
                    nums[j + 1] <- tmp
                    i <- i + 1
                    j <- j - 1
                }
                return(nums)
            },
            
            nextPermutation = function(nums) {
                n <- length(nums)
                i <- n
                j <- n
                
                while (i > 1 && nums[i - 1] >= nums[i]) {
                    i <- i - 1
                }
                i <- i - 1
                
                if (i >= 1) {
                    while (j >= 1 && nums[i] >= nums[j]) {
                    j <- j - 1
                    }
                    tmp <- nums[i]
                    nums[i] <- nums[j]
                    nums[j] <- tmp
                }
                
                nums <- reverse(nums, i + 1, n)
                return(nums)
            }
        )
    )

    nums <- c(1, 2, 3)
    solution <- Solution$new()
    nums <- solution$nextPermutation(nums)
    print(nums)
    ```

=== "Julia"

    ```julia
    mutable struct Solution
    end

    function reverse!(nums::Vector{Int}, i::Int, j::Int)
        while i <= j
            nums[i], nums[j] = nums[j], nums[i]
            i += 1
            j -= 1
        end
    end

    function nextPermutation!(nums::Vector{Int})
        n = length(nums)
        i, j = n, n

        while i > 1 && nums[i - 1] >= nums[i]
            i -= 1
        end
        i -= 1

        if i >= 1
            while j >= 1 && nums[i] >= nums[j]
                j -= 1
            end
            nums[i], nums[j] = nums[j], nums[i]
        end

        reverse!(nums, i + 1, n)
    end

    function main()
        nums = [1, 2, 3]
        nextPermutation!(nums)
        println(nums)
    end

    main()
    ```
