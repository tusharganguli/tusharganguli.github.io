---
layout: post
title: What is a Random Variable? (Draft)
date: 2024-11-17 11:11:11-111
description: exposition on the definition of random variable.
tags: randomvariable probability
categories: probability
related_posts: false
---

# Definition of a Random Variable

According to Gubner [1], Note 2.1, A function X from $$\Omega$$ into $$\mathbb{R}$$ is a random variable if and only if,

$$
    \{\omega \in \Omega : X(\omega) \in B\} \in \mathcal{A}, \; \forall B \in \mathcal{B}
$$

Here:

- $$\Omega$$ : is the sample space.
- $$\omega$$: outcome defined in the sample space.
- $$\mathcal{A}$$: $$\sigma$$-algebra defined on subsets of sample space ($$\Omega$$).
- $$\mathcal{B}$$: Borel $$\sigma$$-field is the smallest $$\sigma$$-algebra on $$\mathbb{R}$$ containing all open subsets of $$\mathbb{R}$$.
- $$B$$: is the Borel set such that $$B \in \mathcal{B}$$.

where:

1. $$\{\omega \in \Omega : X(\omega) \in B \}$$: is the preimage of the Borel set $$B$$ under the random variable $$X$$. It is a subset of $$\Omega$$, representing the set of outcomes $$\omega$$ that map to values in $$B$$ under $$X$$.
2. $$\in \mathcal{A}$$: This subset must belong to the $$\sigma$$-algebra $$\mathcal{A}$$ i.e., the preimage of any Borel set $$B$$ is an event that can be measured (assigned a probability).

If you understand the above definition in its entirety, then I congratulate you on your grasp of the fundamental area of probability. If you do not understand the above and struggle like me, then you are welcome to partake this journey of understanding the nooks and crannies of _"how a random variable is defined"_.

---

# Aim

The purpose of defining a random variable is to be able to study the probability associated with an experiment.

---

# Sample Space, Outcomes and Events

