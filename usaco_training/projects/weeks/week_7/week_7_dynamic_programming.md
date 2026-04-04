# Week 7: Dynamic Programming — Foundations
## Month 2 — Algorithm Development

**Goal**: Implement the 5 essential DP patterns. DP appears in every Silver+ contest.

---

## Learning Materials
- **USACO Guide DP**: https://usaco.guide/silver/dp/
- **USACO Guide Intro DP**: https://usaco.guide/bronze/intro-dp/
- Read both before starting problems

---

## Core C++ to Know Cold

```cpp
#include <bits/stdc++.h>
using namespace std;

// === PATTERN 1: 0/1 KNAPSACK ===
// Items with weight and value; max value in capacity W
// dp[i][w] = max value using first i items with capacity w
int knapsack(int W, vector<int>& weight, vector<int>& value) {
    int n = weight.size();
    vector<vector<int>> dp(n+1, vector<int>(W+1, 0));
    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= W; w++) {
            dp[i][w] = dp[i-1][w];  // don't take item i
            if (w >= weight[i-1])
                dp[i][w] = max(dp[i][w], dp[i-1][w-weight[i-1]] + value[i-1]);
        }
    }
    return dp[n][W];
}
// Space-optimized (1D):
int knapsack_1d(int W, vector<int>& wt, vector<int>& val) {
    vector<int> dp(W+1, 0);
    for (int i = 0; i < wt.size(); i++)
        for (int w = W; w >= wt[i]; w--)  // iterate BACKWARDS for 0/1 knapsack
            dp[w] = max(dp[w], dp[w-wt[i]] + val[i]);
    return dp[W];
}

// === PATTERN 2: LONGEST INCREASING SUBSEQUENCE (LIS) ===
// O(n log n) with patience sorting
int lis(vector<int>& v) {
    vector<int> tails;  // tails[i] = smallest tail of IS of length i+1
    for (int x : v) {
        auto it = lower_bound(tails.begin(), tails.end(), x);
        if (it == tails.end()) tails.push_back(x);
        else *it = x;
    }
    return tails.size();
}

// === PATTERN 3: LONGEST COMMON SUBSEQUENCE (LCS) ===
// O(nm) time and space (or O(n) space with rolling array)
int lcs(string& s, string& t) {
    int n = s.size(), m = t.size();
    vector<vector<int>> dp(n+1, vector<int>(m+1, 0));
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            dp[i][j] = (s[i-1] == t[j-1]) ? dp[i-1][j-1] + 1
                                            : max(dp[i-1][j], dp[i][j-1]);
    return dp[n][m];
}

// === PATTERN 4: DP ON INTERVALS ===
// dp[l][r] = answer for subarray [l, r]
// Fill by increasing interval length
void interval_dp(vector<int>& v) {
    int n = v.size();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int len = 2; len <= n; len++) {
        for (int l = 0; l + len - 1 < n; l++) {
            int r = l + len - 1;
            // dp[l][r] = f(dp[l][k], dp[k+1][r]) for some split k
            for (int k = l; k < r; k++) {
                dp[l][r] = max(dp[l][r], dp[l][k] + dp[k+1][r] + /* cost */0);
            }
        }
    }
}

// === PATTERN 5: DP ON TREES ===
// dp[node] depends on children
// Post-order DFS: compute children first, then parent
vector<int> subtree_size;
void dfs_dp(int u, int parent, vector<vector<int>>& adj) {
    subtree_size[u] = 1;
    for (int v : adj[u]) {
        if (v == parent) continue;
        dfs_dp(v, u, adj);
        subtree_size[u] += subtree_size[v];
    }
}
```

---

## Week 7 Problem Set (25 problems)

### Knapsack / Subset Sum
1. [CSES — Coin Combinations I](https://cses.fi/problemset/task/1635) — unbounded knapsack
2. [CSES — Coin Combinations II](https://cses.fi/problemset/task/1636) — count combinations
3. [CSES — Minimizing Coins](https://cses.fi/problemset/task/1634) — unbounded knapsack min
4. [CSES — 0/1 Knapsack](https://cses.fi/problemset/task/1158) — classic
5. [USACO Silver — Fruit Feast](https://usaco.org/index.php?page=viewproblem2&cpid=574) — 2D knapsack

### LIS / LCS
6. [CSES — Increasing Subsequence](https://cses.fi/problemset/task/1145) — LIS
7. [CSES — Common Subsequence](https://cses.fi/problemset/task/1711) — LCS
8. [Codeforces 1144E — Median String](https://codeforces.com/problemset/problem/1144/E) — LCS variant
9. [USACO Silver — Cowjog](https://usaco.org/index.php?page=viewproblem2&cpid=496) — LIS application
10. [Codeforces 1293D — Aroma's Search](https://codeforces.com/problemset/problem/1293/D) — DP + geometry

### DP on Trees
11. [CSES — Tree Matching](https://cses.fi/problemset/task/1130) — tree DP
12. [CSES — Tree Diameter](https://cses.fi/problemset/task/1131) — tree DP
13. [USACO Silver — Grass Cownoisseur](https://usaco.org/index.php?page=viewproblem2&cpid=862) — SCC + DAG DP

### Mixed
14-25: From https://usaco.guide/silver/dp/ problem list

---

## Daily Breakdown
- **Day 41-42**: Knapsack variants (1D and 2D) + 5 problems
- **Day 43-44**: LIS (O(n log n)) + LCS + 5 problems
- **Day 45**: DP on trees + 5 problems; review/consolidation

## Success Criteria
- [ ] Implement 0/1 knapsack 1D from memory in <10 minutes
- [ ] Implement O(n log n) LIS from memory in <8 minutes
- [ ] Implement LCS from memory in <10 minutes
- [ ] Solve CSES Coin Combinations I (unbounded knapsack)
- [ ] Solve CSES Tree Matching (tree DP)
