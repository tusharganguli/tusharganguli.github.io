# What is a Random Variable? (Draft)
According to Gubner [1], Note 2.1, A function X from $\Omega$ into $\mathbb{R}$ is a random variable if and only if,

$$
    \\{ \{\omega \in \Omega : X(\omega) \in B\} \\} \in \mathcal{A}, \; \forall B \in \mathcal{B}
$$

Here:

$\Omega$ : is the sample space. 
<br> $\omega$: outcome defined in the sample space.
<br> $\mathcal{A}$: $\sigma$-algebra defined on subsets of sample space ($\Omega$).
<br> $\mathcal{B}$: Borel $\sigma$-field is the smallest $\sigma$-algebra on $\mathbb{R}$ containing all open subsets of $\mathbb{R}$.
<br> $B$: is the Borel set such that $B \in \mathcal{B}$.

where:
1. $\\{ \{\omega \in \Omega : X(\omega) \in B \} \\}$: is the preimage of the Borel set $B$ under the random variable $X$. It is a subset of $\Omega$, representing the set of outcomes $\omega$ that map to values in $B$ under $X$.

2. $\in \mathcal{A}$: This subset must belong to the $\sigma$-algebra $\mathcal{A}$ i.e., the preimage of any Borel set $B$ is an event that can be measured (assigned a probability).


If you understand the above definition in its entirety, then I congratulate you on your grasp of the fundamental area of probability. If you do not understand the above and struggle like me, then you are welcome to partake this journey of understanding the nooks and crannies of *"how a random variable is defined"*.

---

# Intuitive Discussion
A random variable $X$ can be defined as follows:

$$
\begin{equation}
    X(\omega) : \Omega \rightarrow \mathbb{R}
\end{equation}
$$

Here:
* $\Omega$ represents the sample space, which is the set of all possible outcomes of the random experiment.
* $\mathbb{R}$ is the set of real numbers.
* $X(\omega)$ represents the random variable that maps the outcome ($omega$) to the real line ($\mathbb{R}$).

A random variable acts as a bridge, translating outcomes ($\omega$) into numerical values ($\mathbb{R}$).
* The outcomes ($\omega$) that belong in the sample space ($\Omega$), correspond to the measure (distribution) that we want to study mathematically. 
* The measure is already present in these outcomes ($\omega$) and we define a random variable ($X$) to map that measure to the Real line ($\mathbb{R}$). 

Let's look at an example.

## Example
Gubner [1], Example 2.1: Let us construct a model for counting the number of heads in a sequence of three coin tosses. For the underlying sample space, we take
$$
    \Omega = \{\text{TTT, TTH, THT, HTT, THH, HTH, HHT, HHH} \},
$$
that contains the eight possible sequences of tosses. 

* Our events of interest are the *number of heads in a sequence of three coin tosses*.
* The number of heads can range from $0$ to $3$. Hence, the random variable will map the events of interest to the following set:
$$
R = \{ 0,1,2,3 \} 
$$
* The mapping will be:
$$
    \Omega \rightarrow R 
$$
* Hence our random variable is defined as:
$$
X(\omega) = \begin{cases}
                0, & \omega = \{\text{TTT}\} \\
                1, & \omega = \{\text{TTH, THT, HTT}\} \\
                2, & \omega = \{\text{THH, HTH, HHT}\} \\
                3, & \omega = \{\text{HHH}\}
            \end{cases}
$$

**Q: What is the difference between events and outcomes?**

The sample space ($\Omega$) is the universal set containing all possible outcomes of the experiment. A subset of the sample space ($A \subseteq \Omega$) represents an event. If an outcome ($\omega$) is part of an event, then $\omega \in A$, where $A$ is a specific subset of $\Omega$.

**Q. Why do we need to define events and outcomes seperately?**

- *Outcomes*: Represents each possible result of an experiment ("Heads" or "Tails" in a coin flip).
- *Events*: Represents sets of outcomes grouped together for a specific scenario ("At least one head in two coin flips"). For each outcome from the sample space there exist a corresponding real number.  

**Q. What is the purpose of this mapping?**

The purpose of this mapping is to study the statistical properties of the random variable $X$, which encapsulates the behavior of outcomes in the sample space. In statistical terms, the properties of a random variable are often described using measures like mean, variance, and higher-order moments. These measures summarize key aspects of the data, such as its central tendency (mean) and variability (variance). Importantly:

- The moments of a random variable $X$ ($\mathbb{E}[X]$ for the mean and $\text{Var}(X)$ for the variance) provide critical insights into the distribution of $X$.
- Higher-order moments (e.g., skewness and kurtosis) offer a more nuanced understanding of the behavior of $X$.

This mathematical representation allows us to gain significant insights into the behavior of a random variable through its moments.

The exercise of creating a random variable is about defining a mapping from a sample space ($\Omega$) to the real numbers ($\mathbb{R}$). At this stage, the focus is on defining the mapping, laying the groundwork for inducing probabilities and distributions—just the act of associating *outcomes with numerical values*.

