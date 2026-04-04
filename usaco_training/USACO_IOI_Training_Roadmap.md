# USACO → IOI Training Roadmap
## World-Class Competitive Programming Path to International Olympiad in Informatics

---

## Ultimate Goal: USA IOI Team

**Target**: Qualify for USA IOI team (4 students selected annually)
**Approach**: Systematic USACO progression + international contest experience
**Language**: C++ (required; Java/Python are too slow for most IOI problems)
**Time Commitment**: 15–25 hours/week depending on division

---

## USACO Competition Facts (State of World 2025–2026)

- **Contests per year**: 4 rounds — December, January, February, and US Open (April/May)
- **Format**: Online, 4 hours per contest, 3 problems, submit as many times as you want
- **Scoring**: Each contest is worth ~1000 points total. Problems have multiple test cases (10–25 each), each worth a fraction of 1000. Solving everything = 1000/1000.
- **Promotion thresholds**: Typically ~750/1000 to advance (set per-contest, announced after)
- **Division structure**: Bronze → Silver → Gold → Platinum (only four divisions; no separate IOI track within USACO)
- **Language**: C++17 recommended. Java/Python accepted but significantly slower — at Gold/Platinum, C++ is essentially required
- **After Platinum**: Top performers at US Open (~20–30 students) are invited to the USACO Training Camp (USACO Camp), held in June. 4 students are selected from camp to represent USA at IOI.
- **Official site**: https://usaco.org/
- **Primary preparation guide**: https://usaco.guide/ (the single most important resource)

---

## Entry Point Diagnostic

Don't assume you need to start at Bronze. Use this 30-minute diagnostic to find your actual entry point:

