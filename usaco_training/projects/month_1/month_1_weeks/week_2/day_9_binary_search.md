# Day 9: Binary Search — `lower_bound`, `upper_bound`, and Binary Search on Answer

## Daily Learning Objectives
- Use `lower_bound`/`upper_bound` on sorted arrays
- Binary search on answer (bisect the solution space)
- Apply to 5 Bronze/Silver-boundary problems

---

## Learning Materials

### USACO Guide: Binary Search
- **URL**: https://usaco.guide/silver/binary-search/
- Read "Binary Search" and "Binary Search on the Answer"

---

## Core C++ to Know Cold

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {1, 3, 5, 7, 9, 11};

    // lower_bound: first element >= x
    auto it = lower_bound(v.begin(), v.end(), 5);
    cout << *it << "\n";              // 5
    cout << it - v.begin() << "\n";  // index 2

    // upper_bound: first element > x
    auto it2 = upper_bound(v.begin(), v.end(), 5);
    cout << *it2 << "\n";            // 7 (first element > 5)

    // Count occurrences of value x in sorted array
    int x = 5;
    int cnt = upper_bound(v.begin(), v.end(), x) 
            - lower_bound(v.begin(), v.end(), x);  // 1

    // Count elements in range [lo, hi] inclusive
    int lo = 3, hi = 7;
    int range_cnt = upper_bound(v.begin(), v.end(), hi)
                  - lower_bound(v.begin(), v.end(), lo);  // 3 (3,5,7)

    // Manual binary search (when lower_bound doesn't directly apply)
    // Pattern: find smallest x such that check(x) is true
    int L = 0, R = 1e9;
    while (L < R) {
        int mid = L + (R - L) / 2;  // avoid overflow
        if (/* check(mid) */ mid >= 5) {
            R = mid;   // mid could be the answer; try smaller
        } else {
            L = mid + 1;  // mid is too small
        }
    }
    // L == R == answer

    return 0;
}
```

**Binary search on answer template** — use when:
- You're minimizing a maximum (or maximizing a minimum)
- The answer has a monotone property: if `x` works, all `x' > x` also work

```cpp
// Example: "Minimum maximum distance" type problem
bool feasible(int mid, vector<int>& pos, int k) {
    int count = 1, prev = pos[0];
    for (int i = 1; i < pos.size(); i++) {
        if (pos[i] - prev >= mid) {
            count++;
            prev = pos[i];
        }
    }
    return count >= k;
}

int solve(vector<int>& pos, int k) {
    sort(pos.begin(), pos.end());
    int lo = 1, hi = pos.back() - pos.front();
    while (lo < hi) {
        int mid = lo + (hi - lo + 1) / 2;  // upper mid for "maximize"
        if (feasible(mid, pos, k)) lo = mid;
        else hi = mid - 1;
    }
    return lo;
}
```

---

## Practice Problems (5 problems)

### Problem 1: Convention (USACO Silver 2018 Feb)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=786
- **Approach**: Binary search on the answer (max cows per bus). Check feasibility by greedy assignment
- **Why this problem**: Classic "binary search on answer" pattern — most common Silver pattern
- **Expected time**: 35 minutes

### Problem 2: CSES — Array Division
- **URL**: https://cses.fi/problemset/task/1085
- **Approach**: Binary search on max subarray sum; greedy feasibility check
- **Expected time**: 25 minutes

### Problem 3: Codeforces 1064B — Equations of Takahashi
- **URL**: https://codeforces.com/problemset/problem/1064/B
- **Approach**: Sort + binary search for counting
- **Expected time**: 20 minutes

### Problem 4: CSES — Ferris Wheel
- **URL**: https://cses.fi/problemset/task/1090
- **Approach**: Sort, then two-pointer (adjacent to binary search concept)
- **Expected time**: 20 minutes

### Problem 5: Codeforces 1101B — Businessmen
- **URL**: https://codeforces.com/problemset/problem/1101/B
- **Approach**: Sort + check uniqueness pattern
- **Expected time**: 15 minutes

---

## Daily Goals
- [ ] Read USACO Guide binary search page
- [ ] Write the binary search template from memory 3 times
- [ ] Solve all 5 problems
- [ ] Identify: which problems use `lower_bound` vs manual binary search?

## Notes & Reflection
- **Key insight**: `lo + (hi - lo) / 2` avoids integer overflow; use `+1` in numerator when you want upper mid
- **Tomorrow's focus**: Complete search (brute force enumeration)