Reference: [Lecture Notes, Introduction to Probability by Bertsekas and Tsitsikilis - Section 1.2](https://vfu.bg/en/e-Learning/Math--Bertsekas_Tsitsiklis_Introduction_to_probability.pdf)

When we run an experiment it could produce many possible outcomes. Set of all such possible outcomes is the sample space ($$\Omega$$) of the experiment. A subset of the sample space, which is the collection of all possible outcomes is called an event.

## Example

Consider an experiment involving ten successive coin tosses.

**Experiment**: Ten successive coin tosses.

**Outcomes**: Each outcome is a $$10$$ character string with each character is either a $$H$$ or $$T$$.

$$
  \begin{align*}
    \omega_1 &= HHTTTHHTHH \\
    \omega_2 &= HTHHTTTHHH
  \end{align*}
$$

**Sample Space**: Set of all such outcomes is the sample space $$\Omega$$

$$
  \Omega = \{ HHHHHHHHHH, HTHHTTTHHH, TTTTTTTTTT, \cdots \}
$$

The total number of possible outcomes of the sample space is, $$\vert\Omega\vert = 2^{10} = 1024$$.

**Events**: An event is a subset of the sample space. Some examples are:

- **First Head Occurs on the 3rd Toss**: The set of outcomes where the first Head appears on the 3rd toss:

  $$
    E_1 = \{\omega \in \Omega : \omega = TTHTTTTTTT \text{ or } \omega = TTHHHHHHHH \text{ and so on.}\}
  $$

- **Exactly 4 Heads in 10 Tosses**: The set of outcomes with exactly 4 Heads:
  $$
  E_2 = \{\omega \in \Omega : \text{Number of } H \text{ in } \omega = 4\}
  $$

The immediate question that arises is how all this fits into the defnition of a random variable. Now let's formally define a random variable $$X$$:

$$
\begin{equation}
    X(\omega) : \Omega \rightarrow \mathbb{R}
\end{equation}
$$

Here:

- $$\Omega$$ represents the sample space, which is the set of all possible outcomes of the random experiment.
- $$\mathbb{R}$$ is the set of real numbers.
- $$X(\omega)$$ represents the random variable that maps the outcome ($$\omega$$) to the real line ($$\mathbb{R}$$). It is also a real-valued function.

A random variable acts as a bridge, translating outcomes ($$\omega$$) into numerical values ($$\mathbb{R}$$).

- The outcomes ($$\omega$$) that belong in the sample space ($$\Omega$$), correspond to the measure (distribution) that we want to study mathematically.
- The measure is already present in these outcomes ($$\omega$$) and we define a random variable ($$X$$) to map that measure to the Real line ($$\mathbb{R}$$).

**Q. What is the purpose of this mapping?**

The purpose of this mapping is to study the statistical properties of the random variable $$X$$, which encapsulates the behavior of outcomes in the sample space. In statistical terms, the properties of a random variable are often described using measures like mean, variance, and higher-order moments. These measures summarize key aspects of the data, such as its central tendency (mean) and variability (variance). The moments of a random variable $$X$$ like mean ($$\mathbb{E}[X]$$) and variance ($$\text{Var}(X)$$) provide critical insights into the distribution of $$X$$. Higher-order moments (e.g., skewness and kurtosis) offer a more nuanced understanding of the behavior of $$X$$. This mathematical representation allows us to gain significant insights into the behavior of a random variable through its moments.The exercise of creating a random variable is about defining a mapping from a sample space ($$\Omega$$) to the real numbers ($$\mathbb{R}$$).

For example, if we define the random variable $$X$$ as the number of heads in 2 successive coin flips, then $$X$$(No of Heads) $$= \{0,1,2\}$$. The mapping defined by the random variable $$X$$ induces a distribution from the underlying probability measure $$P$$ on the sample space $$\Omega$$. In this example we can say that $$X$$ follows a Binomial distribution. This is because the random variable $$X$$ transforms the distribution on the sample space $$\Omega$$ into a distribution on the real line.

---

# Types of Random Variable

Depending on the range of numerical values that the random variable $$X$$ can take, a random variable is either a discrete random variable or a continuous random variable.

- **Discrete Random Variable**: If the range of the random variable is finite or countably infinite.
- **Continuous Random Variable**: If the range of the random variable is uncountably infinite.

---

# Cardinality of a Set

The cardinality of a set refers to the size or number of elements in the set. Sets can have different cardinalities, which are broadly categorized as finite, countably infinite, and uncountably infinite.

- A **countably finite set** is a set with a finite number of elements and can be put into a one-to-one correspondence with the first $$n$$ positive integers, where $$n$$ is finite.

  - **Example**:
    The set $$A = \{2, 4, 6, 8\}$$ has 4 elements that correspond to the natural numbers $$\{1, 2, 3, 4\}$$.

- A **countably infinite set** is a set with infinitely many elements, but these elements can still be put into a one-to-one correspondence with the set of natural numbers ($$\mathbb{N}$$).

  - **Examples**:
    - The set of all natural numbers: $$\mathbb{N} = \{1, 2, 3, 4, \dots\}$$.
    - The set of all integers: $$\mathbb{Z} = \{\dots, -2, -1, 0, 1, 2, \dots\}$$. - $$\mathbb{Z}$$ can be mapped to $$\mathbb{N}$$ using:
      $$
      f(n) =
      \begin{cases}
      2n & \text{if } n > 0 \\
      -2n + 1 & \text{if } n \leq 0
      \end{cases}
      $$
    - The set of even numbers, $$E = \{2, 4, 6, 8, \dots\}$$ maps $$ E \to \mathbb{N} $$ with $$f(n) = 2n$$.

- A set is **uncountably infinite** if it contains infinitely many elements that cannot be put into a one-to-one correspondence with the set of natural numbers ($$\mathbb{N}$$). In other words, the set is too large to be enumerated, even in principle.

  - **Example**:
    The interval $$[0, 1] \subset \mathbb{R}$$ is uncountably infinite. This can be proven by Cantor's diagonal argument, which demonstrates that no list can include all real numbers.

---

# Example of Different Types of Random Variable

- **Discrete Random Variable - Countably Finite**:
  - Experiment: Two successive rolls of a die.
  - Random Variable: The sum of the two rolls is less than $$8$$.
- **Discrete Random Variable - Countably Infinite**:
  - Experiment: Choosing a point $$a$$ from the interval $$[0,1]$$.
  - Random Variable: Associate the value $$a^2$$ to such a point.
- **Discrete Random Variable - Countably Finite**:
  - Experiment: Choosing a point $$a$$ from the interval $$[0,1]$$.
  - Random Variable: Associate the value $$a^2$$ to such a point.

---

# Probability Mass Function

Characterizes the random variable through probabilities of values it can take. Suppose $$X$$ is the random variable. What is the

**Q. What does it mean when we say: The random variable $$X$$ has a binomial distribution?**

If $$X$$ is a binomial random variable with parameters $$n$$ and $$p$$. Then the PMF of $$X$$ is defined as:

$$
  P(\{X=k\}) = \binom{n}{x}p^x(1-p)^{n-x}
$$

The above equation:

- Describes the probabilities of all possible values the random variable $$X$$ can take.
- The event $$\{X=k\}$$ is a subset of the sample space $$\Omega$$ and contains all outcomes ($$\omega$$) for which $$X(\omega) = k$$.

---

# References

1.  Gubner, John A. _Probability and Random Processes for Electrical and Computer Engineers_. Cambridge University Press, 2006.
2.  [Introduction to Probability, by Dimitri P. Bertsekas and John N. Tsitsiklis](https://vfu.bg/en/e-Learning/Math--Bertsekas_Tsitsiklis_Introduction_to_probability.pdf)