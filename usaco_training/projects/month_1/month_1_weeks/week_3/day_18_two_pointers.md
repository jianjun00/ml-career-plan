# Day 18: Two Pointers and Sliding Window

## Daily Learning Objectives
- Implement the two-pointer technique for sorted arrays
- Implement fixed and variable-size sliding windows
- Apply to Bronze/Silver problems requiring O(n) range processing

---

## Learning Materials

### USACO Guide: Two Pointers
- **URL**: https://usaco.guide/silver/two-pointers/
- Read the full page; this pattern appears frequently in Silver

---

## Core C++ to Know Cold

```cpp
#include <bits/stdc++.h>
using namespace std;

// TWO POINTERS on sorted array
// Pattern: find all pairs with sum in range [lo, hi]
int count_pairs_in_range(vector<int>& v, int lo, int hi) {
    sort(v.begin(), v.end());
    int left = 0, right = v.size() - 1;
    int count = 0;
    while (left < right) {
        int s = v[left] + v[right];
        if (s < lo) left++;
        else if (s > hi) right--;
        else {
            // s is in range — count all pairs with this left
            // (need careful counting when duplicates exist)
            count++;
            left++;
        }
    }
    return count;
}

// FIXED SLIDING WINDOW — window of size k
// Find max subarray sum of size k
int max_window_sum(vector<int>& v, int k) {
    int n = v.size();
    int window_sum = 0;
    for (int i = 0; i < k; i++) window_sum += v[i];
    int max_sum = window_sum;
    for (int i = k; i < n; i++) {
        window_sum += v[i] - v[i-k];  // add new, remove old
        max_sum = max(max_sum, window_sum);
    }
    return max_sum;
}

// VARIABLE SLIDING WINDOW — expand right, shrink left when condition violated
// Find smallest subarray with sum >= target
int smallest_subarray_with_sum(vector<int>& v, int target) {
    int n = v.size();
    int left = 0, current_sum = 0, min_len = INT_MAX;
    for (int right = 0; right < n; right++) {
        current_sum += v[right];
        while (current_sum >= target) {
            min_len = min(min_len, right - left + 1);
            current_sum -= v[left++];  // shrink from left
        }
    }
    return (min_len == INT_MAX) ? 0 : min_len;
}

// SORT + TWO POINTERS for counting (Diamond Collector pattern)
int max_diamonds_in_range(vector<int>& sizes, int K) {
    sort(sizes.begin(), sizes.end());
    int n = sizes.size();
    int best = 0, left = 0;
    for (int right = 0; right < n; right++) {
        while (sizes[right] - sizes[left] > K) left++;
        best = max(best, right - left + 1);
    }
    return best;
}
```

---

## Practice Problems (5 problems)

### Problem 1: Diamond Collector (USACO Bronze 2016 Feb)
- **URL**: https://usaco.guide/bronze/intro-sorting/
- **Approach**: Sort + two pointers (shown above)
- **Expected time**: 20 minutes

### Problem 2: Paired Up (USACO Silver 2017 Open)
- **URL**: https://usaco.org/index.php?page=viewproblem2&cpid=738
- **Approach**: Sort + greedy two pointers — pair largest with smallest if possible
- **Expected time**: 25 minutes

### Problem 3: CSES — Sum of Two Values
- **URL**: https://cses.fi/problemset/task/1640
- **Approach**: Sort + two pointers OR hashmap
- **Expected time**: 15 minutes

### Problem 4: Codeforces 279B — Books
- **URL**: https://codeforces.com/problemset/problem/279/B
- **Approach**: Sliding window — max books read within time limit
- **Expected time**: 20 minutes

### Problem 5: Codeforces 1208C — Magic Grid
- **URL**: This is too hard — use: Codeforces 1343B — Balanced Array
- **URL**: https://codeforces.com/problemset/problem/1343/B
- **Approach**: Two-pointer counting
- **Expected time**: 20 minutes

---

## Daily Goals
- [ ] Implement all 3 two-pointer patterns from memory
- [ ] Solve all 5 problems
- [ ] Identify: which problems use fixed window vs variable window?

## Notes & Reflection
- **Key invariant**: In the sliding window, always maintain exactly one invariant. If the window grows too large (condition violated), shrink from the left
- **Tomorrow's focus**: Mock contest 2
