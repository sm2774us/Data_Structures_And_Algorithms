---
tags:
  - Original
---

# Probability

## Problem 1: Committees
A committee of 6 is to be formed from a group of 6 men and 8 women.

- __(a) :__ If you pick the committee (uniformly) at random, what is the probability that everyone in the group is female ?
- __(b) :__ Suppose the committee should be balanced for gender (i.e., 3 men and 3 women) and that two of the men refuse to serve together. How many different committees are possible ?

### Solution - Problem 1: Committees
### Problem 1-a) Probability that everyone in the committee is female

#### Total members available:
- __`6`__ men and __`8`__ women, making a total of __`6 + 8 = 14`__ people.

#### Total possible committees:
The number of ways to select 6 people out of 14 is given by the combination formula:
$$
\binom{n}{r} = \frac{n!}{r! \cdot (n-r)!}
$$
Here, $n = 14$ and $r = 6$:
$$
\binom{14}{6} = \frac{14 \cdot 13 \cdot 12 \cdot 11 \cdot 10 \cdot 9}{6 \cdot 5 \cdot 4 \cdot 3 \cdot 2 \cdot 1} = 3003
$$

#### Favorable outcomes (all females):
To select 6 women out of the 8 available:
$$
\binom{8}{6} = \frac{8 \cdot 7}{2 \cdot 1} = 28
$$

#### Probability:
The probability is the ratio of favorable outcomes to total outcomes:
$$
P(\text{all female}) = \frac{\binom{8}{6}}{\binom{14}{6}} = \frac{28}{3003} \approx 0.00932
$$

Thus, the probability that the committee is entirely female is approximately **0.93%**.

---

### Problem 1-b) Number of gender-balanced committees (3 men and 3 women) under constraints

#### Total committees with gender balance:
We need to select 3 men out of 6 and 3 women out of 8. Without any constraints, the total number of committees is:
$$
\binom{6}{3} \cdot \binom{8}{3}
$$
- Number of ways to select 3 men out of 6:
$$
\binom{6}{3} = \frac{6 \cdot 5 \cdot 4}{3 \cdot 2 \cdot 1} = 20
$$
- Number of ways to select 3 women out of 8:
$$
\binom{8}{3} = \frac{8 \cdot 7 \cdot 6}{3 \cdot 2 \cdot 1} = 56
$$
Total gender-balanced committees:
$$
\binom{6}{3} \cdot \binom{8}{3} = 20 \cdot 56 = 1120
$$

#### Adjust for constraints (two men refuse to serve together):
Let the two men who refuse to serve together be $M_1$ and $M_2$.

1. **Case 1: Both $M_1$ and $M_2$ are selected.**
   If __`M_1`__ and __`M_2`__ are in the committee, we must select __`1`__ additional man from the remaining __`6 - 2 = 4`__ men:
$$
   \binom{4}{1} = 4
$$
   For the women, we still select 3 out of 8:
$$
   \binom{8}{3} = 56
$$
   Total committees in this case:
$$
   \binom{4}{1} \cdot \binom{8}{3} = 4 \cdot 56 = 224
$$

2. **Case 2: At least one of $M_1$ and $M_2$ is excluded.**
   If either $M_1$ or $M_2$ is excluded, the other 3 men are chosen from the remaining $4 + 1 = 5$ men:
$$
   \binom{5}{3} = \frac{5 \cdot 4 \cdot 3}{3 \cdot 2 \cdot 1} = 10
$$
   For the women, we still select 3 out of 8:
$$
   \binom{8}{3} = 56
$$
   Total committees in this case:
$$
   \binom{5}{3} \cdot \binom{8}{3} = 10 \cdot 56 = 560
$$

#### Total gender-balanced committees with constraints:
Subtract the invalid committees (Case 1) from the total:
$$
\text{Valid committees} = 1120 - 224 = 896
$$

---

### Final Answers
- **a)** Probability that the committee is all female: $\mathbf{0.00932}$ (or approximately $0.93\%$$).
- **b)** Total number of valid gender-balanced committees: $\mathbf{896}$.

