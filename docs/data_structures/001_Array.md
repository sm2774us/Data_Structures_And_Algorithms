---
tags:
  - Original
---

# Array

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
