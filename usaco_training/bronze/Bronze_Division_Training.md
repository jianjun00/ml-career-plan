# Bronze Division Training Plan
## 2–3 Month Path to USACO Silver

---

## Objectives

**Target Score**: ~750/1000 (promotion threshold, set per-contest)
**Duration**: 2–3 months
**Time Commitment**: 15 hours/week
**Primary Resource**: [USACO Guide Bronze](https://usaco.guide/bronze/)

### What Bronze Actually Tests
Bronze problems test **careful implementation and problem analysis**, not advanced algorithms. You are expected to know basic C++ (STL) and be able to translate problem descriptions into working code. You do NOT need to implement sorting algorithms from scratch — you use `std::sort()`.

---

## Entry Point Diagnostic

Before starting Bronze, take this 20-minute diagnostic to find your real starting point:

1. Solve [CSES Weird Algorithm](https://cses.fi/problemset/task/1068) — if you can't, start at Module 1 C++ Setup
2. Solve [CSES Repetitions](https://cses.fi/problemset/task/1069) — if you can't, start at Module 2 Simulation
3. Write `std::sort` with a custom comparator on a `vector<pair<int,int>>` from memory — if you can't, start at Module 3
4. If you can do all three in under 20 minutes, skim Modules 1–3 and start at Module 4

**Already have Codeforces rating 1000+?** Skip to Module 4 (Complete Search).

---

## Bronze Curriculum (Based on USACO Guide)

### Module 1 — Getting Started (Week 1)

**C++ Setup**
- Install competitive programming environment (VS Code + competitive companion, or CLion)
- Learn the standard template (see main roadmap)
- Understand USACO file I/O: `freopen("prob.in", "r", stdin)`

**Time Complexity (Critical)**
- O(1), O(log n), O(n), O(n log n), O(n²), O(2^n)
- USACO constraints: n ≤ 100 means O(n³) is fine; n ≤ 100,000 means need O(n log n)
- Memory limit: typically 256 MB; array of 10^8 ints ≈ 400 MB (too big)
- Time limit: typically 2–4 seconds; ~10^8 operations per second is safe estimate

**Essential C++ STL**
```cpp
// Vector
vector<int> v = {3,1,4,1,5};
sort(v.begin(), v.end()); // [1,1,3,4,5]
sort(v.begin(), v.end(), greater<int>()); // [5,4,3,1,1]

// Sort with custom comparator
sort(v.begin(), v.end(), [](int a, int b){ return a > b; });

// Pair
pair<int,int> p = {3, 7};
cout << p.first << " " << p.second; // 3 7

// Map / Set
map<string,int> freq;
freq["apple"]++;
set<int> s;
s.insert(5); s.insert(3);
// s = {3, 5} (sorted)

// Useful functions
min(a, b), max(a, b)
abs(x), __gcd(a, b)
*min_element(v.begin(), v.end())
*max_element(v.begin(), v.end())
```

**Practice**: 10 easy Codeforces problems (rated 800)

---

### Module 2 — Simulation (Week 2)

**What it is**: Directly simulate the process described in the problem. No clever algorithm needed — just careful step-by-step execution.

**Key patterns**:
- State changes over time (tick-by-tick simulation)
- Moving objects in a grid
- Tracking counts, positions, statuses

**Critical skill**: Correctly handling the BOUNDARIES of the simulation (when to stop, edge cases, off-by-one errors).

**Example problems** (from USACO):
- "Counting Liars" — sort then linear scan
- "Fence Painting" — array update simulation
- Many early USACO bronze problems

**Practice**: 20 simulation problems from USACO bronze archive + [CSES Intro section](https://cses.fi/problemset/list/)

**Where to practice**:
- USACO Guide Bronze → Simulation module: https://usaco.guide/bronze/simulation
- "Fence Painting" (2016 November Bronze) — direct simulation on array
- "Mad Scientist" (2020 February Bronze) — BFS-based simulation
- CSES Introductory section: https://cses.fi/problemset/list/ (all 14 problems)

---

### Module 3 — Sorting & Sets (Week 2–3)

**Sorting with STL**
```cpp
// Sort array
int a[] = {5, 3, 1, 4};
sort(a, a+4); // {1, 3, 4, 5}

// Sort vector of pairs by first element, then second
vector<pair<int,int>> v;
sort(v.begin(), v.end()); // sort by first, break ties by second automatically

// Custom: sort by second element descending
sort(v.begin(), v.end(), [](auto& a, auto& b){
    return a.second > b.second;
});
```

**Sets and Maps**
```cpp
set<int> s;
s.insert(x); s.erase(x); s.count(x); // O(log n) each
// lower_bound/upper_bound
auto it = s.lower_bound(5); // first element >= 5

map<int,int> m;
m[key]++; m.count(key); // O(log n)
// Iterate in sorted order automatically
for (auto [k,v] : m) cout << k << " " << v << "\n";
```

**When to use**: Whenever you need to deduplicate, find rank, or look up membership efficiently.

**Practice**: 15 problems involving sorting or sets

---

### Module 4 — Complete Search (Weeks 3–4)

**What it is**: Enumerate all possible solutions and check each one. Works when the search space is small enough (brute force).

**Patterns**:

*Iterating over pairs/triples*:
```cpp
// Check all pairs i < j
for (int i = 0; i < n; i++)
    for (int j = i+1; j < n; j++)
        check(i, j); // O(n²)
```

*Generating all subsets*:
```cpp
for (int mask = 0; mask < (1<<n); mask++) {
    // bit i is in subset if (mask >> i) & 1
    for (int i = 0; i < n; i++)
        if ((mask >> i) & 1) use(a[i]);
}  // O(2^n * n) — only feasible for n ≤ 20
```

*Generating all permutations*:
```cpp
vector<int> perm = {0,1,2,3};
do {
    check(perm);
} while (next_permutation(perm.begin(), perm.end())); // O(n! * n)
```

**Recursion (backtracking)**:
```cpp
void solve(int pos, vector<int>& current) {
    if (pos == n) { check(current); return; }
    for (int choice : choices) {
        current.push_back(choice);
        solve(pos+1, current);
        current.pop_back(); // undo
    }
}
```

**Time budget awareness**: n ≤ 8 → permutations OK (8! = 40320). n ≤ 20 → bitmask subsets OK (2^20 ≈ 10^6). n ≤ 5000 → O(n²) OK.

**Practice**: 25 complete search problems from USACO and Codeforces

**Where to practice**:
- USACO Guide Bronze → Complete Search module: https://usaco.guide/bronze/intro-complete
- "Tricky Triplet" (2018 January Bronze) — O(n³) brute force over triples
- "Counting Liars" (2022 December Bronze) — try each possible median
- CSES: "Apple Division" https://cses.fi/problemset/task/1623 — subset enumeration

---

### Module 5 — Rectangle Geometry (Week 4)

**What it is**: Problems involving 2D coordinates, bounding boxes, and areas. USACO Bronze has a dedicated geometry module because coordinate manipulation is a fundamental implementation skill.

**Core patterns**:

*Rectangle containment*: Does point (x, y) lie inside rectangle (x1,y1)–(x2,y2)?
```cpp
bool inside(int x, int y, int x1, int y1, int x2, int y2) {
    return x >= x1 && x <= x2 && y >= y1 && y <= y2;
}
```

*Rectangle intersection*: Do two rectangles overlap?
```cpp
// Rect A: (ax1,ay1)–(ax2,ay2), Rect B: (bx1,by1)–(bx2,by2)
bool intersects(int ax1,int ay1,int ax2,int ay2,
                int bx1,int by1,int bx2,int by2) {
    return ax1 < bx2 && bx1 < ax2 && ay1 < by2 && by1 < ay2;
}
// Intersection rectangle (if it exists):
int ix1=max(ax1,bx1), iy1=max(ay1,by1);
int ix2=min(ax2,bx2), iy2=min(ay2,by2);
// Area = (ix2-ix1)*(iy2-iy1) if ix2>ix1 && iy2>iy1, else 0
```

*Area of union of two rectangles* (inclusion-exclusion):
```cpp
long long area_union = area(A) + area(B) - area(intersection(A,B));
```

**Implementation pitfalls**:
- Use `long long` when coordinates reach 10^6 (area can be 10^12)
- Inclusive vs. exclusive bounds: a rectangle described as "corners at (1,1) and (3,3)" — is the width 2 or 3? Read the problem carefully.
- Grid problems often use 0-indexed vs. 1-indexed coordinates — pick one and be consistent

**Where to practice**:
- USACO Guide Bronze → Rect Geometry module: https://usaco.guide/bronze/rect-geometry
- All "Recommended" problems in that module (typically 3–5 problems)

**Practice target**: 8–10 rectangle geometry problems

---

### Module 6 — Ad Hoc / Implementation (Week 5)

**What it is**: Problems with no named algorithm — just careful problem reading and code. The "implementation skill" is the algorithm.

**Archetypal patterns** (memorize these; they recur constantly):

*Circular array traversal*: When index wraps around, use modular arithmetic.
```cpp
// Next element after index i in a circular array of size n
int next_idx = (i + 1) % n;
// k steps forward from i
int after_k = (i + k) % n;
```

*Parity / alternating conditions*:
```cpp
// Process elements differently based on odd/even position
for (int i = 0; i < n; i++) {
    if (i % 2 == 0) do_even(a[i]);
    else            do_odd(a[i]);
}
```

*String character counting*:
```cpp
int freq[26] = {};
for (char c : s) freq[c - 'a']++;
// Check if all characters are unique:
bool all_unique = (*max_element(freq, freq+26) == 1);
```

*Simulation with multiple states*: Track every entity's state explicitly. Never try to be clever — just store it all.
```cpp
struct Cow { int x, y, direction; };
vector<Cow> cows(n);
for (int step = 0; step < T; step++) {
    for (auto& c : cows) c = simulate_one_step(c);
}
```

*Off-by-one in ranges*: When a problem says "days 3 through 7", that's 5 days (3,4,5,6,7). Always count on paper.

**Key skill**: Translate the problem statement word-for-word into code. Don't look for cleverness — correctness is the goal.

**Where to practice**:
- USACO Guide Bronze → Ad Hoc module: https://usaco.guide/bronze/ad-hoc
- All "Recommended" + "Normal" problems in that module

**Practice target**: 20 ad hoc USACO bronze problems

---

### Module 7 — Greedy Algorithms (Week 5–6)

**What it is**: Making the locally optimal choice at each step, leading to the global optimum.

**Classic greedy patterns at bronze**:
- Sort by some key then process in order
- Interval problems: "finish earliest deadline first"
- Assign resources greedily

**When to trust greedy**: When you can prove (informally) that the greedy choice never makes things worse, or exchange argument works.

**Warning**: Greedy problems are often tricky because wrong greedy choices look plausible. When in doubt, test with examples.

**Practice**: 15 greedy USACO bronze problems

**Where to practice**:
- USACO Guide Bronze → Greedy module: https://usaco.guide/bronze/intro-greedy
- "Cow Tipping" (2014 November Bronze) — classic greedy on grid
- "Why Did the Cow Cross the Road" (2017 February Bronze) — sorting + greedy

---

### Module 8 — Binary Search (Week 6)

**Binary search on sorted array**:
```cpp
// Find first position where a[pos] >= target
int lo = 0, hi = n;
while (lo < hi) {
    int mid = (lo + hi) / 2;
    if (a[mid] >= target) hi = mid;
    else lo = mid + 1;
}
// lo = answer
// Equivalent: lower_bound(a, a+n, target) - a
```

**Binary search on the answer** (most powerful use):
```cpp
// If check(x) is monotone (false, false, ..., true, true)
// Find smallest x where check(x) == true
int lo = 0, hi = 1e9;
while (lo < hi) {
    int mid = (lo + hi) / 2;
    if (check(mid)) hi = mid;
    else lo = mid + 1;
}
// lo = answer
```

**Practice**: 10 binary search problems

---

## Weekly Schedule

| Day | Activity | Duration |
|-----|----------|----------|
| Monday | USACO Guide module reading + implement code template from scratch | 2.5h |
| Tuesday | USACO Guide module: solve all 'Recommended' problems, then 'Normal' | 2.5h |
| Wednesday | CSES Introductory section + mixed bronze practice | 2h |
| Thursday | Codeforces Div. 3 / Div. 2 A–B problems (rated 800–1200) | 2.5h |
| Friday | 5–8 problems | 2h |
| Saturday | Full 4-hour contest simulation (past USACO bronze) | 4h |
| Sunday | Read editorial for every problem you didn't solve; re-implement from scratch | 1.5h |

**Total**: ~17 hours/week

---

## Monthly Goals

### Month 1
- Complete Modules 1–4
- Solve 80+ problems
- Complete all [CSES Introductory Problems](https://cses.fi/problemset/list/) (14 problems)
- First practice contest simulation

### Month 2
- Complete Modules 5–8
- Solve 80+ problems  
- Simulate 3+ full USACO bronze contests (past contests, timed)
- Consistently solving all 3 problems in simulations

### Month 3 (if needed)
- Solve silver-level problems (early silver topics)
- Reinforce weak areas from simulations
- Target: 150+ total bronze problems solved

---

## Contest Execution Strategy

**Before the 4-hour clock starts**:
- Template ready with file I/O set up
- Scratch paper ready

**First 15 minutes**:
- Read ALL 3 problems
- Estimate difficulty: which is easiest? Mark it.
- Note the constraints on each problem (determines algorithm complexity)

**Problem-solving loop** (per problem):
1. Re-read carefully — understand EXACTLY what's being asked
2. Work through the provided sample on paper first
3. Think about algorithm — what's the time complexity with your approach?
4. Code it up cleanly
5. Test on the provided sample (must match before submitting)
6. Think of edge cases: n=1, all same values, minimum/maximum values
7. Submit

**USACO-specific**: You can submit multiple times! Submit a partial solution early, then improve. A wrong submission costs nothing (no penalty).

**Time management**:
```
0:00–0:15  Read all problems, plan order
0:15–1:00  Solve easiest problem (45 min max)
1:00–2:15  Solve medium problem (75 min max)
2:15–3:30  Solve hard problem (75 min)
3:30–4:00  Buffer: debug, improve, check edge cases
```

---

## Common Bronze Mistakes to Avoid

1. **Integer overflow**: Use `long long` when products or sums might exceed 2×10^9
2. **Off-by-one errors**: Especially in sorting/binary search; test n=1, n=2 cases
3. **Wrong output format**: Check if newline at end is needed, check number formatting
4. **Forgetting to reset state**: If multiple test cases, reset all arrays/variables
5. **Not reading constraints**: Missed constraint leads to wrong algorithm
6. **Overcomplicating**: Bronze problems have simple solutions; if yours is complex, reconsider

---

## Promotion Checklist

Before attempting Silver, verify you can:
- [ ] Solve any bronze problem in under 45 minutes
- [ ] Implement STL sort with custom comparators correctly every time
- [ ] Use `map` and `set` fluently for lookups and ordering
- [ ] Write recursive backtracking (complete search) correctly
- [ ] Understand when brute force fits within time limits (complexity analysis)
- [ ] Score 750+/1000 on at least 2–3 simulated bronze contests

---

## Transitioning to Silver

**When you're ready**: You consistently solve all 3 Bronze problems in under 3 hours during simulated contests. Don't wait for a real contest promotion — start Silver material once your simulations are consistently strong.

**How to transition**: In your final Bronze month, begin reading USACO Guide Silver Module 1 (Prefix Sums) and Module 2 (Two Pointers) in parallel with Bronze practice. These topics do NOT require Bronze completion as a prerequisite — they're just harder.

**What changes at Silver**:
- Problems stop being solvable by "simulate what the problem says"
- You must recognize which algorithm applies — O(n²) brute force won't fit in time
- Reading constraints and immediately mapping them to algorithm complexity becomes mandatory

---

## Resources

| Resource | URL | Use Case |
|----------|-----|---------|
| USACO Guide Bronze | https://usaco.guide/bronze/ | Primary curriculum |
| USACO Past Contests | https://usaco.org/index.php?page=contests | Practice problems |
| CSES Intro Problems | https://cses.fi/problemset/list/ | Canonical practice |
| CP-Algorithms | https://cp-algorithms.com/ | Algorithm reference |
| Codeforces Div. 3 | https://codeforces.com/problemset | A/B problems (rated 800–1200) |
| LearnCpp | https://www.learncpp.com/ | C++ language reference |

---

*Primary source: usaco.guide — the authoritative USACO preparation guide*
