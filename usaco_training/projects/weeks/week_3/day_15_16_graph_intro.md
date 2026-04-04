# Days 15-16: Graph Introduction — BFS and DFS

## Daily Learning Objectives
- Represent graphs as adjacency lists
- Implement BFS (shortest path in unweighted graph) and DFS (connectivity)
- Apply to Bronze-level graph problems (connectivity, reachability)

---

## Learning Materials

### USACO Guide: Graph Theory Introduction
- **URL**: https://usaco.guide/bronze/intro-graphs/
- Read all of "Introduction to Graphs" — Bronze uses basic connectivity only

---

## Core C++ to Know Cold

```cpp
#include <bits/stdc++.h>
using namespace std;

// Adjacency list representation
int n, m;  // n nodes, m edges
vector<int> adj[100005];  // or vector<vector<int>> adj(n+1);

void build_graph() {
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);  // undirected; remove for directed
    }
}

// BFS — finds shortest path / minimum steps in unweighted graph
int bfs(int start, int target, int n) {
    vector<int> dist(n+1, -1);
    queue<int> q;
    q.push(start);
    dist[start] = 0;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        if (u == target) return dist[u];
        for (int v : adj[u]) {
            if (dist[v] == -1) {
                dist[v] = dist[u] + 1;
                q.push(v);
            }
        }
    }
    return -1;  // not reachable
}

// DFS — finds connected components, marks reachability
vector<bool> visited(100005, false);
void dfs(int u) {
    visited[u] = true;
    for (int v : adj[u]) {
        if (!visited[v]) dfs(v);
    }
}

// Count connected components
int count_components(int n) {
    int components = 0;
    for (int i = 1; i <= n; i++) {
        if (!visited[i]) {
            dfs(i);
            components++;
        }
    }
    return components;
}

// DFS iterative (avoids stack overflow for large n)
void dfs_iterative(int start) {
    stack<int> st;
    st.push(start);
    while (!st.empty()) {
        int u = st.top(); st.pop();
        if (visited[u]) continue;
        visited[u] = true;
        for (int v : adj[u]) {
            if (!visited[v]) st.push(v);
        }
    }
}
```

**When to use BFS vs DFS at Bronze**:
- **BFS**: "minimum steps/distance" → always BFS
- **DFS**: "is this reachable?", "count components", "flood fill" → DFS
- For Bronze: both work for pure connectivity. Prefer BFS when path length matters.

---

## Practice Problems (8 problems, Days 15-16)

### Day 15: Connectivity + Reachability

**Problem 1: Moocast (USACO Bronze 2016 Nov)**
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=665
- **Approach**: Try all power values; for each, build graph and check connectivity with DFS/BFS
- **Expected time**: 30 minutes

**Problem 2: Milk Visits (USACO Bronze 2019 Nov)**
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=968
- **Approach**: BFS/DFS to determine which farm type is on the path between two nodes
- **Expected time**: 30 minutes

**Problem 3: CSES — Counting Rooms**
- **URL**: https://cses.fi/problemset/task/1192
- **Approach**: Grid BFS/DFS — classic flood fill
- **Expected time**: 20 minutes

**Problem 4: CSES — Labyrinth**
- **URL**: https://cses.fi/problemset/task/1193
- **Approach**: BFS on grid, reconstruct path
- **Expected time**: 25 minutes

### Day 16: Graph Problems + Advanced Patterns

**Problem 5: Trapped in the Haybale (USACO Bronze 2019 Open)**
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=945
- **Approach**: Build graph from constraints; check reachability
- **Expected time**: 30 minutes

**Problem 6: Codeforces 277A — Learning Languages**
- **URL**: https://codeforces.com/problemset/problem/277/A
- **Approach**: Count connected components in bipartite-like graph
- **Expected time**: 20 minutes

**Problem 7: Codeforces 1093G — Multidimensional Queries (skip, too hard)**
**Problem 7 (actual): Codeforces 580C — Kefa and Park**
- **URL**: https://codeforces.com/problemset/problem/580/C
- **Approach**: DFS from root; count leaves reachable without too many cats
- **Expected time**: 25 minutes

**Problem 8: CSES — Building Roads**
- **URL**: https://cses.fi/problemset/task/1676
- **Approach**: Count components; need (components - 1) roads
- **Expected time**: 15 minutes

---

## Daily Goals
- [ ] Implement BFS and DFS from memory (no template)
- [ ] Solve 4 problems each day (Days 15 and 16)
- [ ] For each problem: draw the graph on paper before coding

## Notes & Reflection
- **BFS queue vs DFS stack**: BFS uses `queue<int>`, DFS uses `stack<int>` or recursion
- **Grid graphs**: 4 directions = 4 neighbors; `dx[]={0,0,1,-1}`, `dy[]={1,-1,0,0}`
- **Tomorrow's focus**: Prefix sums and difference arrays