## Problem 2: Necklaces
Suppose you have beads in **5 different shapes**: ●, ■, ▲, ★, and ♦. You have **6 different beads of every shape**, all in one of six colors: red, green, blue, yellow, black, and white. So every bead has a unique color-shape combination (there is only one blue triangular bead, black circular one, etc.). 

You are going to make a necklace with **7 beads** that should have a particular composition: **3 shapes have to occur precisely twice**. Here are two examples of 7 beads with such a composition:

$$
\{\textcolor{red}{\star}, \textcolor{blue}{\square}, \textcolor{yellow}{\triangle}, \textcolor{orange}{\bullet}, \textcolor{green}{\triangle}, \textcolor{blue}{\square}, \textcolor{red}{\star}\}
$$

or

$$
\{\textcolor{red}{\star}, \textcolor{green}{\triangle}, \textcolor{blue}{\triangle}, \textcolor{yellow}{\diamond}, \textcolor{red}{\bullet}, \textcolor{blue}{\star}, \textcolor{orange}{\diamond}\}.
$$

---

#### (a)
Before we can make a necklace (see part b), we need to pick our beads. You pick your 7 beads (uniformly) at random. **What is the probability that they have the desired composition?**

---

#### (b)
Once we have our beads, we can make a necklace. **We want to order the pairs of shapes symmetrically around the shape occurring once**. As an example, take this necklace, which is symmetrical if you ignore color:

$$
(\textcolor{red}{♦}, \textcolor{blue}{\star}, \textcolor{green}{▲}, \textcolor{orange}{●}, \textcolor{black}{▲}, \textcolor{yellow}{\star}, \textcolor{red}{♦})
$$

How many necklaces can you make from all possible groups of beads with the right composition? Note again that all beads are unique and that a necklace remains the same when you flip it around.

### Solution - Problem 2: Necklaces
To answer this multi-part problem, we need to analyze each part of the question in detail.

---

### Part (a) — Probability of Desired Composition

#### Problem:
We need to pick 7 beads uniformly at random, ensuring they have the desired composition:
- **3 shapes appear twice**.
- **1 shape appears once**.

#### Total Beads:
Each shape (●, ■, ▲, ★, ♦) comes in 6 colors. So, there are $5 \times 6 = 30$ unique beads.

#### Total Ways to Pick 7 Beads:
The total number of ways to pick 7 beads from 30 beads is:
$$
\binom{30}{7}
$$

#### Ways to Pick Beads Matching the Desired Composition:
1. **Step 1: Select 3 shapes to appear twice.**
   There are 5 shapes, and we choose 3 of them:
$$
\binom{5}{3}
$$
   
2. **Step 2: Choose 2 beads (colors) for each of the 3 selected shapes.**
   For each of the 3 shapes, we select 2 colors from 6:
$$
\binom{6}{2} \times \binom{6}{2} \times \binom{6}{2} = \binom{6}{2}^3
$$

3. **Step 3: Choose the 1 shape to appear once.**
   There are 2 remaining shapes (out of the original 5) after selecting 3 for Step 1:
$$
\binom{2}{1} = 2
$$

4. **Step 4: Choose 1 bead (color) for this single shape.**
   For the selected shape, choose 1 bead from 6:
$$
\binom{6}{1} = 6
$$

Combining all of these:
$$
\binom{5}{3} \cdot \binom{6}{2}^3 \cdot \binom{2}{1} \cdot \binom{6}{1}
$$

#### Probability:
The probability is the ratio of favorable outcomes to total outcomes:
$$
P = \frac{\binom{5}{3} \cdot \binom{6}{2}^3 \cdot \binom{2}{1} \cdot \binom{6}{1}}{\binom{30}{7}}
$$

Let us compute this explicitly.

---

### Part (b) — Number of Unique Necklaces

#### Problem:
Once we have the desired composition, we need to count the number of distinct necklaces. A necklace is considered the same if it can be rotated or flipped to match another.

#### Step 1: Symmetry of the Necklace
- The necklace consists of 7 beads arranged in a circle.
- The arrangement must be **symmetric** around the shape that appears once (the center).
- The 3 pairs of shapes must be symmetrically arranged around this center shape.

#### Step 2: Arranging the Shapes
1. **Fix the central shape (appearing once).**
   There is only 1 way to choose this central shape since the shape was already determined in part (a).

2. **Arrange 3 pairs of shapes symmetrically.**
   There are $3!$ ways to arrange the 3 shapes. Since they must be symmetric, we divide by 2 to account for rotation:
$$
   \frac{3!}{2} = 3
$$

3. **Arrange the colors within each shape pair.**
   For each pair, there are 2 ways to order the colors. Since there are 3 pairs:
$$
   2 \times 2 \times 2 = 8
$$

#### Total Unique Necklaces:
Combining all possibilities:
$$
\text{Total Necklaces} = 3 \cdot 8 = 24
$$

---

### Final Answer:

#### (a) Probability:
$$
P = \frac{\binom{5}{3} \cdot \binom{6}{2}^3 \cdot \binom{2}{1} \cdot \binom{6}{1}}{\binom{30}{7}}
$$

#### (b) Unique Necklaces:
$$
\text{Total Necklaces} = 24
$$

## Problem 3: Counting Functions

#### (a)
Let $X$ and $Y$ be two finite sets. How many functions are there from $X$ to $Y$$?
*(If you are unsure about what functions precisely are, check the [Wikipedia entry Function (Mathematics)](https://en.wikipedia.org/wiki/Function_(mathematics))).*

---

#### (b)
How many functions are there from $\{0, 1\}^n$ to $\{0, 1\}^{n^2}$$?

---

#### (c)
Let $A, B \subset \{1, \ldots, 100\}$ be such that $A$ contains all even numbers and $B$ all multiples of 3. Let $X$ be their union and $n \in \mathbb{N}$ some integer. **How many functions $f : \{1, \ldots, n\} \to X$ are there?**

### Solution - Problem 3: Counting
Let's break down and solve each part of **Problem 3: Counting Functions**:

---

### **(a) How many functions are there from $X$ to $Y$$?**

If $X$ has $|X| = m$ elements and $Y$ has $|Y| = n$ elements, then each element of $X$ can be mapped to any of the $n$ elements of $Y$. Thus, the total number of functions is:

$$
n^m
$$

- **Explanation**: For each of the $m$ elements of $X$$, there are $n$ choices in $Y$. Since these choices are independent for each element of $X$$, the total number of functions is the product $n^m$.

---

### **(b) How many functions are there from $\{0, 1\}^n$ to $\{0, 1\}^{n^2}$$?**

1. The set $\{0, 1\}^n$ has $2^n$ elements (since there are $n$ binary positions, each with 2 choices: 0 or 1).
2. The set $\{0, 1\}^{n^2}$ has $2^{n^2}$ elements.

Thus, the total number of functions from $\{0, 1\}^n$ to $\{0, 1\}^{n^2}$ is:

$$
\left(2^{n^2}\right)^{2^n} = 2^{n^2 \cdot 2^n}
$$

- **Explanation**: Each of the $2^n$ elements of the domain $\{0, 1\}^n$ can be mapped to any of the $2^{n^2}$ elements of the codomain $\{0, 1\}^{n^2}$. Thus, the total number of functions is $(2^{n^2})^{2^n}$.

---

### **(c) How many functions $f : \{1, \ldots, n\} \to X$ are there, where $A$ contains all even numbers, $B$ contains all multiples of 3, and $X$ is their union?**

1. **Determine $A$ (even numbers in $\{1, \ldots, 100\}$$):**
   - Even numbers are $2, 4, 6, \ldots, 100$.
   - This is an arithmetic sequence with $a = 2$$, $d = 2$$, and $l = 100$.
   - The number of even numbers is:
     $
     |A| = \frac{100 - 2}{2} + 1 = 50
     $

2. **Determine $B$ (multiples of 3 in $\{1, \ldots, 100\}$$):**
   - Multiples of 3 are $3, 6, 9, \ldots, 99$.
   - This is an arithmetic sequence with $a = 3$$, $d = 3$$, and $l = 99$.
   - The number of multiples of 3 is:
     $
     |B| = \frac{99 - 3}{3} + 1 = 33
     $

3. **Determine $A \cap B$ (multiples of 6 in $\{1, \ldots, 100\}$$):**
   - Multiples of 6 are $6, 12, 18, \ldots, 96$.
   - This is an arithmetic sequence with $a = 6$$, $d = 6$$, and $l = 96$.
   - The number of multiples of 6 is:
     $
     |A \cap B| = \frac{96 - 6}{6} + 1 = 16
     $

4. **Find $|X|$ (size of the union $A \cup B$$):**
   By the principle of inclusion-exclusion:
$$
   |X| = |A| + |B| - |A \cap B| = 50 + 33 - 16 = 67
$$

5. **Count the number of functions $f : \{1, \ldots, n\} \to X$:**
   Each of the $n$ elements in $\{1, \ldots, n\}$ can be mapped to any of the $|X| = 67$ elements in $X$. Thus, the total number of functions is:
$$
   67^n
$$

---

### **Final Answers:**

1. **(a)**: $n^m$  
2. **(b)**: $2^{n^2 \cdot 2^n}$  
3. **(c)**: $67^n$  

# Problem 3: Football

A football coach wants to analyze the performance of his team. After every week he registers the outcome of the match (win or lose, there are no ties) and how his team played (good, fair or hopeless). Consider an experiment that consists of registering these two things.

- __(a)__ Give the sample space of this experiment.

- __(b)__ Let $A$ be the event that the team's performance was hopeless and $B$ the event that the team won. Specify the outcomes in $A$ and $B$.

- __(c)__ Paraphrase the event $(Ω \backslash A) ∪ B$ and give all its outcomes.

- __(d)__ After a couple of weeks, the coach analysed the data and came to the following conclusions: 1) His team is equally likely to win or loose; 2) his team is equally likely to play a good or a hopeless match, but 3) fair performance is half times as likely as good performance. Finally, 4) they never win when they play a hopeless match and 5) never loose when they play a good match. Give a table describing the full probability function.

