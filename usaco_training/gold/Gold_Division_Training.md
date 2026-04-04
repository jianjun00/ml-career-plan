# Gold Division Training Plan
## 8–12 Month Path to USACO Platinum

---

## Objectives

**Target Score**: ~750/1000 (promotion threshold)
**Duration**: 8–12 months
**Time Commitment**: 20 hours/week
**Primary Resources**: [USACO Guide Gold](https://usaco.guide/gold/) + [CSES Problem Set](https://cses.fi/problemset/)

### What Gold Tests
Gold is where competitive programming becomes genuinely difficult. Problems require sophisticated data structures (segment trees with lazy propagation), advanced graph algorithms (Dijkstra, network flow, SCC), and complex DP optimization. Many gold problems combine 2–3 techniques. The gap between Silver and Gold is the largest of any division jump.

**Language**: C++17 is effectively required. Java/Python solutions consistently TLE on Gold problems with n ≤ 200,000.

---

## Entry Point

**Coming from Silver**: Start at Module 1 (Segment Trees). This is the single most important Gold data structure — it appears in ~40% of Gold problems.

**Already comfortable with segment trees?** Take this check: can you implement a lazy propagation segment tree (range add, range sum) from memory in under 20 minutes? If yes, start at Module 4 (Shortest Paths).

**Codeforces rating 2000+?** Skim Modules 1–4 and start at Module 5 (Advanced Graph Theory).

---

## Topic Priority

Gold topics are NOT equal in frequency. This order reflects how often each topic appears in USACO Gold problems:

1. **Segment Trees** — ~40% of Gold problems require a segment tree or BIT
2. **Shortest Paths** (Dijkstra) — ~30% of Gold problems involve weighted graphs
3. **Advanced DP** — ~35% of Gold problems have a DP component
4. **Graph Theory** (SCC, bridges, topological sort) — ~25%
5. **Network Flow** — ~10%, but high-difficulty problems
6. **String Algorithms** — ~10%
7. **Monotone Stack / Sparse Table / SQRT** — ~15%

Start with Modules 1–3 before touching anything else.

---

## Gold Curriculum

### Module 1 — Segment Trees (Months 1–2)

The most important Gold data structure. Master this before anything else.

#### 1A. Basic Segment Tree (Point Update, Range Query)

```cpp
// Iterative segment tree — clean and fast
struct SegTree {
    int n;
    vector<long long> tree;
    SegTree(int n) : n(n), tree(2*n, 0) {}

    void update(int i, long long val) { // set tree[i] = val (point update)
        for (tree[i+=n] = val; i > 1; i >>= 1)
            tree[i>>1] = tree[i] + tree[i^1];
    }

    long long query(int l, int r) { // sum query [l, r) — half-open
        long long res = 0;
        for (l+=n, r+=n; l < r; l>>=1, r>>=1) {
            if (l&1) res += tree[l++];
            if (r&1) res += tree[--r];
        }
        return res;
    }
};
// For min/max: replace + with min/max, initialize with INF/0
```

#### 1B. Segment Tree with Lazy Propagation (Range Update, Range Query)

The standard Gold-level segment tree. Supports: add v to all elements in [l,r], query sum of [l,r].

```cpp
struct LazySegTree {
    int n;
    vector<long long> tree, lazy;
    LazySegTree(int n) : n(n), tree(4*n, 0), lazy(4*n, 0) {}

    void push_down(int node, int l, int r) {
        if (lazy[node]) {
            int mid = (l+r)/2;
            tree[2*node]   += lazy[node] * (mid-l+1);
            lazy[2*node]   += lazy[node];
            tree[2*node+1] += lazy[node] * (r-mid);
            lazy[2*node+1] += lazy[node];
            lazy[node] = 0;
        }
    }

    void update(int node, int l, int r, int ql, int qr, long long val) {
        if (qr < l || r < ql) return;
        if (ql <= l && r <= qr) { tree[node] += val*(r-l+1); lazy[node] += val; return; }
        push_down(node, l, r);
        int mid = (l+r)/2;
        update(2*node, l, mid, ql, qr, val);
        update(2*node+1, mid+1, r, ql, qr, val);
        tree[node] = tree[2*node] + tree[2*node+1];
    }

    long long query(int node, int l, int r, int ql, int qr) {
        if (qr < l || r < ql) return 0;
        if (ql <= l && r <= qr) return tree[node];
        push_down(node, l, r);
        int mid = (l+r)/2;
        return query(2*node,l,mid,ql,qr) + query(2*node+1,mid+1,r,ql,qr);
    }
    // Call: update(1,0,n-1,l,r,val); query(1,0,n-1,l,r);
};
```

#### 1C. Common Lazy Patterns

| Operation | tree stores | lazy stores | push_down rule |
|-----------|-------------|-------------|----------------|
| Range add, range sum | sum of range | pending addition | tree[child] += lazy*len; lazy[child] += lazy |
| Range assign, range sum | sum of range | pending assignment (-1 = none) | tree[child] = lazy*len; lazy[child] = lazy |
| Range add, range max | max of range | pending addition | tree[child] += lazy; lazy[child] += lazy |

**Worked example** — USACO 2016 November Gold "Mowing the Field":
> Farmer has a field, sprinkler waters intervals. After Q add-water events on [l,r], query which cells were watered at least twice.
> Maintain a segment tree where tree[i] = number of times cell i was watered. Range-add 1 to [l,r] for each event. Count cells with value ≥ 2.
> Lazy propagation makes each update O(log n) instead of O(n). Total: O(Q log N).

**Where to practice**:
- USACO Guide Gold → Segment Trees: https://usaco.guide/gold/seg-ext
- CSES Range Queries section: https://cses.fi/problemset/list/ (all 10 problems)
  - Start with: "Range Sum Queries II", "Range Minimum Queries II", "Range Update Queries"
- USACO Gold archive problems tagged "segment tree"

**Practice target**: 25 segment tree problems

---

### Module 2 — Fenwick Trees / BIT (Month 2)

BIT is faster to code than a segment tree when you only need point updates and prefix sum queries. Learn both — segment tree is more general, BIT is more efficient.

```cpp
struct BIT {
    int n; vector<long long> tree;
    BIT(int n) : n(n), tree(n+1, 0) {}
    void update(int i, long long v) { for (i++; i <= n; i += i&-i) tree[i] += v; }
    long long query(int i) { long long s=0; for (i++; i > 0; i -= i&-i) s += tree[i]; return s; }
    long long query(int l, int r) { return query(r) - (l > 0 ? query(l-1) : 0); }
    // Point update + prefix/range sum: O(log n) per operation
};
```

**BIT for order statistics** (count of elements ≤ x after coordinate compression):
```cpp
// After compressing values to [0, MAXV]:
// Insert value x: bit.update(x, +1)
// Count values in [lo, hi]: bit.query(lo, hi)
// Find k-th smallest: binary search on prefix sums
```

**2D BIT** for rectangle sum queries with point updates:
```cpp
struct BIT2D {
    int n, m; vector<vector<long long>> tree;
    BIT2D(int n, int m) : n(n), m(m), tree(n+1, vector<long long>(m+1, 0)) {}
    void update(int x, int y, long long v) {
        for (int i=x+1; i<=n; i+=i&-i)
            for (int j=y+1; j<=m; j+=j&-j) tree[i][j] += v;
    }
    long long query(int x, int y) {
        long long s=0;
        for (int i=x+1; i>0; i-=i&-i)
            for (int j=y+1; j>0; j-=j&-j) s += tree[i][j];
        return s;
    }
};
```

**Worked example** — CSES "Inversion Count":
> Count pairs (i, j) where i < j and a[i] > a[j].
> Coordinate-compress a[] to [0, n-1]. Process elements left to right. For each a[i], query BIT for how many values > a[i] have already been inserted (= inversions ending at i). Then insert a[i] into BIT.
> Each insert/query is O(log n). Total: O(n log n).

**Where to practice**:
- CSES "Dynamic Range Sum Queries": https://cses.fi/problemset/task/1648
- CSES "Inversion Count" (coordinate compress + BIT): https://cses.fi/problemset/task/1082
- USACO Guide Gold → Point Update Range Sum: https://usaco.guide/gold/PURS

**Practice target**: 12 BIT problems

---

### Module 3 — Shortest Path Algorithms (Month 2–3)

#### 3A. Dijkstra (Non-negative weights) — O((V + E) log V)

The most important shortest path algorithm. Memorize this implementation.

```cpp
vector<long long> dijkstra(int src, vector<vector<pair<int,int>>>& adj, int n) {
    // adj[u] = list of {v, weight}
    vector<long long> dist(n, LLONG_MAX);
    priority_queue<pair<long long,int>, vector<pair<long long,int>>, greater<>> pq;
    dist[src] = 0; pq.push({0, src});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue; // stale entry — skip
        for (auto [v, w] : adj[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
    return dist; // dist[i] = shortest distance from src to i
}
```

**Common mistake**: forgetting `if (d > dist[u]) continue` — this causes TLE because stale entries get processed.

#### 3B. Bellman-Ford (negative edge weights, no negative cycles) — O(VE)

```cpp
vector<long long> bellman_ford(int src, vector<tuple<int,int,int>>& edges, int n) {
    // edges = list of {u, v, weight}
    vector<long long> dist(n, LLONG_MAX);
    dist[src] = 0;
    for (int i = 0; i < n-1; i++) // relax n-1 times
        for (auto [u, v, w] : edges)
            if (dist[u] != LLONG_MAX && dist[u] + w < dist[v])
                dist[v] = dist[u] + w;
    // Check for negative cycle: if any edge still relaxes, negative cycle exists
    return dist;
}
```

#### 3C. Floyd-Warshall (all-pairs) — O(V³), use when V ≤ 500

```cpp
// Initialize: dist[i][j] = weight of edge i→j (INF if no edge), dist[i][i] = 0
for (int k = 0; k < n; k++)
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            if (dist[i][k] != INF && dist[k][j] != INF)
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
```

#### 3D. 0-1 BFS (edges weight 0 or 1) — O(V+E), faster than Dijkstra for this case

```cpp
// Use deque: push weight-0 edges to front, weight-1 edges to back
deque<int> dq;
vector<int> dist(n, INT_MAX);
dist[src] = 0; dq.push_front(src);
while (!dq.empty()) {
    int u = dq.front(); dq.pop_front();
    for (auto [v, w] : adj[u]) {
        if (dist[u] + w < dist[v]) {
            dist[v] = dist[u] + w;
            if (w == 0) dq.push_front(v); else dq.push_back(v);
        }
    }
}
```

**Worked example** — USACO 2015 February Gold "Fencing the Cows":
> Cows move between fields connected by roads with given distances. Find shortest time for a cow to travel from field S to field T.
> Classic Dijkstra: build adjacency list of {neighbor, distance}, run Dijkstra from S, answer is dist[T].

**Where to practice**:
- CSES Shortest Paths section: https://cses.fi/problemset/list/ (all 12 problems — do them all)
  - "Shortest Routes I" (Dijkstra), "Shortest Routes II" (Floyd-Warshall), "High Score" (Bellman-Ford with negative edges)
- USACO Guide Gold → Shortest Paths: https://usaco.guide/gold/shortest-paths

**Practice target**: 20 shortest path problems

---

### Module 4 — Minimum Spanning Tree (Month 3)

#### Kruskal's Algorithm (standard — O(E log E))

```cpp
// Sort edges by weight; use DSU to add edge if it connects two different components
sort(edges.begin(), edges.end()); // sort by weight (first element of tuple)
DSU dsu(n);
long long mst_cost = 0;
vector<pair<int,int>> mst_edges;
for (auto [w, u, v] : edges) {
    if (dsu.unite(u, v)) {
        mst_cost += w;
        mst_edges.push_back({u, v});
    }
}
// If mst_edges.size() < n-1, graph is disconnected
```

#### Prim's Algorithm (O(E log V) with priority queue, better for dense graphs)

```cpp
// Grow MST greedily: maintain a priority queue of (edge_weight, node)
// representing the cheapest edge to add each unvisited node
vector<long long> key(n, LLONG_MAX);
vector<bool> inMST(n, false);
priority_queue<pair<long long,int>, vector<pair<long long,int>>, greater<>> pq;
key[0] = 0; pq.push({0, 0});
long long mst_cost = 0;

while (!pq.empty()) {
    auto [d, u] = pq.top(); pq.pop();
    if (inMST[u]) continue;
    inMST[u] = true; mst_cost += d;
    for (auto [v, w] : adj[u]) {
        if (!inMST[v] && w < key[v]) {
            key[v] = w; pq.push({w, v});
        }
    }
}
```

**When to use which**: For sparse graphs (E ≈ V log V), both are similar. For dense graphs (E ≈ V²), Prim's with an adjacency matrix is O(V²) which beats Kruskal's O(E log E).

**Worked example** — USACO 2018 February Gold "Taming the Herd":
> N towns connected by N-1 roads forming a tree. Some roads have travel costs. You must connect all towns at minimum total cost using exactly N-1 of the given M edges (Kruskal's directly).
> Sort all M edges by weight. Run Kruskal's: use DSU to greedily pick the cheapest edge that doesn't form a cycle. Stop after n-1 edges are picked. Sum of those edges = MST cost.

**Where to practice**:
- CSES "Road Reparation": https://cses.fi/problemset/task/1675
- CSES "Road Construction": https://cses.fi/problemset/task/1676
- USACO Guide Gold → Minimum Spanning Tree: https://usaco.guide/gold/mst

**Practice target**: 10 MST problems

---

### Module 5 — Advanced Graph Theory (Month 3–4)

#### 5A. Topological Sort

```cpp
// Kahn's algorithm (BFS-based): O(V+E)
// Returns topological order; if order.size() < n, cycle exists
vector<int> topo_sort(vector<vector<int>>& adj, int n) {
    vector<int> indegree(n, 0);
    for (int u = 0; u < n; u++) for (int v : adj[u]) indegree[v]++;
    queue<int> q;
    for (int i = 0; i < n; i++) if (!indegree[i]) q.push(i);
    vector<int> order;
    while (!q.empty()) {
        int u = q.front(); q.pop(); order.push_back(u);
        for (int v : adj[u]) if (--indegree[v] == 0) q.push(v);
    }
    return order;
}
```

#### 5B. Strongly Connected Components — Tarjan's Algorithm O(V+E)

```cpp
// Outputs SCCs; comp[u] = SCC id of node u
// SCCs are output in reverse topological order of the condensation DAG
int num_scc = 0, timer_val = 0;
vector<int> ord(n,-1), low(n), comp(n,-1);
vector<bool> on_stk(n, false);
stack<int> stk;

function<void(int)> tarjan = [&](int u) {
    ord[u] = low[u] = timer_val++;
    stk.push(u); on_stk[u] = true;
    for (int v : adj[u]) {
        if (ord[v] == -1) { tarjan(v); low[u] = min(low[u], low[v]); }
        else if (on_stk[v]) low[u] = min(low[u], ord[v]);
    }
    if (low[u] == ord[u]) { // u is root of an SCC
        while (true) {
            int v = stk.top(); stk.pop(); on_stk[v] = false; comp[v] = num_scc;
            if (v == u) break;
        }
        num_scc++;
    }
};
for (int i = 0; i < n; i++) if (ord[i] == -1) tarjan(i);
```

#### 5C. Bridges and Articulation Points — O(V+E)

```cpp
// Bridge: edge whose removal disconnects the graph
// low[u] = min discovery time reachable from subtree of u
vector<int> disc(n,-1), low(n); int t=0;
vector<bool> is_bridge(E, false); // mark edge indices

function<void(int,int)> dfs = [&](int u, int par_edge) {
    disc[u] = low[u] = t++;
    for (auto [v, eid] : adj[u]) { // adj stores {neighbor, edge_id}
        if (disc[v] == -1) {
            dfs(v, eid);
            low[u] = min(low[u], low[v]);
            if (low[v] > disc[u]) is_bridge[eid] = true;
        } else if (eid != par_edge) low[u] = min(low[u], disc[v]);
    }
};
```

#### 5D. Euler Tour on Trees

Flattens a tree into an array: subtree of node u occupies interval [in[u], out[u]].

```cpp
int timer = 0;
vector<int> in(n), out(n);
function<void(int,int)> dfs = [&](int u, int p) {
    in[u] = timer++;
    for (int v : adj[u]) if (v != p) dfs(v, u);
    out[u] = timer-1; // out[u] = last position in subtree
};
dfs(root, -1);
// Subtree of u = array positions [in[u], out[u]]
// Apply segment tree on the flattened array for subtree queries
```

**Worked example** — USACO 2012 March Gold "Grass Cownoisseur":
> Directed graph of fields. Cows can move along directed edges. After collapsing SCCs (each SCC = one super-node), determine if cow can return to start from every field.
> Run Tarjan's SCC to condense the graph. If the condensation DAG has any node with out-degree 0 that isn't the start's SCC, cow cannot return from those fields. Topological sort the condensation to reason about reachability.

**Where to practice**:
- CSES Graph Algorithms: https://cses.fi/problemset/list/ (all remaining problems after shortest paths)
  - "Course Schedule" (topological sort), "Strongly Connected Components", "Building Teams" (bipartite check)
- USACO Guide Gold → Graph section: https://usaco.guide/gold/toposort

**Practice target**: 25 advanced graph problems

---

### Module 6 — Network Flow (Month 4–5)

#### Dinic's Algorithm — O(V²E) general, O(E√V) unit graphs

The standard max flow algorithm for competitive programming.

```cpp
struct Dinic {
    struct Edge { int to, rev, cap; };
    vector<vector<Edge>> g; vector<int> level, iter;
    Dinic(int n) : g(n), level(n), iter(n) {}

    void add_edge(int u, int v, int cap) {
        g[u].push_back({v, (int)g[v].size(), cap});
        g[v].push_back({u, (int)g[u].size()-1, 0}); // reverse edge cap=0
    }

    bool bfs(int s, int t) {
        fill(level.begin(),level.end(),-1);
        queue<int> q; level[s]=0; q.push(s);
        while (!q.empty()) {
            int v=q.front(); q.pop();
            for (auto& e:g[v]) if (e.cap>0 && level[e.to]<0) { level[e.to]=level[v]+1; q.push(e.to); }
        }
        return level[t] >= 0;
    }

    int dfs(int v, int t, int f) {
        if (v==t) return f;
        for (int& i=iter[v]; i<(int)g[v].size(); i++) {
            auto& e=g[v][i];
            if (e.cap>0 && level[v]<level[e.to]) {
                int d=dfs(e.to,t,min(f,e.cap));
                if (d>0) { e.cap-=d; g[e.to][e.rev].cap+=d; return d; }
            }
        }
        return 0;
    }

    int max_flow(int s, int t) {
        int flow=0;
        while (bfs(s,t)) { fill(iter.begin(),iter.end(),0); int d; while((d=dfs(s,t,INT_MAX))>0) flow+=d; }
        return flow;
    }
};
```

**Flow applications**:
- **Bipartite matching**: source → left nodes (cap 1), left → right (cap 1), right → sink (cap 1). Max flow = max matching.
- **Min vertex cover**: By König's theorem, min vertex cover = max matching in bipartite graphs.
- **Project selection**: Split nodes into "profit" and "cost"; source/sink represent which project/resource to include.

**Worked example** — USACO 2011 March Gold "Dining":
> N cows, M hay bales, K pairs of (cow, bale) indicating a cow can eat that bale. Each cow eats at most 1 bale; each bale feeds at most 1 cow. Find maximum number of cows fed.
> This is bipartite matching: left = cows, right = bales, edges = preferences. Source → each cow (cap 1), each bale → sink (cap 1), cow → bale edges (cap 1). Run Dinic's. Max flow = max matching = max cows fed.

**Where to practice**:
- CSES Flow section: https://cses.fi/problemset/list/ (all 4 problems)
- USACO Guide Gold → Max Flow: https://usaco.guide/gold/max-flow
- USACO Gold archive problems tagged "flow"

**Practice target**: 15 flow problems

---

### Module 7 — Advanced Dynamic Programming (Months 5–7)

Now that you have the data structures from Modules 1–2, DP problems that require them become tractable.

#### 7A. DP on Trees (Re-rooting)

Compute an answer for every node as the root in O(n) total.

```cpp
// Phase 1: DFS downward — dp_down[u] = answer using only subtree of u
// Phase 2: DFS upward — dp_up[u] = contribution from "rest of tree" (everything not in subtree of u)
// Answer for rooting at u: f(dp_down[u], dp_up[u])
function<void(int,int)> dfs_down = [&](int u, int p) {
    dp_down[u] = base_case;
    for (int v : adj[u]) if (v != p) {
        dfs_down(v, u);
        dp_down[u] = combine(dp_down[u], dp_down[v]);
    }
};
function<void(int,int)> dfs_up = [&](int u, int p) {
    ans[u] = merge(dp_down[u], dp_up[u]);
    for (int v : adj[u]) if (v != p) {
        // dp_up[v] = contribution from u's subtree excluding v's subtree + dp_up[u]
        dp_up[v] = recompute_excluding(u, v, dp_down, dp_up[u]);
        dfs_up(v, u);
    }
};
```

#### 7B. Bitmask DP

For n ≤ 20, enumerate subsets as bitmasks.

```cpp
// TSP variant: dp[mask][u] = min cost visiting exactly the nodes in mask, ending at u
vector<vector<long long>> dp(1<<n, vector<long long>(n, LLONG_MAX));
dp[1<<src][src] = 0;
for (int mask = 0; mask < (1<<n); mask++)
    for (int u = 0; u < n; u++) if ((mask>>u)&1 && dp[mask][u] != LLONG_MAX)
        for (int v = 0; v < n; v++) if (!((mask>>v)&1))
            dp[mask|(1<<v)][v] = min(dp[mask|(1<<v)][v], dp[mask][u] + cost[u][v]);
```

#### 7C. Digit DP

Count integers in [0, N] satisfying a digit-based property.

```cpp
// dp[pos][tight][state]: count valid numbers of length pos
// tight: whether current prefix equals N's prefix (limits next digit choice)
// state: problem-specific (e.g., sum mod k, digit presence flags)
function<long long(int,bool,int)> solve = [&](int pos, bool tight, int state) -> long long {
    if (pos == digits.size()) return is_valid(state);
    if (memo[pos][tight][state] != -1) return memo[pos][tight][state];
    int limit = tight ? digits[pos] : 9;
    long long res = 0;
    for (int d = 0; d <= limit; d++)
        res += solve(pos+1, tight && (d==limit), next_state(state, d));
    return memo[pos][tight][state] = res;
};
```

#### 7D. SOS DP (Sum over Subsets)

```cpp
// f[mask] = sum of a[sub] for all sub that are subsets of mask
// O(n * 2^n) — runs in O(n * 2^n), not O(4^n)
for (int i = 0; i < n; i++)
    for (int mask = 0; mask < (1<<n); mask++)
        if ((mask >> i) & 1) f[mask] += f[mask ^ (1<<i)];
```

#### 7E. Matrix Exponentiation

Compute linear recurrences in O(k³ log n) using fast matrix power.

```cpp
using Matrix = vector<vector<long long>>;
const long long MOD = 1e9+7;

Matrix multiply(const Matrix& A, const Matrix& B) {
    int n = A.size(); Matrix C(n, vector<long long>(n, 0));
    for (int i=0;i<n;i++) for (int k=0;k<n;k++) if (A[i][k])
        for (int j=0;j<n;j++) C[i][j] = (C[i][j] + A[i][k]*B[k][j]) % MOD;
    return C;
}

Matrix matpow(Matrix A, long long p) {
    int n = A.size(); Matrix R(n, vector<long long>(n, 0));
    for (int i=0;i<n;i++) R[i][i] = 1; // identity
    for (; p > 0; p >>= 1) { if (p&1) R = multiply(R,A); A = multiply(A,A); }
    return R;
}
// For Fibonacci: [[1,1],[1,0]]^n gives F(n+1)
```

**Worked example** — USACO 2019 December Gold "Milk Visits":
> Tree with nodes colored W or H. Answer Q path queries: does path from u to v contain at least one node of color c?
> Use Euler tour + segment tree. Precompute for each path whether it contains W or H using tree traversal.
> This combines Euler tour (Module 5D) with segment tree (Module 1) — typical Gold combination.

**Where to practice**:
- CSES DP section: https://cses.fi/problemset/list/ (all 19 problems)
- AtCoder DP Contest: https://atcoder.jp/contests/dp/tasks (all 26 problems — do problems D, E, F, G, H, I, J, K, L, P, R, S, T)
- USACO Guide Gold → DP section: https://usaco.guide/gold/dp

**Practice target**: 50 DP problems

---

### Module 8 — String Algorithms (Month 7)

#### 8A. String Hashing (polynomial rolling hash)

```cpp
// Build hash of string s; query hash of any substring in O(1)
const long long BASE = 131, MOD = 1e18 + 9;
int n = s.size();
vector<long long> h(n+1,0), pw(n+1,1);
for (int i=0;i<n;i++) { h[i+1]=(h[i]*BASE+s[i])%MOD; pw[i+1]=pw[i]*BASE%MOD; }
auto get_hash = [&](int l, int r) { // hash of s[l..r] inclusive
    return (h[r+1] - h[l]*pw[r-l+1] % MOD + MOD*2) % MOD;
};
// Two substrings are equal iff their hashes match
// Use double hashing (two different MODs) to reduce collision probability
```

#### 8B. Z-Algorithm — O(n)

z[i] = length of longest prefix of s that matches starting at s[i].

```cpp
vector<int> z_function(const string& s) {
    int n = s.size(); vector<int> z(n, 0); z[0] = n;
    for (int i=1,l=0,r=0; i<n; i++) {
        if (i < r) z[i] = min(r-i, z[i-l]);
        while (i+z[i] < n && s[z[i]] == s[i+z[i]]) z[i]++;
        if (i+z[i] > r) { l=i; r=i+z[i]; }
    }
    return z;
}
// Pattern matching: run on (pattern + "#" + text)
// z[i] == pattern.size() iff text starting at (i - pattern.size() - 1) matches pattern
```

#### 8C. KMP (Knuth-Morris-Pratt) — O(|P| + |T|)

```cpp
// failure[i] = length of longest proper prefix of p[0..i] that is also a suffix
vector<int> kmp_failure(const string& p) {
    int n = p.size(); vector<int> f(n, 0);
    for (int i=1,j=0; i<n; i++) {
        while (j>0 && p[i]!=p[j]) j=f[j-1];
        if (p[i]==p[j]) j++;
        f[i] = j;
    }
    return f;
}
// Find all occurrences of pattern in text using failure function
```

**Worked example** — USACO 2018 January Gold "Cow at Large":
> Given a string s of length n and pattern p of length m, find all starting positions in s where p occurs.
> Use Z-algorithm: build t = p + "#" + s (length m+1+n). Compute z[] for t. Position i in s (= index m+1+i in t) is a match iff z[m+1+i] == m.
> Output all i where z[m+1+i] == m. O(n+m) time.

**Note on Aho-Corasick**: Multi-pattern matching using a trie + failure links. This algorithm appears occasionally at Gold but is more common at Platinum. If you have time, learn it; if not, deprioritize it relative to Modules 1–7.

**Where to practice**:
- CSES String Algorithms: https://cses.fi/problemset/list/ (first 8 problems)
  - "String Matching" (Z or KMP), "Finding Borders", "Finding Periods"
- USACO Guide Gold → String Hashing: https://usaco.guide/gold/hashing

**Practice target**: 15 string problems

---

### Module 9 — Monotone Stack, Sparse Table, DSU with Rollback (Month 8)

#### 9A. Monotone Stack

```cpp
// Nearest smaller element to the LEFT for each position
vector<int> left_smaller(n, -1);
stack<int> stk;
for (int i = 0; i < n; i++) {
    while (!stk.empty() && a[stk.top()] >= a[i]) stk.pop();
    if (!stk.empty()) left_smaller[i] = stk.top();
    stk.push(i);
}
// Compute right_smaller similarly (iterate right to left)
// Applications: largest rectangle in histogram, next greater element, stock span
```

#### 9B. Sparse Table (O(1) static range min/max)

```cpp
// Preprocessing O(n log n), query O(1) — only for IDEMPOTENT operations (min, max, GCD)
const int LOG = 20;
vector<vector<int>> sparse(LOG, vector<int>(n));
sparse[0] = a;
for (int j=1; j<LOG; j++)
    for (int i=0; i+(1<<j)<=n; i++)
        sparse[j][i] = min(sparse[j-1][i], sparse[j-1][i+(1<<(j-1))]);

auto rmq = [&](int l, int r) { // [l, r] inclusive, O(1)
    int k = __lg(r-l+1);
    return min(sparse[k][l], sparse[k][r-(1<<k)+1]);
};
```

#### 9C. DSU with Rollback (for offline dynamic connectivity)

Standard DSU with path compression cannot be rolled back. Use union by rank without path compression.

```cpp
struct RollbackDSU {
    vector<int> parent, rank;
    vector<pair<int*,int>> history; // {pointer, old_value}
    RollbackDSU(int n) : parent(n), rank(n,0) { iota(parent.begin(),parent.end(),0); }
    int find(int x) { return parent[x]==x ? x : find(parent[x]); } // NO path compression
    bool unite(int x, int y) {
        x=find(x); y=find(y); if(x==y) return false;
        if (rank[x]<rank[y]) swap(x,y);
        history.push_back({&parent[y], parent[y]});
        history.push_back({&rank[x], rank[x]});
        parent[y]=x; if(rank[x]==rank[y]) rank[x]++;
        return true;
    }
    void rollback() { // undo last unite
        for (int i=0;i<2;i++) { *history.back().first=history.back().second; history.pop_back(); }
    }
    int save() { return history.size(); }
    void restore(int checkpoint) { while((int)history.size()>checkpoint) { *history.back().first=history.back().second; history.pop_back(); } }
};
```

**Worked example** — USACO 2016 February Gold "Fencing the Cows":
> Array of N values. Answer Q queries: what is the minimum element in range [l, r]?
> Build sparse table in O(n log n): `sparse[j][i] = min(a[i..i+2^j-1])`. For query [l,r]: let k = floor(log2(r-l+1)), answer is `min(sparse[k][l], sparse[k][r-2^k+1])`. O(1) per query.

**Note on Mo's Algorithm**: Offline range queries in O((n+q)√n). This technique is rare at Gold (appears in ~2–3 Gold problems ever). It is more common at Platinum. If time is limited, deprioritize it.

**Where to practice**:
- CSES "Maximum Subarray Sum II" (monotone deque): https://cses.fi/problemset/task/1644
- CSES "Sliding Median" (two BITs or two heaps): https://cses.fi/problemset/task/1076
- USACO Guide Gold → Monotone Stack: https://usaco.guide/gold/stacks

**Practice target**: 12 problems

---

## Weekly Schedule

| Day | Activity | Duration |
|-----|----------|----------|
| Monday | USACO Guide module: read + implement algorithm from scratch (no copy-paste) | 3h |
| Tuesday | USACO Guide module: all "Recommended" problems, then "Normal" until stuck | 3h |
| Wednesday | CSES section matching current module (exact links above); submit to judge | 3h |
| Thursday | Codeforces Div. 1 B/C problems (rated 1800–2400) from problemset | 3h |
| Friday | USACO Gold archive: 3 past Gold problems, timed at 75 min each | 2.5h |
| Saturday | Full 4-hour timed USACO Gold contest simulation (pick a year from usaco.org) | 4h |
| Sunday | Read editorial for every problem you didn't solve; re-implement correct solution | 1.5h |

**Total**: ~20 hours/week

---

## Monthly Goals

| Month | Primary Topic | CSES Section | Problem Target |
|-------|--------------|--------------|---------------|
| 1 | Segment Trees (lazy) | Range Queries (all 10) | 30 |
| 2 | BIT + Shortest Paths | Shortest Paths (all 12) | 35 |
| 3 | MST + Advanced Graphs | Graph Algorithms remainder | 30 |
| 4–5 | Network Flow | Flows (all 4) | 25 |
| 5–7 | Advanced DP | DP section (all 19) + AtCoder DP contest | 60 |
| 7 | String Algorithms | String Algorithms (first 8) | 20 |
| 8 | Monotone Stack + Sparse Table | Remaining CSES | 20 |
| 9–12 | Weak area review + Platinum exposure | All 300 CSES problems done | 70 |

**Total**: 290+ Gold-level problems

---

## Contest Execution (Gold)

Gold problems are harder to categorize on sight. Constraints tell you the required complexity, but not always the algorithm. Strategy:

```
0:00–0:15  Read all 3 problems; note constraints for each
           n ≤ 200,000 → O(n log n) → likely seg tree, Dijkstra, or similar
           n ≤ 5,000 → O(n²) → likely O(n²) DP or brute force

0:15–1:15  Easiest problem (60 min max)
           — 10 min: understand + think algorithm
           — 35 min: implement
           — 15 min: test + debug

1:15–2:30  Medium problem (75 min)

2:30–3:45  Hard problem (75 min)
           If completely stuck after 30 min: implement O(n²) brute force
           for partial credit on small test cases

3:45–4:00  Improve partial solutions; double-check output format
```

**Partial credit is critical at Gold**: An O(n²) solution may score 300–500/1000 on small subtests. Always submit something rather than nothing.

---

## Promotion Checklist

Before advancing to Platinum, verify:
- [ ] Implement lazy propagation segment tree (range add + range sum) from memory in under 20 minutes
- [ ] Code Dijkstra correctly from memory (including stale-entry skip)
- [ ] Code Dinic's max flow correctly from memory
- [ ] Run Tarjan's SCC algorithm correctly
- [ ] Write DP with bitmask states
- [ ] Implement Z-algorithm + KMP for string matching from memory
- [ ] Score 750+/1000 on at least 2 simulated Gold contests
- [ ] Solved all 300 CSES problems (or within 10–20 of completion)

---

## Transitioning to Platinum

**When you're ready**: Consistently solve 2 of 3 Gold problems in simulations; Codeforces rating 2000+; all (or nearly all) CSES problems solved.

**What changes at Platinum**:
- Identifying the algorithm is the hard part, not implementing it
- Problems are designed to look unsolvable — the key insight is hidden
- Expect to spend 30–90 minutes thinking before writing a single line of code
- Segment trees, HLD, centroid decomposition, suffix arrays — you must implement all of these without referencing documentation

**First Platinum topics to start**: Begin with USACO Guide Platinum → Segment Tree Beats and CHT/Convex Hull Trick, as these are direct extensions of Gold skills.

---

## Resources

| Resource | URL | Use Case |
|----------|-----|---------|
| USACO Guide Gold | https://usaco.guide/gold/ | Primary curriculum |
| CSES Problem Set | https://cses.fi/problemset/ | Canonical problems (target: all 300) |
| CP-Algorithms | https://cp-algorithms.com/ | All algorithm implementations |
| Competitive Programmer's Handbook | https://cses.fi/book/ | Chapters 15–25 (free PDF) |
| Codeforces Div. 1 | https://codeforces.com/problemset | B/C problems (rated 2000–2400) |
| AtCoder ARC | https://atcoder.jp/contests/arc | C/D problems |
| AtCoder DP Contest | https://atcoder.jp/contests/dp/tasks | All 26 DP problems |

---

*Primary source: usaco.guide — the authoritative USACO preparation resource*
