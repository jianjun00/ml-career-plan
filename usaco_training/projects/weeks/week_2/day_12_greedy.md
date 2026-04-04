# Day 12: Greedy Algorithms

## Daily Learning Objectives
- Recognize greedy-applicable problems (exchange argument)
- Implement sort-then-greedy patterns
- Solve 5 greedy problems

---

## Learning Materials

### USACO Guide: Greedy Algorithms
- **URL**: https://usaco.guide/bronze/intro-greedy/
- Read all bronze-level greedy examples

---

## When Greedy Works

A greedy algorithm makes the locally optimal choice at each step and never backtracks. It works when you can prove the greedy choice never hurts future choices.

**Exchange argument** (informal): Suppose you have an optimal solution that differs from the greedy solution. Can you swap adjacent elements to transform it into the greedy solution without worsening the result? If yes, greedy is correct.

**Most common Bronze greedy patterns**:
1. Sort by one criterion, then greedily assign
2. Sort by ratio/difference, then take greedily
3. Process in order (smallest/largest first), count what fits

```cpp
#include <bits/stdc++.h>
using namespace std;

// Example: Activity selection — maximize number of non-overlapping intervals
// Greedy: always pick the interval that ends earliest
int max_activities(vector<pair<int,int>>& intervals) {
    // Sort by end time
    sort(intervals.begin(), intervals.end(), [](auto& a, auto& b){
        return a.second < b.second;
    });
    int count = 0, last_end = INT_MIN;
    for (auto& [start, end] : intervals) {
        if (start >= last_end) {
            count++;
            last_end = end;
        }
    }
    return count;
}

// Example: Assign cows to stalls to minimize max distance
// Greedy: sort positions, binary search on answer, greedy feasibility check
bool can_place(vector<int>& pos, int k, int min_dist) {
    int placed = 1, prev = pos[0];
    for (int i = 1; i < pos.size(); i++) {
        if (pos[i] - prev >= min_dist) {
            placed++;
            prev = pos[i];
        }
    }
    return placed >= k;
}
```

**Anti-pattern**: Greedy fails when local choices cascade into globally bad outcomes. If in doubt, test on a small counterexample before committing.

---

## Practice Problems (5 problems)

### Problem 1: Lemonade Line (if not done Day 11)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=835
- **Greedy insight**: Sort by patience desc; cow joins if patience >= current queue length
- **Expected time**: 20 minutes

### Problem 2: Guess the Animal (USACO Bronze 2020 Feb)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=1026
- **Approach**: Find attributes shared by all animals in a group; greedy attribute selection
- **Expected time**: 25 minutes

### Problem 3: Air Cownditioning (USACO Bronze 2017 Feb)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=670
- **Greedy insight**: Process intervals left to right; extend solution minimally to cover uncovered spots
- **Expected time**: 25 minutes

### Problem 4: Codeforces 1206B — Make Product Equal
- **URL**: https://codeforces.com/problemset/problem/1206/B
- **Greedy insight**: Greedily replace zeros based on count parity
- **Expected time**: 15 minutes

### Problem 5: Codeforces 580C — Kefaa and Strings
- **URL**: https://codeforces.com/problemset/problem/580/C
- **Approach**: Greedy construction of palindrome-like structure
- **Expected time**: 20 minutes

---

## Daily Goals
- [ ] Read USACO Guide greedy page
- [ ] Implement the activity selection algorithm from memory
- [ ] Solve all 5 problems
- [ ] For each problem: state the greedy choice and why it's safe

## Notes & Reflection
- **Greedy pitfall**: If you can't articulate why the greedy choice is safe, try brute force first and verify on examples
- **Tomorrow's focus**: Mock contest (Week 2 review)
