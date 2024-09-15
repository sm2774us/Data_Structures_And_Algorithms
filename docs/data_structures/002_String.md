---
tags:
  - Original
---

# String

## Shortest String That Contains Three Strings

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
