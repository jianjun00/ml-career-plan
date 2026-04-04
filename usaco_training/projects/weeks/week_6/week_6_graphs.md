# Week 6: Graph Algorithms — Dijkstra, Union-Find, Topological Sort
## Month 2 — Algorithm Development

**Goal**: Implement Dijkstra's algorithm, Union-Find, and topological sort. These are the 3 most important Silver graph algorithms.

---

## Learning Materials
- **USACO Guide Shortest Paths**: https://usaco.guide/silver/shortest-paths/
- **USACO Guide Union-Find**: https://usaco.guide/silver/dsu/
- **USACO Guide Topological Sort**: https://usaco.guide/silver/toposort/

---

## Core C++ to Know Cold

```cpp
#include <bits/stdc++.h>
using namespace std;

// === DIJKSTRA'S ALGORITHM (shortest paths, non-negative weights) ===
// Time: O((V + E) log V)
const int INF = 1e9;
typedef pair<int,int> pii;

vector<int> dijkstra(int src, int n, vector<vector<pii>>& adj) {
    // adj[u] = {(weight, v), ...}
    vector<int> dist(n+1, INF);
    priority_queue<pii, vector<pii>, greater<pii>> pq;  // min heap: {dist, node}
    dist[src] = 0;
    pq.push({0, src});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue;  // stale entry — skip
        for (auto [w, v] : adj[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}

// === UNION-FIND (Disjoint Set Union) ===
// Supports: union(a, b) in O(α(n)), find(a) in O(α(n))
struct DSU {
    vector<int> parent, rank_;
    DSU(int n) : parent(n), rank_(n, 0) {
        iota(parent.begin(), parent.end(), 0);  // parent[i] = i
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);  // path compression
        return parent[x];
    }
    bool unite(int a, int b) {
        a = find(a); b = find(b);
        if (a == b) return false;  // already same component
        if (rank_[a] < rank_[b]) swap(a, b);
        parent[b] = a;
        if (rank_[a] == rank_[b]) rank_[a]++;
        return true;  // successfully merged
    }
    bool connected(int a, int b) { return find(a) == find(b); }
};

// === TOPOLOGICAL SORT (Kahn's algorithm — BFS-based) ===
// For DAGs: order nodes so all edges go left to right
vector<int> toposort(int n, vector<vector<int>>& adj) {
    vector<int> indegree(n+1, 0);
    for (int u = 1; u <= n; u++)
        for (int v : adj[u]) indegree[v]++;

    queue<int> q;
    for (int i = 1; i <= n; i++)
        if (indegree[i] == 0) q.push(i);

    vector<int> order;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        order.push_back(u);
        for (int v : adj[u]) {
            if (--indegree[v] == 0) q.push(v);
        }
    }
    // If order.size() != n: graph has a cycle (not a DAG)
    return order;
}
```

---

## Week 6 Problem Set (25 problems)

### Dijkstra
1. [CSES — Shortest Routes I](https://cses.fi/problemset/task/1671) — standard Dijkstra
2. [CSES — Shortest Routes II](https://cses.fi/problemset/task/1672) — Floyd-Warshall (all pairs)
3. [CSES — High Score](https://cses.fi/problemset/task/1673) — Bellman-Ford (negative edges)
4. [Wormhole Sort](https://usaco.org/index.php?page=viewproblem2&cpid=992) — binary search on max edge weight + DSU
5. [USACO Silver — Shortcut](https://usaco.org/index.php?page=viewproblem2&cpid=899) — Dijkstra + tree reconstruction

### Union-Find
6. [CSES — Building Roads](https://cses.fi/problemset/task/1676) — DSU connectivity
7. [CSES — Road Construction](https://cses.fi/problemset/task/1676) — DSU + tracking
8. [Closing the Farm](https://usaco.org/index.php?page=viewproblem2&cpid=644) — reverse DSU (add edges)
9. [Moocast](https://usaco.org/index.php?page=viewproblem2&cpid=665) — DSU for connectivity
10. [Codeforces 1209D — Distinct Characters Queries](https://codeforces.com/problemset/problem/1209/D) — offline DSU

### Topological Sort
11. [CSES — Course Schedule](https://cses.fi/problemset/task/1679) — basic toposort
12. [CSES — Longest Flight Route](https://cses.fi/problemset/task/1680) — DP on DAG
13. [USACO Silver — Milking Order](https://usaco.org/index.php?page=viewproblem2&cpid=838) — toposort + binary search
14. [Codeforces 510C — Fox and Names](https://codeforces.com/problemset/problem/510/C) — toposort on characters

### Mixed Graph Problems
15-25: CSES Graph Problems section (https://cses.fi/problemset/list/)

---

## Daily Breakdown
- **Day 35-36**: Dijkstra — implement, test on CSES, then USACO problems
- **Day 37-38**: Union-Find — implement DSU struct, solve 5 problems
- **Day 39-40**: Topological sort + DP on DAG — implement, solve 5 problems + contest practice

## Success Criteria
- [ ] Implement Dijkstra from memory in <15 minutes
- [ ] Implement DSU (with path compression + rank) from memory in <10 minutes
- [ ] Implement Kahn's toposort from memory in <10 minutes
- [ ] Solve CSES Shortest Routes I with Dijkstra
- [ ] Solve CSES Course Schedule with Kahn's algorithm
