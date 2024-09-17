---
tags:
  - Original
---

# String

## Longest Palindromic Substring - `Manacher's Algorithm`

### Problem Description: [LeetCode - Problem 5 - Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation
1. PreProcess Function:
   - This function transforms the input string to handle even-length palindromes.
   - It adds special characters '^' and '$' at the start and end, and '#' between each character.
   - Example: "abc" becomes "^#a#b#c#$"

2. Main Algorithm:
   - Initialize array P to store palindrome lengths.
   - Variables 'center' and 'right' keep track of the rightmost palindrome boundary.
   - Iterate through the transformed string T:
     a. If i is within the right boundary, use previously computed values to initialize P[i].
     b. Expand palindrome around center i as far as possible.
     c. Update 'center' and 'right' if a longer palindrome is found.
   - Find the maximum value in P, which represents the longest palindrome.
   - Calculate the start position in the original string and return the substring.

### Complexity Analysis
__Time Complexity: `O(n)`__
- PreProcess function: O(n)
- Main loop: Although there's a nested while loop, each character is visited at most twice.
  - The outer loop runs n times.
  - The inner while loop can expand at most n times in total across all iterations of the outer loop.
- Finding the maximum in P: O(n)
- Overall: O(n) + O(n) + O(n) = O(n)

__Space Complexity: `O(n)`__
- T (preprocessed string): O(n)
- P (palindrome length array): O(n)
- Other variables use constant space
- Overall: O(n)

### Key Points

1. Manacher's algorithm efficiently handles both odd and even-length palindromes by preprocessing the string.
2. It reuses previously computed information to avoid unnecessary comparisons.
3. The algorithm maintains a rightmost palindrome boundary to optimize expansions.
4. Despite the nested loops, the linear time complexity is achieved because each character is processed at most twice.

This algorithm significantly improves upon the naive `O(n^3)` or dynamic programming `O(n^2)` approaches, achieving linear time complexity for finding the longest palindromic substring.

### Solutions

=== "Python"

    ```python
    class Solution:
        def longestPalindrome(self, s: str) -> str:
            """
            :type s: str
            :rtype: str
            """
            def preProcess(s: str) -> str:
                if not s:
                    return ['^', '$']
                T = ['^']
                for c in s:
                    T +=  ['#', c]
                T += ['#', '$']
                return T

            T = preProcess(s)
            P = [0] * len(T)
            center, right = 0, 0
            for i in range(1, len(T) - 1):
                i_mirror = 2 * center - i
                if right > i:
                    P[i] = min(right - i, P[i_mirror])
                else:
                    P[i] = 0

                while T[i + 1 + P[i]] == T[i - 1 - P[i]]:
                    P[i] += 1

                if i + P[i] > right:
                    center, right = i, i + P[i]

            max_i = 0
            for i in range(1, len(T) - 1):
                if P[i] > P[max_i]:
                    max_i = i
            start = (max_i - 1 - P[max_i]) // 2
            return s[start : start + P[max_i]]
    ```

=== "C++"

    ```cpp
    #include <iostream>
    #include <vector>
    #include <string>
    #include <algorithm>

    class Solution {
    public:
        std::string longestPalindrome(const std::string& s) {
            if (s.empty()) return "";

            std::string T = preProcess(s);
            std::vector<int> P(T.size(), 0);
            int center = 0, right = 0;

            for (int i = 1; i < T.size() - 1; i++) {
                int i_mirror = 2 * center - i;

                if (right > i)
                    P[i] = std::min(right - i, P[i_mirror]);

                while (T[i + 1 + P[i]] == T[i - 1 - P[i]])
                    P[i]++;

                if (i + P[i] > right) {
                    center = i;
                    right = i + P[i];
                }
            }

            int max_i = 0;
            for (int i = 1; i < T.size() - 1; i++) {
                if (P[i] > P[max_i])
                    max_i = i;
            }

            int start = (max_i - 1 - P[max_i]) / 2;
            return s.substr(start, P[max_i]);
        }

    private:
        std::string preProcess(const std::string& s) {
            if (s.empty()) return "^$";
            std::string T = "^";
            for (char c : s) {
                T += "#" + std::string(1, c);
            }
            T += "#$";
            return T;
        }
    };

    int main() {
        Solution solution;
        std::string input = "babad";
        std::cout << "Longest Palindromic Substring: " << solution.longestPalindrome(input) << std::endl;
    }
    ```

=== "Rust"

    ```rust
    struct Solution;

    impl Solution {
        pub fn longest_palindrome(&self, s: &str) -> String {
            let T = self.pre_process(s);
            let mut P = vec![0; T.len()];
            let (mut center, mut right) = (0, 0);

            for i in 1..T.len() - 1 {
                let i_mirror = 2 * center - i;
                if right > i {
                    P[i] = P[i_mirror].min(right - i);
                }
                while T.chars().nth(i + 1 + P[i]).unwrap() == T.chars().nth(i - 1 - P[i]).unwrap() {
                    P[i] += 1;
                }
                if i + P[i] > right {
                    center = i;
                    right = i + P[i];
                }
            }

            let mut max_i = 0;
            for i in 1..T.len() - 1 {
                if P[i] > P[max_i] {
                    max_i = i;
                }
            }

            let start = (max_i - 1 - P[max_i]) / 2;
            s[start..start + P[max_i]].to_string()
        }

        fn pre_process(&self, s: &str) -> String {
            if s.is_empty() {
                return "^$".to_string();
            }
            let mut T = String::from("^");
            for c in s.chars() {
                T.push_str(&format!("#{}", c));
            }
            T.push_str("#$");
            T
        }
    }

    fn main() {
        let solution = Solution {};
        let input = "babad";
        println!("Longest Palindromic Substring: {}", solution.longest_palindrome(input));
    }
    ```

=== "C#"

    ```csharp
    using System;

    public class Solution {
        public string LongestPalindrome(string s) {
            if (string.IsNullOrEmpty(s)) return "";
            string T = PreProcess(s);
            int[] P = new int[T.Length];
            int center = 0, right = 0;

            for (int i = 1; i < T.Length - 1; i++) {
                int i_mirror = 2 * center - i;
                if (right > i) {
                    P[i] = Math.Min(right - i, P[i_mirror]);
                }

                while (T[i + 1 + P[i]] == T[i - 1 - P[i]]) {
                    P[i]++;
                }

                if (i + P[i] > right) {
                    center = i;
                    right = i + P[i];
                }
            }

            int max_i = 0;
            for (int i = 1; i < T.Length - 1; i++) {
                if (P[i] > P[max_i]) {
                    max_i = i;
                }
            }

            int start = (max_i - 1 - P[max_i]) / 2;
            return s.Substring(start, P[max_i]);
        }

        private string PreProcess(string s) {
            if (string.IsNullOrEmpty(s)) return "^$";
            var T = "^";
            foreach (var c in s) {
                T += "#" + c;
            }
            T += "#$";
            return T;
        }
    }

    class Program {
        static void Main() {
            Solution solution = new Solution();
            string input = "babad";
            Console.WriteLine("Longest Palindromic Substring: " + solution.LongestPalindrome(input));
        }
    }
    ```

=== "Java"

    ```java
    public class Solution {

        public String longestPalindrome(String s) {
            if (s.isEmpty()) return "";
            String T = preProcess(s);
            int[] P = new int[T.length()];
            int center = 0, right = 0;

            for (int i = 1; i < T.length() - 1; i++) {
                int i_mirror = 2 * center - i;
                if (right > i) {
                    P[i] = Math.min(right - i, P[i_mirror]);
                }

                while (T.charAt(i + 1 + P[i]) == T.charAt(i - 1 - P[i])) {
                    P[i]++;
                }

                if (i + P[i] > right) {
                    center = i;
                    right = i + P[i];
                }
            }

            int max_i = 0;
            for (int i = 1; i < T.length() - 1; i++) {
                if (P[i] > P[max_i]) {
                    max_i = i;
                }
            }

            int start = (max_i - 1 - P[max_i]) / 2;
            return s.substring(start, start + P[max_i]);
        }

        private String preProcess(String s) {
            if (s.isEmpty()) return "^$";
            StringBuilder T = new StringBuilder("^");
            for (char c : s.toCharArray()) {
                T.append("#").append(c);
            }
            T.append("#$");
            return T.toString();
        }

        public static void main(String[] args) {
            Solution solution = new Solution();
            String input = "babad";
            System.out.println("Longest Palindromic Substring: " + solution.longestPalindrome(input));
        }
    }
    ```

=== "Scala"

    ```scala
    class Solution {
        def longestPalindrome(s: String): String = {
            if (s.isEmpty) return ""
            val T = preProcess(s)
            val P = Array.fill(T.length)(0)
            var center = 0
            var right = 0

            for (i <- 1 until T.length - 1) {
                val i_mirror = 2 * center - i
                if (right > i) {
                    P(i) = Math.min(right - i, P(i_mirror))
                }

                while (T(i + 1 + P(i)) == T(i - 1 - P(i))) {
                    P(i) += 1
                }

                if (i + P(i) > right) {
                    center = i
                    right = i + P(i)
                }
            }

            var max_i = 0
            for (i <- 1 until T.length - 1) {
                if (P(i) > P(max_i)) max_i = i
            }

            val start = (max_i - 1 - P(max_i)) / 2
            s.substring(start, start + P(max_i))
        }

        private def preProcess(s: String): String = {
            if (s.isEmpty) "^$"
            else "^" + s.flatMap(c => "#" + c.toString) + "#$"
        }
    }

    object Main extends App {
        val solution = new Solution()
        val input = "babad"
        println(s"Longest Palindromic Substring: ${solution.longestPalindrome(input)}")
    }

    ```

=== "Kotlin"

    ```kotlin
    class Solution {
        fun longestPalindrome(s: String): String {
            if (s.isEmpty()) return ""

            val T = preProcess(s)
            val P = IntArray(T.length)
            var center = 0
            var right = 0

            for (i in 1 until T.length - 1) {
                val iMirror = 2 * center - i
                if (right > i) {
                    P[i] = Math.min(right - i, P[iMirror])
                } else {
                    P[i] = 0
                }

                // Attempt to expand palindrome centered at i
                while (T[i + 1 + P[i]] == T[i - 1 - P[i]]) {
                    P[i]++
                }

                // If palindrome centered at i expands past right, adjust center based on expanded palindrome
                if (i + P[i] > right) {
                    center = i
                    right = i + P[i]
                }
            }

            // Find the maximum element in P to locate the longest palindrome
            var max_i = 0
            for (i in 1 until T.length - 1) {
                if (P[i] > P[max_i]) {
                    max_i = i
                }
            }

            val start = (max_i - 1 - P[max_i]) / 2
            return s.substring(start, start + P[max_i])
        }

        private fun preProcess(s: String): String {
            if (s.isEmpty()) return "^$"
            val sb = StringBuilder("^")
            for (c in s) {
                sb.append("#").append(c)
            }
            sb.append("#$")
            return sb.toString()
        }
    }

    fun main() {
        val solution = Solution()
        val input = "babad"
        println("Longest Palindromic Substring: ${solution.longestPalindrome(input)}")
    }
    ```

=== "Go"

    ```go
    package main

    import (
        "fmt"
    )

    type Solution struct{}

    func (s Solution) longestPalindrome(str string) string {
        if len(str) == 0 {
            return ""
        }

        T := s.preProcess(str)
        P := make([]int, len(T))
        center, right := 0, 0

        for i := 1; i < len(T)-1; i++ {
            i_mirror := 2*center - i
            if right > i {
                P[i] = min(right-i, P[i_mirror])
            }

            for T[i+1+P[i]] == T[i-1-P[i]] {
                P[i]++
            }

            if i+P[i] > right {
                center = i
                right = i + P[i]
            }
        }

        max_i := 0
        for i := 1; i < len(T)-1; i++ {
            if P[i] > P[max_i] {
                max_i = i
            }
        }

        start := (max_i - 1 - P[max_i]) / 2
        return str[start : start+P[max_i]]
    }

    func (s Solution) preProcess(str string) string {
        if len(str) == 0 {
            return "^$"
        }
        T := "^"
        for _, c := range str {
            T += "#" + string(c)
        }
        T += "#$"
        return T
    }

    func min(a, b int) int {
        if a < b {
            return a
        }
        return b
    }

    func main() {
        solution := Solution{}
        input := "babad"
        fmt.Println("Longest Palindromic Substring:", solution.longestPalindrome(input))
    }
    ```

