# Regression Testing - Answers

We will use the following scenario.

Let us suppose that a program $p$ under test has ten branches, denoted $b_1 \ldots b_{10}$.

Further, suppose that there is a regression test suite $T$ that contains six test cases, $t_1 \ldots t_6$ such that:

- The execution with $t_1$ leads to the following branches being covered: $b_1, b_3, b_7$.
- The execution with $t_2$ leads to the following branches being covered: $b_2, b_5, b_7$.
- The execution with $t_3$ leads to the following branches being covered: $b_1, b_4, b_8, b_{10}$.
- The execution with $t_4$ leads to the following branches being covered: $b_2, b_6, b_9, b_{10}$.
- The execution with $t_5$ leads to the following branches being covered: $b_2, b_5, b_8, b_9, b_{10}$.
- The execution with $t_6$ leads to the following branches being covered: $b_1, b_4, b_7$.

First of all, for convenience, let us build the coverage matrix, where 1 indicates that a test covers a branch:

| Test\Branch | $b_1$ | $b_2$ | $b_3$ | $b_4$ | $b_5$ | $b_6$ | $b_7$ | $b_8$ | $b_9$ | $b_{10}$ |
| :-------- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :----: |
| $t_1$     |   1   |   0   |   1   |   0   |   0   |   0   |   1   |   0   |   0   |    0     |
| $t_2$     |   0   |   1   |   0   |   0   |   1   |   0   |   1   |   0   |   0   |    0     |
| $t_3$     |   1   |   0   |   0   |   1   |   0   |   0   |   0   |   1   |   0   |    1     |
| $t_4$     |   0   |   1   |   0   |   0   |   0   |   1   |   0   |   0   |   1   |    1     |
| $t_5$     |   0   |   1   |   0   |   0   |   1   |   0   |   0   |   1   |   1   |    1     |
| $t_6$     |   1   |   0   |   0   |   1   |   0   |   0   |   1   |   0   |   0   |    0     |

## Test Suite Minimisation

### Task 1

1. Apply the Greedy Algorithm to the above scenario to produce a reduced test suite that still achieves 100\% branch coverage. Consider whether there is more than one possible test suite that could be returned by the Greedy Algorithm.
2. Apply the Additional Greedy Algorithm to the above scenario to produce a reduced test suite that still achieves 100% branch coverage. At each step, determine the additional coverage provided by each test case not already chosen. Again, consider whether there is more than one possible solution.
3. Apply the algorithm based on uniquely-covered goals proposed by Harrold et al.

#### Sample Solution

The greedy algorithm will follow these steps:
1. Start with the empty set $\emptyset$
2. Add the test with highest number of covered branches: $\{t_5\}$ ($t_5$ covers five branches)
3. Next, there are two choices with the highest number of covered branches (four): $t_3$ and $t_4$, so we pick either of them, say $t_3$: $\{t_5, t_3\}$
4. The next obvious candidate is now $t_4$ as it is the only test covering 4 branches: $\{t_5, t_3, t_4\}$
5. We now have three tests that cover three branches each ($t_1$, $t_2$ and $t_6$), so we pick one of them, say $t_1$:  $\{t_5, t_3, t_4, t_1\}$. At this step, we get lucky because $t_1$ covers precisely the only remaining uncovered branches $b_3$ and $b_7$. Therefore the algorithm stops and the reduced test suite is $\{t_5, t_3, t_4, t_1\}$. Had we picked for example $t_2$ or $t_6$ in this step, we would have had to continue for one or two more iterations to achieve full coverage.

The same algorithm can be applied in a more analytical way as follows. First, find out the number of branches covered by each test case: 3, 3, 4, 4, 5, 3 for $t_1..t_6$, respectively. Thus, there are 12 possible orders in which test cases might be considered, which can be represented by the following:

$\{t_5\}\{t_3t_4, t_4t_3\}$
$\{t_1 t_2 t_6, t_1 t_6 t_2, t_2 t_1 t_6, t_2 t_6 t_1, t_6 t_1 t_2, t_6 t_2 t_1\}$

