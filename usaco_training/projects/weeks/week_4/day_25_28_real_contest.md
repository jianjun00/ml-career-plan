# Days 25-28: Real Competition Week

## Real Contest Participation

### USACO Contest (December/January/February/March — whichever is next)
- **Register**: https://usaco.org (free, create account)
- **Schedule**: Contests open for ~4 days (pick any 4-hour window)
- **Goal**: Solve 2+ problems → promotion to Silver (~750 points threshold, varies per contest)

### Pre-Contest Checklist (Day 25)
- [ ] Read the USACO problem format guide: https://usaco.org/index.php?page=howtocompete
- [ ] Test your file I/O template compiles and runs
- [ ] Set up your editor with code snippets for:
  - `#include <bits/stdc++.h>` + `using namespace std;`
  - `freopen("prob.in","r",stdin); freopen("prob.out","w",stdout);`
  - Vector/sort/BFS templates
- [ ] Sleep well. Contest performance degrades significantly on insufficient sleep.

### USACO File I/O (mandatory — forgetting this is instant 0)
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    // REQUIRED: Replace "problem" with actual problem name
    freopen("problem.in", "r", stdin);
    freopen("problem.out", "w", stdout);

    // Your solution
    int n;
    cin >> n;
    // ...
    cout << "answer\n";

    return 0;
}
```

---

## Contest Day Strategy (Day 26)

### 4-Hour Contest Plan
```
0:00 - 0:15  Read all 3 problems fully. Don't code yet.
             Classify each: easy / medium / hard.
             Write down: "Problem X is [pattern], I'll [approach]."

0:15 - 1:30  Solve easiest problem. 
             Submit partial solution if stuck at 60 min.
             Move on — don't get stuck on one problem.

1:30 - 3:00  Solve medium problem.
             If stuck after 45 min: implement brute force for partial credit.

3:00 - 3:45  Attempt hardest problem OR improve partial solutions.

3:45 - 4:00  Final check: file names correct? Output format matches?
             Re-read problem constraints to catch missed edge cases.
```

### Partial Credit Strategy
USACO scores by test case. If you can't solve the full problem:
```cpp
// Brute force solution for small test cases (partial credit)
// If n <= 20, this gets credit from test cases with small n
if (n <= 20) {
    // O(2^n) brute force
} else {
    // Fast solution (if you have time to implement)
}
```

---

## Post-Contest Review (Days 27-28)

### Immediate After Contest
1. Record your score from https://usaco.org (results posted within minutes)
2. Check if you promoted: "Congratulations" message = Silver

### Deep Review Protocol (Day 27)
For each problem — whether you solved it or not:
1. Read the official editorial: https://usaco.org/current/data/sol_[problem]_bronze_[month][year].html
2. Understand the intended solution fully
3. Implement the editorial solution if yours differed

### Problem Analysis Template
```
Problem: _______________
Your approach: _________
Correct approach: ______
Why you missed it: _____
Pattern to remember: ___
```

### Day 28: Month 1 Retrospective
| Metric | Target | Actual |
|--------|--------|--------|
| USACO problems solved | 50+ | |
| USACO Bronze promotion | Yes (~750 pts) | |
| Codeforces rating | 1000+ | |
| Total problems | 150+ | |
| Mock contests completed | 3 | |

**Month 2 Preview** — What you'll learn:
- `map`, `set`, `priority_queue` (STL mastery)
- BFS for shortest paths (weighted) — Dijkstra
- Union-Find (disjoint sets)
- Dynamic programming introduction
- Full Silver curriculum

---

## Notes
- **Contest score**:
- **Promoted?**: Yes / No
- **Biggest lesson from Month 1**:
