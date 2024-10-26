---
tags:
  - Original
---

# Permutations

## Display the number of permutations for a list of `n` integers.

### Problem Description:
Display the number of permutations for a list of `n` integers.

Bonus: Can you provide the in-place merge, improved space complexity solution.

!!! tip

    Python solution is the easiest to understand so always start with that.

### Solution Explanation

The Python solution use **Heap's Algorithm**, an efficient method for generating permutations in-place. I'll explain how the algorithm works, and then break down the **time** and **space complexity** for each version.

#### **Heap's Algorithm Overview**
Heap's Algorithm is an iterative method for generating permutations of a list in-place (without using additional space for the entire result). It works as follows:
1. **Control Array (`c`)**: This array keeps track of how many times each element has been swapped in the current level of the algorithm.
2. **Swapping**: For each position `i`, the algorithm decides whether to swap the element at position `i` with the first element (for even `i`) or with the element at position `c[i]` (for odd `i`).
3. **Permutation Generation**: It generates a new permutation after each swap. The process continues until all permutations have been generated.
4. **Termination**: The algorithm terminates when the `c` array contains only zeros, indicating that all permutations have been generated.

#### **Explanation**
- **Initial Setup**: We start with a list `nums` that contains the numbers from 1 to 10 (or any other list). The control array `c` is initialized with zeros and has the same length as `nums`.
- **Yielding the Initial Permutation**: The first permutation (which is just the original list) is yielded immediately.
- **Iterative Loop**: The algorithm iterates through the list, swapping elements and generating new permutations.
  - If `c[i] < i`, it checks whether `i` is even or odd:
    - **Even `i`**: The element at position `i` is swapped with the first element.
    - **Odd `i`**: The element at position `i` is swapped with the element at `c[i]`.
  - The new permutation is yielded after each swap.
  - After a permutation is generated, `c[i]` is incremented, and the index `i` is reset to zero to continue the next round of swaps.
- **Termination**: The loop terminates when all permutations have been generated.

#### **Time Complexity**
- **Swap Operations**: For each permutation, the algorithm performs up to `n` swaps, where `n` is the length of the list (`n = 10` in this case).
- **Permutations**: There are `n!` permutations (for `n = 10`, this is `10! = 3,628,800`).
- **Total Time Complexity**: $$O(n \times n!)$$, where `n` is the size of the list.
  - Each permutation requires `O(n)` swaps.
  - There are `n!` permutations to generate.

#### **Space Complexity**
- **Control Array**: The `c` array has size `n`, so it requires `O(n)` space.
- **Current Permutation**: The current list of length `n` requires `O(n)` space.
- **Stack Memory**: Since this solution uses `yield`, it doesn't store all permutations in memory at once, only one at a time.
- **Total Space Complexity**: $$O(n)$$, which is dominated by the `c` array and the current permutation.

### Solutions

---

=== "Python"

	```python
    from typing import List, Generator

    def generate_permutations(nums: List[int]) -> Generator[List[int], None, None]:
        n = len(nums)
        c = [0] * n  # Control array for the iterative process
        
        yield nums[:]  # Yield the initial permutation
        
        i = 0
        while i < n:
            if c[i] < i:
                if i % 2 == 0:
                    nums[0], nums[i] = nums[i], nums[0]  # Swap for even i
                else:
                    nums[c[i]], nums[i] = nums[i], nums[c[i]]  # Swap for odd i
                yield nums[:]  # Yield the next permutation
                c[i] += 1
                i = 0
            else:
                c[i] = 0
                i += 1
    ```

=== "C++"

	```cpp
    #include <iostream>
    #include <vector>
    #include <iterator>

    // Function to generate permutations using Heap's Algorithm
    std::vector<std::vector<int>> generate_permutations(std::vector<int> nums) {
        std::vector<std::vector<int>> permutations;
        int n = nums.size();
        std::vector<int> c(n, 0);  // Control array for the iterative process

        // Add the initial permutation
        permutations.push_back(nums);

        int i = 0;
        while (i < n) {
            if (c[i] < i) {
                if (i % 2 == 0) {
                    std::swap(nums[0], nums[i]);
                } else {
                    std::swap(nums[c[i]], nums[i]);
                }
                permutations.push_back(nums);
                c[i] += 1;
                i = 0;
            } else {
                c[i] = 0;
                i += 1;
            }
        }
        return permutations;
    }

    // Main method to demonstrate the usage
    int main() {
        std::vector<int> nums = {1, 2, 3};
        
        auto permutations = generate_permutations(nums);

        // Print all generated permutations
        std::cout << "Generated permutations:\n";
        for (const auto& perm : permutations) {
            for (int num : perm) {
                std::cout << num << ' ';
            }
            std::cout << '\n';
        }

        return 0;
    }
    ```

=== "Rust"

	```rust
    fn generate_permutations(nums: Vec<i32>) -> impl Iterator<Item = Vec<i32>> {
        let mut nums = nums.clone();
        let n = nums.len();
        let mut c = vec![0; n];
        let mut i = 0;

        std::iter::from_fn(move || {
            if i == 0 {
                i += 1;
                return Some(nums.clone());
            }
            
            while i < n {
                if c[i] < i {
                    if i % 2 == 0 {
                        nums.swap(0, i);
                    } else {
                        nums.swap(c[i], i);
                    }
                    c[i] += 1;
                    i = 0;
                    return Some(nums.clone());
                } else {
                    c[i] = 0;
                    i += 1;
                }
            }
            None
        })
    }
    ```