Now consider the possible reduced test suites. For all orders, we include $t_5, t_3, t_4$. These cover all branches except for: $b_3$ and $b_7$. Further, $b_3$ is only covered by test case $t_1$, which also covers $b_7$, and so the algorithm will stop once $t_1$ has been added. We therefore obtain the following possible reduced test suites.

- $\{t_5, t_3, t_4, t_1\}$ (smallest)
- $\{t_5, t_3, t_4, t_2, t_1\}$
- $\{t_5, t_3, t_4, t_6, t_1\}$
- $\{t_5, t_3, t_4, t_2, t_6, t_1\}$


The additional greedy algorithm would also start with the test case $t_5$, as it has the maximum coverage. It then updates the coverage information, to give the number of currently uncovered branches that a remaining test case covers, we get:

- For $t_1$: $b_1, b_3, b_7$
- For $t_2$: $b_7$
- For $t_3$: $b_1, b_4$
- For $t_4$: $b_6$
- For $t_6$: $b_1, b_4, b_7$

There is now a choice of next test case: $t_1$ or $t_6$. Let us suppose that we choose $t_6$ and so we now have $\{t_5, t_6\}$.

If we then update the coverage information, to give the number of currently uncovered branches that a test case covers, we get:

- For $t_1$: $b_3$
- For $t_2$: None
- For $t_3$: None
- For $t_4$: $b_6$

At this point, we must include in the reduced test suite both $t_1$ and $t_4$ as they cover different yet-uncovered branches and we must discard $t_2$ and $t_3$, as they would not contribute any additional coverage. The order in which $t_1$ and $t_4$ are added does not matter and the reduced test suite is
$\{t_5, t_6, t_1, t_4\}$.

Finally, the algorithm by Harrold et al. would proceed as follows:

First, we list the tests that cover each goal:

- $b_1$: $t_1$, $t_3$, $t_6$
- $b_2$: $t_2$,$t_4$,$t_5$
- $b_3$: $t_1$
- $b_4$: $t_3$,$t_6$
- $b_5$: $t_2$,$t_5$
- $b_6$: $t_4$
- $b_7$: $t_1$,$t_2$,$t_6$
- $b_8$: $t_3$,$t_5$
- $b_9$: $t_4$, $t_5$
- $b_{10}$: $t_3$, $t_4$, $t_5$

We look at goals covered by a single test. In this example, we have $b_3$ and $b_6$ therefore we add the covering tests: $\{t_1, t_4\}$

Next, we discard all the goals that have been covered by the selection $\{t_1, t_4\}$ (namely $b_1$, $b_2$, $b_3$, $b_6$, $b_7$, $b_9$ and $b_{10}$) and update the list of remaining uncovered goals:


- $b_4$: $t_3$,$t_6$
- $b_5$: $t_2$,$t_5$
- $b_8$: $t_3$,$t_5$

We are left only with goals covered by two tests. Let's pick one at random, e.g., $b_4$, which is covered by tests $t_3$ and $t_6$. Between these, we pick $t_3$, because it covers more yet-uncovered branches than $t_6$ ($b_4$ and $b_8$ vs only $b_4$). Our selection is now $\{t_1, t_4, t_3\}$ and our remaining list of goals to cover is:

- $b_5$: $t_2$,$t_5$

Since there are no more goals left to cover, we add any of the tests covering $b_5$, e.g., $t_2$. The resulting minimised suite is:

$\{t_1, t_4, t_3, t_2\}$

## Test Prioritisation

### Task 2

1. Apply the Greedy Algorithm to the above scenario to produce an execution order for the test cases. If there is more than one possible result, then list them all.

#### Sample Solution

The greedy algorithm aims to maximise coverage as quickly as possible, so it would start by picking $t_5$, which covers the highest number of goals:

$\{t_5\}$

Then, since $t_3$ and $t_4$ cover the same number of goals, either of their permutations would come next:

$\{t_5\}\{t_3t_4 | t_4t_3\}$

