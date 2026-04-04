# Week 5: Advanced STL Data Structures
## Month 2 — Algorithm Development

**Goal**: Master `map`, `set`, `multiset`, `priority_queue`. These appear in 60%+ of Silver problems.

---

## Learning Materials
- **USACO Guide**: https://usaco.guide/silver/data-struct/
- **USACO Guide**: https://usaco.guide/silver/stacks-queues/
- Read both pages in full before starting problems

---

## Core C++ to Know Cold

```cpp
#include <bits/stdc++.h>
using namespace std;

// --- MAP ---
map<string,int> freq;
freq["cow"]++;
freq["cow"]++;
cout << freq["cow"];     // 2
cout << freq.count("pig");  // 0 (doesn't exist)
// Iterate in sorted key order:
for (auto& [key, val] : freq) cout << key << " " << val << "\n";
// Find and erase:
auto it = freq.find("cow");
if (it != freq.end()) freq.erase(it);

// --- SET ---
set<int> s = {3, 1, 4, 1, 5};  // {1,3,4,5} — duplicates dropped, sorted
s.insert(2);   // {1,2,3,4,5}
s.erase(3);    // {1,2,4,5}
cout << *s.begin();              // 1 (smallest)
cout << *s.rbegin();             // 5 (largest)
cout << *s.lower_bound(3);       // 4 (first >= 3)

// --- MULTISET (allows duplicates) ---
multiset<int> ms = {3,1,4,1,5};
ms.insert(1);
cout << ms.count(1);  // 3
ms.erase(ms.find(1)); // erase ONE occurrence only (not all!)

// --- PRIORITY QUEUE ---
// Max heap (default):
priority_queue<int> pq;
pq.push(3); pq.push(1); pq.push(5);
cout << pq.top();  // 5 (max)
pq.pop();
// Min heap:
priority_queue<int, vector<int>, greater<int>> min_pq;
min_pq.push(3); min_pq.push(1);
cout << min_pq.top();  // 1 (min)

// Priority queue of pairs (max by first element):
priority_queue<pair<int,int>> pq2;
pq2.push({5, 10});
pq2.push({3, 20});
cout << pq2.top().first;  // 5

// --- UNORDERED_MAP (O(1) avg, no ordering) ---
unordered_map<int,int> umap;
umap[5]++;
// Use for pure frequency counting when order doesn't matter
// CAUTION: worst case O(n) per operation due to hash collisions
// For competition: use map if n <= 10^5; unordered_map if n > 10^5
```

---

## Week 5 Problem Set (25 problems)

### Core Problems (do all)
1. [Mooyo Mooyo](https://usaco.org/index.php?page=viewproblem2&cpid=860) — set + BFS
2. [Angry Cows](https://usaco.org/index.php?page=viewproblem2&cpid=592) — priority queue + BFS
3. [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) — stack
4. [CSES — Sliding Window Minimum](https://cses.fi/problemset/task/1076) — deque
5. [CSES — Josephus Problem II](https://cses.fi/problemset/task/2163) — ordered set (policy tree)

### Silver-Level Problems
6. [Wormhole Sort](https://usaco.org/index.php?page=viewproblem2&cpid=992) — sorted edges + Union-Find or binary search
7. [Convention II](https://usaco.org/index.php?page=viewproblem2&cpid=859) — priority queue simulation
8. [Closest Pair of Points](https://cses.fi/problemset/task/2194) — set for sweep line
9. [CSES — Room Allocation](https://cses.fi/problemset/task/1164) — priority queue (events)
10. [CSES — Collecting Numbers](https://cses.fi/problemset/task/2216) — multiset or set

### Codeforces Problems (1200-1400)
11. [1234B — Social Network](https://codeforces.com/problemset/problem/1234/B) — map + set
12. [1060C — Maximum Subrectangle](https://codeforces.com/problemset/problem/1060/C) — map + max query
13. [600C — Make Palindrome](https://codeforces.com/problemset/problem/600/C) — frequency map
14-25: From USACO Guide Silver data structures problems list

---

## Daily Breakdown
- **Day 31-32**: `map`, `set`, `multiset` fundamentals + problems 1-10
- **Day 33-34**: `priority_queue` + stack/deque patterns + problems 11-25

## Success Criteria
- [ ] Can implement a frequency counter using `unordered_map` in <2 minutes
- [ ] Can use `set::lower_bound` to find nearest element
- [ ] Can implement sliding window minimum with `deque` from memory
- [ ] Solve CSES Room Allocation (priority queue scheduling pattern)