=== "TypeScript"

    ```typescript
    class Solution {
        reverseList(list1: number[], list2: number[]): number[] {
            let result: number[] = [];
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

            result = result.concat(list1.slice(i));
            result = result.concat(list2.slice(j));

            return result;
        }
    }

    const main = (): void => {
        const list1 = [1, 3, 5, 7];
        const list2 = [2, 4, 6, 8];

        const solution = new Solution();
        console.log("Merged List:", solution.reverseList(list1, list2));
    }

    main();
    ```

=== "R"

    ```r
    #library(R6)
    #Solution <- R6::R6Class("Solution",
    Solution <- setRefClass(
        "Solution",
        methods = list(
            reverseList = function(list1, list2) {
                result <- c()
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
            }
        )
    )

    main <- function() {
        list1 <- c(1, 3, 5, 7)
        list2 <- c(2, 4, 6, 8)

        solution <- Solution$new()
        cat("Merged List:", solution$reverseList(list1, list2), "\n")
    }

    main()
    ```

=== "Julia"

    ```julia
    module SolutionModule

    struct Solution
    end

    function reverseList(::Solution, list1::Vector{Int}, list2::Vector{Int})::Vector{Int}
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

        result
    end

    function main()
        list1 = [1, 3, 5, 7]
        list2 = [2, 4, 6, 8]

        solution = Solution()
        println("Merged List: ", reverseList(solution, list1, list2))
    end

    end

    using .SolutionModule
    SolutionModule.main()
    ```

## Zigzag Conversion

### Problem Description: [LeetCode - Problem 6 - Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation
This solution implements the zigzag pattern by iterating through each row and adding characters in the zigzag order.

1. If there's only one row, return the string as is.
2. Calculate the step size: 2 * numRows - 2. This represents the number of characters before the pattern repeats.
3. Iterate through each row:
   a. For the first and last rows, add characters at intervals of the step size.
   b. For middle rows, add two characters: one from the current position, and another from (j + step - 2 * i).
4. Return the constructed zigzag string.

### Complexity Analysis
- __Time Complexity: `O(n)`, where `n` is the length of the input string. We iterate through each character once.__
- __Space Complexity: `O(n)` to store the result string.__

### Solutions

=== "Python"

    ```python
    class Solution:
        def convert(self, s: str, numRows: int) -> str:
            """
            :type s: str
            :type numRows: int
            :rtype: str
            """
            if numRows == 1:
                return s
            step, zigzag = 2 * numRows - 2, ""
            for i in range(numRows):
                for j in range(i, len(s), step):
                    zigzag += s[j]
                    if 0 < i < numRows - 1 and j + step - 2 * i < len(s):
                        zigzag += s[j + step - 2 * i]
            return zigzag
    ```

=== "C++"

    ```cpp
    #include <string>

    class Solution {
    public:
        std::string convert(const std::string &s, int numRows) {
            if (numRows == 1) return s;
            
            std::string zigzag;
            int step = 2 * numRows - 2;

            for (int i = 0; i < numRows; ++i) {
                for (int j = i; j < s.size(); j += step) {
                    zigzag += s[j];
                    if (i > 0 && i < numRows - 1 && j + step - 2 * i < s.size()) {
                        zigzag += s[j + step - 2 * i];
                    }
                }
            }
            return zigzag;
        }
    };
    ```

=== "Rust"

    ```rust
    pub struct Solution;

    impl Solution {
        pub fn convert(s: String, numRows: i32) -> String {
            if numRows == 1 {
                return s;
            }

            let mut zigzag = String::new();
            let step = 2 * numRows - 2;

            for i in 0..numRows {
                let mut j = i;
                while j < s.len() as i32 {
                    zigzag.push(s.chars().nth(j as usize).unwrap());
                    if i > 0 && i < numRows - 1 && j + step - 2 * i < s.len() as i32 {
                        zigzag.push(s.chars().nth((j + step - 2 * i) as usize).unwrap());
                    }
                    j += step;
                }
            }
            zigzag
        }
    }
    ```

=== "C#"

    ```csharp
    public class Solution {
        public string Convert(string s, int numRows) {
            if (numRows == 1) return s;
            
            string zigzag = "";
            int step = 2 * numRows - 2;

            for (int i = 0; i < numRows; i++) {
                for (int j = i; j < s.Length; j += step) {
                    zigzag += s[j];
                    if (i > 0 && i < numRows - 1 && j + step - 2 * i < s.Length) {
                        zigzag += s[j + step - 2 * i];
                    }
                }
            }
            return zigzag;
        }
    }
    ```

=== "Java"

    ```java
    class Solution {
        public String convert(String s, int numRows) {
            if (numRows == 1) return s;
            
            StringBuilder zigzag = new StringBuilder();
            int step = 2 * numRows - 2;

            for (int i = 0; i < numRows; i++) {
                for (int j = i; j < s.length(); j += step) {
                    zigzag.append(s.charAt(j));
                    if (i > 0 && i < numRows - 1 && j + step - 2 * i < s.length()) {
                        zigzag.append(s.charAt(j + step - 2 * i));
                    }
                }
            }
            return zigzag.toString();
        }
    }
    ```

=== "Scala"

    ```scala
    class Solution {
        def convert(s: String, numRows: Int): String = {
            if (numRows == 1) return s
            
            val zigzag = new StringBuilder()
            val step = 2 * numRows - 2

            for (i <- 0 until numRows) {
                for (j <- i until s.length by step) {
                    zigzag.append(s(j))
                    if (i > 0 && i < numRows - 1 && j + step - 2 * i < s.length) {
                        zigzag.append(s(j + step - 2 * i))
                    }
                }
            }
            zigzag.toString()
        }
    }
    ```

=== "Kotlin"

    ```kotlin
    class Solution {
        fun convert(s: String, numRows: Int): String {
            if (numRows == 1) return s
            
            val zigzag = StringBuilder()
            val step = 2 * numRows - 2

            for (i in 0 until numRows) {
                for (j in i until s.length step step) {
                    zigzag.append(s[j])
                    if (i > 0 && i < numRows - 1 && j + step - 2 * i < s.length) {
                        zigzag.append(s[j + step - 2 * i])
                    }
                }
            }
            return zigzag.toString()
        }
    }
    ```

=== "Go"

    ```go
    package main

    import "strings"

    type Solution struct{}

    func (Solution) Convert(s string, numRows int) string {
        if numRows == 1 {
            return s
        }

        zigzag := strings.Builder{}
        step := 2*numRows - 2

        for i := 0; i < numRows; i++ {
            for j := i; j < len(s); j += step {
                zigzag.WriteByte(s[j])
                if i > 0 && i < numRows-1 && j+step-2*i < len(s) {
                    zigzag.WriteByte(s[j+step-2*i])
                }
            }
        }
        return zigzag.String()
    }
    ```

=== "TypeScript"

    ```typescript
    class Solution {
        convert(s: string, numRows: number): string {
            if (numRows === 1) return s;
            
            let zigzag = "";
            const step = 2 * numRows - 2;

            for (let i = 0; i < numRows; i++) {
                for (let j = i; j < s.length; j += step) {
                    zigzag += s[j];
                    if (i > 0 && i < numRows - 1 && j + step - 2 * i < s.length) {
                        zigzag += s[j + step - 2 * i];
                    }
                }
            }
            return zigzag;
        }
    }
    ```

=== "R"

    ```r
    #library(R6)
    #Solution <- R6::R6Class("Solution",
    Solution <- setRefClass(
        "Solution",
        methods = list(
            convert = function(s, numRows) {
                if (numRows == 1) return(s)
                
                zigzag <- ""
                step <- 2 * numRows - 2

                for (i in seq_len(numRows)) {
                    for (j in seq(i, nchar(s), by = step)) {
                        zigzag <- paste0(zigzag, substr(s, j, j))
                        if (i > 1 && i < numRows && j + step - 2 * i <= nchar(s)) {
                            zigzag <- paste0(zigzag, substr(s, j + step - 2 * i, j + step - 2 * i))
                        }
                    }
                }
                zigzag
            }
        )
    )
    ```

=== "Julia"

    ```julia
    module SolutionModule

    struct Solution
    end

    function reverseList(::Solution, s::String, numRows::Int)::String
        if numRows == 1
            return s
        end

        zigzag = ""
        step = 2 * numRows - 2

        for i in 1:numRows
            for j in i:step:length(s)
                zigzag *= s[j]
                if i > 1 && i < numRows && j + step - 2 * i <= length(s)
                    zigzag *= s[j + step - 2 * i]
                end
            end
        end
        return zigzag
    end

    end
    ```

## String to Integer (`atoi`)

### Problem Description: [LeetCode - Problem 8 - String to Integer (`atoi`)](https://leetcode.com/problems/string-to-integer-atoi/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation
1. Skip leading whitespace.
2. Check for a sign (+ or -).
3. Iterate through digits, building the result:
   a. Check for overflow before adding each digit.
   b. If overflow would occur, return INT_MAX or INT_MIN based on the sign.
4. Apply the sign to the result and return.

### Complexity Analysis
- __Time Complexity: `O(n)`, where `n` is the length of the input string. We iterate through the string once.__
- __Space Complexity: `O(1)`, as we only use a constant amount of extra space.__

### Solutions

=== "Python"

    ```python
    import sys

    class Solution:
        def myAtoi(self, str: str) -> int:
            """
            :type str: str
            :rtype: int
            """
            #INT_MAX =  2147483647
            #INT_MIN = -2147483648
            INT_MAX = sys.maxsize
            INT_MIN = -sys.maxsize - 1
            result = 0

            if not str:
                return result

            i = 0
            while i < len(str) and str[i].isspace():
                i += 1

            if len(str) == i:
                return result

            sign = 1
            if str[i] == "+":
                i += 1
            elif str[i] == "-":
                sign = -1
                i += 1

            while i < len(str) and '0' <= str[i] <= '9':
                if result > (INT_MAX - int(str[i])) / 10:
                    return INT_MAX if sign > 0 else INT_MIN
                result = result * 10 + int(str[i])
                i += 1

            return sign * result
    ```

=== "C++"

    ```cpp
    #include <string>
    #include <climits>

    class Solution {
    public:
        int myAtoi(const std::string &s) {
            int i = 0, sign = 1;
            long result = 0;

            // Skip whitespaces
            while (i < s.length() && s[i] == ' ') i++;

            // Check for optional sign
            if (i < s.length() && (s[i] == '+' || s[i] == '-')) {
                sign = (s[i] == '-') ? -1 : 1;
                i++;
            }

            // Convert digits to integer
            while (i < s.length() && isdigit(s[i])) {
                result = result * 10 + (s[i++] - '0');
                if (result * sign >= INT_MAX) return INT_MAX;
                if (result * sign <= INT_MIN) return INT_MIN;
            }

            return result * sign;
        }
    };
    ```

