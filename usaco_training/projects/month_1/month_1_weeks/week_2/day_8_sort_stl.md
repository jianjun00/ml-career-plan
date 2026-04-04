# Day 8: Sorting with STL — `std::sort` and Custom Comparators

## Daily Learning Objectives
- Master `std::sort` with default and custom comparators
- Sort pairs, structs, and multi-key data
- Apply sort to 5 Bronze-level problems fluently

> **Competition rule**: You use `std::sort()` in contests — never implement sorting from scratch. Know algorithm complexities to pick the right approach; let the STL do the work.

---

## Learning Materials

### USACO Guide: Sorting
- **URL**: https://usaco.guide/bronze/intro-sorting/
- Read all of "Introduction to Sorting" and "Sorting with Custom Comparators"
- Practice the code examples as you go

---

## Core C++ to Know Cold

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    // 1. Sort ints
    vector<int> v = {5, 2, 8, 1, 9};
    sort(v.begin(), v.end());               // [1,2,5,8,9]
    sort(v.begin(), v.end(), greater<int>()); // [9,8,5,2,1]

    // 2. Sort pairs (by first, then second automatically)
    vector<pair<int,int>> p = {{3,1},{1,5},{3,2}};
    sort(p.begin(), p.end()); // {1,5},{3,1},{3,2}

    // 3. Custom lambda comparator
    sort(p.begin(), p.end(), [](const pair<int,int>& a, const pair<int,int>& b){
        if (a.second != b.second) return a.second < b.second;
        return a.first < b.first;  // tiebreak
    });

    // 4. Sorting a struct
    struct Cow { string name; int weight; int id; };
    vector<Cow> cows = {{"Bessie",500,1},{"Elsie",600,2},{"Mildred",400,3}};
    // Sort by weight ascending; tiebreak by name lexicographic
    sort(cows.begin(), cows.end(), [](const Cow& a, const Cow& b){
        if (a.weight != b.weight) return a.weight < b.weight;
        return a.name < b.name;
    });

    // 5. Sort-then-check: find pair with sum = target
    sort(v.begin(), v.end());
    int lo = 0, hi = (int)v.size() - 1;
    while (lo < hi) {
        int s = v[lo] + v[hi];
        if (s == 10) { cout << "found"; break; }
        else if (s < 10) lo++;
        else hi--;
    }

    return 0;
}
```

**Complexity quick reference**:
- `std::sort` is O(n log n) — handles n ≤ 10^6 comfortably
- n ≤ 1,000: nested O(n²) loops also fine
- n ≤ 100: even O(n³) is fine

---

## Practice Problems (5 problems)

### Problem 1: Diamond Collector (USACO Bronze)
- **URL**: https://usaco.guide/bronze/intro-sorting/
- **Approach**: Sort diamonds by size, then for each diamond as the smallest, binary search (or two pointers) for the largest within range K. Time: O(n log n)
- **Key insight**: After sorting, the answer window slides — this is the sort-then-sweep pattern
- **Expected time**: 25 minutes

### Problem 2: Livestock Lineup (USACO Bronze 2019 Feb)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=965
- **Approach**: Constraints are small (n ≤ 8) — try all permutations or use topological sort on adjacency constraints
- **Expected time**: 30 minutes

### Problem 3: Moocast (USACO Bronze 2016 Nov)  
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=665
- **Approach**: Try all possible radii; for each radius, BFS/DFS. But also: sort by distance threshold
- **Expected time**: 30 minutes

### Problem 4: Codeforces 1157A — Reorder the Array
- **URL**: https://codeforces.com/problemset/problem/1157/A
- **Approach**: Sort, then greedily count pairs where a[i] > a[i-1]
- **Expected time**: 15 minutes

### Problem 5: Codeforces 1041A — Heist
- **URL**: https://codeforces.com/problemset/problem/1041/A
- **Approach**: Sort unique values, find max consecutive gap
- **Expected time**: 15 minutes

---

## Daily Goals
- [ ] Read USACO Guide sorting page
- [ ] Write all 5 code snippets above from memory (no copy-paste)
- [ ] Solve all 5 problems
- [ ] For each problem: write your complexity in a comment

## Notes & Reflection
- **Key learnings**:
- **Difficult parts**:
- **Tomorrow's focus**: binary search with `lower_bound`/`upper_bound`
