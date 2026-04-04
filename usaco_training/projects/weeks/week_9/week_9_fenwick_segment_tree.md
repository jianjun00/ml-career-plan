# Week 9: Fenwick Tree and Segment Tree
## Month 3 — Advanced Algorithms

**Goal**: Implement Fenwick (BIT) and segment tree for range queries + point updates.

---

## Learning Materials
- **USACO Guide Fenwick Tree**: https://usaco.guide/gold/BIT/
- **USACO Guide Segment Tree**: https://usaco.guide/gold/seg/
- Read Fenwick first (simpler); segment tree after

---

## Core C++ to Know Cold

```cpp
#include <bits/stdc++.h>
using namespace std;

// === FENWICK TREE (Binary Indexed Tree) ===
// Supports: point update + prefix sum query in O(log n)
struct Fenwick {
    int n;
    vector<int> tree;
    Fenwick(int n) : n(n), tree(n+1, 0) {}

    void update(int i, int delta) {  // add delta to position i (1-indexed)
        for (; i <= n; i += i & (-i))
            tree[i] += delta;
    }

    int query(int i) {  // prefix sum [1, i]
        int sum = 0;
        for (; i > 0; i -= i & (-i))
            sum += tree[i];
        return sum;
    }

    int query(int l, int r) {  // range sum [l, r]
        return query(r) - query(l-1);
    }
};

// Usage:
// Fenwick bit(n);
// bit.update(3, 5);  // add 5 at position 3
// bit.query(1, 5);   // sum from 1 to 5

// === SEGMENT TREE (point update, range query) ===
// More general than Fenwick; supports max/min/custom queries
struct SegTree {
    int n;
    vector<int> tree;  // tree[1] = root; tree[2i], tree[2i+1] = children
    SegTree(int n) : n(n), tree(2*n, 0) {}

    void update(int pos, int val) {  // set position pos to val (0-indexed)
        pos += n;  // leaf position
        tree[pos] = val;
        while (pos > 1) {
            pos /= 2;
            tree[pos] = tree[2*pos] + tree[2*pos+1];  // sum; replace with max for max tree
        }
    }

    int query(int l, int r) {  // range sum [l, r) (0-indexed, exclusive r)
        int res = 0;
        for (l += n, r += n; l < r; l /= 2, r /= 2) {
            if (l & 1) res += tree[l++];
            if (r & 1) res += tree[--r];
        }
        return res;
    }
};

// === SEGMENT TREE WITH LAZY PROPAGATION ===
// For range updates (not just point updates)
// This is more complex — implement only if needed for Gold
struct LazySegTree {
    int n;
    vector<long long> tree, lazy;
    LazySegTree(int n) : n(n), tree(4*n, 0), lazy(4*n, 0) {}

    void push_down(int node, int l, int r) {
        if (lazy[node] != 0) {
            int mid = (l + r) / 2;
            tree[2*node] += lazy[node] * (mid - l + 1);
            tree[2*node+1] += lazy[node] * (r - mid);
            lazy[2*node] += lazy[node];
            lazy[2*node+1] += lazy[node];
            lazy[node] = 0;
        }
    }

    void update(int node, int l, int r, int ql, int qr, long long val) {
        if (qr < l || r < ql) return;
        if (ql <= l && r <= qr) {
            tree[node] += val * (r - l + 1);
            lazy[node] += val;
            return;
        }
        push_down(node, l, r);
        int mid = (l + r) / 2;
        update(2*node, l, mid, ql, qr, val);
        update(2*node+1, mid+1, r, ql, qr, val);
        tree[node] = tree[2*node] + tree[2*node+1];
    }

    long long query(int node, int l, int r, int ql, int qr) {
        if (qr < l || r < ql) return 0;
        if (ql <= l && r <= qr) return tree[node];
        push_down(node, l, r);
        int mid = (l + r) / 2;
        return query(2*node, l, mid, ql, qr) + query(2*node+1, mid+1, r, ql, qr);
    }
};
```

---

## Week 9 Problem Set (20 problems)

### Fenwick Tree
1. [CSES — Dynamic Range Sum Queries](https://cses.fi/problemset/task/1648) — basic Fenwick
2. [CSES — Dynamic Range Minimum Queries](https://cses.fi/problemset/task/1649) — segment tree min
3. [CSES — Number of Inversions](https://cses.fi/problemset/task/1703) — Fenwick for inversion count
4. [USACO Gold — Haircut](https://usaco.org/index.php?page=viewproblem2&cpid=1041) — BIT for counting
5. [Codeforces 1093G — Multidimensional Queries](https://codeforces.com/problemset/problem/1093/G)

### Segment Tree
6. [CSES — Range Update Queries](https://cses.fi/problemset/task/1651) — lazy propagation
7. [CSES — Range Queries and Copies](https://cses.fi/problemset/task/1737) — persistent segment tree
8. [USACO Gold — Springboards](https://usaco.org/index.php?page=viewproblem2&cpid=995)
9. [USACO Gold — Cow Land](https://usaco.org/index.php?page=viewproblem2&cpid=921) — HLD + segment tree
10. [CSES — Hotel Queries](https://cses.fi/problemset/task/1143) — segment tree walk

### Mixed Problems
11-20: From CSES "Range Queries" section

---

## Daily Breakdown
- **Day 51-52**: Implement Fenwick from memory; solve 5 problems
- **Day 53-54**: Implement iterative segment tree from memory; solve 5 problems
- **Day 55**: Lazy propagation concept; solve 5 problems; contest simulation

## Success Criteria
- [ ] Implement Fenwick tree from memory in <8 minutes
- [ ] Implement iterative segment tree from memory in <12 minutes
- [ ] Solve CSES Number of Inversions with Fenwick
- [ ] Solve CSES Range Update Queries with lazy segment tree