=== "Rust"

    ```rust
    pub struct Solution;

    impl Solution {
        pub fn my_atoi(s: String) -> i32 {
            let mut chars = s.chars().peekable();
            let mut result: i64 = 0;
            let mut sign = 1;
            let mut started = false;

            while let Some(&ch) = chars.peek() {
                if !started && ch == ' ' {
                    chars.next();
                    continue;
                }
                if !started && (ch == '-' || ch == '+') {
                    sign = if ch == '-' { -1 } else { 1 };
                    chars.next();
                    started = true;
                    continue;
                }
                if let Some(digit) = ch.to_digit(10) {
                    result = result * 10 + digit as i64;
                    if result * sign > i32::MAX as i64 {
                        return i32::MAX;
                    }
                    if result * sign < i32::MIN as i64 {
                        return i32::MIN;
                    }
                    chars.next();
                    started = true;
                } else {
                    break;
                }
            }

            (result * sign) as i32
        }
    }
    ```

=== "C#"

    ```csharp
    using System;

    public class Solution {
        public int MyAtoi(string s) {
            int i = 0, sign = 1;
            long result = 0;

            while (i < s.Length && s[i] == ' ') i++;

            if (i < s.Length && (s[i] == '+' || s[i] == '-')) {
                sign = (s[i] == '-') ? -1 : 1;
                i++;
            }

            while (i < s.Length && Char.IsDigit(s[i])) {
                result = result * 10 + (s[i++] - '0');
                if (result * sign >= int.MaxValue) return int.MaxValue;
                if (result * sign <= int.MinValue) return int.MinValue;
            }

            return (int)(result * sign);
        }
    }
    ```

=== "Java"

    ```java
    class Solution {
        public int myAtoi(String s) {
            int i = 0, sign = 1;
            long result = 0;

            while (i < s.length() && s.charAt(i) == ' ') i++;

            if (i < s.length() && (s.charAt(i) == '+' || s.charAt(i) == '-')) {
                sign = (s.charAt(i) == '-') ? -1 : 1;
                i++;
            }

            while (i < s.length() && Character.isDigit(s.charAt(i))) {
                result = result * 10 + (s.charAt(i++) - '0');
                if (result * sign >= Integer.MAX_VALUE) return Integer.MAX_VALUE;
                if (result * sign <= Integer.MIN_VALUE) return Integer.MIN_VALUE;
            }

            return (int)(result * sign);
        }
    }
    ```

=== "Scala"

    ```scala
    class Solution {
        def myAtoi(s: String): Int = {
            var i = 0
            var sign = 1
            var result: Long = 0

            while (i < s.length && s(i) == ' ') i += 1

            if (i < s.length && (s(i) == '+' || s(i) == '-')) {
                sign = if (s(i) == '-') -1 else 1
                i += 1
            }

            while (i < s.length && s(i).isDigit) {
                result = result * 10 + (s(i) - '0')
                if (result * sign >= Int.MaxValue) return Int.MaxValue
                if (result * sign <= Int.MinValue) return Int.MinValue
                i += 1
            }

            (result * sign).toInt
        }
    }
    ```

=== "Kotlin"

    ```kotlin
    class Solution {
        fun myAtoi(s: String): Int {
            var i = 0
            var sign = 1
            var result: Long = 0

            while (i < s.length && s[i] == ' ') i++

            if (i < s.length && (s[i] == '+' || s[i] == '-')) {
                sign = if (s[i] == '-') -1 else 1
                i++
            }

            while (i < s.length && s[i].isDigit()) {
                result = result * 10 + (s[i++] - '0')
                if (result * sign >= Int.MAX_VALUE) return Int.MAX_VALUE
                if (result * sign <= Int.MIN_VALUE) return Int.MIN_VALUE
            }

            return (result * sign).toInt()
        }
    }
    ```

=== "Go"

    ```go
    package main

    import (
        "math"
        "unicode"
    )

    type Solution struct{}

    func (Solution) MyAtoi(s string) int {
        i, sign := 0, 1
        var result int64 = 0

        // Skip whitespaces
        for i < len(s) && s[i] == ' ' {
            i++
        }

        // Check for optional sign
        if i < len(s) && (s[i] == '-' || s[i] == '+') {
            if s[i] == '-' {
                sign = -1
            }
            i++
        }

        // Convert digits to integer
        for i < len(s) && unicode.IsDigit(rune(s[i])) {
            result = result*10 + int64(s[i]-'0')
            if result*int64(sign) > math.MaxInt32 {
                return math.MaxInt32
            }
            if result*int64(sign) < math.MinInt32 {
                return math.MinInt32
            }
            i++
        }

        return int(result * int64(sign))
    }
    ```

=== "TypeScript"

    ```typescript
    class Solution {
        myAtoi(s: string): number {
            let i = 0;
            let sign = 1;
            let result = 0;

            while (i < s.length && s[i] === ' ') i++;

            if (i < s.length && (s[i] === '+' || s[i] === '-')) {
                sign = s[i] === '-' ? -1 : 1;
                i++;
            }

            while (i < s.length && /\d/.test(s[i])) {
                result = result * 10 + (s[i].charCodeAt(0) - '0'.charCodeAt(0));
                if (result * sign > 2 ** 31 - 1) return 2 ** 31 - 1;
                if (result * sign < -(2 ** 31)) return -(2 ** 31);
                i++;
            }

            return result * sign;
        }
    }
    ```

=== "R"

    ```r
    #library(R6)
    #Solution <- R6::R6Class("Solution",
    Solution <- setRefClass(
        "Solution",
        methods = list(
            my_atoi = function(s) {
                i <- 1
                sign <- 1
                result <- 0

                while (i <= nchar(s) && substr(s, i, i) == " ") {
                    i <- i + 1
                }

                if (i <= nchar(s) && (substr(s, i, i) == "+" || substr(s, i, i) == "-")) {
                    sign <- ifelse(substr(s, i, i) == "-", -1, 1)
                    i <- i + 1
                }

                while (i <= nchar(s) && grepl("[0-9]", substr(s, i, i))) {
                    result <- result * 10 + as.numeric(substr(s, i, i))
                    if (result * sign >= .Machine$integer.max) return(.Machine$integer.max)
                    if (result * sign <= -.Machine$integer.max) return(-.Machine$integer.max)
                    i <- i + 1
                }

                result * sign
            }
        )
    )
    ```

=== "Julia"

    ```julia
    module SolutionModule

    struct Solution
    end

    function my_atoi(::Solution, s::String)::Int
        i = 1
        sign = 1
        result = 0

        while i <= length(s) && s[i] == ' '
            i += 1
        end

        if i <= length(s) && (s[i] == '+' || s[i] == '-')
            sign = s[i] == '-' ? -1 : 1
            i += 1
        end

        while i <= length(s) && isdigit(s[i])
            result = result * 10 + (s[i] - '0')
            if result * sign >= typemax(Int32)
                return typemax(Int32)
            end
            if result * sign <= typemin(Int32)
                return typemin(Int32)
            end
            i += 1
        end

        return result * sign
    end

    end
    ```

## Longest Common Prefix

### Problem Description: [LeetCode - Problem 14 - Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation
This solution finds the longest common prefix among an array of strings.

1. If the array is empty, return an empty string.
2. Iterate through characters of the first string:
   a. Compare this character with the corresponding character in all other strings.
   b. If a mismatch is found or we reach the end of any string, return the prefix up to this point.
3. If we complete the loop, the entire first string is the common prefix.

### Complexity Analysis
- __Time Complexity: `O(S)`, where `S` is the sum of all characters in all strings. In the worst case, we compare every character of every string.__
- __Space Complexity: `O(1)`, as we only use a constant amount of extra space.__

### Solutions

=== "Python"

    ```python
    class Solution:
        def longestCommonPrefix(self, strs: List[str]) -> str:
            """
            :type strs: List[str]
            :rtype: str
            """
            if not strs:
                return ""

            for i in range(len(strs[0])):
                for string in strs[1:]:
                    if i >= len(string) or string[i] != strs[0][i]:
                        return strs[0][:i]
            return strs[0]
    ```

=== "C++"

    ```cpp
    #include <iostream>
    #include <vector>
    #include <string>

    class Solution {
    public:
        std::string longestCommonPrefix(const std::vector<std::string>& strs) {
            if (strs.empty()) return "";
            std::string prefix = strs[0];
            for (int i = 1; i < strs.size(); ++i) {
                while (strs[i].find(prefix) != 0) {
                    prefix = prefix.substr(0, prefix.size() - 1);
                    if (prefix.empty()) return "";
                }
            }
            return prefix;
        }
    };

    int main() {
        Solution solution;
        std::vector<std::string> strs = {"flower", "flow", "flight"};
        std::cout << "Longest Common Prefix: " << solution.longestCommonPrefix(strs) << std::endl;
        return 0;
    }
    ```

=== "Rust"

    ```rust
    struct Solution;

    impl Solution {
        pub fn longest_common_prefix(strs: Vec<String>) -> String {
            if strs.is_empty() {
                return "".to_string();
            }
            let mut prefix = strs[0].clone();
            for s in &strs[1..] {
                while !s.starts_with(&prefix) {
                    prefix.pop();
                    if prefix.is_empty() {
                        return "".to_string();
                    }
                }
            }
            prefix
        }
    }

    fn main() {
        let strs = vec!["flower".to_string(), "flow".to_string(), "flight".to_string()];
        let result = Solution::longest_common_prefix(strs);
        println!("Longest Common Prefix: {}", result);
    }
    ```

=== "C#"

    ```csharp
    class Solution {
        public String longestCommonPrefix(String[] strs) {
            if (strs == null || strs.length == 0) return "";
            String prefix = strs[0];
            for (int i = 1; i < strs.length; i++) {
                while (strs[i].indexOf(prefix) != 0) {
                    prefix = prefix.substring(0, prefix.length() - 1);
                    if (prefix.isEmpty()) return "";
                }
            }
            return prefix;
        }

        public static void main(String[] args) {
            Solution solution = new Solution();
            String[] strs = {"flower", "flow", "flight"};
            System.out.println("Longest Common Prefix: " + solution.longestCommonPrefix(strs));
        }
    }
    ```

=== "Java"

    ```java
    class Solution {
        public String longestCommonPrefix(String[] strs) {
            if (strs == null || strs.length == 0) return "";
            String prefix = strs[0];
            for (int i = 1; i < strs.length; i++) {
                while (strs[i].indexOf(prefix) != 0) {
                    prefix = prefix.substring(0, prefix.length() - 1);
                    if (prefix.isEmpty()) return "";
                }
            }
            return prefix;
        }

        public static void main(String[] args) {
            Solution solution = new Solution();
            String[] strs = {"flower", "flow", "flight"};
            System.out.println("Longest Common Prefix: " + solution.longestCommonPrefix(strs));
        }
    }
    ```

=== "Scala"

    ```scala
    object Solution {
        def longestCommonPrefix(strs: Array[String]): String = {
            if (strs.isEmpty) return ""
            var prefix = strs(0)
            for (s <- strs.tail) {
                while (!s.startsWith(prefix)) {
                    prefix = prefix.substring(0, prefix.length - 1)
                    if (prefix.isEmpty) return ""
                }
            }
            prefix
        }

        def main(args: Array[String]): Unit = {
            val strs = Array("flower", "flow", "flight")
            println(s"Longest Common Prefix: ${longestCommonPrefix(strs)}")
        }
    }
    ```

=== "Kotlin"

    ```kotlin
    class Solution {
        fun longestCommonPrefix(strs: Array<String>): String {
            if (strs.isEmpty()) return ""
            var prefix = strs[0]
            for (i in 1 until strs.size) {
                while (!strs[i].startsWith(prefix)) {
                    prefix = prefix.substring(0, prefix.length - 1)
                    if (prefix.isEmpty()) return ""
                }
            }
            return prefix
        }
    }

    fun main() {
        val solution = Solution()
        val strs = arrayOf("flower", "flow", "flight")
        println("Longest Common Prefix: ${solution.longestCommonPrefix(strs)}")
    }
    ```

