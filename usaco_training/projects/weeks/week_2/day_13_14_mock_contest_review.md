# Days 13-14: Mock Contest + Review

## Day 13: Mock USACO Bronze Contest (4 hours)

### Format
- **Duration**: 4 hours strict timer
- **Problems**: Pick any 3 problems from [USACO 2019 Feb Bronze](https://usaco.org/index.php?page=feb19results)
- **Environment**: Write in your editor, use file I/O (`freopen`)
- **Rules**: No external help, no searching solutions

### Contest Strategy (4-hour breakdown)
```
0:00 - 0:15  Read all 3 problems. Identify easiest → hardest.
0:15 - 1:45  Solve Problem 1 (easiest). Submit early.
1:45 - 3:15  Solve Problem 2 (medium). Submit when working.
3:15 - 3:45  Attempt Problem 3. Partial credit > nothing.
3:45 - 4:00  Review, clean up, final submission.
```

**USACO file I/O template** (required for real contests):
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    // Replace "problem" with the actual problem name (e.g., "mixmilk")
    freopen("problem.in", "r", stdin);
    freopen("problem.out", "w", stdout);

    // Your solution here
    int n;
    cin >> n;
    cout << n << "\n";

    return 0;
}
```

### Problems to Use
1. [Cow Dating](https://usaco.org/index.php?page=viewproblem2&cpid=910) — greedy/sort
2. [Sleepy Cow Sorting](https://usaco.org/index.php?page=viewproblem2&cpid=912) — simulation
3. [Measuring Traffic](https://usaco.org/index.php?page=viewproblem2&cpid=917) — careful implementation

### Record Your Results
| Problem | Time to Solve | Correct? | Issues |
|---------|--------------|---------|--------|
| Problem 1 | | | |
| Problem 2 | | | |
| Problem 3 | | | |

---

## Day 14: Contest Review + Week 2 Assessment

### Contest Debrief (90 minutes)
For each problem you did NOT solve fully:
1. Read the official editorial (USACO editorials linked from problem page)
2. Implement the editorial solution yourself (no copy-paste)
3. Identify which pattern you missed: sort? complete search? simulation? greedy?

### Week 2 Self-Assessment
Answer honestly:
- [ ] Can I write `std::sort` with a lambda comparator in <2 minutes from memory?
- [ ] Can I write the binary search template correctly in <3 minutes?
- [ ] Do I know when brute force is fast enough (constraint → complexity table)?
- [ ] Can I simulate a step-by-step process without update-order bugs?

**If any are "no"**: spend today's remaining time drilling that specific skill.

### Targeted Practice (2 hours)
Choose 3 problems from your weakest area today:

**Sort/custom comparator**: https://usaco.guide/bronze/intro-sorting/
**Binary search**: https://cses.fi/problemset/ (search "search" tag)
**Complete search**: https://usaco.guide/bronze/complete-search/
**Simulation**: https://usaco.guide/bronze/simulation/

### Week 2 Problem Count Target
- Total this week: 25+ problems
- At least 10 from USACO (official grader)
- At least 5 Codeforces (rated 800-1000)

---

## Notes & Reflection
- **Contest problems I couldn't solve**:
- **Pattern gap identified**:
- **Week 3 focus area**:
