# Week 8: Silver Contest Preparation + Real Contest
## Month 2 — Algorithm Development

**Goal**: Consolidate Silver-level skills. Pass the USACO Silver division.

---

## Silver Self-Assessment (do this before Day 46)

Can you implement these from memory in the given time?
| Algorithm | Target Time | Pass? |
|-----------|------------|-------|
| Dijkstra (min heap) | 15 min | |
| Union-Find with path compression | 10 min | |
| Kahn's toposort | 10 min | |
| 0/1 Knapsack 1D | 10 min | |
| LIS O(n log n) | 8 min | |
| BFS + DFS | 5 min each | |
| `std::sort` + lambda | 2 min | |
| Prefix sum 1D/2D | 5 min | |
| Two pointers | 5 min | |

**If any are "No"**: spend Days 46-47 drilling those specifically.

---

## Days 46-47: Targeted Silver Drilling

### Problem Set: USACO Silver Recent Contests
Work through these in the order listed (recent → harder):
1. [Hoof, Paper, Scissors](https://usaco.org/index.php?page=viewproblem2&cpid=694) — DP
2. [Mooyo Mooyo](https://usaco.org/index.php?page=viewproblem2&cpid=860) — BFS + flood fill
3. [Milk Visits](https://usaco.org/index.php?page=viewproblem2&cpid=968) — graph reachability
4. [Wormhole Sort](https://usaco.org/index.php?page=viewproblem2&cpid=992) — DSU + binary search
5. [Convention](https://usaco.org/index.php?page=viewproblem2&cpid=786) — binary search on answer
6. [Lunchtime](https://usaco.org/index.php?page=viewproblem2&cpid=835) — greedy
7. [Rest Stops](https://usaco.org/index.php?page=viewproblem2&cpid=788) — greedy
8. [Icy Perimeter](https://usaco.org/index.php?page=viewproblem2&cpid=895) — BFS + counting

---

## Day 48: Mock Silver Contest (4 hours)

### Contest
Pick [USACO 2020 Jan Silver](https://usaco.org/index.php?page=jan20results):
1. [Word Processor](https://usaco.org/index.php?page=viewproblem2&cpid=987) — simulation
2. [Loan Repayment](https://usaco.org/index.php?page=viewproblem2&cpid=990) — binary search
3. [Wormhole Sort](https://usaco.org/index.php?page=viewproblem2&cpid=992) — DSU + binary search

### Rules
- 4 hours strict
- File I/O required
- No external resources

---

## Days 49-50: Review + Real Contest

### Day 49: Mock Contest Debrief
For each problem not fully solved:
1. Read editorial
2. Implement solution
3. Run all test cases

### Day 50: Real USACO Silver Contest

**Promotion threshold**: ~750 points (varies per contest)
**Typical score distribution**: 
- 3/3 correct = 1000 pts → Gold promotion
- 2/3 correct ≈ 600-800 pts → usually Silver promotion
- 1/3 correct ≈ 333 pts → stay Silver

**Contest strategy for Silver** (4 hours):
```
0:00 - 0:20  Read all 3 problems. Classify each.
             Silver problems: one easy (simulation/greedy), one medium, one hard.
0:20 - 1:30  Easy problem — solve completely, test edge cases.
1:30 - 3:00  Medium problem — solve completely.
3:00 - 4:00  Hard problem — attempt, get partial credit if possible.
             For partial credit: implement O(n^3) brute force for small n.
```

---

## Month 2 Success Criteria
- [ ] USACO Silver promotion (~750 points)
- [ ] Can implement Dijkstra, DSU, and DP patterns from memory
- [ ] Codeforces rating 1400+
- [ ] 180+ problems solved this month

**Month 3 Preview**: Advanced data structures (Fenwick tree, segment tree), advanced graph (SCC, bridges), advanced DP (bitmask, digit), and Gold-level preparation.
