---
tags:
  - Original
---

# Probability

## Combinatorics & Probability Formulae Reference 📊

### 1. Basic Counting Principles 🔢

#### Multiplication Principle
- If operation A can be done in $$m$$ ways
- And operation B can be done in $$n$$ ways
- Then A and B can be done in $$m \times n$$ ways

#### Addition Principle
- If operation A can be done in $$m$$ ways
- Or operation B can be done in $$n$$ ways
- Then A or B can be done in $$m + n$$ ways

### 2. Permutations 🔄

#### Basic Permutations
- $$P(n) = n!$$
- Orders n distinct objects

#### Partial Permutations
- $$P(n,r) = \frac{n!}{(n-r)!}$$
- Orders r items from n distinct objects

#### Permutations with Repetition
- With $$n$$ objects, where object type $$i$$ appears $$k_i$$ times
- $$P(n; k_1,k_2,\ldots,k_m) = \frac{n!}{k_1!k_2!\cdots k_m!}$$

#### Circular Permutations
- $$P_{circular}(n) = (n-1)!$$
- Arranges n items in a circle

### 3. Combinations 🎯

#### Basic Combinations
- $$C(n,r) = \binom{n}{r} = \frac{n!}{r!(n-r)!}$$
- Selects r items from n distinct objects

#### Combinations with Repetition
- $$C_{rep}(n,r) = \binom{n+r-1}{r} = \binom{n+r-1}{n-1}$$
- Selects r items from n types, with repetition allowed

#### Pascal's Triangle Properties
- $$\binom{n}{r} = \binom{n}{n-r}$$
- $$\binom{n}{r} = \binom{n-1}{r-1} + \binom{n-1}{r}$$
- $$\sum_{r=0}^n \binom{n}{r} = 2^n$$

### 4. Basic Probability Rules 🎲

#### Probability Axioms
1. $$0 \leq P(A) \leq 1$$
2. $$P(\Omega) = 1$$ (total probability)
3. For disjoint events: $$P(A \cup B) = P(A) + P(B)$$

#### Complement Rule
- $$P(A^c) = 1 - P(A)$$

#### General Addition Rule
- $$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$

#### Conditional Probability
- $$P(A|B) = \frac{P(A \cap B)}{P(B)}$$
- $$P(A \cap B) = P(A|B)P(B)$$ (Chain Rule)

#### Independence
- $$P(A \cap B) = P(A)P(B)$$
- $$P(A|B) = P(A)$$

### 5. Bayes' Theorem 🔄

#### Basic Form
- $$P(A|B) = \frac{P(B|A)P(A)}{P(B)}$$

#### Extended Form
- $$P(A_i|B) = \frac{P(B|A_i)P(A_i)}{\sum_j P(B|A_j)P(A_j)}$$

### 6. Random Variables 📊

#### Expected Value
- Discrete: $$E(X) = \sum x_i P(X = x_i)$$
- Continuous: $$E(X) = \int_{-\infty}^{\infty} x f(x) dx$$

#### Variance
- $$Var(X) = E[(X-\mu)^2] = E(X^2) - [E(X)]^2$$
- $$\sigma = \sqrt{Var(X)}$$

#### Properties
- $$E(aX + b) = aE(X) + b$$
- $$Var(aX + b) = a^2Var(X)$$
- $$E(X + Y) = E(X) + E(Y)$$
- $$Var(X + Y) = Var(X) + Var(Y)$$ (if independent)

### 7. Common Probability Distributions 📈

#### Bernoulli
- $$P(X = 1) = p$$, $$P(X = 0) = 1-p$$
- $$E(X) = p$$
- $$Var(X) = p(1-p)$$

#### Binomial
- $$P(X = k) = \binom{n}{k}p^k(1-p)^{n-k}$$
- $$E(X) = np$$
- $$Var(X) = np(1-p)$$

#### Geometric
- $$P(X = k) = p(1-p)^{k-1}$$
- $$E(X) = \frac{1}{p}$$
- $$Var(X) = \frac{1-p}{p^2}$$

#### Poisson
- $$P(X = k) = \frac{\lambda^k e^{-\lambda}}{k!}$$
- $$E(X) = \lambda$$
- $$Var(X) = \lambda$$

### 8. Useful Identities 📝

#### Summation Formulas
- $$\sum_{k=1}^n k = \frac{n(n+1)}{2}$$
- $$\sum_{k=1}^n k^2 = \frac{n(n+1)(2n+1)}{6}$$

#### Probability Properties
- Law of Total Probability: $$P(A) = \sum_i P(A|B_i)P(B_i)$$
- $$P(A_1 \cap A_2 \cap \cdots \cap A_n) = P(A_1)P(A_2|A_1)P(A_3|A_1A_2)\cdots$$

#### Series Expansions
- $$e^x = 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots$$
- $$(1+x)^n = 1 + nx + \frac{n(n-1)}{2!}x^2 + \cdots$$