Finally, since $t_1$, $t_2$, and $t_6$ cover three goals each, either of their permutations could follow. All possible valid orderings can therefore be represented as:

$\{t_5\}\{t_3t_4 | t_4t_3\}
\{t_1 t_2 t_6 | t_1 t_6 t_2 | t_2 t_1 t_6 | t_2 t_6 t_1 | t_6 t_1 t_2 | t_6 t_2 t_1\}$

For example, a concrete valid order of execution would be: $t_5 t_3 t_4 t_2 t_1 t_6$

### Task 3

Consider the ordering you produced in the previous task. If the greedy algorithm can produce more than one ordering then choose one to be used in the following. Given an ordering of the six test cases, we can look at the coverage provided by the first $n$ test cases, where $1 \leq n \leq 6$.

1. For what lengths (values of $n$) does the ordering produce the same branch coverage?
2. What is the value of $n$ that leads to the largest difference in coverage, and how large is this difference?

#### Sample Solution

Consider the following order: $t_5 t_3 t_4 t_2 t_1 t_6$.

The branch coverage provided by prefixes (i.e., incrementally) is given by the following:

- $t_5$: 5 branches
- $t_5 t_3$: 7 branches
- $t_5 t_3 t_4$: 8 branches
- $t_5 t_3 t_4 t_2$: 9 branches
- $t_5 t_3 t_4 t_2 t_1$: 10 branches
- $t_5 t_3 t_4 t_2 t_1 t_6$: 10 branches

At length 5 and length 6 all 10 branches are covered.

The biggest increase in coverage happens at length 2, with the increase of two covered goals.

## Test Selection

Recall method [`SpringUtils.duplicateLetters`](../code/lib/src/main/java/uk/ac/shef/com3529/stringutils/StringUtils.java) from the first week of the semester, for which you are given the following test suite.

```java
public class AlternativeStringUtilsTest {

    @Test
    public void shouldReturnEmptySet() {
        Set<Character> resultSet = duplicateLetters("");
        assertFalse(resultSet.contains('s'));
    }

    @Test
    public void shouldNotReturnNonRepeatedChar() {
        Set<Character> resultSet = duplicateLetters("bugs");
        assertFalse(resultSet.contains('s'));
    }

    @Test
    public void shouldReturnRepeatedChar() {
        Set<Character> resultSet = duplicateLetters("software testing");
        assertTrue(resultSet.contains('t'));
    }
}
```

### Task 4

1. Apply a test selection algorithm to decide which test to execute given the statement on line 25 was accidentally changed from `duplicates.add(si);` to `duplicates.clear();`. Which test(s) should be selected?

#### Sample Solution

First, we need to draw the CFG for the original program, and then the one for the modified one:

<img src="7-regression-testing-files/stringutils-cfg.drawio.png" alt="CFGs" width="500"/>

The difference in node 8 is illustrated with a dashed outline.

Next, we build the edge traversal sequences for each of our tests.

- `shouldReturnEmptySet`: (entry, 1-2), (1-2, 3), (3, return)
- `shouldNotReturnNonRepeatedChar`: (entry, 1-2), (1-2, 3), (3, 4), (4, 5), (5, 6), (6, 7), (7, 9), (9, 5), (5, 10), (10, 3), (3, return)
- `shouldReturnRepeatedChar`: (entry, 1-2), (1-2, 3), (3, 4), (4, 5), (5, 6), (6, 7), (7, 8), (7, 9), (9, 5), (5, 10), (10, 3), (3, return)

We then perform a parallel traversal of the two CFGs, where we observe a difference in the target node when looking at node 7: the statement in node 8 has changed. Therefore, we search the edge traversal sequences of our tests to identify a test that exercises edge (7, 8). The only candidate is `shouldReturnRepeatedChar`.

We complete the parallel traversal without spotting any other differences.

The output of the test selection algorithm therefore contains a single test `shouldReturnRepeatedChar`.

It may seem like a trivial algorithm with such a small example. Browse the module's [source code](../code/lib/src/main/java/) directory and try to find a more challenging example where you can apply the same technique.