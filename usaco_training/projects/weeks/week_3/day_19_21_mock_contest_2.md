# Days 19-21: Mock Contest 2 + Targeted Review

## Day 19: Mock USACO Bronze Contest — Harder Set (4 hours)

### Contest
- **Problems**: [USACO 2020 Feb Bronze](https://usaco.org/index.php?page=feb20results)
  1. [Triangles](https://usaco.org/index.php?page=viewproblem2&cpid=1021) — geometry/sorting
  2. [Swapity Swapity Swap](https://usaco.org/index.php?page=viewproblem2&cpid=1024) — simulation
  3. [Photoshoot](https://usaco.org/index.php?page=viewproblem2&cpid=1023) — complete search

### Rules
- 4 hours strict; no external resources
- Use file I/O (`freopen("triangles.in","r",stdin)`)
- Submit to USACO official grader after finishing

### Record Results
| Problem | Solved? | Time | Score | Issue |
|---------|---------|------|-------|-------|
| Triangles | | | | |
| Swapity Swap | | | | |
| Photoshoot | | | | |

---

## Day 20: Deep-dive Review of Contest Problems

For each problem you didn't solve fully:

### Review Protocol
1. Read editorial: https://usaco.org/current/data/sol_[problem]_bronze_feb20.html
2. Implement solution from scratch after reading (no copy-paste)
3. Run on all official test cases

### Common Patterns to Reinforce

**Triangles** uses sort + prefix sum:
```cpp
// For each vertical line x_i, count pairs of points with same x
// For max triangle area: need max horizontal segment at same y
// Key: sort by x, then by y. Use prefix max for "furthest left" at each y.
// Area formula: base * height / 2 where base = horizontal seg, height = |y_i - y_j|
```

**Photoshoot** uses complete search + constraint propagation:
```cpp
// a[i] + a[i+1] = b[i] is given
// Fix a[0] = k, then a[1] = b[0] - k, a[2] = b[1] - a[1], ...
// Try all k from 1 to n; check if resulting array is valid permutation
for (int k = 1; k <= n; k++) {
    vector<int> a(n);
    a[0] = k;
    bool valid = true;
    for (int i = 0; i < n-1 && valid; i++) {
        a[i+1] = b[i] - a[i];
        if (a[i+1] < 1 || a[i+1] > n) valid = false;
    }
    if (valid) { /* check if permutation */ }
}
```

---

## Day 21: Week 3 Assessment + Gap Filling

### Self-Assessment
| Skill | Confident? | Time to Implement |
|-------|-----------|------------------|
| BFS from scratch | | |
| DFS from scratch | | |
| 1D prefix sum | | |
| 2D prefix sum | | |
| Two pointers | | |
| Sliding window | | |
| `std::sort` + lambda | | |
| Binary search template | | |

**Target**: Every row should be "Yes, < 5 minutes."

### Gap Filling Practice
Pick from whichever skill has the most "No" marks:
- **BFS/DFS gap**: CSES Graph Problems section (https://cses.fi/problemset/list/)
- **Prefix sum gap**: https://usaco.guide/silver/prefix-sums/
- **Two pointer gap**: https://cses.fi/problemset/ "Sorting and Searching" section

### Week 3 Target: 25+ problems solved this week

---

## Notes & Reflection
- **Patterns I still can't write from memory**:
- **Plan for Week 4**:
