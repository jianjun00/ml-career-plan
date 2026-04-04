# Days 22-24: Comprehensive Review + Weakness Targeting

## Objective
Consolidate all Month 1 skills. By end of Day 24 you should solve any Bronze problem from 2018-2022 in under 45 minutes on average.

---

## Day 22: Bronze Problem Blitz — 10 Problems

Solve 10 recent USACO Bronze problems. Use a timer. Record your time.

**Problem list** (pick from different contests):
1. [Cow Gymnastics](https://usaco.org/index.php?page=viewproblem2&cpid=963) — counting pairs
2. [Fence Painting](https://usaco.org/index.php?page=viewproblem2&cpid=567) — interval union  
3. [Contaminated Milk](https://usaco.org/index.php?page=viewproblem2&cpid=569) — simulation
4. [Livestock Lineup](https://usaco.org/index.php?page=viewproblem2&cpid=965) — permutation search
5. [Where Am I?](https://usaco.org/index.php?page=viewproblem2&cpid=964) — string matching
6. [The Bovine Shuffle](https://usaco.org/index.php?page=viewproblem2&cpid=768) — cycle detection
7. [Fence Planning](https://usaco.org/index.php?page=viewproblem2&cpid=1062) — DFS on grid
8. [Stuck in a Rut](https://usaco.org/index.php?page=viewproblem2&cpid=1061) — simulation with sorting
9. [Sleeping in Class](https://usaco.org/index.php?page=viewproblem2&cpid=1080) — binary search
10. [Counting Socks](https://usaco.org/index.php?page=viewproblem2&cpid=1260) — frequency count

**Timer log**:
| Problem | Time | Solved? | Pattern Used |
|---------|------|---------|-------------|
| Cow Gymnastics | | | |
| ... | | | |

**Target**: 7/10 solved, average < 40 min. If below 5/10, repeat Day 22 with new problems before moving on.

---

## Day 23: Targeted Weakness Drill

From your Day 22 log, identify which patterns you missed most. Then:

### Pattern Review Checklist
```cpp
// PATTERN 1: Interval problems (union, intersection, coverage)
// Sort by left endpoint, then sweep
vector<pair<int,int>> intervals;
sort(intervals.begin(), intervals.end());
int covered = 0, cur_end = INT_MIN;
for (auto [l, r] : intervals) {
    if (l > cur_end) { covered += r - l + 1; cur_end = r; }
    else if (r > cur_end) { covered += r - cur_end; cur_end = r; }
}

// PATTERN 2: Frequency map
map<int,int> freq;
for (int x : v) freq[x]++;
// or unordered_map for O(1) avg
unordered_map<int,int> freq2;
for (int x : v) freq2[x]++;

// PATTERN 3: Cycle detection in permutation
// Find cycles: from node i, follow f[i] until you return to i
vector<bool> vis(n+1, false);
for (int i = 0; i < n; i++) {
    if (!vis[i]) {
        int j = i, cycle_len = 0;
        while (!vis[j]) { vis[j] = true; j = f[j]; cycle_len++; }
        // process cycle of length cycle_len
    }
}

// PATTERN 4: Minimum spanning connection
// Sort edges by weight, greedy connect components (Union-Find)
// (This is Silver-level but appears in harder Bronze problems)
```

### Day 23 Practice: 8 problems
From your weakest pattern — pick 8 problems of that type from USACO or CSES.

---

## Day 24: Final Assessment — Simulated Contest

### 2-Hour Bronze Assessment
Solve these 3 problems in 2 hours (simulates 2/3 of a real contest):

1. [Herding (USACO Bronze 2014 Feb)](https://usaco.org/index.php?page=viewproblem2&cpid=351)
2. [Milk Measurement (USACO Bronze 2019 Dec)](https://usaco.org/index.php?page=viewproblem2&cpid=1003)
3. [Uncomfortable Cows (USACO Bronze 2019 Open)](https://usaco.org/index.php?page=viewproblem2&cpid=940)

**Pass criteria for Week 4 Gate**:
- Solve 2 of 3 within 2 hours → Ready for real contest
- Solve 1 of 3 → Drill more USACO Bronze before competing
- Solve 0 → Repeat Weeks 2-3 targeted practice

---

## Notes & Reflection
- **Top 3 weakest patterns going into the real contest**:
  1. 
  2. 
  3. 
- **Real contest date**:
