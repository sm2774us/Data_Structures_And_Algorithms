# String

## Remove Duplicates from Sorted Array

Problem Description: [LeetCode - Problem 26 - Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

!!! tip

    Python solution is the easiest to understand so always start with that.

Solutions:

=== "Python3"

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

=== "C++20"

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
    struct Solution end

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
