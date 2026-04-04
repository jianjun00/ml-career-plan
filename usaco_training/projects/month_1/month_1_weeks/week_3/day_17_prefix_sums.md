# Day 17: Prefix Sums and Difference Arrays

## Daily Learning Objectives
- Compute prefix sums for O(1) range queries
- Use difference arrays for O(1) range updates
- Apply to Bronze/Silver boundary problems

---

## Learning Materials

### USACO Guide: Prefix Sums
- **URL**: https://usaco.guide/silver/prefix-sums/
- Read "1D Prefix Sums" and "2D Prefix Sums" — both appear in contests

---

## Core C++ to Know Cold

```cpp
#include <bits/stdc++.h>
using namespace std;

// 1D PREFIX SUMS
// Build: O(n), Query range [l, r]: O(1)
void prefix_sum_1d() {
    int n = 5;
    vector<int> a = {3, 1, 4, 1, 5};   // 0-indexed
    vector<int> pre(n+1, 0);
    for (int i = 0; i < n; i++)
        pre[i+1] = pre[i] + a[i];
    // pre[i] = sum of a[0..i-1]
    // sum(l, r) inclusive 0-indexed:
    auto range_sum = [&](int l, int r) {
        return pre[r+1] - pre[l];
    };
    cout << range_sum(1, 3) << "\n"; // a[1]+a[2]+a[3] = 1+4+1 = 6
}

// 2D PREFIX SUMS
// Build: O(nm), Query rectangle: O(1)
void prefix_sum_2d() {
    int R = 3, C = 3;
    vector<vector<int>> a = {{1,2,3},{4,5,6},{7,8,9}};
    vector<vector<int>> pre(R+1, vector<int>(C+1, 0));
    for (int i = 1; i <= R; i++)
        for (int j = 1; j <= C; j++)
            pre[i][j] = a[i-1][j-1] + pre[i-1][j] + pre[i][j-1] - pre[i-1][j-1];
    // sum of rectangle (r1,c1) to (r2,c2) 1-indexed:
    auto rect_sum = [&](int r1, int c1, int r2, int c2) {
        return pre[r2][c2] - pre[r1-1][c2] - pre[r2][c1-1] + pre[r1-1][c1-1];
    };
    cout << rect_sum(1,1,2,2) << "\n"; // 1+2+4+5 = 12
}

// DIFFERENCE ARRAY — O(1) range update, O(n) build final array
// Use when you need many range-add updates then one final read
void difference_array() {
    int n = 6;
    vector<int> diff(n+2, 0);  // extra space for boundary

    // Add +x to range [l, r] inclusive (0-indexed)
    auto range_add = [&](int l, int r, int x) {
        diff[l] += x;
        diff[r+1] -= x;  // cancel effect after r
    };

    range_add(1, 3, 5);  // add 5 to positions 1,2,3
    range_add(2, 4, 3);  // add 3 to positions 2,3,4

    // Reconstruct final array with prefix sum
    vector<int> result(n, 0);
    int running = 0;
    for (int i = 0; i < n; i++) {
        running += diff[i];
        result[i] = running;
    }
    // result = [0, 5, 8, 8, 3, 0]
}
```

---

## Practice Problems (5 problems)

### Problem 1: Breed Counting (USACO Bronze 2012 Nov)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=180
- **Approach**: 1D prefix sum — count each breed in any range in O(1)
- **Expected time**: 20 minutes

### Problem 2: Haybale Stacking (USACO Bronze 2012 Dec)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=104
- **Approach**: Difference array — each operation is a range add
- **Expected time**: 25 minutes

### Problem 3: Blocked Billboard II (USACO Bronze 2019 Jan)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=783
- **Approach**: 2D prefix sum for rectangle area computation
- **Expected time**: 25 minutes

### Problem 4: CSES — Subarray Sums I
- **URL**: https://cses.fi/problemset/task/1660
- **Approach**: Prefix sum, count pairs
- **Expected time**: 20 minutes

### Problem 5: CSES — Range Update Queries
- **URL**: https://cses.fi/problemset/task/1651
- **Approach**: Difference array + prefix sum reconstruction
- **Expected time**: 20 minutes

---

## Daily Goals
- [ ] Implement 1D and 2D prefix sum from memory
- [ ] Implement difference array from memory
- [ ] Solve all 5 problems
- [ ] Verify the formula for 2D prefix sum rectangle query (draw it out)

## Notes & Reflection
- **The formula**: `rect_sum(r1,c1,r2,c2) = pre[r2][c2] - pre[r1-1][c2] - pre[r2][c1-1] + pre[r1-1][c1-1]` — inclusion-exclusion
- **Tomorrow's focus**: Two pointers / sliding window
