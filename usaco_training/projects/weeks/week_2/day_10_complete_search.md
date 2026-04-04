# Day 10: Complete Search (Brute Force Enumeration)

## Daily Learning Objectives
- Enumerate all possible solutions when constraints allow it
- Know when brute force is fast enough (constraint analysis)
- Apply nested loops, permutation enumeration, and subset enumeration

---

## Learning Materials

### USACO Guide: Complete Search
- **URL**: https://usaco.guide/bronze/complete-search/
- Read all sections; focus on nested loops and generating subsets

---

## Core C++ to Know Cold

```cpp
#include <bits/stdc++.h>
using namespace std;

// Pattern 1: Nested loop enumeration
// Try all pairs (i, j) where i < j
void all_pairs(vector<int>& v) {
    int n = v.size();
    for (int i = 0; i < n; i++)
        for (int j = i+1; j < n; j++)
            // O(n^2) — ok for n <= 5000
            cout << v[i] << " " << v[j] << "\n";
}

// Pattern 2: All permutations (n <= 8)
void all_permutations(vector<int>& v) {
    sort(v.begin(), v.end());  // must start sorted for next_permutation
    do {
        for (int x : v) cout << x << " ";
        cout << "\n";
    } while (next_permutation(v.begin(), v.end()));
    // O(n!) — use only for n <= 8
}

// Pattern 3: All subsets via bitmask (n <= 20)
void all_subsets(int n) {
    for (int mask = 0; mask < (1 << n); mask++) {
        // mask encodes which elements are selected
        for (int i = 0; i < n; i++) {
            if (mask & (1 << i)) cout << i << " ";  // i-th bit set
        }
        cout << "\n";
    }
    // O(2^n * n) — ok for n <= 20
}

// Pattern 4: Grid enumeration (visit all cells)
void grid_search(vector<vector<int>>& grid) {
    int R = grid.size(), C = grid[0].size();
    int dx[] = {0,0,1,-1};
    int dy[] = {1,-1,0,0};
    for (int r = 0; r < R; r++) {
        for (int c = 0; c < C; c++) {
            // try all 4 neighbors
            for (int d = 0; d < 4; d++) {
                int nr = r + dx[d], nc = c + dy[d];
                if (nr >= 0 && nr < R && nc >= 0 && nc < C) {
                    // valid neighbor
                }
            }
        }
    }
}
```

**When is brute force fast enough?**
| n | Max complexity | Approach |
|---|---------------|----------|
| ≤ 8 | O(n!) | `next_permutation` |
| ≤ 20 | O(2^n) | Bitmask enumeration |
| ≤ 100 | O(n³) | Triple nested loop |
| ≤ 3,000 | O(n²) | Double nested loop |
| ≤ 100,000 | O(n log n) | Sort + binary search |

---

## Practice Problems (5 problems)

### Problem 1: Photoshoot (USACO Bronze 2020 Feb)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=1023
- **Approach**: Fix first element, reconstruct the rest. Enumerate all possibilities for first element
- **Expected time**: 25 minutes

### Problem 2: Bovine Genomics (USACO Bronze 2016 Feb)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=619
- **Approach**: For each position, brute force check if it distinguishes groups
- **Expected time**: 20 minutes

### Problem 3: The Bucket List (USACO Bronze 2018 Nov)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=856
- **Approach**: Enumerate all time steps; for each, count buckets needed
- **Expected time**: 20 minutes

### Problem 4: Codeforces 1144B — Parity Alternation
- **URL**: https://codeforces.com/problemset/problem/1144/B
- **Approach**: Enumerate placements; check conditions
- **Expected time**: 15 minutes

### Problem 5: CSES — Coin Combinations I (warm-up for DP later)
- **URL**: https://cses.fi/problemset/task/1635
- **Approach**: For small cases, brute force with nested loops to understand the problem structure before DP
- **Expected time**: 20 minutes (brute force ok for small n)

---

## Daily Goals
- [ ] Read USACO Guide complete search page
- [ ] Implement all 4 enumeration patterns from memory
- [ ] Solve all 5 problems
- [ ] For each problem: write n and confirm brute force is fast enough

## Notes & Reflection
- **Key rule**: Before optimizing, check if n is small enough for brute force. If O(n²) ≤ 10^8 operations, brute force first, then optimize if needed
- **Tomorrow's focus**: Simulation — executing process step-by-step
