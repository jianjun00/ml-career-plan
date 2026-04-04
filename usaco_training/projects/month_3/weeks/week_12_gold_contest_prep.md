# Week 12: Gold Contest Preparation + Real Contest
## Month 3 — Advanced Algorithms

**Goal**: Consolidate Gold-level skills. Aim for USACO Gold promotion.

---

## Gold Division Overview

**What Gold tests**: You need ALL of these:
- Segment tree / Fenwick tree (week 9)
- Dijkstra / Bellman-Ford (week 6)
- DSU / Union-Find (week 6)
- Bitmask DP / DP on trees (weeks 7, 11)
- Binary search on answer (week 5/9)
- SCC / bridges (week 10)
- Euler paths (week 10)
- String hashing + KMP (not yet covered — see below)

**Promotion threshold**: ~750 points (3 problems ≈ 1000; 2 correct ≈ 600-800)

---

## String Algorithms (Quick Reference — Not Covered in Weeks 9-11)

```cpp
// === POLYNOMIAL HASHING — O(n) build, O(1) substring compare ===
const long long MOD = 1e9 + 7, BASE = 31;

struct StringHash {
    vector<long long> h, pw;
    StringHash(string& s) : h(s.size()+1, 0), pw(s.size()+1, 1) {
        for (int i = 0; i < s.size(); i++) {
            h[i+1] = (h[i] * BASE + (s[i] - 'a' + 1)) % MOD;
            pw[i+1] = pw[i] * BASE % MOD;
        }
    }
    // Hash of s[l..r] 0-indexed
    long long get(int l, int r) {
        return (h[r+1] - h[l] * pw[r-l+1] % MOD + MOD * MOD) % MOD;
    }
};

// === KMP — O(n+m) pattern matching ===
vector<int> kmp_failure(string& pat) {
    int m = pat.size();
    vector<int> fail(m, 0);
    for (int i = 1; i < m; i++) {
        int j = fail[i-1];
        while (j > 0 && pat[i] != pat[j]) j = fail[j-1];
        if (pat[i] == pat[j]) j++;
        fail[i] = j;
    }
    return fail;
}

vector<int> kmp_search(string& text, string& pat) {
    vector<int> fail = kmp_failure(pat);
    vector<int> matches;
    int j = 0;
    for (int i = 0; i < text.size(); i++) {
        while (j > 0 && text[i] != pat[j]) j = fail[j-1];
        if (text[i] == pat[j]) j++;
        if (j == pat.size()) { matches.push_back(i - j + 1); j = fail[j-1]; }
    }
    return matches;
}
```

---

## Days 66-67: Gold Problem Blitz

Solve 8 USACO Gold problems in 2 days (4 per day):
1. [Springboards](https://usaco.org/index.php?page=viewproblem2&cpid=995) — segment tree + DP
2. [Why Did the Cow Cross the Road III](https://usaco.org/index.php?page=viewproblem2&cpid=722) — Fenwick inversions
3. [Cow Land](https://usaco.org/index.php?page=viewproblem2&cpid=921) — HLD
4. [Shortcut](https://usaco.org/index.php?page=viewproblem2&cpid=899) — Dijkstra + tree
5. [I Hate Knapsack](https://usaco.org/index.php?page=viewproblem2&cpid=1067) — knapsack optimization
6. [Talent Show](https://usaco.org/index.php?page=viewproblem2&cpid=621) — fractional knapsack + binary search
7. [Grass Cownoisseur](https://usaco.org/index.php?page=viewproblem2&cpid=862) — SCC + DAG DP
8. [Moocast](https://usaco.org/index.php?page=viewproblem2&cpid=1085) — Kruskal's + DSU

---

## Day 68: Mock Gold Contest (4 hours)

**Contest**: [USACO 2020 Feb Gold](https://usaco.org/index.php?page=feb20results)
1. [Delegation](https://usaco.org/index.php?page=viewproblem2&cpid=1019) — tree DP
2. [Help Yourself](https://usaco.org/index.php?page=viewproblem2&cpid=1018) — segment tree
3. [Triangles](https://usaco.org/index.php?page=viewproblem2&cpid=1017) — math + sorting

**Scoring target**: ≥600 points (2/3 correct)

---

## Days 69-70: Real Contest + Month 3 Wrap-up

### Real USACO Gold Contest

**Before contest**:
- [ ] All algorithm implementations ready to type from memory
- [ ] Know time complexity of everything
- [ ] Comfortable with C++ template, file I/O

**Contest strategy** (Gold is harder — 4 hours for 3 hard problems):
```
0:00 - 0:20  Read all 3 problems; classify by pattern
0:20 - 1:45  Easiest problem (usually DP or graph) — full solution
1:45 - 3:15  Medium problem — full solution; or brute force for partial
3:15 - 4:00  Hard problem — at minimum, implement O(n^3) for small test cases
```

### Month 3 Success Metrics
| Metric | Target | Actual |
|--------|--------|--------|
| USACO Gold promotion | ~750 pts | |
| Problems solved | 120+ | |
| Algorithms implemented from memory | 12+ | |
| Mock contests completed | 2 | |

---

## 3-Month Program Retrospective

After completing all 3 months:
- **Bronze division**: Week 4 real contest
- **Silver division**: Week 8 real contest
- **Gold division**: Week 12 real contest

**Next steps** (Months 4+):
- Platinum division preparation (Platinum Training file)
- IOI Training Camp selection (top ~25 in USA)
- Advanced topics: Heavy-Light Decomposition, Centroid Decomposition, FFT, Suffix Array
