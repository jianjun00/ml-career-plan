# Silver Division Training Plan
## 4–6 Month Path to USACO Gold

---

## Objectives

**Target Score**: ~750/1000 (promotion threshold)
**Duration**: 4–6 months
**Time Commitment**: 18 hours/week
**Primary Resource**: [USACO Guide Silver](https://usaco.guide/silver/)

### What Silver Actually Tests
Silver introduces real algorithmic techniques. You must know prefix sums, two pointers, binary search on answers, BFS/DFS on graphs, DSU, and introductory DP. Problems often require combining 2–3 techniques. Time complexity analysis is critical — O(n²) usually won't pass at Silver with n ≤ 200,000.

---

## Entry Point

**Coming from Bronze**: Start at Module 1. You should already be comfortable with STL (sort, map, set, vector), basic recursion, and complexity analysis.

**Already know prefix sums and two pointers?** Start at Module 5 (BFS/DFS). Check by solving [CSES Subarray Sums I](https://cses.fi/problemset/task/1660) and [CSES Subarray Sums II](https://cses.fi/problemset/task/1661) — if under 20 minutes, skip Modules 1–2.

**Codeforces rating 1400+?** Skim Modules 1–4, start at Module 5.

---

## Silver Curriculum (Per USACO Guide)

### Module 1 — Prefix Sums (Week 1)

**Why it matters**: Convert O(n) range sum queries to O(1) after O(n) preprocessing.

**1D Prefix Sum**:
```cpp
int a[] = {3, 1, 4, 1, 5, 9, 2};
int n = 7;
vector<int> pre(n+1, 0);
for (int i = 0; i < n; i++) pre[i+1] = pre[i] + a[i];

// Sum of a[l..r] (0-indexed, inclusive)
auto query = [&](int l, int r) { return pre[r+1] - pre[l]; };
```

**2D Prefix Sum**:
```cpp
// pre[i][j] = sum of rectangle (0,0) to (i-1,j-1)
for (int i = 1; i <= n; i++)
    for (int j = 1; j <= m; j++)
        pre[i][j] = a[i][j] + pre[i-1][j] + pre[i][j-1] - pre[i-1][j-1];

// Sum of rectangle (r1,c1) to (r2,c2) (1-indexed):
auto query2D = [&](int r1, int c1, int r2, int c2) {
    return pre[r2][c2] - pre[r1-1][c2] - pre[r2][c1-1] + pre[r1-1][c1-1];
};
```

**Difference Array** (range updates, point query):
```cpp
// Add v to all a[l..r]: diff[l] += v; diff[r+1] -= v;
// Recover a[i] = prefix sum of diff
```

**Practice**: 15 prefix sum problems from CSES + USACO

**Where to practice** (do in this order):
1. CSES "Subarray Sums I": https://cses.fi/problemset/task/1660
2. CSES "Subarray Sums II": https://cses.fi/problemset/task/1661
3. CSES "Subarray Divisibility": https://cses.fi/problemset/task/1662
4. USACO Guide Silver → Prefix Sums module (all "Recommended" problems): https://usaco.guide/silver/prefix-sums

**Worked example** — USACO 2016 November Silver "Counting Haybales":
> N haybales on positions 1..B, Q queries each asking "how many haybales in [L, R]?"
> Naive: O(N) per query = O(NQ) total → too slow for N,Q=100,000.
> Build prefix sum pre[i] = count of haybales in positions 1..i.
> Query [L,R] = pre[R] - pre[L-1] in O(1). Total: O(N + Q).

---

### Module 2 — Two Pointers & Sliding Window (Week 1–2)

**Two Pointers**: Maintain two indices `l` and `r` that both move rightward (total O(n) moves).

```cpp
// Find all pairs (i,j) with i < j and a[i]+a[j] == target in sorted array
// O(n) after O(n log n) sort
sort(a.begin(), a.end());
int l = 0, r = n-1;
while (l < r) {
    if (a[l] + a[r] == target) { use(l,r); l++; r--; }
    else if (a[l] + a[r] < target) l++;
    else r--;
}
```

**Sliding Window** (fixed or variable size):
```cpp
// Maximum sum subarray of size exactly k: O(n)
int sum = 0;
for (int i = 0; i < n; i++) {
    sum += a[i];
    if (i >= k) sum -= a[i-k];
    if (i >= k-1) ans = max(ans, sum);
}

// Smallest window with sum >= S (variable size): O(n)
int l = 0, sum = 0, ans = INT_MAX;
for (int r = 0; r < n; r++) {
    sum += a[r];
    while (sum >= S) {
        ans = min(ans, r - l + 1);
        sum -= a[l++];
    }
}
```

**Monotone Deque** (sliding window min/max):
```cpp
// Maximum in each window of size k: O(n)
deque<int> dq; // stores indices, front = maximum
for (int i = 0; i < n; i++) {
    while (!dq.empty() && a[dq.back()] <= a[i]) dq.pop_back();
    dq.push_back(i);
    if (dq.front() <= i - k) dq.pop_front();
    if (i >= k-1) ans[i-k+1] = a[dq.front()];
}
```

**Practice**: 15 two pointer / sliding window problems

**Where to practice**:
1. CSES "Apartments": https://cses.fi/problemset/task/1084
2. CSES "Sum of Two Values": https://cses.fi/problemset/task/1640
3. USACO Guide Silver → Two Pointers module: https://usaco.guide/silver/two-pointers

**Worked example** — USACO 2017 February Silver "Why Did the Cow Cross the Road II":
> N cows each want to eat grass in interval [a_i, b_i]. Find minimum number of cows to remove so no two remaining intervals overlap.
> Sort by right endpoint. Two pointers: for each cow, find how many previous cows have right endpoint < current left endpoint.
> This is the classic "interval scheduling" two-pointer pattern.

---

### Module 3 — Binary Search on Answers (Week 2)

**Distinction from Bronze**: Bronze introduced binary search on sorted arrays (`lower_bound`). Silver binary search is different — you binary search on the *answer* itself. The array may not be sorted; instead, you write a `check(x)` function that tests "is answer x feasible?" and binary search for the threshold.

**The key insight**: Many optimization problems ("find the minimum X such that...") have a monotone structure: if X is feasible, so is X+1. This makes binary search applicable even when there's no array to search.

```cpp
// Template: find minimum x such that check(x) is true
// check(x) must be monotone: false for x < ans, true for x >= ans

auto check = [&](long long mid) -> bool {
    // Can we achieve the goal if the answer is mid?
    // Return true if mid is feasible
};

long long lo = 0, hi = 1e9; // search bounds
while (lo < hi) {
    long long mid = lo + (hi - lo) / 2;
    if (check(mid)) hi = mid;
    else lo = mid + 1;
}
// lo = minimum feasible answer
```

**Classic problems**: "Minimize maximum distance," "Can we place k objects with minimum separation x?", "Binary search on time"

**Practice**: 20 binary search problems from USACO + CSES (sorting-and-searching section)

**Where to practice**:
1. CSES "Factory Machines": https://cses.fi/problemset/task/1620
2. CSES "Array Division": https://cses.fi/problemset/task/1085
3. USACO Guide Silver → Binary Search module: https://usaco.guide/silver/binary-search

**Worked example** — USACO 2016 February Silver "Fencing the Cows":
> N cows must be separated by at least D distance. Maximize D.
> check(D): sort cow positions, greedily place cows — place next cow only if distance from last ≥ D. If we can place all N, D is feasible.
> Binary search on D ∈ [0, max_position]. O(N log N) sort + O(N log(max_pos)) binary search.

---

### Module 4 — Custom Sorting & Coordinate Compression (Week 3)

**Custom Sort**:
```cpp
struct Interval { int l, r; };
vector<Interval> intervals;
// Sort by start time, break ties by end time descending
sort(intervals.begin(), intervals.end(), [](const Interval& a, const Interval& b){
    if (a.l != b.l) return a.l < b.l;
    return a.r > b.r;
});
```

**Coordinate Compression**: When values are large (up to 10^9) but count is small (n ≤ 10^5), compress them to [0, n-1].
```cpp
vector<int> vals = {100, 500, 3, 500, 100};
vector<int> sorted_unique = vals;
sort(sorted_unique.begin(), sorted_unique.end());
sorted_unique.erase(unique(sorted_unique.begin(), sorted_unique.end()), sorted_unique.end());

// Compress: original value → rank
auto compress = [&](int x) {
    return lower_bound(sorted_unique.begin(), sorted_unique.end(), x) - sorted_unique.begin();
};
```

**Worked example** — USACO 2016 February Silver "Milk Pails":
> You have n buckets with varying sizes. Find the pair of buckets whose sizes differ by the minimum amount.
> Coordinate-compress bucket sizes; sort them. Answer is `min(sorted[i+1] - sorted[i])` over all i.
> Custom sort puts them in order; the answer is a simple adjacent-difference scan.

**Practice**: 10 problems involving custom sorting or coordinate compression

---

### Module 5 — BFS, DFS, and Flood Fill (Week 3–4)

**BFS** (shortest path in unweighted graph, or level-by-level traversal):
```cpp
// BFS from source, returns distance to each node
vector<int> bfs(int src, vector<vector<int>>& adj, int n) {
    vector<int> dist(n, -1);
    queue<int> q;
    dist[src] = 0; q.push(src);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int v : adj[u]) {
            if (dist[v] == -1) { dist[v] = dist[u]+1; q.push(v); }
        }
    }
    return dist;
}
```

**DFS** (connectivity, cycle detection, topological sort foundations):
```cpp
// DFS on graph to find connected component
vector<bool> visited(n, false);
function<void(int)> dfs = [&](int u) {
    visited[u] = true;
    for (int v : adj[u]) if (!visited[v]) dfs(v);
};
dfs(0);
```

**Flood Fill** (BFS/DFS on grids):
```cpp
// 4-directional flood fill
int dx[] = {0,0,1,-1}, dy[] = {1,-1,0,0};
// BFS from (r,c)
queue<pair<int,int>> q;
visited[r][c] = true; q.push({r,c});
while (!q.empty()) {
    auto [x,y] = q.front(); q.pop();
    for (int d = 0; d < 4; d++) {
        int nx = x+dx[d], ny = y+dy[d];
        if (valid(nx,ny) && !visited[nx][ny]) {
            visited[nx][ny] = true; q.push({nx,ny});
        }
    }
}
```

**Multi-source BFS**: Start from multiple sources simultaneously (push all sources to queue with dist=0).

**Worked example** — USACO 2016 February Silver "Fenced In":
> Given a grid of fields separated by fences, some fences are removed. Find the number of connected components after removals.
> Map each field to a grid cell. BFS/DFS: from any unvisited cell, flood-fill through cells connected by removed fences. Each flood-fill call = one connected component.
> Key translation: "fields connected" means the fence between them is gone → model as graph edges, run flood fill.

**Practice**: 25 BFS/DFS problems from CSES (Graph Algorithms section, first 10 problems) + USACO silver

---

### Module 6 — Trees (Basic) (Week 4–5)

**Tree properties**: n nodes, n-1 edges, connected, acyclic. Root at node 0 or 1.

**DFS on trees** to compute subtree sizes, depths, parents:
```cpp
vector<int> parent(n,-1), depth(n,0), subtree_size(n,1);
function<void(int,int)> dfs = [&](int u, int p) {
    parent[u] = p;
    for (int v : adj[u]) {
        if (v != p) {
            depth[v] = depth[u]+1;
            dfs(v, u);
            subtree_size[u] += subtree_size[v];
        }
    }
};
dfs(0, -1);
```

**Common Silver tree problems**: subtree sums, path sums from root, finding the k-th ancestor, counting nodes at a given depth. These all use basic DFS — no advanced LCA needed.

**Lowest Common Ancestor (LCA)**: Finding LCA by walking both nodes up to the same depth, then stepping up together, is O(n) per query and sufficient for most Silver problems with small n. Binary lifting LCA (O(n log n) preprocessing, O(log n) query) is a **Gold-level technique** and not required for Silver.

```cpp
// Simple O(n) LCA by parent walking — sufficient for Silver
int lca_simple(int u, int v) {
    // Make u the deeper node
    while (depth[u] > depth[v]) u = parent[u];
    while (depth[v] > depth[u]) v = parent[v];
    while (u != v) { u = parent[u]; v = parent[v]; }
    return u;
}
```

**Where to practice**:
- CSES "Subordinates": https://cses.fi/problemset/task/1674
- CSES "Tree Distances I": https://cses.fi/problemset/task/1132
- USACO Guide Silver → Tree Algorithms module: https://usaco.guide/silver/dfs

**Worked example** — USACO 2019 February Silver "Sleepy Cow Sorting":
> Tree of n nodes. Each node has a value. For each node u, compute the sum of values in u's subtree.
> Run DFS from root. At each node, sum up `dp[v]` for all children v, then add the node's own value.
> `dp[u] = value[u] + sum(dp[v] for v in children(u))` — straightforward post-order DFS.

**Practice**: 12 tree problems — CSES Tree Algorithms section problems 1–6

---

### Module 7 — Disjoint Set Union / Union-Find (Week 5)

**DSU** answers: "Are nodes u and v in the same connected component?"

```cpp
struct DSU {
    vector<int> parent, rank;
    DSU(int n) : parent(n), rank(n,0) { iota(parent.begin(),parent.end(),0); }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]); // path compression
        return parent[x];
    }
    bool unite(int x, int y) {
        x = find(x); y = find(y);
        if (x == y) return false;
        if (rank[x] < rank[y]) swap(x,y);
        parent[y] = x;
        if (rank[x] == rank[y]) rank[x]++;
        return true;
    }
    bool same(int x, int y) { return find(x) == find(y); }
};
```

**Applications**: Kruskal's MST, offline connectivity queries, grouping equivalence classes.

**Worked example** — USACO 2016 January Silver "Moocast":
> N cows, each with a radio of range R. Find the minimum R such that all cows can communicate (directly or via chain).
> Binary search on R. For a given R, connect all pairs of cows within distance R. Check if the entire graph is one component using DSU: iterate edges in distance order, unite endpoints, stop when `dsu.find(0) == dsu.find(i)` for all i.
> This is Kruskal's MST core: union edges by weight, stop when connected.

**Practice**: 10 DSU problems

---

### Module 8 — Dynamic Programming (Introduction) (Weeks 5–6)

**Core idea**: Break problem into overlapping subproblems; memoize to avoid recomputation.

**1D DP — Longest Increasing Subsequence (LIS)**:
```cpp
// O(n²) DP: dp[i] = length of LIS ending at a[i]
vector<int> dp(n, 1);
for (int i = 1; i < n; i++)
    for (int j = 0; j < i; j++)
        if (a[j] < a[i]) dp[i] = max(dp[i], dp[j]+1);
// O(n log n) with patience sorting / binary search on tails
```

**2D DP — Grid Paths**:
```cpp
// dp[i][j] = number of paths to (i,j) from (0,0)
dp[0][0] = 1;
for (int i = 0; i < n; i++)
    for (int j = 0; j < m; j++)
        if (!blocked[i][j]) {
            if (i > 0) dp[i][j] += dp[i-1][j];
            if (j > 0) dp[i][j] += dp[i][j-1];
        }
```

**0/1 Knapsack**:
```cpp
// dp[w] = max value with weight capacity w
vector<int> dp(W+1, 0);
for (auto [weight, value] : items)
    for (int w = W; w >= weight; w--) // iterate backwards!
        dp[w] = max(dp[w], dp[w-weight] + value);
```

**DP state design**: The most important skill. Ask: what information defines a subproblem? That is your state.

**Worked example** — USACO 2016 March Silver "Landscaping":
> You have n items each with weight w[i] and value v[i]. Pick items with total weight ≤ W to maximize value.
> Classic 0/1 knapsack: `dp[j] = max value achievable with capacity j`. Iterate items outer, capacity inner (backwards).
> After filling the DP table, answer is `max(dp[0..W])`.

**Practice**: 30 DP problems — start with [AtCoder DP Contest](https://atcoder.jp/contests/dp/tasks) (problems A–E, J, K) + CSES DP section (first 10 problems)

---

### Module 9 — Greedy Algorithms (Advanced) (Week 6)

**Advanced greedy patterns at Silver**:

*Interval scheduling (maximize non-overlapping intervals)*:
Sort by end time. Greedily pick the interval that ends earliest, skip any overlapping with it, repeat.
```cpp
sort(intervals.begin(), intervals.end(), [](auto& a, auto& b){ return a.second < b.second; });
int count = 0, last_end = INT_MIN;
for (auto [l, r] : intervals) {
    if (l >= last_end) { count++; last_end = r; }
}
```

*Exchange argument* — how to prove greedy is optimal:
Suppose you have an optimal solution that differs from your greedy solution at some position. Show that swapping to match greedy doesn't make things worse (or strictly improves). If you can prove this for any adjacent swap, the greedy is globally optimal.

**Example**: Why does sorting by end time maximize non-overlapping intervals?
> Suppose greedy picks interval A (ends at 5) but optimal picks B (ends at 7) at the same step. Both start at ≥ current position. Everything compatible with B (starts after 7) is also compatible with A (starts after 5), but A additionally allows things starting in (5,7]. Swapping B→A can only enable more future choices. So greedy ≥ optimal. ∎

*Task assignment*: Assign n tasks to n machines to minimize makespan.
Sort tasks descending by duration; assign each task to the currently least-loaded machine (use a priority queue).

**Where to practice**:
- USACO Guide Silver → Greedy module: https://usaco.guide/silver/greedy-sorting
- "Paired Up" (2017 February Silver) — sort + greedy pairing
- CSES "Tasks and Deadlines": https://cses.fi/problemset/task/1630

**Practice**: 12 greedy problems

---

## Weekly Schedule

| Day | Activity | Duration |
|-----|----------|----------|
| Monday | Read USACO Guide module; implement the key algorithm from scratch without copy-pasting | 2.5h |
| Tuesday | USACO Guide module: all 'Recommended' problems, then start 'Normal' — stop when you're stuck more than 30 min | 3h |
| Wednesday | CSES section for current topic (exact links in each module above); submit to judge | 2.5h |
| Thursday | AtCoder ABC D/E problems (difficulty matched to current module) using Kenkoooo: https://kenkoooo.com/atcoder/ | 2.5h |
| Friday | AtCoder ABC / Codeforces D problems | 2.5h |
| Saturday | 4-hour timed USACO silver past contest | 4h |
| Sunday | Editorial review + re-implement solutions you missed | 1h |

**Total**: ~18 hours/week

---

## Monthly Goals

### Month 1 (Modules 1–4)
- Prefix sums + two pointers + binary search + sorting
- 90+ problems solved
- All CSES Sorting and Searching section (35 problems)

### Month 2 (Modules 5–7)
- BFS/DFS + trees + DSU
- 90+ problems solved
- CSES Graph Algorithms section: first 12 problems
- CSES Tree Algorithms section: first 8 problems

### Month 3 (Modules 8–9 + review)
- DP fundamentals + advanced greedy
- 80+ problems solved
- AtCoder DP Contest: problems A–K
- Simulate 4 silver contests; target 750+ on each

### Month 4–6 (if needed)
- Drill weak areas identified from simulations
- Begin gold-level topics (Segment Trees, Dijkstra)
- Target: 350+ total silver problems solved

---

## Contest Execution Strategy

**Silver difficulty insight**: Silver problems are harder to classify than Bronze. One problem may look like sorting but actually needs two pointers or DP. Don't assume — analyze constraints and think about what algorithm fits the time complexity.

**During contest**:
```
0:00–0:15  Read all 3 problems, estimate difficulty
0:15–1:15  Solve easiest (60 min max)
1:15–2:30  Solve medium (75 min)
2:30–3:45  Solve hard (75 min)
3:45–4:00  Buffer: debug, submit improvements
```

**Partial credit on USACO**: USACO scores each test case independently. If you have a brute force O(n²) solution that handles small test cases, submit it. You may get 30–60% of points, which counts toward your score.

**After WA on a submission**: Don't spam re-submit. Trace through your logic with the sample input on paper. Check edge cases (n=1, all zeros, maximum n). Use stress testing.

---

## Stress Testing at Silver Level

```bash
# gen.py generates random small test cases
# brute.cpp is your slow-but-correct O(n²) solution
# fast.cpp is your O(n log n) solution to test

while true; do
    python3 gen.py > test.in
    ./brute < test.in > expected.out
    ./fast < test.in > actual.out
    if ! diff -q expected.out actual.out > /dev/null; then
        echo "BUG FOUND!"
        cat test.in
        break
    fi
done
```

---

## Promotion Checklist

Before advancing to Gold, verify you can:
- [ ] Implement prefix sums (1D and 2D) from memory in under 5 minutes
- [ ] Write two pointers / sliding window correctly without referencing notes
- [ ] Implement BFS and DFS on both graphs and grids correctly
- [ ] Write DSU (union-find with path compression) from memory
- [ ] Solve binary search on answer problems: define check(x), set bounds, apply template
- [ ] Implement 0/1 knapsack DP correctly
- [ ] Score 750+/1000 on at least 2–3 simulated silver contests

---

## Transitioning to Gold

**When you're ready**: Consistently solve 2 of 3 Silver problems in under 2.5 hours in simulated contests, and have solved 200+ Silver-level problems total.

**Don't wait for real contest promotion**: Begin Gold material (Segment Trees, Dijkstra) in your last Silver month. These are purely additive — you can practice them without abandoning Silver.

**What changes at Gold**:
- Data structures become load-bearing: you can't solve most Gold problems without knowing segment trees
- A single problem may require combining 3 algorithms (e.g., BFS + segment tree + coordinate compression)
- Problem statements become ambiguous intentionally — you must figure out the correct interpretation from examples
- C++ is now effectively required; Java/Python consistently TLE

**First Gold topics to start**: Segment Trees (Module 1 of Gold plan) → BIT (Module 2). Master these before moving to DP and graphs.

---

## Resources

| Resource | URL | Use Case |
|----------|-----|---------|
| USACO Guide Silver | https://usaco.guide/silver/ | Primary curriculum |
| CSES Problem Set | https://cses.fi/problemset/ | Canonical practice problems |
| Competitive Programmer's Handbook | https://cses.fi/book/ | Free textbook, chapters 5–15 |
| AtCoder DP Contest | https://atcoder.jp/contests/dp/tasks | DP practice |
| CP-Algorithms | https://cp-algorithms.com/ | Algorithm implementations |
| Codeforces | https://codeforces.com/problemset | Div. 2 C/D (rated 1400–1900) |

---

*Primary source: usaco.guide — the authoritative USACO preparation resource*