- __(e)__ Last week's match has been hopeless. What is the probability that they've won? (If you didn't solve part (d), use a fancy probability function of your choice, but make sure to include it in your solution.)

## Solution - Problem 4: Football
Let’s solve this step by step. 

---

### **(a) Give the sample space of this experiment.**

The outcomes of the experiment consist of two attributes:
1. **Match outcome**: Either **win** or **lose**.
2. **Performance**: Either **good**, **fair**, or **hopeless**.

Thus, the sample space is the Cartesian product of these two attributes:

$$
\Omega = \{(w, g), (w, f), (w, h), (l, g), (l, f), (l, h)\}
$$

Where:
- $w$ = win, $l$ = lose
- $g$ = good, $f$ = fair, $h$ = hopeless

---

### **(b) Specify the outcomes in $A$ and $B$.**

__1.__ **Event $A$:** The team's performance was **hopeless**.  
   Outcomes in $A$:
$$
   A = \{(w, h), (l, h)\}
$$

__2.__ **Event $B$:** The team **won**.  
   Outcomes in $B$:
$$
   B = \{(w, g), (w, f), (w, h)\}
$$

---

### **(c) Paraphrase the event $(\Omega \setminus A) \cup B$ and give its outcomes.**

1. **Complement $\Omega \setminus A$:** Outcomes where the performance was **not hopeless** (i.e., either good or fair):
$$
   \Omega \setminus A = \{(w, g), (w, f), (l, g), (l, f)\}
$$

2. **Union $(\Omega \setminus A) \cup B$:** Outcomes where the performance was **not hopeless** or the team **won**.  
   Combining $\Omega \setminus A$ and $B$$, we get:
$$
   (\Omega \setminus A) \cup B = \{(w, g), (w, f), (l, g), (l, f), (w, h)\}
$$

**Paraphrase:** The event $(\Omega \setminus A) \cup B$ represents all cases where the performance was **not hopeless** or the team **won**.

---

### **(d) Give a table describing the full probability function.**

Based on the coach’s conclusions:
__1.__ The team is equally likely to **win** or **lose**:
$$
   P(\text{win}) = P(\text{lose}) = 0.5
$$

__2.__ The team is equally likely to play a **good** or **hopeless** match, and **fair** performance is half as likely as **good** performance:
$$
   P(\text{good}) = P(\text{hopeless}) = \frac{2}{5}, \quad P(\text{fair}) = \frac{1}{5}
$$

__3.__ The team **never wins** when they play a hopeless match:
$$
   P((w, h)) = 0
$$

__4.__ The team **never loses** when they play a good match:
$$
   P((l, g)) = 0
$$

To compute the remaining probabilities, we distribute the probabilities based on the product of match outcome and performance likelihoods, ensuring that the constraints are satisfied.

| Outcome    | Probability                |
|------------|----------------------------|
| $(w, g)$ | $0.5 \times \frac{2}{5} = 0.2$   |
| $(w, f)$ | $0.5 \times \frac{1}{5} = 0.1$   |
| $(w, h)$ | $0$                            |
| $(l, g)$ | $0$                            |
| $(l, f)$ | $0.5 \times \frac{1}{5} = 0.1$   |
| $(l, h)$ | $0.5 \times \frac{2}{5} = 0.2$   |

---

### **(e) What is the probability that they’ve won, given the match was hopeless?**

Using the definition of conditional probability:

$$
P(\text{win} \mid \text{hopeless}) = \frac{P(\text{win and hopeless})}{P(\text{hopeless})}
$$

__1.__ **Compute $P(\text{win and hopeless})$:** From the table:
$$
   P((w, h)) = 0
$$

2. **Compute $P(\text{hopeless})$:** Sum of probabilities for $\text{hopeless}$ outcomes:
$$
   P(\text{hopeless}) = P((w, h)) + P((l, h)) = 0 + 0.2 = 0.2
$$

3. **Substitute into the formula:**
$$
   P(\text{win} \mid \text{hopeless}) = \frac{0}{0.2} = 0
$$

**Final Answer:** If the match was hopeless, the probability that they’ve won is **0**.

---

## Problem 4: A probability urn

An urn contains 3 red balls, 5 green balls, and one blue ball. You randomly draw a ball, don't put it back, but add two balls of the colors you did not draw. (For example: if you have drawn a red ball, add a green and a blue ball to the urn.) What is the probability that the second ball is red? 

**Hint:** Draw a tree!

---

## Problem 5: Probability functions

Let $(\Omega, A, P)$ be a finite probability space and $A, B \in A$ two events.

### (a) 1pt 
Show that $P$ is monotone: if $A \subseteq B$$, then $P(A) \leq P(B)$.

### (b) 1pt 
Show that if $A \subset B$$, then $P(B \setminus A) = P(B) - P(A)$.  
**Hint:** Use (a).

### (c) 1pt 
Now assume that $A$ and $B$ are independent and prove that $A^c = \Omega \setminus A$ and $B$ are also independent.  
**Hint:** Draw a Venn diagram and post a question to the forum if you need more hints.


## Problem n: Exercise Survey Problem

### Problem Description
- 1000 people were asked about their preferred method of exercise.

- The following table shows the results, grouped by age: 

| | 18-22 | 23-27 | 28-32 | 33-37 | Total |
|-|-|-|-|-|-|
|Run|54|40|42|66|202|
|Bike|77|68|90|70|305|
|Swim|28|43|50|52|173|
|Other|90|78|71|81|320|
|Total|249|229|253|269|1000|

- You meet a 33 year old who took survey. 

- Compute the probability (in fraction) she prefers swimming.

### Solution - Problem n: Exercise
- We know that __the person is 33 years old__, which places them in the **33-37 age group**.

- To compute the probability that this person prefers swimming, we need to focus on __the total number of people in the 33-37 age group__ and __the number of people in that age group who prefer swimming__.

- From the table - the total number of people in the __33-37 age group__ is **269**.

- The number of people in the 33-37 age group who prefer swimming is **52**.

The probability __`P`__ that a randomly selected person from the 33-37 age group prefers swimming is given by the ratio of people who prefer swimming to the total number of people in that age group:

$$
P = \frac{52}{269}
$$

Thus, the probability that the 33-year-old prefers swimming is:

$$
\frac{52}{269}
$$