=== "C#"

	```csharp
    using System.Collections.Generic;

    public class Permutations {
        public static IEnumerable<List<int>> GeneratePermutations(List<int> nums) {
            int n = nums.Count;
            var c = new int[n];
            yield return new List<int>(nums);

            int i = 0;
            while (i < n) {
                if (c[i] < i) {
                    if (i % 2 == 0) {
                        (nums[0], nums[i]) = (nums[i], nums[0]);
                    } else {
                        (nums[c[i]], nums[i]) = (nums[i], nums[c[i]]);
                    }
                    yield return new List<int>(nums);
                    c[i]++;
                    i = 0;
                } else {
                    c[i] = 0;
                    i++;
                }
            }
        }
    }
	```

=== "Java"

	```java
    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.Iterator;
    import java.util.List;

    public class Permutations {
        public static Iterable<List<Integer>> generatePermutations(List<Integer> nums) {
            return () -> new Iterator<>() {
                private final int[] c = new int[nums.size()];
                private int i = 0;
                private final List<Integer> current = new ArrayList<>(nums);
                private boolean first = true;

                @Override
                public boolean hasNext() {
                    return first || i < nums.size();
                }

                @Override
                public List<Integer> next() {
                    if (first) {
                        first = false;
                        return new ArrayList<>(current);
                    }
                    while (i < nums.size()) {
                        if (c[i] < i) {
                            if (i % 2 == 0) {
                                Collections.swap(current, 0, i);
                            } else {
                                Collections.swap(current, c[i], i);
                            }
                            c[i]++;
                            i = 0;
                            return new ArrayList<>(current);
                        } else {
                            c[i] = 0;
                            i++;
                        }
                    }
                    return null;
                }
            };
        }
    }
	```

=== "Scala"

	```scala
    def generatePermutations(nums: List[Int]): Iterator[List[Int]] = {
        val c = Array.fill(nums.size)(0)
        var i = 0
        var current = nums.toArray

        Iterator.continually {
            if (i == 0) {
                Some(current.toList)
            } else {
                while (i < current.length) {
                    if (c(i) < i) {
                        if (i % 2 == 0) {
                            val tmp = current(0)
                            current(0) = current(i)
                            current(i) = tmp
                        } else {
                            val tmp = current(c(i))
                            current(c(i)) = current(i)
                            current(i) = tmp
                        }
                        c(i) += 1
                        i = 0
                        return Some(current.toList)
                    } else {
                        c(i) = 0
                        i += 1
                    }
                }
                None
            }
        }.flatten
    }
	```

=== "Kotlin"

	```kotlin
    fun generatePermutations(nums: MutableList<Int>): Sequence<List<Int>> = sequence {
        val c = IntArray(nums.size)
        yield(nums.toList())

        var i = 0
        while (i < nums.size) {
            if (c[i] < i) {
                if (i % 2 == 0) {
                    nums[0] = nums[i].also { nums[i] = nums[0] }
                } else {
                    nums[c[i]] = nums[i].also { nums[i] = nums[c[i]] }
                }
                yield(nums.toList())
                c[i]++
                i = 0
            } else {
                c[i] = 0
                i++
            }
        }
    }
	```

=== "Go"

	```go
    package main

    func generatePermutations(nums []int) [][]int {
        n := len(nums)
        result := [][]int{}
        c := make([]int, n)
        
        result = append(result, append([]int{}, nums...))
        
        i := 0
        for i < n {
            if c[i] < i {
                if i%2 == 0 {
                    nums[0], nums[i] = nums[i], nums[0]
                } else {
                    nums[c[i]], nums[i] = nums[i], nums[c[i]]
                }
                result = append(result, append([]int{}, nums...))
                c[i]++
                i = 0
            } else {
                c[i] = 0
                i++
            }
        }
        return result
    }
	```

=== "TypeScript"

	```typescript
    function* generatePermutations(nums: number[]): Generator<number[]> {
        const n = nums.length;
        const c = Array(n).fill(0);
        
        yield [...nums];
        
        let i = 0;
        while (i < n) {
            if (c[i] < i) {
                if (i % 2 === 0) {
                    [nums[0], nums[i]] = [nums[i], nums[0]];
                } else {
                    [nums[c[i]], nums[i]] = [nums[i], nums[c[i]]];
                }
                yield [...nums];
                c[i]++;
                i = 0;
            } else {
                c[i] = 0;
                i++;
            }
        }
    }
	```

=== "R"

	```r
    generate_permutations <- function(nums) {
        n <- length(nums)
        c <- integer(n)
        perms <- list(nums)
        
        i <- 1
        while (i <= n) {
            if (c[i] < i) {
                if (i %% 2 == 1) {
                    nums[c(i, 1)] <- nums[c(1, i)]
                } else {
                    nums[c(c[i] + 1, i)] <- nums[c(i, c[i] + 1)]
                }
                perms <- append(perms, list(nums))
                c[i] <- c[i] + 1
                i <- 1
            } else {
                c[i] <- 0
                i <- i + 1
            }
        }
        perms
    }
	```

=== "Julia"

	```julia
    function generate_permutations(nums::Vector{Int})
        n = length(nums)
        c = zeros(Int, n)
        result = [copy(nums)]

        i = 1
        while i <= n
            if c[i] < i
                if iseven(i)
                    nums[1], nums[i+1] = nums[i+1], nums[1]
                else
                    nums[c[i]+1], nums[i+1] = nums[i+1], nums[c[i]+1]
                end
                push!(result, copy(nums))
                c[i] += 1
                i = 1
            else
                c[i] = 0
                i += 1
            end
        end
        result
    end
	```
