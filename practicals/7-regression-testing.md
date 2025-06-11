# Regression Testing

We will use the following scenario.

Let us suppose that a program $p$ under test has ten branches, denoted $b_1 \ldots b_{10}$.

Further, suppose that there is a regression test suite $T$ that contains six test cases, $t_1 \ldots t_6$ such that:

- The execution with $t_1$ leads to the following branches being covered: $b_1, b_3, b_7$.
- The execution with $t_2$ leads to the following branches being covered: $b_2, b_5, b_7$.
- The execution with $t_3$ leads to the following branches being covered: $b_1, b_4, b_8, b_{10}$.
- The execution with $t_4$ leads to the following branches being covered: $b_2, b_6, b_9, b_{10}$.
- The execution with $t_5$ leads to the following branches being covered: $b_2, b_5, b_8, b_9, b_{10}$.
- The execution with $t_6$ leads to the following branches being covered: $b_1, b_4, b_7$.

## Test Suite Minimisation

### Task 1

1. Apply the Greedy Algorithm to the above scenario to produce a reduced test suite that still achieves 100\% branch coverage. Consider whether there is more than one possible test suite that could be returned by the Greedy Algorithm.
2. Apply the Additional Greedy Algorithm to the above scenario to produce a reduced test suite that still achieves 100% branch coverage. At each step, determine the additional coverage provided by each test case not already chosen. Again, consider whether there is more than one possible solution.
3. Apply the algorithm based on uniquely-covered goals proposed by Harrold et al.

## Test Prioritisation

### Task 2

1. Apply the Greedy Algorithm to the above scenario to produce an execution order for the test cases. If there is more than one possible result, then list them all.

### Task 3

Consider the ordering you produced in the previous task. If the greedy algorithm can produce more than one ordering then choose one to be used in the following. Given an ordering of the six test cases, we can look at the coverage provided by the first $n$ test cases, where $1 \leq n \leq 6$.

1. For what lengths (values of $n$) does the ordering produce the same branch coverage?
2. What is the value of $n$ that leads to the largest difference in coverage, and how large is this difference?

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