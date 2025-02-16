# A simple program to verify the expectation formula
## Problem description
***
### Coin pattern
Your coin produces heads with probability $p$. You flip your coin $n≥3$ times. What is the expected number of times that the pattern $HTH$ appears?
Example: If the $n$ flips are $⟨H,T,H,T,H,T,H⟩$ then the pattern $HTH$ appears three times.
## Solution
***
Let $X$ be the number of times the $HTH$ pattern appears. Then $$X \sim Binomial(n-2,p^2(1-p))$$ and $$E[X]=(n-2)p^2(1-p)$$
## Attempting to prove using code
***
I wrote this Java program to simulate $n$ flips of a fair coin where $p=0.5$ (to make my life easier). 
It prompts the user for an input of $n$, and displays all possible outcomes with an $HTH$ pattern inside it and $X$ of that outcome. Note that `true` represents $H$ and `false` represents $T$. 
It will then display the $n$, total number of outcomes for each possible value of $X$, and the summed expectation using the definition of $E[X]$, where $$E[X]=\sum_x{xProb[X=x]}$$ It will also calculate $E[X]$ using the solution $E[X]=(n-2)p^2(1-p)$ so we can check if the values match. 
### Sample run
***
```
Input number of flips: 5
[true, true, true, false, true] number of HTH: 1
[true, true, false, true, true] number of HTH: 1
[true, true, false, true, false] number of HTH: 1
[true, false, true, true, true] number of HTH: 1
[true, false, true, true, false] number of HTH: 1
[true, false, true, false, true] number of HTH: 2
[true, false, true, false, false] number of HTH: 1
[false, true, true, false, true] number of HTH: 1
[false, true, false, true, true] number of HTH: 1
[false, true, false, true, false] number of HTH: 1
[false, false, true, false, true] number of HTH: 1
For n = 5
Number of outcomes when X = 1: 10
Number of outcomes when X = 2: 1
E[X] = 0.375
Using formula (n-2)(1-p)p^2 when p = 0.5: 0.375
```
## Acknowledgements
***
I asked [DeepSeek](https://chat.deepseek.com/) to eliminate my recursive function calls to generate all coin flip outcomes. Here is its response:

<blockquote>
To implement the given recursive algorithm iteratively, we need to simulate the recursion using a **stack**. The stack will keep track of the state (i.e., the current index and whether we are flipping the current bit or not). Here's how you can do it:

---

### Iterative Implementation Using a Stack

```java
import java.util.Stack;

public class FlipAlgorithm {
    // Helper method to count HTH patterns (assumed to be implemented)
    public static int countHTH(boolean[] flips) {
        // Implementation of countHTH logic goes here
        // For example, count occurrences of "HTH" in the sequence
        int count = 0;
        for (int i = 0; i < flips.length - 2; i++) {
            if (!flips[i] && flips[i + 1] && !flips[i + 2]) {
                count++;
            }
        }
        return count;
    }

    public static void genFlipIterative(boolean[] flips, int[] numOfEvents) {
        Stack<int[]> stack = new Stack<>();
        stack.push(new int[]{0, 0}); // {index, state}

        while (!stack.isEmpty()) {
            int[] current = stack.pop();
            int index = current[0];
            int state = current[1];

            if (index == flips.length - 1) {
                // Flip the last bit and record
                flips[index] = !flips[index];
                numOfEvents[countHTH(flips)]++;
                flips[index] = !flips[index];
                numOfEvents[countHTH(flips)]++;
            } else {
                if (state == 0) {
                    // First visit: flip the current bit and explore further
                    flips[index] = !flips[index];
                    stack.push(new int[]{index, 1}); // Mark as visited
                    stack.push(new int[]{index + 1, 0}); // Move to next index
                } else if (state == 1) {
                    // Second visit: unflip the current bit and explore further
                    flips[index] = !flips[index];
                    stack.push(new int[]{index, 2}); // Mark as visited
                    stack.push(new int[]{index + 1, 0}); // Move to next index
                }
                // No need to handle state == 2, as it means we're done with this index
            }
        }
    }

    public static void main(String[] args) {
        boolean[] flips = new boolean[3]; // Example: 3 flips
        int[] numOfEvents = new int[10]; // Example: array to store counts

        genFlipIterative(flips, numOfEvents);

        // Print the results
        for (int i = 0; i < numOfEvents.length; i++) {
            System.out.println("Number of sequences with " + i + " HTH patterns: " + numOfEvents[i]);
        }
    }
}
```

---

### Explanation of the Code

1. **Stack Usage**:
    - The stack stores the current state of the algorithm, which includes:
        - The current `index` being processed.
        - The `state` of the current index (0: first visit, 1: second visit, 2: done).

2. **Simulating Recursion**:
    - When `state == 0`, we flip the current bit and push the next index onto the stack.
    - When `state == 1`, we unflip the current bit and push the next index onto the stack.
    - When `index == flips.length - 1`, we process the last bit and record the result.

3. **Base Case**:
    - When the index reaches the last element (`flips.length - 1`), we flip the bit, record the result, unflip the bit, and record the result again.

4. **Recording Results**:
    - The `numOfEvents` array is updated with the count of HTH patterns for each sequence.

---

### Example Walkthrough

For `flips = [false, false, false]` (length 3), the algorithm will generate all possible sequences of flips and count the number of HTH patterns in each sequence.

---

### Key Points
- The stack simulates the call stack of the recursive version.
- The `state` variable ensures that each bit is flipped and unflipped in the correct order.
- This approach avoids stack overflow issues that can occur with deep recursion.

Let me know if you need further clarification!
</blockquote>