**Test 1 — Bronze readiness**: Solve [CSES Weird Algorithm](https://cses.fi/problemset/task/1068) and [CSES Repetitions](https://cses.fi/problemset/task/1069). If both under 15 minutes: you're at Bronze.

**Test 2 — Silver readiness**: Solve [CSES Subarray Sums I](https://cses.fi/problemset/task/1660) (prefix sums) and [CSES Building Roads](https://cses.fi/problemset/task/1676) (BFS/DSU). If both under 20 minutes total: skip Bronze, start at Silver.

**Test 3 — Gold readiness**: Solve [CSES Dynamic Range Sum Queries](https://cses.fi/problemset/task/1648) (segment tree) and [CSES Shortest Routes I](https://cses.fi/problemset/task/1671) (Dijkstra). If both under 25 minutes total: skip Bronze and Silver, start at Gold.

**Test 4 — Platinum readiness**: Can you implement HLD + segment tree from memory (path queries on trees)? Can you implement centroid decomposition? If yes: start at Platinum.

**Codeforces rating as a proxy**:
| CF Rating | USACO Entry Point |
|-----------|-----------------|
| < 1000 | Bronze |
| 1000–1400 | Silver |
| 1400–2000 | Gold |
| 2000+ | Platinum |

Note: CF rating is not a perfect proxy — USACO style differs from Codeforces. Use the CSES tests above for a more accurate assessment.

---

## Division Progression Overview

| Division | Duration | Time/Week | Promotion Score | IOI Equivalent |
|----------|----------|-----------|-----------------|----------------|
| Bronze | 2–3 months | 15h | ~750/1000 | — |
| Silver | 4–6 months | 18h | ~750/1000 | — |
| Gold | 8–12 months | 20h | ~750/1000 | IOI Bronze range |
| Platinum | 6–12 months | 22h | Top ~25 at US Open | IOI Silver/Gold range |
| IOI Camp | 1 month | All-in | Top 4 at camp | IOI team |

---

## Phase 1: Bronze Division (Months 1–3)

### Core Curriculum (per USACO Guide — the authoritative source)

**Module 1 — Getting Started**
- I/O: file-based (`freopen`) and standard I/O; fast I/O (`ios_base::sync_with_stdio(false); cin.tie(NULL)`)
- Time complexity: O(n), O(n log n), O(n²), knowing which fits in 2–4 second limits
- C++ STL basics: `vector`, `sort()`, `pair`, `map`, `set`
- Competitive programming template

**Module 2 — Simulation**
- Directly implementing what the problem describes
- Careful state tracking, loop correctness

**Module 3 — Rectangle Geometry**
- 2D coordinate problems, intersection, area

**Module 4 — Sorting & Sets**
- `std::sort()` with custom comparators
- `std::set`, `std::map`, `std::multiset` — when to use each
- Coordinate compression basics

**Module 5 — Complete Search (Brute Force)**
- Iterating over all possible solutions
- Nested loops, basic recursion
- Generating permutations/subsets
- Pruning search trees

**Module 6 — Ad Hoc / Implementation**
- Problems with no standard algorithm — just careful coding
- String processing, simulation, counting

**Module 7 — Greedy Algorithms**
- Making locally optimal choices
- Classic greedy patterns: sorting by some key, interval scheduling
- When greedy works vs. when DP is needed

**Module 8 — Binary Search (Introduction)**
- Binary search on a sorted array
- Binary search on the answer (monotone predicate)

### Practice Target: 150–200 bronze problems
### Primary Resources
- [USACO Guide Bronze](https://usaco.guide/bronze/)
- [USACO past contest archive](https://usaco.org/index.php?page=contests) — all bronze problems
- [CSES Problem Set](https://cses.fi/problemset/) — Introductory section (14 problems)
- [Codeforces Div. 2 A/B problems](https://codeforces.com/problemset) (rated 800–1300)

### Contest Strategy (Bronze)
- Read all 3 problems first: 10 min
- Estimate difficulty order; start with easiest
- Bronze problems are largely about careful implementation, not exotic algorithms
- Submit early (USACO allows multiple re-submissions within the 4-hour window)
- After first AC on a problem, test more edge cases then move on
- Time allocation: easy (45 min) → medium (75 min) → hard (90 min) → buffer (30 min)

---

## Phase 2: Silver Division (Months 4–9)

### Core Curriculum

**Module 1 — Prefix Sums**
- 1D prefix sums for range sum queries in O(1)
- 2D prefix sums for rectangle sum queries
- Difference arrays for range updates

**Module 2 — Two Pointers & Sliding Window**
- Two-pointer technique for sorted arrays
- Sliding window for substring/subarray problems
- Monotone deque (deque-based sliding window min/max)

**Module 3 — Binary Search on Answers**
- Parametric search: check(x) → find smallest x with check(x) = true
- Problems where the answer is monotone (e.g., "minimum time to complete tasks")

**Module 4 — Sorting with Custom Comparators**
- `std::sort` with lambda or custom comparator
- Sorting structs by multiple keys
- Coordinate compression applied to real problems

**Module 5 — Recursion & Complete Search (Advanced)**
- DFS-based backtracking
- Recursive enumeration with pruning

**Module 6 — Flood Fill & Graph Basics**
- BFS/DFS on grids (flood fill)
- Graph representations: adjacency list
- Connected components (BFS/DFS)
- Bipartite graphs

**Module 7 — Tree Algorithms (Basic)**
- Trees as special graphs
- DFS on trees: subtree sizes, depths, parents
- Lowest Common Ancestor (LCA) — binary lifting

**Module 8 — Disjoint Set Union (Union-Find)**
- Path compression + union by rank
- Offline connectivity queries
- Connected component tracking with merges

**Module 9 — Greedy Algorithms (Advanced)**
- Interval scheduling, activity selection
- Exchange argument proofs
- Greedy with sorting as preprocessing

**Module 10 — Introduction to Dynamic Programming**
- 1D DP: Fibonacci, longest increasing subsequence (LIS)
- 2D DP: grid paths, edit distance
- Knapsack (0/1 and unbounded)
- DP state design and identifying recurrences

### Practice Target: 300–400 silver problems
### Primary Resources
- [USACO Guide Silver](https://usaco.guide/silver/)
- [CSES Problem Set](https://cses.fi/problemset/) — Sorting, Searching, Dynamic Programming sections (50+ problems)
- [Competitive Programmer's Handbook](https://cses.fi/book/) — Chapters 1–15 (free PDF by Antti Laaksonen)
- [AtCoder Beginner Contests](https://atcoder.jp/contests/abc) — D/E problems (~rated 1000–1800)
- [Codeforces](https://codeforces.com/problemset) — Div. 2 C/D problems (rated 1400–1900)

---

## Phase 3: Gold Division (Months 10–18)

### Core Curriculum

**Module 1 — Dynamic Programming (Advanced)**
- Bitmask DP (subsets, TSP)
- DP on Trees: subtree DP, rerooting technique
- SOS (Sum over Subsets) DP
- Digit DP
- Interval DP
- Matrix Exponentiation for linear recurrences

**Module 2 — Segment Trees**
- Point update, range query (sum, min, max)
- Range update with lazy propagation
- 2D segment trees (or segment tree + BIT)
- Segment tree with custom merge (range GCD, range XOR)

**Module 3 — Fenwick Trees (BIT)**
- Classic BIT for prefix sums
- BIT for order statistics
- 2D BIT

**Module 4 — Shortest Path Algorithms**
- Dijkstra (priority queue implementation)
- Bellman-Ford, SPFA (for negative edges)
- Floyd-Warshall (all-pairs)
- Shortest path on DAGs (topological + DP)
- 0-1 BFS (deque-based BFS for 0/1 edge weights)

**Module 5 — Minimum Spanning Tree**
- Kruskal's algorithm (DSU-based)
- Prim's algorithm
- Borůvka's algorithm concept

**Module 6 — Advanced Graph Theory**
- Topological sort (DFS-based and Kahn's algorithm)
- Strongly Connected Components (Kosaraju, Tarjan)
- Biconnected components, articulation points, bridges
- Euler tour on trees

**Module 7 — Network Flow**
- Max flow: Dinic's algorithm (standard for USACO)
- Min cut (max-flow min-cut theorem)
- Bipartite matching (König's theorem)
- Flow with lower bounds

**Module 8 — String Algorithms**
- String hashing (polynomial rolling hash)
- Z-algorithm and KMP (pattern matching)
- Aho-Corasick (multi-pattern matching)

**Module 9 — Sqrt Decomposition**
- Block decomposition for range queries
- Mo's algorithm for offline range queries

**Module 10 — Data Structure Techniques**
- Monotone stack and monotone queue
- Sparse table for static RMQ (O(1) query)
- DSU with rollback

### Practice Target: 400–500 gold problems
### Primary Resources
- [USACO Guide Gold](https://usaco.guide/gold/)
- [CSES Problem Set](https://cses.fi/problemset/) — Graph Algorithms, Range Queries, Tree Algorithms sections (60+ problems)
- [AtCoder Regular Contests](https://atcoder.jp/contests/arc) — C/D problems
- [Codeforces](https://codeforces.com/problemset) — Div. 1 B/C (rated 2000–2500)
- [CP-Algorithms](https://cp-algorithms.com/) — reference implementations

---

## Phase 4: Platinum Division (Months 19–30)

### Core Curriculum

**Module 1 — Advanced Segment Trees**
- Segment Tree Beats (Ji driver segmentation) — range assign, range min/max with conditions
- Persistent Segment Tree (offline range rank queries)
- Li Chao Tree (convex hull trick in segment tree form)
- Segment tree on sequences of values (merge sort tree)

**Module 2 — Convex Hull Trick / Monotone CHT**
- CHT for linear function DP transitions
- Li Chao tree as general CHT
- Divide and Conquer DP optimization
- Knuth's optimization for interval DP

**Module 3 — Centroid Decomposition**
- Computing centroid of a tree
- Distance queries on trees with centroid decomposition
- Centroid decomposition + data structures

**Module 4 — Heavy-Light Decomposition (HLD)**
- Decomposing tree paths into O(log n) chains
- HLD + segment tree for path queries and updates
- HLD + lazy propagation

**Module 5 — Link-Cut Trees (LCT)**
- Splay tree-based LCT
- Dynamic tree connectivity, path queries
- Access, link, cut operations

**Module 6 — Suffix Structures**
- Suffix Array (O(n log² n) construction via prefix doubling)
- LCP array (Kasai's algorithm)
- Suffix Array applications: longest repeated substring, number of distinct substrings
- Suffix Automaton (SAM) — O(n) construction, counting distinct substrings

**Module 7 — Advanced String Algorithms**
- Aho-Corasick (advanced applications)
- Palindrome structures: Manacher's algorithm, palindrome tree (Eertree)
- Lyndon factorization, Booth's algorithm

**Module 8 — SQRT Decomposition (Advanced)**
- Block decomposition with offline sorting
- Mo's algorithm with updates
- Block DP (blocks of sqrt size for range operations)

**Module 9 — Advanced DP Techniques**
- WQS Binary Search (Aliens Trick) for DP with cardinality constraints
- Parallel Binary Search
- Slope trick DP
- DP on trees with virtual trees

**Module 10 — Advanced Graph Techniques**
- Min Cost Max Flow (MCMF with SPFA)
- Hall's theorem applications
- Dominator trees
- 2-SAT (satisfiability on implications)
- Offline LCT / offline dynamic connectivity (CDQ divide and conquer)

**Module 11 — Advanced Mathematics**
- Fast Fourier Transform (FFT) and NTT (number-theoretic transform)
- Chinese Remainder Theorem (extended)
- Linear basis / XOR basis
- Polynomial operations (multiplication, inverse, log, exp)
- Sprague-Grundy theorem (combinatorial game theory for competitive programming)

### Practice Target: 300–400 platinum problems + IOI problems
### Primary Resources
- [USACO Guide Platinum](https://usaco.guide/platinum/)
- [oj.uz](https://oj.uz/) — the best platform for IOI and IOI-style problems (all past IOI problems, CEOI, COCI, etc.)
- [CSES Problem Set](https://cses.fi/problemset/) — Advanced Techniques, Geometry sections
- [Codeforces Div. 1 C/D/E](https://codeforces.com/problemset) — rated 2500–3200
- [AtCoder Grand Contests](https://atcoder.jp/contests/agc) — C/D/E problems
- [IOI problems (ioi.org)](https://ioi.org/tasks/) — all problems from IOI 2013–present
- [USACO Training Camp problems](https://usaco.org/index.php?page=training)

---

## IOI Qualification Process (USA)

### Step 1: USACO Platinum
- Reach Platinum and perform consistently well throughout the year
- The US Open (April/May) is the most important contest — top performers get invited to camp

### Step 2: USACO Training Camp
- ~20–30 students invited based on USACO performance (primarily US Open + full-year performance)
- Held in early June (1 week, in-person)
- ~4 selection exams at camp (IOI-style: 5 hours, 3 problems, partial scoring)
- Top 4 are selected as the USA IOI team

### Step 3: IOI
- 2 days × 5 hours = 10 hours total
- 3 problems per day, 6 problems total
- Each problem worth 100 points (600 total possible)
- Partial scoring: solve easy subtasks first, then improve
- C++ is the de-facto language (Java/Python allowed but impractical for most problems)
- IOI 2024: Alexandria, Egypt; IOI 2025: Almaty, Kazakhstan; IOI 2026: TBD

### IOI Medal Thresholds (approximate)
- Gold: ~400/600 (top ~8% of participants)
- Silver: ~280/600 (top ~25%)
- Bronze: ~175/600 (top ~50%)

---

## World-Class Resources (Complete List)

### The Essential Tier (Use Daily)
| Resource | URL | Why |
|----------|-----|-----|
| USACO Guide | https://usaco.guide/ | Structured syllabus, curated problems per topic |
| CSES Problem Set | https://cses.fi/problemset/ | 300 canonical problems, gold standard for practice |
| Competitive Programmer's Handbook | https://cses.fi/book/ | Free PDF, covers everything Bronze→Platinum |
| CP-Algorithms | https://cp-algorithms.com/ | Best implementations + explanations of every algorithm |
| USACO Past Contests | https://usaco.org/index.php?page=contests | Official archive of all past problems |

### Practice Platforms
| Platform | URL | Best Use |
|----------|-----|---------|
| Codeforces | https://codeforces.com/ | Regular rated contests (Div. 1/2/3), huge problem set |
| AtCoder | https://atcoder.jp/ | High-quality problems, ABC (easy) to ARC/AGC (hard) |
| oj.uz | https://oj.uz/ | IOI and IOI-style problems (CEOI, COCI, JOI, Baltic) |
| Kenkoooo AtCoder Tracker | https://kenkoooo.com/atcoder/ | Track solved AtCoder problems, virtual contests |
| Virtual Judge | https://vjudge.net/ | Aggregates problems from many judges |
| SPOJ | https://www.spoj.com/ | Large classic problem archive |

### University Courses
| Course | URL | Level |
|--------|-----|-------|
| MIT 6.006 Introduction to Algorithms | https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-fall-2011/ | Bronze–Silver theory |
| MIT 6.046 Design and Analysis of Algorithms | https://ocw.mit.edu/courses/6-046j-design-and-analysis-of-algorithms-spring-2015/ | Gold–Platinum theory |
| MIT 6.851 Advanced Data Structures | https://ocw.mit.edu/courses/6-851-advanced-data-structures-spring-2017/ | Platinum+ |
| Stanford CS161 | https://web.stanford.edu/class/cs161/ | Silver–Gold |
| CMU 15-451 Algorithms | https://www.cs.cmu.edu/~15451-f21/ | Gold–Platinum |

### Textbooks
| Book | Author | Level |
|------|--------|-------|
| Competitive Programmer's Handbook | Antti Laaksonen | All levels (free PDF) |
| Competitive Programming 4 | Steven & Felix Halim | All levels (most comprehensive) |
| Introduction to Algorithms (CLRS) | Cormen et al. | Theory reference |
| Algorithm Design | Kleinberg & Tardos | Theory reference |
| Guide to Competitive Programming | Laaksonen | Structured learning |

---

## Stress Testing (Essential Technique)

Stress testing is one of the most important competitive programming techniques. It involves:
1. Writing a brute-force solution that is definitely correct
2. Writing your optimized solution
3. Generating random small test cases
4. Running both solutions and comparing output until they differ

```cpp
// stress_test.sh
while true; do
    python3 gen.py > input.txt
    ./brute < input.txt > expected.txt
    ./solution < input.txt > actual.txt
    if ! diff -q expected.txt actual.txt > /dev/null; then
        echo "FOUND DIFFERENCE!"
        cat input.txt
        break
    fi
done
```

Use stress testing whenever your solution passes samples but WAs on unknown test cases.

---

## Competitive Programming C++ Template

```cpp
#include <bits/stdc++.h>
using namespace std;

// Fast I/O
#define FASTIO ios_base::sync_with_stdio(false); cin.tie(NULL)

// Type aliases
using ll = long long;
using pii = pair<int,int>;
using pll = pair<ll,ll>;
using vi = vector<int>;
using vl = vector<ll>;

// Constants
const int MOD = 1e9 + 7;
const int INF = 1e9;
const ll LINF = 1e18;

// USACO file I/O (bronze/silver problems use file I/O)
void setIO(string name = "") {
    FASTIO;
    if (name.size()) {
        freopen((name + ".in").c_str(), "r", stdin);
        freopen((name + ".out").c_str(), "w", stdout);
    }
}

int main() {
    setIO("problemname"); // or setIO() for stdin/stdout
    // code here
}
```

---

## Annual Training Calendar

| Month | Focus | Contest |
|-------|-------|---------|
| September–November | Build/reinforce current division skills | — |
| December | Contest practice + review | USACO December contest |
| January | Fix December weaknesses | USACO January contest |
| February | Intensive practice | USACO February contest |
| March–April | Deep focus on next-division material | Prep for US Open |
| May | Peak performance | USACO US Open |
| June | (Platinum only) Camp preparation or camp | USACO Training Camp |
| July–August | IOI (if selected) or advance prep for next year | IOI |

---

## Division Transition Guide

### Bronze → Silver
**Signal you're ready**: Solve all 3 Bronze problems in under 3 hours in at least 2 simulated contests. Real promotion is confirmation, not prerequisite.

**Start Silver material when**: You're consistently getting AC on 90%+ of Bronze practice problems. Don't wait for a real contest.

**What you gain**: Silver introduces the first "real" algorithms — O(n log n) techniques that aren't just careful simulation. The mental model shifts from "implement what the problem says" to "recognize which algorithm applies."

**First Silver topic to learn**: Prefix Sums (Module 1). It builds on nothing you don't already know from Bronze and unlocks many Silver problems immediately.

---

### Silver → Gold
**Signal you're ready**: Solve 2 of 3 Silver problems in under 2.5 hours consistently. Solved 200+ Silver-level problems total.

**Start Gold material when**: You're past the midpoint of Silver training. Segment Trees and Dijkstra don't require Silver completion — begin them in your last Silver month.

**What you gain**: Gold requires data structure fluency. You cannot pattern-match your way through Gold — you must be able to build a segment tree, run Dijkstra, and write DSU from memory without thinking.

**First Gold topic to learn**: Segment Trees (Module 1 of Gold plan). This unlocks the most Gold problems of any single topic.

---

### Gold → Platinum
**Signal you're ready**: Solved all (or nearly all) 300 CSES problems. Codeforces rating 2000+. Solve 2 of 3 Gold problems consistently in simulations.

**What you gain**: Platinum shifts the challenge from implementation to insight. The algorithms are known (centroid decomp, HLD, suffix arrays) — the problem is recognizing which one applies to a problem you've never seen before.

**Key mindset shift**: At Gold, you implement a known algorithm. At Platinum, you must discover the algorithm through mathematical reasoning about the problem. Practice spending 30–60 minutes thinking without coding before committing to an approach.

**First Platinum topic to learn**: Convex Hull Trick (CHT) — it's the most direct extension of Gold DP skills.

---

### Platinum → IOI Team
**Signal you're ready**: Top ~25 at USACO US Open. Codeforces rating 2400+. Can solve IOI-style problems with partial scoring.

**What changes**: IOI problems have subtasks. A partial solution that handles n ≤ 1000 in O(n³) earns real points. Never leave a problem blank.

---

## Progress Benchmarks

### Bronze
- Solve any bronze problem within 45 minutes
- Know O(n), O(n log n), O(n²) complexity and when each fits in 4s
- Fluent with `sort`, `set`, `map`, `pair`, `vector` in C++ STL

### Silver
- Solve prefix sums, two pointers, basic DP problems under 60 minutes
- Implement BFS/DFS/flood fill correctly every time
- Write DSU from memory in under 5 minutes

### Gold
- Implement segment tree with lazy propagation from memory
- Code Dijkstra, Dinic's max flow correctly
- Identify when a problem needs advanced DP vs. greedy vs. graph

### Platinum
- Recognize CHT / D&C DP / aliens trick applicability
- Implement centroid decomposition and HLD correctly
- Solve IOI subtask partial scoring problems efficiently

---

**Total Timeline**: 24–30 months from Bronze to IOI team consideration
**Total Problems**: 1500–2500 across all divisions
**Total Practice Hours**: 2500–3500

*Last updated: April 2026*
*Primary source: usaco.guide, usaco.org, cses.fi*