=== "Go"

    ```go
    package main

    import (
        "fmt"
        "strings"
    )

    type Solution struct{}

    func (s Solution) LongestCommonPrefix(strs []string) string {
        if len(strs) == 0 {
            return ""
        }
        prefix := strs[0]
        for i := 1; i < len(strs); i++ {
            for !strings.HasPrefix(strs[i], prefix) {
                prefix = prefix[:len(prefix)-1]
                if len(prefix) == 0 {
                    return ""
                }
            }
        }
        return prefix
    }

    func main() {
        solution := Solution{}
        strs := []string{"flower", "flow", "flight"}
        fmt.Println("Longest Common Prefix:", solution.LongestCommonPrefix(strs))
    }
    ```

=== "TypeScript"

    ```typescript
    class Solution {
        longestCommonPrefix(strs: string[]): string {
            if (strs.length === 0) return "";
            let prefix = strs[0];
            for (let i = 1; i < strs.length; i++) {
                while (!strs[i].startsWith(prefix)) {
                    prefix = prefix.substring(0, prefix.length - 1);
                    if (prefix === "") return "";
                }
            }
            return prefix;
        }
    }

    function main() {
        const solution = new Solution();
        const strs = ["flower", "flow", "flight"];
        console.log(`Longest Common Prefix: ${solution.longestCommonPrefix(strs)}`);
    }

    main();
    ```

=== "R"

    ```r
    #library(R6)
    #Solution <- R6::R6Class("Solution",
    Solution <- setRefClass("Solution", methods = list(
            longestCommonPrefix = function(strs) {
                if (length(strs) == 0) return("")
                prefix <- strs[1]
                for (i in 2:length(strs)) {
                    while (substr(strs[i], 1, nchar(prefix)) != prefix) {
                        prefix <- substr(prefix, 1, nchar(prefix) - 1)
                        if (prefix == "") return("")
                    }
                }
                return(prefix)
            }
        )
    )

    main <- function() {
        solution <- Solution$new()
        strs <- c("flower", "flow", "flight")
        cat("Longest Common Prefix:", solution$longestCommonPrefix(strs), "\n")
    }

    main()
    ```

=== "Julia"

    ```julia
    struct Solution
    end

    function longest_common_prefix(strs::Vector{String})::String
        if isempty(strs)
            return ""
        end
        prefix = strs[1]
        for i in 2:length(strs)
            while !startswith(strs[i], prefix)
                prefix = prefix[1:end-1]
                if isempty(prefix)
                    return ""
                end
            end
        end
        return prefix
    end

    function main()
        strs = ["flower", "flow", "flight"]
        prefix = longest_common_prefix(strs)
        println("Longest Common Prefix: ", prefix)
    end

    main()
    ```

## Implement `strStr()` - `Knuth-Morris-Platt (KMP) Algorithm`

### Problem Description: [LeetCode - Problem 28 - Implement `strStr()`](https://leetcode.com/problems/implement-strstr/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

### Complexity Analysis

### Solutions

=== "Python"

    ```python
    from typing import List

    class Solution:
        def strStr(self, haystack: str, needle: str) -> int:
            """
            :type haystack: str
            :type needle: str
            :rtype: int
            """
            if not needle:
                return 0

            return self.KMP(haystack, needle)

        def KMP(self, text: str, pattern: str) -> int:
            prefix = self.getPrefix(pattern)
            j = -1
            for i in range(len(text)):
                while j > -1 and pattern[j + 1] != text[i]:
                    j = prefix[j]
                if pattern[j + 1] == text[i]:
                    j += 1
                if j == len(pattern) - 1:
                    return i - j
            return -1

        def getPrefix(self, pattern: str) -> List[int]:
            prefix = [-1] * len(pattern)
            j = -1
            for i in range(1, len(pattern)):
                while j > -1 and pattern[j + 1] != pattern[i]:
                    j = prefix[j]
                if pattern[j + 1] == pattern[i]:
                    j += 1
                prefix[i] = j
            return prefix
    ```

=== "C++"

    ```cpp
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

## Add Binary

### Problem Description: [LeetCode - Problem 67 - Add Binary](https://leetcode.com/problems/add-binary/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

### Complexity Analysis

### Solutions

=== "Python"

    ```python
    class Solution:
        # @param a, a string
        # @param b, a string
        # @return a string
        def addBinary(self, a: str, b: str) -> str:
            result, carry, val = "", 0, 0
            for i in range(max(len(a), len(b))):
                val = carry
                if i < len(a):
                    val += int(a[-(i + 1)])
                if i < len(b):
                    val += int(b[-(i + 1)])
                carry, val = divmod(val, 2)
                result += str(val)
            if carry:
                result += str(carry)
            return result[::-1]

    #from itertools import izip_longest
    #
    #
    #class Solution:
    #    def addBinary(self, a: str, b: str) -> str:
    #        """
    #        :type a: str
    #        :type b: str
    #        :rtype: str
    #        """
    #        result = ""
    #        carry = 0
    #        for x, y in izip_longest(reversed(a), reversed(b), fillvalue="0"):
    #            carry, remainder = divmod(int(x)+int(y)+carry, 2)
    #            result += str(remainder)
    #        
    #        if carry:
    #            result += str(carry)
    #        
    #        return result[::-1]
    ```

=== "C++"

    ```cpp
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

## Text Justification

### Problem Description: [LeetCode - Problem 68 - Add Binary](https://leetcode.com/problems/text-justification/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

### Complexity Analysis

### Solutions

=== "Python"

    ```python
    from typing import List
    class Solution:
        def fullJustify(self, words: str, maxWidth: str) -> List[str]:
            """
            :type words: List[str]
            :type maxWidth: int
            :rtype: List[str]
            """
            def addSpaces(i, spaceCnt, maxWidth, is_last):
                if i < spaceCnt:
                    # For the last line of text, it should be left justified,
                    # and no extra space is inserted between words.
                    return 1 if is_last else (maxWidth // spaceCnt) + int(i < maxWidth % spaceCnt)
                return 0

            def connect(words, maxWidth, begin, end, length, is_last):
                s = []  # The extra space O(k) is spent here.
                n = end - begin
                for i in range(n):
                    s += words[begin + i],
                    s += ' ' * addSpaces(i, n - 1, maxWidth - length, is_last),
                # For only one word in a line.
                line = "".join(s)
                if len(line) < maxWidth:
                    line += ' ' * (maxWidth - len(line))
                return line

            res = []
            begin, length = 0, 0
            for i in range(len(words)):
                if length + len(words[i]) + (i - begin) > maxWidth:
                    res += connect(words, maxWidth, begin, i, length, False),
                    begin, length = i, 0
                length += len(words[i])

            # Last line.
            res += connect(words, maxWidth, begin, len(words), length, True),
            return res
    ```

=== "C++"

    ```cpp
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

## Valid Palindrome

### Problem Description: [LeetCode - Problem 125 - Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

### Complexity Analysis

### Solutions

=== "Python"

    ```python
    class Solution:
        # @param s, a string
        # @return a boolean
        def isPalindrome(self, s: str) -> bool:
            i, j = 0, len(s) - 1
            while i < j:
                while i < j and not s[i].isalnum():
                    i += 1
                while i < j and not s[j].isalnum():
                    j -= 1
                if s[i].lower() != s[j].lower():
                    return False
                i, j = i + 1, j - 1
            return True
    ```

=== "C++"

    ```cpp
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

## Reverse Words in a String

### Problem Description: [LeetCode - Problem 151 - Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

### Complexity Analysis

### Solutions

=== "Python"

    ```python
    class Solution:
        ## @param s, a string
        ## @return a string
        #def reverseWords(self, s: str) -> str:
        #    return ' '.join(reversed(s.split()))

        ## @param s, a string
        ## @return a string
        #def reverseWords(self, s: str) -> str:
        #    return" ".join(s.split()[::-1])

        # @param s, a string
        # @return a string
        # Without built-in
        def reverseWords(self, s: str) -> str:
            word=""
            res=""
            for i in s+" ":
                if i!=" ":
                    word+=i
                elif word:
                    res=word+" "+res
                    word=""
            return res[:-1]
    ```

=== "C++"

    ```cpp
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

## Shortest Palindrome - `Manacher's Algorithm`

### Problem Description: [LeetCode - Problem 214 - Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

### Complexity Analysis

### Solutions

=== "Python"

    ```python
    class Solution:
        def shortestPalindrome(self, s: str) -> str:
            """
            :type s: str
            :rtype: str
            """
            def preProcess(s):
                if not s:
                    return ['^', '$']
                string = ['^']
                for c in s:
                    string +=  ['#', c]
                string += ['#', '$']
                return string

            string = preProcess(s)
            palindrome = [0] * len(string)
            center, right = 0, 0
            for i in range(1, len(string) - 1):
                i_mirror = 2 * center - i
                if right > i:
                    palindrome[i] = min(right - i, palindrome[i_mirror])
                else:
                    palindrome[i] = 0

                while string[i + 1 + palindrome[i]] == string[i - 1 - palindrome[i]]:
                    palindrome[i] += 1

                if i + palindrome[i] > right:
                    center, right = i, i + palindrome[i]

            max_len = 0
            for i in range(1, len(string) - 1):
                if i - palindrome[i] == 1:
                    max_len = palindrome[i]
            return s[len(s)-1:max_len-1:-1] + s
    ```

=== "C++"

    ```cpp
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

## Valid Anagram

### Problem Description: [LeetCode - Problem 242 - Valid Anagram](https://leetcode.com/problems/valid-anagram/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

### Complexity Analysis

### Solutions

=== "Python"

    ```python
    import collections

    class Solution:
        def isAnagram(self, s: str, t: str) -> bool:
            """
            :type s: str
            :type t: str
            :rtype: bool
            """
            if len(s) != len(t):
                return False
            count = collections.defaultdict(int)
            for c in s:
                count[c] += 1
            for c in t:
                count[c] -= 1
                if count[c] < 0:
                    return False
            return True
    ```

=== "C++"

    ```cpp
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

### Problem Description: [LeetCode - Problem 2800 - Shortest String That Contains Three Strings](https://leetcode.com/problems/shortest-string-that-contains-three-strings/)

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

__1. Suffix Array Construction (`build_suffix_array`):__
   - This function builds a suffix array for a given string using the prefix doubling algorithm.
   - It starts by sorting suffixes based on their first character, then iteratively sorts them based on longer prefixes (doubling the length each time).

__2. LCP Array Construction (`build_lcp_array`):__
   - This function builds the Longest Common Prefix (LCP) array from the suffix array and the original string.
   - It uses Kasai's algorithm to compute the LCP values efficiently.

__3. Finding Overlap (`find_overlap`):__
   - This function finds the maximum overlap between two strings using the suffix array and LCP array.
   - It combines the two strings with a separator, builds the suffix array and LCP array, and then finds the maximum LCP value between suffixes from different strings.

__4. Merging Strings (`merge_strings`):__
   - This function merges three strings by finding the overlap between the first two, then finding the overlap of the result with the third string.

__5. Main Function (`minimumString`):__
   - This function generates all permutations of the three input strings and finds the shortest merged string among them.
   - It uses the `merge_strings` function for each permutation and selects the shortest result (or lexicographically smallest if there are multiple shortest results).

### Complexity Analysis

__1. Suffix Array Construction [`build_suffix_array` - TC: `O(n log n)` ; SC: `O(n)`] :__
   - __Time Complexity: `O(n log n)`, where `n` is the length of the string.__
   - __Space Complexity: `O(n)` for storing the suffix array and temporary arrays.__

__2. LCP Array Construction [`build_lcp_array` - TC: `O(n)` ; SC: `O(n)`] :__
   - __Time Complexity: `O(n)`, where `n` is the length of the string.__
   - __Space Complexity: `O(n)` for storing the LCP array and rank array.__

__3. Finding Overlap [`find_overlap` - TC: `O(n log n)` ; SC: `O(n)`] :__
   - __Time Complexity: `O(n log n)` due to suffix array construction, where `n` is the total length of both strings.__
   - __Space Complexity: `O(n)` for storing the combined string, suffix array, and LCP array.__