For example, if we define the random variable $X$ as the number of heads in 2 successive coin flips, then $X$(No of Heads) $= \{0,1,2\}$. No probabilities are assigned directly to the random variable $X$ at this stage; they are derived from the underlying probability measure $P$ on the sample space.

The mapping defined by the random variable $X$ induces a distribution from the underlying probability measure $P$ on the sample space $\Omega$.  

In the above example, we can say that $X$ follows a Binomial distribution. This identification arises because the random variable $X$ transforms the distribution on the sample space $\Omega$ into a distribution on the real line.


**Q. What does it mean when we say: The random variable $X$ is normally distributed?**

- $X$ is a random variable with a Gaussian distribution $N(m, \sigma^2)$, where $m$ is the mean and $\sigma^2$ is the variance, denoted as:
  $$
  X \sim N(m, \sigma^2)
  $$
- This means that the random variable $X$, which maps outcomes from the sample space $\Omega$ to the real line $\mathbb{R}$, induces a normal distribution on $\mathbb{R}$ with mean $m$ and variance $\sigma^2$.
- The induced distribution on $\mathbb{R}$ is described by the probability density function of the normal distribution.

The distribution is present in the sample space $\Omega$, but when we map outcomes via $X(\omega)$, this distribution is transformed (or induced) onto the real line $\mathbb{R}$. This allows us to study the random variable $X$ and its properties in the mathematical context of $\mathbb{R}$.

---

# Let's Deep Dive
In the previous section, we explored an example of defining a random variable, and the process appeared straightforward. However, this simplicity arose because of the implicit property of the sample space being countably finite. The sample space could also be countably infinite or uncountably infinite. Let's see what these terms mean and how it helps in revising the definition of a random variable.

## Countably Finite Set
A **countably finite set** is a set with a finite number of elements and can be put into a one-to-one correspondence with the first $n$ positive integers, where $n$ is finite.
- **Example**:
  - $A = \{2, 4, 6, 8\}$ has 4 elements that correspond to the natural numbers $\{1, 2, 3, 4\}$.

## Countably Infinite Set
A **countably infinite set** is a set with infinitely many elements, but these elements can still be put into a one-to-one correspondence with the set of natural numbers ($\mathbb{N}$).

- **Definition**: A set is countably infinite if there exists a bijection (one-to-one and onto mapping) between the set and the set of natural numbers $\mathbb{N} = \{1, 2, 3, \dots\}$.
- **Examples**:
  - The set of all natural numbers: $\mathbb{N} = \{1, 2, 3, 4, \dots\}$.
  - The set of all integers: $\mathbb{Z} = \{\dots, -2, -1, 0, 1, 2, \dots\}$.
    - $\mathbb{Z}$ can be mapped to $\mathbb{N}$ using:
        $$ f(n) = 
        \begin{cases} 
        2n & \text{if } n > 0 \\
        -2n + 1 & \text{if } n \leq 0 
        \end{cases} 
        $$
  - The set of even numbers, $E = \{2, 4, 6, 8, \dots\}$ maps $ E \to \mathbb{N} $ with $f(n) = 2n$.


## Uncountably Infinite Set
A set is **uncountably infinite** if it contains infinitely many elements that cannot be put into a one-to-one correspondence with the set of natural numbers ($\mathbb{N}$). In other words, the set is too large to be enumerated, even in principle.

**Properties**
- An uncountably infinite set has a **greater cardinality** than countable sets like $\mathbb{N}$ or $\mathbb{Z}$.
- The term "uncountable" reflects the impossibility of listing all elements of the set, even using an infinite process.

**Example**

The interval $[0, 1] \subset \mathbb{R}$ is uncountably infinite. This can be proven by Cantor's diagonal argument, which demonstrates that no list can include all real numbers.

---
# Revisit the definition

Now that we understand different types of sample spaces, let's revisit our definition of a random variable as presented in Equation (1):
$$
    X(\omega) : \Omega \rightarrow \mathbb{R}
$$

Here, $\omega$ represents an outcome within the sample space $\Omega$. The sample space ($\Omega$) can vary based on its cardinality and can be:
- **Countably Finite**: For instance, when $\Omega$ consists of outcomes from a finite experiment like flipping a coin a fixed number of times.
- **Countably Infinite**: For example, when $\Omega$ includes outcomes indexed by natural numbers, such as set of all integers.
- **Uncountably Infinite**: Such as when $\Omega$ is the set of all possible real numbers, like measuring precise temperatures or distances. These outcomes form a continuous sample space, which is inherently uncountable.

To build our concept further, we need to get a grasp of certain fundamental theory in mathematics:

* Set Theory
* Axioms of Probability
* Probability Space

We will introduce these concepts as we refine our construction of the definition of a random variable.





# References

1.  Gubner, John A. *Probability and Random Processes for Electrical and Computer Engineers*. Cambridge University Press, 2006.