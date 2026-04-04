# Day 11: Simulation — Step-by-Step Process Execution

## Daily Learning Objectives
- Translate problem descriptions into direct code (no clever algorithm needed)
- Avoid common simulation bugs: off-by-one, wrong update order, forgetting state
- Solve 5 simulation problems

---

## Learning Materials

### USACO Guide: Simulation
- **URL**: https://usaco.guide/bronze/simulation/
- Read all examples; pay attention to update-order traps

---

## What Simulation Means

Simulation problems say "do exactly what the problem describes." There is no insight — the insight IS the code. The skill is:
1. Read the problem statement precisely
2. Map each sentence to a line of code
3. Handle edge cases in the stated order

**Common bugs in simulation**:
```cpp
// BUG 1: Updating state while iterating (use a copy or process in correct order)
vector<int> v = {1, 2, 3, 4, 5};
// WRONG: modifying v[i] then using it to update v[i+1]
for (int i = 0; i < n-1; i++)
    v[i+1] += v[i];  // v[i] already modified!

// CORRECT: process in correct direction, or use temporary
vector<int> next_v = v;
for (int i = 0; i < n-1; i++)
    next_v[i+1] += v[i];  // read from old v
v = next_v;

// BUG 2: Off-by-one in ranges
// Problem says "days 1 to N" but arrays are 0-indexed
for (int day = 1; day <= N; day++)
    process(day - 1);  // explicit offset

// BUG 3: Forgetting to reset state between test cases
// If T test cases, reset all globals/arrays before each
```

**Template for simulation**:
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int n;
    cin >> n;

    // Read initial state
    vector<int> state(n);
    for (int i = 0; i < n; i++) cin >> state[i];

    // Simulate steps
    int steps;
    cin >> steps;
    for (int t = 0; t < steps; t++) {
        // Apply one step of the process
        vector<int> next = state;
        for (int i = 0; i < n; i++) {
            // update next[i] based on state (old values)
        }
        state = next;
    }

    // Output result
    for (int x : state) cout << x << "\n";
    return 0;
}
```

---

## Practice Problems (5 problems)

### Problem 1: Lemonade Line (USACO Bronze 2018 Jan)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=835
- **Approach**: Sort cows by patience descending; simulate queue joining
- **Expected time**: 20 minutes

### Problem 2: Cow Tipping (USACO Bronze 2019 Feb — avoid, too hard)
### Problem 2 (actual): Shell Game (USACO Bronze 2016 Feb)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=891
- **Approach**: Simulate 3 possible starting positions; track where the pea ends up
- **Expected time**: 20 minutes

### Problem 3: The Cow-Signal (USACO Bronze 2016 Feb)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=608
- **Approach**: Simulate scaling: each cell of original becomes a K×K block
- **Expected time**: 15 minutes

### Problem 4: Milk Factory (USACO Bronze 2019 Jan)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=897
- **Approach**: Simulate graph traversal — which nodes can reach the silo?
- **Expected time**: 25 minutes

### Problem 5: Codeforces 1304B — Longest Palindrome
- **URL**: https://codeforces.com/problemset/problem/1304/B
- **Approach**: Simulate palindrome construction — frequency counting + placement rules
- **Expected time**: 20 minutes

---

## Daily Goals
- [ ] Read USACO Guide simulation page
- [ ] Identify and name the simulation bugs in the examples above
- [ ] Solve all 5 problems
- [ ] For each problem: describe the state and the update rule in one sentence before coding

## Notes & Reflection
- **Key discipline**: Read the problem 2x before writing code. The bug is usually in your reading, not your code
- **Tomorrow's focus**: Greedy algorithms