__4. Merging Strings [`merge_strings` - TC: `O(n log n)` ; SC: `O(n)`] :__
   - __Time Complexity: `O(n log n)`, where `n` is the total length of all three strings.__
   - __Space Complexity: `O(n)` for storing the merged strings.__

__5. Main Function [`minimumString` - TC: `O(n log n)` ; SC: `O(n)`] :__
   - __Time Complexity: `O(n log n)`, where `n` is the total length of all input strings. Although we try 6 permutations, this is a constant factor.__
   - __Space Complexity: `O(n)` for storing the merged strings.__

__Overall Complexity Analysis [TC: `O(n log n)` ; SC: `O(n)`] :__
- __Time Complexity: `O(n log n)`, where n is the total length of all input strings. This is dominated by the suffix array construction, which is done multiple times but always with a total string length proportional to `n`.__
- __Space Complexity: `O(n)` for storing the suffix arrays, LCP arrays, and merged strings.__

- __Summary :__
This solution is theoretically more efficient than __`O(n^2)`__ solutions, especially for large inputs. The use of suffix arrays allows for efficient string matching and overlap finding. However, it's worth noting that:

1. The implementation is more complex than simpler approaches, which could make it more prone to bugs and harder to maintain.
2. For smaller inputs, simpler O(n^2) solutions might actually be faster due to lower constant factors and better cache performance.
3. This solution still tries all permutations of the three strings, which isn't necessary in theory but simplifies the implementation.

In practice, the choice between this solution and simpler ones would depend on the expected size and characteristics of the input strings, as well as the importance of optimizing for worst-case versus average-case performance.

### Solutions

=== "Python"

    ```python
    from typing import List, Tuple

    class Solution:
        def minimumString(self, string1: str, string2: str, string3: str) -> str:
            def build_suffix_array(text: str) -> List[int]:
                text_length = len(text)
                suffix_array = list(range(text_length))
                rank = [ord(char) for char in text]
                temp_rank = [0] * text_length
                chunk_size = 1
                while chunk_size < text_length:
                    suffix_array.sort(key=lambda i: (rank[i], rank[i+chunk_size] if i+chunk_size < text_length else -1))
                    for i in range(1, text_length):
                        temp_rank[i] = temp_rank[i-1] + (
                            (rank[suffix_array[i]], rank[suffix_array[i]+chunk_size] if suffix_array[i]+chunk_size < text_length else -1) !=
                            (rank[suffix_array[i-1]], rank[suffix_array[i-1]+chunk_size] if suffix_array[i-1]+chunk_size < text_length else -1)
                        )
                    for i in range(text_length):
                        rank[suffix_array[i]] = temp_rank[i]
                    if temp_rank[-1] == text_length - 1:
                        break
                    chunk_size *= 2
                return suffix_array

            def build_lcp_array(text: str, suffix_array: List[int]) -> List[int]:
                text_length = len(text)
                rank = [0] * text_length
                for i in range(text_length):
                    rank[suffix_array[i]] = i
                lcp = [0] * (text_length - 1)
                k = 0
                for i in range(text_length):
                    if rank[i] == text_length - 1:
                        k = 0
                        continue
                    j = suffix_array[rank[i] + 1]
                    while i + k < text_length and j + k < text_length and text[i+k] == text[j+k]:
                        k += 1
                    lcp[rank[i]] = k
                    if k:
                        k -= 1
                return lcp

            def find_overlap(string_a: str, string_b: str) -> Tuple[str, int]:
                combined_string = string_a + '#' + string_b
                suffix_array = build_suffix_array(combined_string)
                lcp_array = build_lcp_array(combined_string, suffix_array)
                
                max_overlap = 0
                for i in range(len(combined_string) - 1):
                    if (suffix_array[i] < len(string_a)) != (suffix_array[i+1] < len(string_a)):
                        max_overlap = max(max_overlap, lcp_array[i])
                
                if string_a in string_b:
                    return string_b, len(string_a)
                elif string_b in string_a:
                    return string_a, len(string_b)
                elif max_overlap > 0:
                    if suffix_array[i] < len(string_a):
                        return string_a + string_b[max_overlap:], max_overlap
                    else:
                        return string_b + string_a[max_overlap:], max_overlap
                else:
                    return string_a + string_b, 0

            def merge_strings(s1: str, s2: str, s3: str) -> str:
                merged_s1_s2, _ = find_overlap(s1, s2)
                final_merged, _ = find_overlap(merged_s1_s2, s3)
                return final_merged

            all_permutations = [
                (string1, string2, string3), (string1, string3, string2),
                (string2, string1, string3), (string2, string3, string1),
                (string3, string1, string2), (string3, string2, string1)
            ]
            return min((merge_strings(x[0], x[1], x[2]) for x in all_permutations), key=lambda s: (len(s), s))
    ```

=== "C++"

    ```cpp
    #include <string>
    #include <vector>
    #include <algorithm>
    #include <tuple>

    class Solution {
    public:
        std::string minimumString(const std::string& string1, const std::string& string2, const std::string& string3) {
            auto build_suffix_array = [](const std::string& text) -> std::vector<int> {
                int text_length = text.length();
                std::vector<int> suffix_array(text_length), rank(text_length), temp_rank(text_length);
                for (int i = 0; i < text_length; ++i) {
                    suffix_array[i] = i;
                    rank[i] = text[i];
                }
                int chunk_size = 1;
                while (chunk_size < text_length) {
                    std::sort(suffix_array.begin(), suffix_array.end(), [&](int i, int j) {
                        return std::make_pair(rank[i], i + chunk_size < text_length ? rank[i + chunk_size] : -1) <
                            std::make_pair(rank[j], j + chunk_size < text_length ? rank[j + chunk_size] : -1);
                    });
                    temp_rank[0] = 0;
                    for (int i = 1; i < text_length; ++i) {
                        temp_rank[i] = temp_rank[i - 1] +
                                    (std::make_pair(rank[suffix_array[i]], suffix_array[i] + chunk_size < text_length ? rank[suffix_array[i] + chunk_size] : -1) !=
                                        std::make_pair(rank[suffix_array[i - 1]], suffix_array[i - 1] + chunk_size < text_length ? rank[suffix_array[i - 1] + chunk_size] : -1));
                    }
                    for (int i = 0; i < text_length; ++i) {
                        rank[suffix_array[i]] = temp_rank[i];
                    }
                    if (temp_rank[text_length - 1] == text_length - 1) {
                        break;
                    }
                    chunk_size *= 2;
                }
                return suffix_array;
            };

            auto build_lcp_array = [](const std::string& text, const std::vector<int>& suffix_array) -> std::vector<int> {
                int text_length = text.length();
                std::vector<int> rank(text_length), lcp(text_length - 1);
                for (int i = 0; i < text_length; ++i) {
                    rank[suffix_array[i]] = i;
                }
                int k = 0;
                for (int i = 0; i < text_length; ++i) {
                    if (rank[i] == text_length - 1) {
                        k = 0;
                        continue;
                    }
                    int j = suffix_array[rank[i] + 1];
                    while (i + k < text_length && j + k < text_length && text[i + k] == text[j + k]) {
                        k++;
                    }
                    lcp[rank[i]] = k;
                    if (k) {
                        k--;
                    }
                }
                return lcp;
            };

            auto find_overlap = [&](const std::string& string_a, const std::string& string_b) -> std::tuple<std::string, int> {
                std::string combined_string = string_a + "#" + string_b;
                auto suffix_array = build_suffix_array(combined_string);
                auto lcp_array = build_lcp_array(combined_string, suffix_array);

                int max_overlap = 0;
                for (int i = 0; i < combined_string.length() - 1; ++i) {
                    if ((suffix_array[i] < string_a.length()) != (suffix_array[i + 1] < string_a.length())) {
                        max_overlap = std::max(max_overlap, lcp_array[i]);
                    }
                }

                if (string_a.find(string_b) != std::string::npos) {
                    return {string_b, static_cast<int>(string_a.length())};
                } else if (string_b.find(string_a) != std::string::npos) {
                    return {string_a, static_cast<int>(string_b.length())};
                } else if (max_overlap > 0) {
                    if (suffix_array[max_overlap] < string_a.length()) {
                        return {string_a + string_b.substr(max_overlap), max_overlap};
                    } else {
                        return {string_b + string_a.substr(max_overlap), max_overlap};
                    }
                } else {
                    return {string_a + string_b, 0};
                }
            };

            auto merge_strings = [&](const std::string& s1, const std::string& s2, const std::string& s3) -> std::string {
                auto [merged_s1_s2, _] = find_overlap(s1, s2);
                auto [final_merged, __] = find_overlap(merged_s1_s2, s3);
                return final_merged;
            };

            std::vector<std::tuple<std::string, std::string, std::string>> all_permutations = {
                {string1, string2, string3}, {string1, string3, string2},
                {string2, string1, string3}, {string2, string3, string1},
                {string3, string1, string2}, {string3, string2, string1}
            };

            std::string result = *std::min_element(all_permutations.begin(), all_permutations.end(), [&](auto& a, auto& b) {
                return merge_strings(std::get<0>(a), std::get<1>(a), std::get<2>(a)) < 
                    merge_strings(std::get<0>(b), std::get<1>(b), std::get<2>(b));
            });

            return result;
        }
    };
    ```

=== "Rust"

    ```rust
    struct Solution;

    impl Solution {
        fn build_suffix_array(text: &str) -> Vec<usize> {
            let text_length = text.len();
            let mut suffix_array: Vec<usize> = (0..text_length).collect();
            let mut rank: Vec<i32> = text.chars().map(|c| c as i32).collect();
            let mut temp_rank = vec![0; text_length];
            let mut chunk_size = 1;
            
            while chunk_size < text_length {
                suffix_array.sort_by_key(|&i| {
                    let second_rank = if i + chunk_size < text_length {
                        rank[i + chunk_size]
                    } else {
                        -1
                    };
                    (rank[i], second_rank)
                });
                
                temp_rank[0] = 0;
                for i in 1..text_length {
                    let first_pair = (rank[suffix_array[i]], if suffix_array[i] + chunk_size < text_length { rank[suffix_array[i] + chunk_size] } else { -1 });
                    let second_pair = (rank[suffix_array[i - 1]], if suffix_array[i - 1] + chunk_size < text_length { rank[suffix_array[i - 1] + chunk_size] } else { -1 });
                    temp_rank[i] = temp_rank[i - 1] + if first_pair != second_pair { 1 } else { 0 };
                }

                for i in 0..text_length {
                    rank[suffix_array[i]] = temp_rank[i];
                }

                if temp_rank[text_length - 1] == text_length as i32 - 1 {
                    break;
                }
                chunk_size *= 2;
            }
            
            suffix_array
        }

        fn build_lcp_array(text: &str, suffix_array: &Vec<usize>) -> Vec<usize> {
            let text_length = text.len();
            let mut rank = vec![0; text_length];
            let mut lcp = vec![0; text_length - 1];
            
            for i in 0..text_length {
                rank[suffix_array[i]] = i;
            }
            
            let mut k = 0;
            for i in 0..text_length {
                if rank[i] == text_length - 1 {
                    k = 0;
                    continue;
                }
                let j = suffix_array[rank[i] + 1];
                while i + k < text_length && j + k < text_length && text.as_bytes()[i + k] == text.as_bytes()[j + k] {
                    k += 1;
                }
                lcp[rank[i]] = k;
                if k > 0 {
                    k -= 1;
                }
            }
            
            lcp
        }

        fn find_overlap(string_a: &str, string_b: &str) -> (String, usize) {
            let combined_string = format!("{}#{}", string_a, string_b);
            let suffix_array = build_suffix_array(&combined_string);
            let lcp_array = build_lcp_array(&combined_string, &suffix_array);
            
            let mut max_overlap = 0;
            for i in 0..combined_string.len() - 1 {
                if (suffix_array[i] < string_a.len()) != (suffix_array[i + 1] < string_a.len()) {
                    max_overlap = max_overlap.max(lcp_array[i]);
                }
            }

            if string_a.contains(string_b) {
                (string_b.to_string(), string_a.len())
            } else if string_b.contains(string_a) {
                (string_a.to_string(), string_b.len())
            } else if max_overlap > 0 {
                if suffix_array[max_overlap] < string_a.len() {
                    (format!("{}{}", string_a, &string_b[max_overlap..]), max_overlap)
                } else {
                    (format!("{}{}", string_b, &string_a[max_overlap..]), max_overlap)
                }
            } else {
                (format!("{}{}", string_a, string_b), 0)
            }
        }

        fn merge_strings(s1: &str, s2: &str, s3: &str) -> String {
            let (merged_s1_s2, _) = find_overlap(s1, s2);
            let (final_merged, _) = find_overlap(&merged_s1_s2, s3);
            final_merged
        }

        fn minimum_string(string1: &str, string2: &str, string3: &str) -> String {
            let all_permutations = vec![
                (string1, string2, string3),
                (string1, string3, string2),
                (string2, string1, string3),
                (string2, string3, string1),
                (string3, string1, string2),
                (string3, string2, string1),
            ];
            
            all_permutations.into_iter()
                .map(|(s1, s2, s3)| merge_strings(s1, s2, s3))
                .min_by_key(|s| (s.len(), s.clone()))
                .unwrap()
        }
    }
    ```

