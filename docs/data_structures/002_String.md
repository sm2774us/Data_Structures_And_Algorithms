# String

## Shortest String That Contains Three Strings

Problem Description: [LeetCode - Problem 2800 - Shortest String That Contains Three Strings](https://leetcode.com/problems/shortest-string-that-contains-three-strings/)

!!! tip

    Python solution is the easiest to understand so always start with that.

Solutions:

=== "Python3"

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

=== "C++20"

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

=== "Go"

    ```go
    package main

    import (
        "strings"
        "sort"
    )

    package main

    import (
        "fmt"
        "sort"
        "strings"
    )

    type Solution struct{}

    func (s *Solution) getPrefix(pattern string) []int {
    }

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

=== "R"

    ```r
    Solution <- R6::R6Class("Solution",
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
    module Solution

    function build_suffix_array(text::String)::Vector{Int}
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

    function build_lcp_array(text::String, suffix_array::Vector{Int})::Vector{Int}
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

    function find_overlap(stringA::String, stringB::String)::Tuple{String, Int}
        combined_string = stringA * "#" * stringB
        suffix_array = build_suffix_array(combined_string)
        lcp_array = build_lcp_array(combined_string, suffix_array)
        
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

    function merge_strings(s1::String, s2::String, s3::String)::String
        merged_s1_s2, _ = find_overlap(s1, s2)
        final_merged, _ = find_overlap(merged_s1_s2, s3)
        final_merged
    end

    function minimum_string(string1::String, string2::String, string3::String)::String
        permutations = [
            (string1, string2, string3),
            (string1, string3, string2),
            (string2, string1, string3),
            (string2, string3, string1),
            (string3, string1, string2),
            (string3, string2, string1)
        ]
        
        result = permutations[1]
        result_string = merge_strings(result[1], result[2], result[3])
        
        for perm in permutations
            merged = merge_strings(perm[1], perm[2], perm[3])
            if length(merged) < length(result_string) || (length(merged) == length(result_string) && merged < result_string)
                result_string = merged
            end
        end
        
        result_string
    end

    end # module

    # Example usage:
    using .Solution
    println(Solution.minimum_string("abc", "bca", "aaa"))
    ```
