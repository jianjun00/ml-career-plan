# Week 10: Advanced Graph Algorithms
## Month 3 — Advanced Algorithms

**Goal**: Implement SCC, bridges/articulation points, and Euler paths. These appear in USACO Gold.

---

## Learning Materials
- **USACO Guide SCC**: https://usaco.guide/gold/SCC/
- **USACO Guide Bridges**: https://usaco.guide/gold/bridges/
- **CP-Algorithms**: https://cp-algorithms.com/graph/

---

## Core C++ to Know Cold

```cpp
#include <bits/stdc++.h>
using namespace std;

// === STRONGLY CONNECTED COMPONENTS (Tarjan's or Kosaraju's) ===
// Tarjan's SCC — single pass DFS, O(V + E)
int n, scc_count;
vector<int> adj[100005];
int disc[100005], low[100005], timer_val = 0;
bool on_stack[100005];
stack<int> st;
int scc_id[100005];  // which SCC each node belongs to

void tarjan_dfs(int u) {
    disc[u] = low[u] = ++timer_val;
    st.push(u);
    on_stack[u] = true;
    for (int v : adj[u]) {
        if (!disc[v]) {
            tarjan_dfs(v);
            low[u] = min(low[u], low[v]);
        } else if (on_stack[v]) {
            low[u] = min(low[u], disc[v]);
        }
    }
    if (low[u] == disc[u]) {  // u is root of SCC
        while (true) {
            int v = st.top(); st.pop();
            on_stack[v] = false;
            scc_id[v] = scc_count;
            if (v == u) break;
        }
        scc_count++;
    }
}

// === BRIDGES (edges whose removal disconnects graph) ===
// Tarjan's bridge-finding
vector<pair<int,int>> bridges;
void find_bridges_dfs(int u, int parent) {
    disc[u] = low[u] = ++timer_val;
    for (int v : adj[u]) {
        if (!disc[v]) {
            find_bridges_dfs(v, u);
            low[u] = min(low[u], low[v]);
            if (low[v] > disc[u]) bridges.push_back({u, v});
        } else if (v != parent) {
            low[u] = min(low[u], disc[v]);
        }
    }
}

// === EULER PATH / CIRCUIT ===
// Hierholzer's algorithm — O(E)
// Exists if: 0 nodes with odd degree (circuit) OR exactly 2 (path)
vector<int> euler_path;
void find_euler(int u) {
    while (!adj[u].empty()) {
        int v = adj[u].back(); adj[u].pop_back();
        find_euler(v);
    }
    euler_path.push_back(u);
}
// After calling, reverse euler_path to get the path in order
```

---

## Week 10 Problem Set (20 problems)

### SCC
1. [CSES — Planets and Kingdoms](https://cses.fi/problemset/task/1683) — find SCCs
2. [CSES — Giant Pizza](https://cses.fi/problemset/task/1684) — 2-SAT using SCC
3. [USACO Gold — Grass Cownoisseur](https://usaco.org/index.php?page=viewproblem2&cpid=862) — SCC + DAG DP
4. [Codeforces 919D — Substring](https://codeforces.com/problemset/problem/919/D) — DAG DP

### Bridges and Articulation Points
5. [CSES — Forbidden Cities](https://cses.fi/problemset/task/1705) — bridges
6. [Codeforces 1000E — We Need More Bosses](https://codeforces.com/problemset/problem/1000/E) — bridges + SCC
7. [USACO Gold — Detective Work](https://usaco.org/index.php?page=viewproblem2&cpid=494) — articulation points

### Euler Paths
8. [CSES — Mail Delivery](https://cses.fi/problemset/task/1691) — Euler circuit
9. [CSES — Teleporters Path](https://cses.fi/problemset/task/1693) — Euler path
10. [Codeforces 723E — One-Way Reform](https://codeforces.com/problemset/problem/723/E) — Euler orientation

### Mixed Advanced Graph
11-20: From CSES "Advanced" graph section and USACO Gold problem list

---

## Daily Breakdown
- **Day 56-57**: Tarjan's SCC implementation + 5 problems
- **Day 58**: Bridges/articulation points + 3 problems
- **Day 59**: Euler paths + 3 problems
- **Day 60**: Mixed practice + contest simulation

## Success Criteria
- [ ] Implement Tarjan's SCC from memory
- [ ] Implement bridge-finding from memory
- [ ] Solve CSES Giant Pizza (2-SAT via SCC)
- [ ] Solve CSES Mail Delivery (Euler circuit)