=== "C#"

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;

    public class Solution {
        public string MinimumString(string string1, string string2, string string3) {
            List<int> BuildSuffixArray(string text) {
                int textLength = text.Length;
                var suffixArray = Enumerable.Range(0, textLength).ToList();
                var rank = text.Select(c => (int)c).ToList();
                var tempRank = new List<int>(new int[textLength]);
                int chunkSize = 1;
                
                while (chunkSize < textLength) {
                    suffixArray.Sort((i, j) => {
                        int secondRankI = i + chunkSize < textLength ? rank[i + chunkSize] : -1;
                        int secondRankJ = j + chunkSize < textLength ? rank[j + chunkSize] : -1;
                        return (rank[i], secondRankI).CompareTo((rank[j], secondRankJ));
                    });
                    
                    for (int i = 1; i < textLength; i++) {
                        tempRank[i] = tempRank[i - 1] + (
                            (rank[suffixArray[i]], suffixArray[i] + chunkSize < textLength ? rank[suffixArray[i] + chunkSize] : -1) != 
                            (rank[suffixArray[i - 1]], suffixArray[i - 1] + chunkSize < textLength ? rank[suffixArray[i - 1] + chunkSize] : -1) 
                            ? 1 : 0);
                    }
                    for (int i = 0; i < textLength; i++) {
                        rank[suffixArray[i]] = tempRank[i];
                    }
                    if (tempRank[textLength - 1] == textLength - 1) {
                        break;
                    }
                    chunkSize *= 2;
                }
                return suffixArray;
            }

            List<int> BuildLCPArray(string text, List<int> suffixArray) {
                int textLength = text.Length;
                var rank = new int[textLength];
                for (int i = 0; i < textLength; i++) {
                    rank[suffixArray[i]] = i;
                }
                var lcp = new List<int>(new int[textLength - 1]);
                int k = 0;
                for (int i = 0; i < textLength; i++) {
                    if (rank[i] == textLength - 1) {
                        k = 0;
                        continue;
                    }
                    int j = suffixArray[rank[i] + 1];
                    while (i + k < textLength && j + k < textLength && text[i + k] == text[j + k]) {
                        k++;
                    }
                    lcp[rank[i]] = k;
                    if (k > 0) {
                        k--;
                    }
                }
                return lcp;
            }

            (string, int) FindOverlap(string stringA, string stringB) {
                string combinedString = stringA + "#" + stringB;
                var suffixArray = BuildSuffixArray(combinedString);
                var lcpArray = BuildLCPArray(combinedString, suffixArray);
                
                int maxOverlap = 0;
                for (int i = 0; i < combinedString.Length - 1; i++) {
                    if ((suffixArray[i] < stringA.Length) != (suffixArray[i + 1] < stringA.Length)) {
                        maxOverlap = Math.Max(maxOverlap, lcpArray[i]);
                    }
                }

                if (stringA.Contains(stringB)) {
                    return (stringB, stringA.Length);
                } else if (stringB.Contains(stringA)) {
                    return (stringA, stringB.Length);
                } else if (maxOverlap > 0) {
                    if (suffixArray[maxOverlap] < stringA.Length) {
                        return (stringA + stringB.Substring(maxOverlap), maxOverlap);
                    } else {
                        return (stringB + stringA.Substring(maxOverlap), maxOverlap);
                    }
                } else {
                    return (stringA + stringB, 0);
                }
            }

            string MergeStrings(string s1, string s2, string s3) {
                var (mergedS1S2, _) = FindOverlap(s1, s2);
                var (finalMerged, _) = FindOverlap(mergedS1S2, s3);
                return finalMerged;
            }

            var allPermutations = new List<(string, string, string)> {
                (string1, string2, string3),
                (string1, string3, string2),
                (string2, string1, string3),
                (string2, string3, string1),
                (string3, string1, string2),
                (string3, string2, string1)
            };

            return allPermutations
                .Select(p => MergeStrings(p.Item1, p.Item2, p.Item3))
                .OrderBy(result => (result.Length, result))
                .First();
        }
    }
    ```


=== "Java"

    ```java
    import java.util.Arrays;
    import java.util.List;

    public class Solution {

        public static String minimumString(String string1, String string2, String string3) {
            List<Integer> buildSuffixArray(String text) {
                int textLength = text.length();
                Integer[] suffixArray = new Integer[textLength];
                for (int i = 0; i < textLength; i++) {
                    suffixArray[i] = i;
                }
                int[] rank = new int[textLength];
                for (int i = 0; i < textLength; i++) {
                    rank[i] = text.charAt(i);
                }
                int[] tempRank = new int[textLength];
                int chunkSize = 1;

                while (chunkSize < textLength) {
                    Arrays.sort(suffixArray, (i, j) -> {
                        int secondRankI = i + chunkSize < textLength ? rank[i + chunkSize] : -1;
                        int secondRankJ = j + chunkSize < textLength ? rank[j + chunkSize] : -1;
                        return rank[i] == rank[j] ? Integer.compare(secondRankI, secondRankJ) : Integer.compare(rank[i], rank[j]);
                    });

                    tempRank[suffixArray[0]] = 0;
                    for (int i = 1; i < textLength; i++) {
                        tempRank[suffixArray[i]] = tempRank[suffixArray[i - 1]] + (
                                (rank[suffixArray[i]] != rank[suffixArray[i - 1]] ||
                                        (suffixArray[i] + chunkSize < textLength ? rank[suffixArray[i] + chunkSize] : -1) !=
                                                (suffixArray[i - 1] + chunkSize < textLength ? rank[suffixArray[i - 1] + chunkSize] : -1)) ? 1 : 0
                        );
                    }
                    for (int i = 0; i < textLength; i++) {
                        rank[i] = tempRank[i];
                    }
                    if (tempRank[textLength - 1] == textLength - 1) {
                        break;
                    }
                    chunkSize *= 2;
                }

                return Arrays.asList(suffixArray);
            }

            List<Integer> buildLCPArray(String text, List<Integer> suffixArray) {
                int textLength = text.length();
                int[] rank = new int[textLength];
                for (int i = 0; i < textLength; i++) {
                    rank[suffixArray.get(i)] = i;
                }
                int[] lcp = new int[textLength - 1];
                int k = 0;
                for (int i = 0; i < textLength; i++) {
                    if (rank[i] == textLength - 1) {
                        k = 0;
                        continue;
                    }
                    int j = suffixArray.get(rank[i] + 1);
                    while (i + k < textLength && j + k < textLength && text.charAt(i + k) == text.charAt(j + k)) {
                        k++;
                    }
                    lcp[rank[i]] = k;
                    if (k > 0) {
                        k--;
                    }
                }

                return Arrays.asList(lcp);
            }

            String mergeStrings(String s1, String s2, String s3) {
                String mergedS1S2 = findOverlap(s1, s2).getFirst();
                return findOverlap(mergedS1S2, s3).getFirst();
            }

            Pair<String, Integer> findOverlap(String stringA, String stringB) {
                String combinedString = stringA + "#" + stringB;
                List<Integer> suffixArray = buildSuffixArray(combinedString);
                List<Integer> lcpArray = buildLCPArray(combinedString, suffixArray);

                int maxOverlap = 0;
                for (int i = 0; i < combinedString.length() - 1; i++) {
                    if ((suffixArray.get(i) < stringA.length()) != (suffixArray.get(i + 1) < stringA.length())) {
                        maxOverlap = Math.max(maxOverlap, lcpArray.get(i));
                    }
                }

                if (stringA.contains(stringB)) {
                    return new Pair<>(stringB, stringA.length());
                } else if (stringB.contains(stringA)) {
                    return new Pair<>(stringA, stringB.length());
                } else if (maxOverlap > 0) {
                    return suffixArray.get(maxOverlap) < stringA.length()
                            ? new Pair<>(stringA + stringB.substring(maxOverlap), maxOverlap)
                            : new Pair<>(stringB + stringA.substring(maxOverlap), maxOverlap);
                } else {
                    return new Pair<>(stringA + stringB, 0);
                }
            }

            List<Pair<String, String, String>> allPermutations = List.of(
                    new Pair<>(string1, string2, string3),
                    new Pair<>(string1, string3, string2),
                    new Pair<>(string2, string1, string3),
                    new Pair<>(string2, string3, string1),
                    new Pair<>(string3, string1, string2),
                    new Pair<>(string3, string2, string1)
            );

            return allPermutations.stream()
                    .map(p -> mergeStrings(p.getFirst(), p.getSecond(), p.getThird()))
                    .min(Comparator.comparingInt(String::length).thenComparing(String::compareTo))
                    .get();
        }
    }
    ```

=== "Scala"

    ```scala
    object Solution {
        def minimumString(string1: String, string2: String, string3: String): String = {

            def buildSuffixArray(text: String): Array[Int] = {
                val textLength = text.length
                val suffixArray = (0 until textLength).toArray
                val rank = text.map(_.toInt).toArray
                val tempRank = Array.ofDim[Int](textLength)
                var chunkSize = 1

                while (chunkSize < textLength) {
                    java.util.Arrays.sort(suffixArray, (i: Int, j: Int) => {
                    val secondRankI = if (i + chunkSize < textLength) rank(i + chunkSize) else -1
                    val secondRankJ = if (j + chunkSize < textLength) rank(j + chunkSize) else -1
                    if (rank(i) == rank(j)) Integer.compare(secondRankI, secondRankJ) else Integer.compare(rank(i), rank(j))
                    })

                    tempRank(suffixArray(0)) = 0
                    for (i <- 1 until textLength) {
                    val currentSecondRank = if (suffixArray(i) + chunkSize < textLength) rank(suffixArray(i) + chunkSize) else -1
                    val prevSecondRank = if (suffixArray(i - 1) + chunkSize < textLength) rank(suffixArray(i - 1) + chunkSize) else -1
                    tempRank(suffixArray(i)) = tempRank(suffixArray(i - 1)) + (if (rank(suffixArray(i)) != rank(suffixArray(i - 1)) || currentSecondRank != prevSecondRank) 1 else 0)
                    }
                    for (i <- 0 until textLength) {
                    rank(i) = tempRank(i)
                    }
                    if (tempRank(textLength - 1) == textLength - 1) {
                    return suffixArray
                    }
                    chunkSize *= 2
                }

                suffixArray
            }

            def buildLCPArray(text: String, suffixArray: Array[Int]): Array[Int] = {
                val textLength = text.length
                val rank = Array.ofDim[Int](textLength)
                val lcp = Array.ofDim[Int](textLength - 1)
                for (i <- 0 until textLength) {
                    rank(suffixArray(i)) = i
                }
                var k = 0
                for (i <- 0 until textLength) {
                    if (rank(i) == textLength - 1) {
                    k = 0
                    } else {
                    val j = suffixArray(rank(i) + 1)
                    while (i + k < textLength && j + k < textLength && text.charAt(i + k) == text.charAt(j + k)) {
                        k += 1
                    }
                    lcp(rank(i)) = k
                    if (k > 0) k -= 1
                    }
                }

                lcp
            }

            def mergeStrings(s1: String, s2: String, s3: String): String = {
                val (mergedS1S2, _) = findOverlap(s1, s2)
                val (finalMerged, _) = findOverlap(mergedS1S2, s3)
                finalMerged
            }

            def findOverlap(stringA: String, stringB: String): (String, Int) = {
                val combinedString = s"$stringA#$stringB"
                val suffixArray = buildSuffixArray(combinedString)
                val lcpArray = buildLCPArray(combinedString, suffixArray)

                var maxOverlap = 0
                for (i <- 0 until combinedString.length - 1) {
                    if ((suffixArray(i) < stringA.length) != (suffixArray(i + 1) < stringA.length)) {
                    maxOverlap = math.max(maxOverlap, lcpArray(i))
                    }
                }

                if (stringA.contains(stringB)) {
                    (stringB, stringA.length)
                } else if (stringB.contains(stringA)) {
                    (stringA, stringB.length)
                } else if (maxOverlap > 0) {
                    if (suffixArray(maxOverlap) < stringA.length) {
                    (stringA + stringB.substring(maxOverlap), maxOverlap)
                    } else {
                    (stringB + stringA.substring(maxOverlap), maxOverlap)
                    }
                } else {
                    (stringA + stringB, 0)
                }
            }

            val allPermutations = Seq(
            (string1, string2, string3),
            (string1, string3, string2),
            (string2, string1, string3),
            (string2, string3, string1),
            (string3, string1, string2),
            (string3, string2, string1)
            )

            allPermutations
            .map { case (a, b, c) => mergeStrings(a, b, c) }
            .minBy(s => (s.length, s))
        }
    }
    ```

=== "Kotlin"

    ```kotlin
    import kotlin.math.max

    class Solution {
        fun minimumString(string1: String, string2: String, string3: String): String {
            
            fun buildSuffixArray(text: String): List<Int> {
                val textLength = text.length
                var suffixArray = (0 until textLength).toList()
                val rank = text.map { it.toInt() }
                var tempRank = List(textLength) { 0 }
                var chunkSize = 1
                
                while (chunkSize < textLength) {
                    suffixArray = suffixArray.sortedWith(compareBy({ i -> Pair(rank[i], rank.getOrElse(i + chunkSize) { -1 }) }))
                    for (i in 1 until textLength) {
                        tempRank[i] = tempRank[i - 1] + if (
                            Pair(rank[suffixArray[i]],
                                rank.getOrElse(suffixArray[i] + chunkSize) { -1 }) != Pair(rank[suffixArray[i - 1]],
                                rank.getOrElse(suffixArray[i - 1] + chunkSize) { -1 })
                        ) 1 else 0
                    }
                    for (i in 0 until textLength) {
                        rank[suffixArray[i]] = tempRank[i]
                    }
                    if (tempRank[textLength - 1] == textLength - 1) {
                        break
                    }
                    chunkSize *= 2
                }
                return suffixArray
            }

            fun buildLCPArray(text: String, suffixArray: List<Int>): List<Int> {
                val textLength = text.length
                val rank = MutableList(textLength) { 0 }
                suffixArray.forEachIndexed { i, sa -> rank[sa] = i }
                val lcp = MutableList(textLength - 1) { 0 }
                var k = 0
                
                for (i in 0 until textLength) {
                    if (rank[i] == textLength - 1) {
                        k = 0
                        continue
                    }
                    var j = suffixArray[rank[i] + 1]
                    while (i + k < textLength && j + k < textLength && text[i + k] == text[j + k]) {
                        k++
                    }
                    lcp[rank[i]] = k
                    if (k > 0) k--
                }
                return lcp
            }

            fun findOverlap(stringA: String, stringB: String): Pair<String, Int> {
                val combinedString = "$stringA#$stringB"
                val suffixArray = buildSuffixArray(combinedString)
                val lcpArray = buildLCPArray(combinedString, suffixArray)
                
                var maxOverlap = 0
                for (i in 0 until combinedString.length - 1) {
                    if ((suffixArray[i] < stringA.length) != (suffixArray[i + 1] < stringA.length)) {
                        maxOverlap = max(maxOverlap, lcpArray[i])
                    }
                }
                
                if (stringA in stringB) {
                    return Pair(stringB, stringA.length)
                } else if (stringB in stringA) {
                    return Pair(stringA, stringB.length)
                } else if (maxOverlap > 0) {
                    return if (suffixArray[maxOverlap] < stringA.length) {
                        Pair(stringA + stringB.substring(maxOverlap), maxOverlap)
                    } else {
                        Pair(stringB + stringA.substring(maxOverlap), maxOverlap)
                    }
                } else {
                    return Pair(stringA + stringB, 0)
                }
            }

            fun mergeStrings(s1: String, s2: String, s3: String): String {
                val (mergedS1S2, _) = findOverlap(s1, s2)
                val (finalMerged, _) = findOverlap(mergedS1S2, s3)
                return finalMerged
            }

            val allPermutations = listOf(
                Triple(string1, string2, string3), Triple(string1, string3, string2),
                Triple(string2, string1, string3), Triple(string2, string3, string1),
                Triple(string3, string1, string2), Triple(string3, string2, string1)
            )

            return allPermutations.map { (a, b, c) -> mergeStrings(a, b, c) }.minByOrNull { it.length } ?: ""
        }
    }

    fun main() {
        val solution = Solution()
        val result = solution.minimumString("abc", "bca", "aaa")
        println("Minimum String: $result")
    }
    ```

=== "Go"

    ```go
    package main

    import (
        "fmt"
        "sort"
        "strings"
    )

    type Solution struct{}

    func (s *Solution) buildSuffixArray(text string) []int {
        textLength := len(text)
        suffixArray := make([]int, textLength)
        rank := make([]int, textLength)
        tempRank := make([]int, textLength)

        for i := 0; i < textLength; i++ {
            suffixArray[i] = i
            rank[i] = int(text[i])
        }

        chunkSize := 1
        for chunkSize < textLength {
            sort.SliceStable(suffixArray, func(i, j int) bool {
                firstRankI := rank[suffixArray[i]]
                secondRankI := -1
                if suffixArray[i]+chunkSize < textLength {
                    secondRankI = rank[suffixArray[i]+chunkSize]
                }

                firstRankJ := rank[suffixArray[j]]
                secondRankJ := -1
                if suffixArray[j]+chunkSize < textLength {
                    secondRankJ = rank[suffixArray[j]+chunkSize]
                }

                if firstRankI == firstRankJ {
                    return secondRankI < secondRankJ
                }
                return firstRankI < firstRankJ
            })

            tempRank[suffixArray[0]] = 0
            for i := 1; i < textLength; i++ {
                if rank[suffixArray[i]] != rank[suffixArray[i-1]] || (suffixArray[i]+chunkSize < textLength && rank[suffixArray[i]+chunkSize] != rank[suffixArray[i-1]+chunkSize]) {
                    tempRank[suffixArray[i]] = tempRank[suffixArray[i-1]] + 1
                } else {
                    tempRank[suffixArray[i]] = tempRank[suffixArray[i-1]]
                }
            }

            for i := 0; i < textLength; i++ {
                rank[i] = tempRank[i]
            }

            if tempRank[textLength-1] == textLength-1 {
                break
            }

            chunkSize *= 2
        }

        return suffixArray
    }

    func (s *Solution) buildLCPArray(text string, suffixArray []int) []int {
        textLength := len(text)
        rank := make([]int, textLength)
        lcp := make([]int, textLength-1)

        for i := 0; i < textLength; i++ {
            rank[suffixArray[i]] = i
        }

        k := 0
        for i := 0; i < textLength; i++ {
            if rank[i] == textLength-1 {
                k = 0
                continue
            }
            j := suffixArray[rank[i]+1]
            for i+k < textLength && j+k < textLength && text[i+k] == text[j+k] {
                k++
            }
            lcp[rank[i]] = k
            if k > 0 {
                k--
            }
        }

        return lcp
    }

    func (s *Solution) findOverlap(stringA, stringB string) (string, int) {
        combinedString := stringA + "#" + stringB
        suffixArray := s.buildSuffixArray(combinedString)
        lcpArray := s.buildLCPArray(combinedString, suffixArray)

        maxOverlap := 0
        for i := 0; i < len(combinedString)-1; i++ {
            if (suffixArray[i] < len(stringA)) != (suffixArray[i+1] < len(stringA)) {
                if lcpArray[i] > maxOverlap {
                    maxOverlap = lcpArray[i]
                }
            }
        }

        if strings.Contains(stringA, stringB) {
            return stringA, len(stringA)
        } else if strings.Contains(stringB, stringA) {
            return stringB, len(stringB)
        } else if maxOverlap > 0 {
            if suffixArray[maxOverlap] < len(stringA) {
                return stringA + stringB[maxOverlap:], maxOverlap
            }
            return stringB + stringA[maxOverlap:], maxOverlap
        } else {
            return stringA + stringB, 0
        }
    }

    func (s *Solution) mergeStrings(s1, s2, s3 string) string {
        mergedS1S2, _ := s.findOverlap(s1, s2)
        finalMerged, _ := s.findOverlap(mergedS1S2, s3)
        return finalMerged
    }

    func (s *Solution) minimumString(string1, string2, string3 string) string {
        permutations := [][]string{
            {string1, string2, string3},
            {string1, string3, string2},
            {string2, string1, string3},
            {string2, string3, string1},
            {string3, string1, string2},
            {string3, string2, string1},
        }

        var result string
        for _, perm := range permutations {
            merged := s.mergeStrings(perm[0], perm[1], perm[2])
            if result == "" || len(merged) < len(result) || (len(merged) == len(result) && merged < result) {
                result = merged
            }
        }

        return result
    }

    func main() {
        s := Solution{}
        fmt.Println(s.minimumString("abc", "def", "ghi"))
    }
    ```

=== "TypeScript"

    ```typescript
    class Solution {
        buildSuffixArray(text: string): number[] {
            const textLength = text.length;
            const suffixArray = Array.from({ length: textLength }, (_, i) => i);
            let rank = text.split('').map(char => char.charCodeAt(0));
            const tempRank = Array(textLength).fill(0);
            let chunkSize = 1;

            while (chunkSize < textLength) {
                suffixArray.sort((i, j) => {
                    const secondRankI = i + chunkSize < textLength ? rank[i + chunkSize] : -1;
                    const secondRankJ = j + chunkSize < textLength ? rank[j + chunkSize] : -1;
                    return rank[i] === rank[j] ? secondRankI - secondRankJ : rank[i] - rank[j];
                });

                tempRank[suffixArray[0]] = 0;
                for (let i = 1; i < textLength; i++) {
                    const currentSecondRank = suffixArray[i] + chunkSize < textLength ? rank[suffixArray[i] + chunkSize] : -1;
                    const prevSecondRank = suffixArray[i - 1] + chunkSize < textLength ? rank[suffixArray[i - 1] + chunkSize] : -1;
                    tempRank[suffixArray[i]] = tempRank[suffixArray[i - 1]] + (rank[suffixArray[i]] !== rank[suffixArray[i - 1]] || currentSecondRank !== prevSecondRank ? 1 : 0);
                }
                rank = [...tempRank];

                if (tempRank[textLength - 1] === textLength - 1) {
                    break;
                }

                chunkSize *= 2;
            }

            return suffixArray;
        }

        buildLCPArray(text: string, suffixArray: number[]): number[] {
            const textLength = text.length;
            const rank = Array(textLength).fill(0);
            const lcp = Array(textLength - 1).fill(0);

            suffixArray.forEach((sa, i) => rank[sa] = i);

            let k = 0;
            for (let i = 0; i < textLength; i++) {
                if (rank[i] === textLength - 1) {
                    k = 0;
                    continue;
                }
                const j = suffixArray[rank[i] + 1];
                while (i + k < textLength && j + k < textLength && text[i + k] === text[j + k]) {
                    k++;
                }
                lcp[rank[i]] = k;
                if (k > 0) k--;
            }

            return lcp;
        }

        mergeStrings(s1: string, s2: string, s3: string): string {
            const mergedS1S2 = this.findOverlap(s1, s2).merged;
            return this.findOverlap(mergedS1S2, s3).merged;
        }

        findOverlap(stringA: string, stringB: string): { merged: string, overlap: number } {
            const combinedString = `${stringA}#${stringB}`;
            const suffixArray = this.buildSuffixArray(combinedString);
            const lcpArray = this.buildLCPArray(combinedString, suffixArray);

            let maxOverlap = 0;
            for (let i = 0; i < combinedString.length - 1; i++) {
                const isDifferentSide = (suffixArray[i] < stringA.length) !== (suffixArray[i + 1] < stringA.length);
                if (isDifferentSide && lcpArray[i] > maxOverlap) {
                    maxOverlap = lcpArray[i];
                }
            }

            if (stringA.includes(stringB)) {
                return { merged: stringA, overlap: stringA.length };
            } else if (stringB.includes(stringA)) {
                return { merged: stringB, overlap: stringB.length };
            } else if (maxOverlap > 0) {
                if (suffixArray[maxOverlap] < stringA.length) {
                    return { merged: stringA + stringB.slice(maxOverlap), overlap: maxOverlap };
                }
                return { merged: stringB + stringA.slice(maxOverlap), overlap: maxOverlap };
            } else {
                return { merged: stringA + stringB, overlap: 0 };
            }
        }

        minimumString(string1: string, string2: string, string3: string): string {
            const permutations = [
                [string1, string2, string3],
                [string1, string3, string2],
                [string2, string1, string3],
                [string2, string3, string1],
                [string3, string1, string2],
                [string3, string2, string1]
            ];

            return permutations
                .map(([a, b, c]) => this.mergeStrings(a, b, c))
                .reduce((min, current) => current.length < min.length || (current.length === min.length && current < min) ? current : min);
        }
    }

    const solution = new Solution();
    console.log(solution.minimumString("abc", "bca", "aaa"));
    ```

=== "R"

    ```r
    #library(R6)
    #Solution <- R6::R6Class("Solution",
    Solution <- setRefClass("Solution",    
        public = list(
            build_suffix_array <- function(text) {
                text_length <- nchar(text)
                suffix_array <- 0:(text_length - 1)
                rank <- as.integer(charToRaw(substr(text, suffix_array + 1, suffix_array + 1)))
                temp_rank <- integer(text_length)
                chunk_size <- 1
                
                while (chunk_size < text_length) {
                    suffix_array <- suffix_array[order(rank[suffix_array + 1], rank[suffix_array + chunk_size])]
                    
                    temp_rank[suffix_array[1]] <- 0
                    for (i in 2:text_length) {
                    current_second_rank <- ifelse(suffix_array[i] + chunk_size < text_length, rank[suffix_array[i] + chunk_size], -1)
                    prev_second_rank <- ifelse(suffix_array[i - 1] + chunk_size < text_length, rank[suffix_array[i - 1] + chunk_size], -1)
                    temp_rank[suffix_array[i]] <- temp_rank[suffix_array[i - 1]] + 
                        ((rank[suffix_array[i]] != rank[suffix_array[i - 1]]) || (current_second_rank != prev_second_rank))
                    }
                    
                    rank <- temp_rank
                    
                    if (temp_rank[text_length] == text_length - 1) {
                    break
                    }
                    
                    chunk_size <- chunk_size * 2
                }
                
                suffix_array
            }

            build_lcp_array <- function(text, suffix_array) {
                text_length <- nchar(text)
                rank <- integer(text_length)
                lcp <- integer(text_length - 1)
                
                for (i in seq_along(suffix_array)) {
                    rank[suffix_array[i] + 1] <- i - 1
                }
                
                k <- 0
                for (i in 0:(text_length - 1)) {
                    if (rank[i + 1] == text_length - 1) {
                    k <- 0
                    next
                    }
                    
                    j <- suffix_array[rank[i + 1] + 2]
                    while (i + k < text_length && j + k < text_length && substr(text, i + k + 1, i + k + 1) == substr(text, j + k + 1, j + k + 1)) {
                    k <- k + 1
                    }
                    
                    lcp[rank[i + 1]] <- k
                    if (k > 0) k <- k - 1
                }
                
                lcp
            }

            find_overlap <- function(stringA, stringB) {
                combined_string <- paste0(stringA, "#", stringB)
                suffix_array <- build_suffix_array(combined_string)
                lcp_array <- build_lcp_array(combined_string, suffix_array)
                
                max_overlap <- 0
                for (i in seq_along(suffix_array) - 1) {
                    if ((suffix_array[i] < nchar(stringA)) != (suffix_array[i + 1] < nchar(stringA))) {
                    max_overlap <- max(max_overlap, lcp_array[i])
                    }
                }
                
                if (grepl(stringB, stringA)) {
                    return(list(merged = stringA, overlap = nchar(stringA)))
                } else if (grepl(stringA, stringB)) {
                    return(list(merged = stringB, overlap = nchar(stringB)))
                } else if (max_overlap > 0) {
                    if (suffix_array[1] < nchar(stringA)) {
                    return(list(merged = paste0(stringA, substr(stringB, max_overlap + 1, nchar(stringB))), overlap = max_overlap))
                    } else {
                    return(list(merged = paste0(stringB, substr(stringA, max_overlap + 1, nchar(stringA))), overlap = max_overlap))
                    }
                } else {
                    return(list(merged = paste0(stringA, stringB), overlap = 0))
                }
            }

            merge_strings <- function(s1, s2, s3) {
                merged_s1_s2 <- find_overlap(s1, s2)$merged
                final_merged <- find_overlap(merged_s1_s2, s3)$merged
                final_merged
            }

            minimum_string <- function(string1, string2, string3) {
                permutations <- list(
                    c(string1, string2, string3),
                    c(string1, string3, string2),
                    c(string2, string1, string3),
                    c(string2, string3, string1),
                    c(string3, string1, string2),
                    c(string3, string2, string1)
                )
                
                result <- permutations[[1]]
                result_string <- merge_strings(result[1], result[2], result[3])
                
                for (perm in permutations) {
                    merged <- merge_strings(perm[1], perm[2], perm[3])
                    if (nchar(merged) < nchar(result_string) || (nchar(merged) == nchar(result_string) && merged < result_string)) {
                    result_string <- merged
                    }
                }
                
                result_string
            }
        )
    )

    # Example usage:
    solution <- Solution$new()
    solution$minimumString("abc", "def", "ghi")
    print(solution$minimumString("abc", "bca", "aaa"))
    ```

=== "Julia"

    ```julia
    module SolutionModule

    using Printf

    export Solution

    # Define the Solution class
    mutable struct Solution
        function Solution() end
    end

    # Method to build suffix array
    function Solution.build_suffix_array(s::Solution, text::String)::Vector{Int}
        text_length = length(text)
        suffix_array = 0:text_length-1
        rank = [Int(text[i]) for i in 1:text_length]
        temp_rank = zeros(Int, text_length)
        chunk_size = 1
        
        while chunk_size < text_length
            sort!(suffix_array, by = i -> (rank[suffix_array[i] + 1], suffix_array[i] + chunk_size <= text_length ? rank[suffix_array[i] + chunk_size + 1] : -1))
            
            temp_rank[suffix_array[1] + 1] = 0
            for i in 2:text_length
                current_second_rank = suffix_array[i] + chunk_size <= text_length ? rank[suffix_array[i] + chunk_size + 1] : -1
                prev_second_rank = suffix_array[i - 1] + chunk_size <= text_length ? rank[suffix_array[i - 1] + chunk_size + 1] : -1
                temp_rank[suffix_array[i] + 1] = temp_rank[suffix_array[i - 1] + 1] + (rank[suffix_array[i] + 1] != rank[suffix_array[i - 1] + 1] || current_second_rank != prev_second_rank ? 1 : 0)
            end
            
            rank .= temp_rank
            
            if temp_rank[text_length] == text_length - 1
                break
            end
            
            chunk_size *= 2
        end
        
        suffix_array
    end

    # Method to build LCP array
    function Solution.build_lcp_array(s::Solution, text::String, suffix_array::Vector{Int})::Vector{Int}
        text_length = length(text)
        rank = zeros(Int, text_length)
        lcp = zeros(Int, text_length - 1)
        
        for (i, sa) in enumerate(suffix_array)
            rank[sa + 1] = i - 1
        end
        
        k = 0
        for i in 0:text_length-1
            if rank[i + 1] == text_length - 1
                k = 0
                continue
            end
            
            j = suffix_array[rank[i + 1] + 2]
            while i + k < text_length && j + k < text_length && text[i + k + 1] == text[j + k + 1]
                k += 1
            end
            
            lcp[rank[i + 1]] = k
            if k > 0
                k -= 1
            end
        end
        
        lcp
    end

    # Method to find overlap
    function Solution.find_overlap(s::Solution, stringA::String, stringB::String)::Tuple{String, Int}
        combined_string = stringA * "#" * stringB
        suffix_array = Solution.build_suffix_array(s, combined_string)
        lcp_array = Solution.build_lcp_array(s, combined_string, suffix_array)
        
        max_overlap = 0
        for i in 1:length(suffix_array) - 1
            if (suffix_array[i] < length(stringA)) != (suffix_array[i + 1] < length(stringA))
                max_overlap = max(max_overlap, lcp_array[i])
            end
        end
        
        if occursin(stringB, stringA)
            return (stringA, length(stringA))
        elseif occursin(stringA, stringB)
            return (stringB, length(stringB))
        elseif max_overlap > 0
            if suffix_array[max_overlap] < length(stringA)
                return (stringA * stringB[max_overlap + 1:end], max_overlap)
            else
                return (stringB * stringA[max_overlap + 1:end], max_overlap)
            end
        else
            return (stringA * stringB, 0)
        end
    end

    # Method to merge strings
    function Solution.merge_strings(s::Solution, s1::String, s2::String, s3::String)::String
        merged_s1_s2, _ = Solution.find_overlap(s, s1, s2)
        final_merged, _ = Solution.find_overlap(s, merged_s1_s2, s3)
        final_merged
    end

    # Method to find minimum string
    function Solution.minimum_string(s::Solution, string1::String, string2::String, string3::String)::String
        permutations = [
            (string1, string2, string3),
            (string1, string3, string2),
            (string2, string1, string3),
            (string2, string3, string1),
            (string3, string1, string2),
            (string3, string2, string1)
        ]
        
        result = permutations[1]
        result_string = Solution.merge_strings(s, result[1], result[2], result[3])
        
        for perm in permutations
            merged = Solution.merge_strings(s, perm[1], perm[2], perm[3])
            if length(merged) < length(result_string) || (length(merged) == length(result_string) && merged < result_string)
                result_string = merged
            end
        end
        
        result_string
    end

    end

    # Usage
    using .SolutionModule

    sol = SolutionModule.Solution()
    println(SolutionModule.Solution.minimum_string(sol, "abc", "bca", "aaa"))
    ```
