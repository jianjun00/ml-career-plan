# Week 11: Advanced Dynamic Programming
## Month 3 — Advanced Algorithms

**Goal**: Master bitmask DP, digit DP, and DP optimizations. These are Gold-level patterns.

---

## Learning Materials
- **USACO Guide Bitmask DP**: https://usaco.guide/gold/dp-bitmask/
- **USACO Guide Digit DP**: https://usaco.guide/gold/digit-dp/
- **USACO Guide DP Optimizations**: https://usaco.guide/plat/dp-opt/

---

## Core C++ to Know Cold

```cpp
#include <bits/stdc++.h>
using namespace std;

// === BITMASK DP ===
// Use when n <= 20 and you need to track a subset of elements
// Classic: Traveling Salesman Problem (TSP)
// dp[mask][i] = min cost to visit all cities in `mask`, ending at city i
int tsp(vector<vector<int>>& dist) {
    int n = dist.size();
    int full = (1 << n) - 1;
    vector<vector<int>> dp(1 << n, vector<int>(n, INT_MAX/2));
    dp[1][0] = 0;  // start at city 0
    for (int mask = 1; mask <= full; mask++) {
        for (int u = 0; u < n; u++) {
            if (!(mask & (1 << u))) continue;
            if (dp[mask][u] == INT_MAX/2) continue;
            for (int v = 0; v < n; v++) {
                if (mask & (1 << v)) continue;  // already visited
                int new_mask = mask | (1 << v);
                dp[new_mask][v] = min(dp[new_mask][v], dp[mask][u] + dist[u][v]);
            }
        }
    }
    int ans = INT_MAX;
    for (int u = 1; u < n; u++)
        ans = min(ans, dp[full][u] + dist[u][0]);  // return to start
    return ans;
}

// === DIGIT DP ===
// Count numbers in [0, N] satisfying a property
// State: (position, tight, property_state)
// "tight" = whether current prefix equals the prefix of N
string N;
int dp_digit[20][2][/* property states */2];
bool computed[20][2][2];

// Count numbers in [0, N] with no two consecutive same digits
int solve(int pos, bool tight, bool prev_digit, int prev) {
    if (pos == N.size()) return 1;
    if (computed[pos][tight][prev_digit]) return dp_digit[pos][tight][prev_digit];
    computed[pos][tight][prev_digit] = true;
    int limit = tight ? (N[pos] - '0') : 9;
    int res = 0;
    for (int d = 0; d <= limit; d++) {
        if (prev_digit && d == prev) continue;  // no consecutive same digit
        res += solve(pos+1, tight && d == limit, true, d);
    }
    return dp_digit[pos][tight][prev_digit] = res;
}

// === DIVIDE AND CONQUER DP OPTIMIZATION ===
// Applies when: dp[i][j] = min(dp[i-1][k] + cost(k,j)) for k < j
// AND opt(i,j) <= opt(i,j+1) (monotone optimum property)
// Reduces O(n^3) to O(n^2 log n) or O(n^2)
// (Implementation omitted — look up "divide and conquer DP" when needed)
```

---

## Week 11 Problem Set (20 problems)

### Bitmask DP
1. [CSES — Hamiltonian Flights](https://cses.fi/problemset/task/1690) — bitmask DP on paths
2. [CSES — Elevator Rides](https://cses.fi/problemset/task/1653) — bitmask DP optimization
3. [USACO Gold — Mooriokart](https://usaco.org/index.php?page=viewproblem2&cpid=648) — bitmask DP
4. [Codeforces 1209E — Rotate Columns](https://codeforces.com/problemset/problem/1209/E) — bitmask DP

### Digit DP
5. [Count numbers with digit sum divisible by K](https://cses.fi/problemset/task/2220) — digit DP
6. [Codeforces 1073E — Segment Sum](https://codeforces.com/problemset/problem/1073/E) — digit DP
7. [Codeforces 1033F — Placing Robots](https://codeforces.com/problemset/problem/1033/F) — digit DP

### DP Optimizations
8. [USACO Gold — Circular Barn Revisited](https://usaco.org/index.php?page=viewproblem2&cpid=624) — divide and conquer DP
9. [CSES — Subarray Distinct Values](https://cses.fi/problemset/task/2428) — offline DP
10. [Codeforces 896C — Willem, Chtholly and Seniorious](https://codeforces.com/problemset/problem/896/C)

### Gold DP Problems
11-20: USACO Gold DP problems from https://usaco.guide/gold/dp/

---

## Daily Breakdown
- **Day 61-62**: Bitmask DP (TSP template) + 5 problems
- **Day 63**: Digit DP + 3 problems
- **Day 64**: DP optimizations (convex hull trick concept) + 3 problems
- **Day 65**: Mixed advanced DP + contest simulation

## Success Criteria
- [ ] Implement bitmask DP for TSP-like problem from memory
- [ ] Implement basic digit DP framework from memory
- [ ] Solve CSES Hamiltonian Flights
- [ ] Solve at least 2 USACO Gold DP problems
