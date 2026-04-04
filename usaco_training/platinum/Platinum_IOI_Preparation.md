# Platinum Division & IOI Preparation
## 12-Month Path to USA IOI Team

---

## Objectives

**USACO Target**: Consistent high performance in Platinum; top ~25 at US Open
**IOI Target**: USA IOI team (4 students selected at Training Camp in June)
**Duration**: 12 months
**Time Commitment**: 22+ hours/week
**Primary Resources**: [USACO Guide Platinum](https://usaco.guide/platinum/) + [oj.uz](https://oj.uz/) + [CSES](https://cses.fi/problemset/)

### What Platinum Tests
Platinum problems require research-level implementation skills in competitive programming. You must know advanced data structures (segment tree beats, persistent segment trees, Li Chao tree), advanced DP optimizations (CHT, D&C DP, Aliens trick), tree decompositions (centroid, HLD, LCT), and string structures (suffix arrays, suffix automata). Problems often have no obvious algorithm — you must discover the key insight before implementing.

**Critical difference from Gold**: At Gold, once you identify the algorithm, the implementation is standard. At Platinum, identifying the algorithm IS the hard part. Expect to spend 30–90 minutes thinking before coding on hard problems.

---

## Platinum Curriculum

### Module 1 — Advanced Segment Trees (Month 1)

#### 1A. Segment Tree Beats (Ji Driver Segmentation)

Supports range operations like "set all elements > x to x" in amortized O(n log² n).

```cpp
// Track: max value, second max, count of max, sum
// When updating range [l,r] with operation "clamp to x" (a[i] = min(a[i], x)):
// If x >= max: do nothing
// If x > second_max (and x < max): update lazily — new max = x, sum decreases by (max-x)*count_max
// If x <= second_max: recurse into children
```

**Applications**: Range chmin + range sum queries; covered in USACO Guide Platinum.

#### 1B. Persistent Segment Tree

```cpp
// Each update creates a new root; old roots still accessible
// Represents "version history" of the segment tree
// Space: O(n log n) total (each update adds O(log n) nodes)
struct Node { int left, right, val; };
vector<Node> nodes; // node pool
int roots[MAXN]; // roots[i] = root of version i

int update(int prev, int l, int r, int pos, int val) {
    int cur = nodes.size(); nodes.push_back(nodes[prev]);
    if (l == r) { nodes[cur].val += val; return cur; }
    int mid = (l+r)/2;
    if (pos <= mid) nodes[cur].left = update(nodes[prev].left, l, mid, pos, val);
    else nodes[cur].right = update(nodes[prev].right, mid+1, r, pos, val);
    nodes[cur].val = nodes[nodes[cur].left].val + nodes[nodes[cur].right].val;
    return cur;
}
// query(roots[r], roots[l-1], ...) for range [l,r] queries on immutable array
```

**Applications**: K-th smallest in range [l,r], offline range inversions.

#### 1C. Li Chao Tree (for CHT with arbitrary queries)

```cpp
// Supports: add line y = mx + b; query: maximum y at point x
struct LiChaoTree {
    struct Line { long long m, b; long long eval(long long x) { return m*x+b; } };
    int n; vector<Line> tree;
    LiChaoTree(int n) : n(n), tree(2*n, {0, LLONG_MIN/2}) {}
    void add_line(int node, int l, int r, Line line) {
        int mid = (l+r)/2;
        bool left_better = line.eval(l) > tree[node].eval(l);
        bool mid_better  = line.eval(mid) > tree[node].eval(mid);
        if (mid_better) swap(tree[node], line);
        if (l == r) return;
        if (left_better != mid_better) add_line(2*node, l, mid, line);
        else add_line(2*node+1, mid+1, r, line);
    }
    long long query(int node, int l, int r, int x) {
        long long res = tree[node].eval(x);
        if (l == r) return res;
        int mid = (l+r)/2;
        if (x <= mid) return max(res, query(2*node, l, mid, x));
        else return max(res, query(2*node+1, mid+1, r, x));
    }
};
```

**Practice**: 20 advanced segment tree problems from USACO Platinum archive + oj.uz

---

### Module 2 — DP Optimization Techniques (Month 2)

#### 2A. Convex Hull Trick (CHT) — Classical

For DP recurrences of the form: `dp[j] = min/max over i<j of (dp[i] + cost(i,j))` where `cost(i,j)` factors into a linear function of j with slope/intercept depending on i.

```cpp
// Minimize dp[j] = min_{i<j} (a[i]*j + b[i]) — monotone slopes, monotone query
// Use a stack of lines; O(n) total
struct CHT {
    struct Line { long long m, b; long long eval(long long x) { return m*x+b; } };
    deque<Line> hull;
    bool bad(Line l1, Line l2, Line l3) {
        return (__int128)(l3.b-l1.b)*(l1.m-l2.m) <= (__int128)(l2.b-l1.b)*(l1.m-l3.m);
    }
    void add(long long m, long long b) {
        Line l = {m, b};
        while (hull.size() >= 2 && bad(hull[hull.size()-2], hull.back(), l)) hull.pop_back();
        hull.push_back(l);
    }
    long long query(long long x) { // monotone query
        while (hull.size() > 1 && hull[0].eval(x) >= hull[1].eval(x)) hull.pop_front();
        return hull[0].eval(x);
    }
};
```

#### 2B. Divide and Conquer DP

When the optimal split point `opt(i)` is monotone: `opt(i) <= opt(i+1)`. Solve in O(n log n) instead of O(n²).

```cpp
void solve(int l, int r, int optl, int optr) {
    if (l > r) return;
    int mid = (l+r)/2, opt = optl;
    for (int k = optl; k <= min(mid, optr); k++)
        if (cost(k, mid) < cost(opt, mid)) opt = k;
    dp[mid] = cost(opt, mid);
    solve(l, mid-1, optl, opt);
    solve(mid+1, r, opt, optr);
}
```

#### 2C. Knuth's Optimization

For interval DP `dp[i][j] = min_{i<=k<j} (dp[i][k] + dp[k+1][j] + cost(i,j))` when `cost` satisfies the quadrangle inequality. Reduces O(n³) to O(n²).

```cpp
// opt[i][j] = optimal split point
// Constraint: opt[i][j-1] <= opt[i][j] <= opt[i+1][j]
for (int len = 2; len <= n; len++)
    for (int i = 0; i+len-1 < n; i++) {
        int j = i+len-1;
        opt[i][j] = i;
        for (int k = opt[i][j-1]; k <= opt[i+1][j]; k++)
            if (dp[i][k] + dp[k+1][j] + cost[i][j] < dp[i][j])
                dp[i][j] = dp[i][k]+dp[k+1][j]+cost[i][j], opt[i][j]=k;
    }
```

#### 2D. WQS Binary Search (Aliens Trick)

For DP problems with constraint "use exactly K items" where the answer is concave/convex in K.

```cpp
// Binary search on penalty λ for using one more item
// For each λ, solve unconstrained DP; track item count used
// Find λ such that item count = K
```

#### 2E. Slope Trick

For DP recurrences where the cost function is piecewise linear and convex. Track the slope change points in a priority queue.

**Practice**: 30 DP optimization problems — [AtCoder DP Contest](https://atcoder.jp/contests/dp) hard problems + oj.uz + Codeforces rated 2600–3000

---

### Module 3 — Centroid Decomposition (Month 3)

**Core idea**: Recursively find the centroid of the tree; the centroid is the node whose removal results in no subtree of size > n/2. Depth of centroid decomposition = O(log n).

```cpp
// centroid decomposition: O(n log n) total processing
vector<int> subtree_size(n), centroid_parent(n,-1);
vector<bool> removed(n,false);

int get_subtree_size(int u, int p) {
    subtree_size[u] = 1;
    for (int v : adj[u]) if (v != p && !removed[v]) subtree_size[u] += get_subtree_size(v,u);
    return subtree_size[u];
}

int get_centroid(int u, int p, int tree_size) {
    for (int v : adj[u]) if (v != p && !removed[v])
        if (subtree_size[v] > tree_size/2) return get_centroid(v,u,tree_size);
    return u;
}

void decompose(int u, int p) {
    int sz = get_subtree_size(u, -1);
    int c = get_centroid(u, -1, sz);
    centroid_parent[c] = p;
    removed[c] = true;
    for (int v : adj[c]) if (!removed[v]) decompose(v, c);
}

// For path queries: for each query (u,v), answer = f(u,c) + f(v,c) - f(root,c)
// for each centroid c on the path from u to root of centroid tree
```

**Applications**: Count paths with given length/sum, distance queries on trees.

**Practice**: 15 centroid decomposition problems

---

### Module 4 — Heavy-Light Decomposition (HLD) (Month 3–4)

**Core idea**: Partition tree into O(log n) "heavy" chains. Any path in the tree is covered by O(log n) chains. Apply segment tree to each chain → path queries in O(log² n).

```cpp
// HLD implementation
vector<int> parent(n), depth(n), heavy(n,-1), head(n), pos(n);
int cur_pos = 0;

int dfs(int u, int p, int d) {
    parent[u]=p; depth[u]=d; int size=1, max_size=0;
    for (int v : adj[u]) if (v != p) {
        int child_size = dfs(v,u,d+1); size += child_size;
        if (child_size > max_size) { max_size=child_size; heavy[u]=v; }
    }
    return size;
}

void decompose(int u, int h) {
    head[u]=h; pos[u]=cur_pos++;
    if (heavy[u] != -1) decompose(heavy[u], h); // continue heavy chain
    for (int v : adj[u]) if (v != parent[u] && v != heavy[u]) decompose(v,v);
}

// Path query from u to v: walk up chains, query segment tree on each chain
long long path_query(int u, int v) {
    long long result = 0;
    while (head[u] != head[v]) {
        if (depth[head[u]] < depth[head[v]]) swap(u,v);
        result += seg.query(pos[head[u]], pos[u]); // segment tree query
        u = parent[head[u]];
    }
    if (depth[u] > depth[v]) swap(u,v);
    result += seg.query(pos[u], pos[v]);
    return result;
}
```

**Applications**: Path sum/min/max queries with updates, LCA in O(log n), subtree updates.

**Practice**: 15 HLD problems

---

### Module 5 — Link-Cut Trees (Month 4–5)

LCT supports dynamic tree operations in O(log n) amortized: link (add edge), cut (remove edge), path queries.

**Core**: Maintain "preferred paths" using splay trees. Each node's splay key is its depth in the represented tree.

```cpp
// Full Link-Cut Tree with path XOR aggregate
// Supports: link(u,v), cut(u,v), connected(u,v), path_query(u,v), update(u,val)
struct LCT {
    struct Node {
        int ch[2], p, val, agg;
        bool rev;
    };
    vector<Node> t;

    LCT(int n) : t(n+1, {{0,0}, 0, 0, 0, false}) {}

    bool isRoot(int x) { return t[t[x].p].ch[0]!=x && t[t[x].p].ch[1]!=x; }
    bool dir(int x) { return t[t[x].p].ch[1]==x; }
    void push_up(int x) { t[x].agg = t[t[x].ch[0]].agg ^ t[x].val ^ t[t[x].ch[1]].agg; }

    void flip(int x) { swap(t[x].ch[0], t[x].ch[1]); t[x].rev^=1; }
    void push_down(int x) {
        if (t[x].rev) {
            if (t[x].ch[0]) flip(t[x].ch[0]);
            if (t[x].ch[1]) flip(t[x].ch[1]);
            t[x].rev=0;
        }
    }

    void rotate(int x) {
        int y=t[x].p, z=t[y].p, k=dir(x);
        if (!isRoot(y)) t[z].ch[dir(y)]=x; t[x].p=z;
        t[y].ch[k]=t[x].ch[k^1];
        if (t[x].ch[k^1]) t[t[x].ch[k^1]].p=y;
        t[x].ch[k^1]=y; t[y].p=x;
        push_up(y); push_up(x);
    }

    void splay(int x) {
        static int stk[500005]; int top=0;
        stk[top++]=x;
        for (int i=x; !isRoot(i); i=t[i].p) stk[top++]=t[i].p;
        while (top) push_down(stk[--top]);
        while (!isRoot(x)) {
            int y=t[x].p;
            if (!isRoot(y)) rotate(dir(x)==dir(y)?y:x);
            rotate(x);
        }
    }

    void access(int x) {
        int last=0;
        for (int y=x; y; y=t[y].p) { splay(y); t[y].ch[1]=last; push_up(y); last=y; }
        splay(x);
    }

    void makeRoot(int x) { access(x); flip(x); }

    int findRoot(int x) {
        access(x);
        while (t[x].ch[0]) { push_down(x); x=t[x].ch[0]; }
        splay(x); return x;
    }

    void link(int x, int y) {
        makeRoot(x);
        if (findRoot(y)!=x) t[x].p=y; // only link if different components
    }

    void cut(int x, int y) {
        makeRoot(x); access(y);
        if (t[y].ch[0]==x && !t[x].ch[1]) { t[y].ch[0]=0; t[x].p=0; push_up(y); }
    }

    bool connected(int x, int y) { makeRoot(x); return findRoot(y)==x; }

    int query(int x, int y) { makeRoot(x); access(y); return t[y].agg; } // XOR on path x→y

    void update(int x, int v) { splay(x); t[x].val=v; push_up(x); }
};
// Usage: LCT lct(n); lct.update(u, val); lct.link(u,v); lct.cut(u,v); lct.query(u,v);
// Aggregate can be changed from XOR to sum/min/max by modifying push_up and agg field
```

**Operations and complexity**:
- `access(x)`: O(log n) amortized — makes x the root of its splay tree and connects it to the true root
- `makeRoot(x)`: O(log n) — re-roots the represented tree at x (uses path reversal via `rev` flag)
- `link(u,v)`: O(log n) — adds edge between two different trees
- `cut(u,v)`: O(log n) — removes edge (u,v) from tree
- `query(u,v)`: O(log n) — aggregate (XOR here; can be sum/min/max) on path from u to v

**Where to practice**:
- USACO Guide Platinum → Link-Cut Trees: https://usaco.guide/plat/link-cut-tree
- CSES "Dynamic Tree Connectivity": https://cses.fi/problemset/task/2236
- Codeforces problems tagged "link cut tree" (rated 2700+)

**Practice**: 10 LCT problems (difficult — take your time)

---

### Module 6 — Suffix Array & LCP (Month 5)

**Suffix Array (O(n log² n) construction via prefix doubling)**

**Suffix Array (SA)**: Sorted array of all suffixes of string S. SA[i] = start of the i-th lexicographically smallest suffix.

```cpp
// Prefix doubling method: O(n log² n) — sufficient for USACO (n ≤ 10^5 strings)
// SA-IS is O(n) but significantly harder to implement correctly; not required for USACO
vector<int> suffix_array(string s) {
    s += '$'; int n = s.size();
    vector<int> sa(n), rank_(n), tmp(n);
    iota(sa.begin(),sa.end(),0);
    for (int i=0;i<n;i++) rank_[i]=s[i];
    for (int k=1;k<n;k<<=1) {
        auto cmp=[&](int a, int b){
            if(rank_[a]!=rank_[b]) return rank_[a]<rank_[b];
            int ra=a+k<n?rank_[a+k]:-1, rb=b+k<n?rank_[b+k]:-1;
            return ra<rb;
        };
        sort(sa.begin(),sa.end(),cmp);
        tmp[sa[0]]=0;
        for(int i=1;i<n;i++) tmp[sa[i]]=tmp[sa[i-1]]+(cmp(sa[i-1],sa[i])?1:0);
        rank_=tmp;
        if(rank_[sa[n-1]]==n-1) break;
    }
    return vector<int>(sa.begin()+1, sa.end()); // remove sentinel
}
```

**LCP Array** (Kasai's algorithm, O(n)):
```cpp
// lcp[i] = length of longest common prefix of SA[i] and SA[i-1]
vector<int> build_lcp(string s, vector<int>& sa) {
    int n=s.size(); vector<int> rank_(n), lcp(n);
    for(int i=0;i<n;i++) rank_[sa[i]]=i;
    for(int i=0,h=0;i<n;i++) {
        if(rank_[i]>0) {
            int j=sa[rank_[i]-1];
            while(i+h<n&&j+h<n&&s[i+h]==s[j+h]) h++;
            lcp[rank_[i]]=h;
            if(h) h--;
        }
    }
    return lcp;
}
```

**Suffix Array applications (with concrete examples)**:
- *Longest repeated substring*: max value in LCP array = length of longest repeated substring. The substring is `s.substr(SA[argmax(LCP)], max(LCP))`.
- *Number of distinct substrings*: `n*(n+1)/2 - sum(LCP)`. Each suffix contributes `(n - SA[i])` total substrings, minus the `LCP[i]` that it shares with the previous suffix.
- *Longest common substring of two strings A and B*: concatenate as `A + "#" + B`, build SA+LCP; find max LCP[i] where SA[i] and SA[i-1] are from different strings.
- *Check if pattern P occurs in text T*: binary search on SA to find first suffix ≥ P and verify.

**Applications**: Longest repeated substring, number of distinct substrings (= n*(n+1)/2 - sum(lcp)), longest common substring of 2 strings, string compression.

**Practice**: 15 suffix array problems — CSES String Algorithms section (last 8 problems)

---

### Module 7 — Suffix Automaton (SAM) (Month 5–6)

The most space-efficient structure for all substrings: O(n) nodes, O(n) construction.

```cpp
struct SAM {
    struct State { int len, link; map<char,int> next; };
    vector<State> st; int last;
    SAM() { st.push_back({0,-1,{}}); last=0; }
    void extend(char c) {
        int cur=st.size(); st.push_back({st[last].len+1,-1,{}});
        int p=last;
        while(p!=-1&&!st[p].next.count(c)){ st[p].next[c]=cur; p=st[p].link; }
        if(p==-1) st[cur].link=0;
        else {
            int q=st[p].next[c];
            if(st[p].len+1==st[q].len) st[cur].link=q;
            else {
                int clone=st.size(); st.push_back({st[p].len+1,st[q].link,st[q].next});
                while(p!=-1&&st[p].next[c]==q){ st[p].next[c]=clone; p=st[p].link; }
                st[q].link=st[cur].link=clone;
            }
        }
        last=cur;
    }
};
```

**Applications**: Count occurrences of each substring, longest common substring, number of distinct substrings (for each state: `st[v].len - st[st[v].link].len` distinct substrings end at this state).

**Practice**: 10 SAM problems

---

### Module 8 — Advanced Combinatorics and FFT (Month 6–7)

#### 8A. Fast Fourier Transform (FFT)

```cpp
// Multiply two polynomials in O(n log n) using FFT
// Applications: polynomial multiplication, string convolution, count pairs
using cd = complex<double>;
void fft(vector<cd>&a, bool inv) {
    int n=a.size();
    for(int i=1,j=0;i<n;i++){
        int bit=n>>1;
        for(;j&bit;bit>>=1) j^=bit;
        j^=bit; if(i<j) swap(a[i],a[j]);
    }
    for(int len=2;len<=n;len<<=1){
        double ang=2*M_PI/len*(inv?-1:1);
        cd wlen(cos(ang),sin(ang));
        for(int i=0;i<n;i+=len){
            cd w(1);
            for(int j=0;j<len/2;j++,w*=wlen){
                cd u=a[i+j], v=a[i+j+len/2]*w;
                a[i+j]=u+v; a[i+j+len/2]=u-v;
            }
        }
    }
    if(inv) for(auto&x:a) x/=n;
}
```

**Critical precision caveat**: The FFT using `complex<double>` accumulates floating-point errors. For large polynomials (degree > 10^5) or when coefficients are large integers, the rounded output may be wrong. Rule of thumb:
- If the answer requires exact integers AND coefficients × polynomial degree > 10^15, use NTT (Number Theoretic Transform) instead
- NTT uses modular arithmetic (exact) at the cost of only working modulo a specific prime

**When to use FFT vs NTT**:
- Need exact integer output for polynomial multiplication → use NTT with MOD = 998244353
- Floating-point answer is acceptable (e.g., convolution for probability) → FFT is fine
- Input values are small (< 1000) and n < 200,000 → FFT is usually safe

**NTT** (Number Theoretic Transform): Same as FFT but with modular arithmetic. Use MOD = 998244353 (has primitive root 3).

#### 8B. Linear Basis (XOR Basis)

```cpp
// Insert numbers into a linear basis (over GF(2))
// Supports: max XOR of a subset, k-th smallest XOR subset
struct LinearBasis {
    vector<long long> basis;
    bool insert(long long x) {
        for (long long b : basis) x = min(x, x^b);
        if (x) { basis.push_back(x); for(int i=basis.size()-1;i>0&&basis[i]>basis[i-1];i--) swap(basis[i],basis[i-1]); }
        return x != 0;
    }
    long long max_xor() {
        long long res=0;
        for (long long b : basis) res = max(res, res^b);
        return res;
    }
};
```

#### 8C. Sprague-Grundy Theorem

- Grundy number (nimber) of a game state = mex of Grundy numbers of states reachable in one move
- Combined game: XOR of Grundy numbers of components
- Applications: Nim variants, token-moving games on graphs

**Practice**: 20 math/FFT problems

---

### Module 9 — Advanced Graph Techniques (Month 7–8)

#### 9A. 2-SAT

Solve satisfiability for clauses of form (x OR y) where x,y are literals.

```cpp
// Build implication graph: (x OR y) = (~x → y) AND (~y → x)
// Run SCC; if x and ~x are in same SCC → unsatisfiable
// Otherwise: x = true iff comp[x] > comp[~x] in topological order
```

#### 9B. Min Cost Max Flow

```cpp
// SPFA-based MCMF: find augmenting path with minimum cost using Bellman-Ford/SPFA
// Each augmenting path: O(VE) with SPFA, better in practice
// Alternatively: Johnson's algorithm / potential-based Dijkstra for non-negative reduced costs
```

#### 9C. Dominator Trees

For a directed graph with source s, t dominates u if every path from s to u passes through t.

#### 9D. Offline Dynamic Connectivity

Using CDQ divide and conquer to handle insertions and deletions of edges offline.

**Practice**: 20 advanced graph problems from oj.uz + Codeforces

---

### Module 10 — IOI-Specific Training (Months 8–12)

#### What IOI Requires Beyond USACO Platinum

IOI problems have a different flavor than USACO:
- **Partial scoring by subtasks**: Each problem has 5–10 subtasks, each worth different points. A solution that handles n ≤ 1000 might score 20–40 points. ALWAYS solve the easy subtasks first.
- **Interaction problems**: Your solution communicates with a judge by calling library functions. Requires different thinking.
- **Output-only problems**: Given fixed input files, submit output files. Explore heuristics.
- **C++ is the only practical language**: At IOI, you must use C++ for any time-critical solution.

#### IOI Problem Strategy

```
For each problem:
1. Read carefully; understand ALL subtasks (not just full constraints)
2. Implement brute force for smallest subtasks first (often 10–20 points for n ≤ 100)
3. Look for structural observations (tree DP, greedy, divide and conquer)
4. Implement each subtask's solution and verify against brute force
5. Combine for full solution if possible

Time allocation per 5-hour day:
- All 3 problems: 20 minutes reading
- Choose your best problem: ~2h first pass
- Second problem: ~1.5h
- Third: ~1h partial solutions
- Buffer for debugging: 30 min
```

#### IOI Past Problems Practice

Work through these problems from oj.uz (chronological within difficulty):

**IOI 2024 (Alexandria)**:
- Nile, Sphinx's Riddle, Message (interactive), Competition, Mosaic, Tree

**IOI 2023 (Szeged)**:
- Overtaking, Longest Trip (interactive), Soccer Stadium, Packing Biscuits, Sequence, Closing Time

**IOI 2022 (Yogyakarta)**:
- Catfish Farm, Unscrambling a Sequence, Prison Break (interactive), Rarest Coincidence, Circuit, Thousands Islands

**IOI 2021 (remote)**:
- Dungeons Game, Bit Shift Registers, Keys, Radio Towers, Vision Program, Fountain Parks

**IOI 2020 (remote)**:
- Comparing Shovels, Carnival Tickets, Packing Biscuits, Connecting Supertrees, Crocodile's Underground City, Teams

**Tip**: Use oj.uz to submit and get immediate feedback on subtasks. Compare your approach with editorial solutions after solving.

---

### Month-by-Month Training Plan

| Month | Topics | Target Problems |
|-------|--------|----------------|
| 1 | Advanced segment trees (beats, persistent, Li Chao) | 30 |
| 2 | DP optimization (CHT, D&C DP, Knuth, WQS) | 35 |
| 3 | Centroid decomposition + HLD | 30 |
| 4–5 | LCT + suffix structures (SA, LCP, SAM) | 35 |
| 5–6 | FFT/NTT, linear basis, Grundy, advanced math | 25 |
| 7–8 | 2-SAT, MCMF, advanced graph techniques | 25 |
| 8–12 | IOI problems: 2020–2024 (all problems) | 60+ |

**Plus**: Codeforces Div. 1 D/E (rated 2800+) weekly; AtCoder Grand Contests C/D/E

---

## Weekly Schedule (Platinum/IOI)

| Day | Activity | Duration |
|-----|----------|----------|
| Monday | New algorithm study + implementation | 3h |
| Tuesday | 5–7 targeted problems on current topic | 3.5h |
| Wednesday | oj.uz IOI problem (past problem) | 3h |
| Thursday | Codeforces Div. 1 C/D (rated 2500–3000) | 3h |
| Friday | AtCoder Grand Contest (AGC) A–C | 3h |
| Saturday | 5-hour IOI simulation (pick a year's Day 1) | 5h |
| Sunday | Editorial review + re-implement all problems missed | 1.5h |

**Total**: ~22 hours/week

---

## IOI Selection Process (USA)

### Path to IOI Team

1. **Reach Platinum** (prerequisite — you cannot attend camp without Platinum)

2. **US Open Performance** (April/May)
   - The most important USACO contest for team selection
   - Top ~25 scorers across Platinum are invited to Training Camp
   - Some invitations based on full-year performance, not just US Open

3. **USACO Training Camp** (June, 1 week, in-person)
   - ~25–30 students attend
   - Run ~4 selection contests (IOI format: 5 hours, 3 problems, partial scoring)
   - Cumulative score determines final ranking
   - Top 4 are selected as the USA IOI team

4. **IOI Competition** (July)
   - 2 days × 5 hours = 6 problems total
   - Each problem: 100 points (600 total possible)
   - Partial credit for subtasks
   - C++ strongly preferred (Java/Python have severe TLE risk)
   - IOI 2025: Almaty, Kazakhstan

### How to Get Invited to Camp

- Be in top 25 at US Open (competitive)
- Strong performance throughout the year builds your reputation with USACO coaches
- The camp invitation is also influenced by performance in December/January/February contests

---

## Key Resources for Platinum/IOI

| Resource | URL | Why Critical |
|----------|-----|-------------|
| USACO Guide Platinum | https://usaco.guide/platinum/ | Authoritative platinum curriculum |
| oj.uz | https://oj.uz/ | All IOI/CEOI/JOI/Baltic problems with judge |
| CSES Advanced Techniques | https://cses.fi/problemset/ | Advanced algorithms section |
| CP-Algorithms | https://cp-algorithms.com/ | All algorithm implementations + proofs |
| Codeforces Div. 1 | https://codeforces.com/problemset | D/E problems (rated 2800–3500) |
| AtCoder Grand Contests | https://atcoder.jp/contests/agc | AGC C/D/E — world-class problems |
| IOI Official Tasks | https://ioi.org/tasks/ | All IOI problems 1989–present |
| Competitive Programmer's Handbook | https://cses.fi/book/ | Chapters 25–30 (advanced) |
| Benq's CP templates | https://github.com/bqi343/cp-notebook | Reference templates from USACO #1 ranker |

---

## USACO Training Camp Preparation

The camp selection exams are IOI-style (5 hours, 3 problems, partial scoring). Specific preparation:

1. **IOI editorial reading**: For every IOI problem from 2018–2024, read the editorial after attempting. Understand the key observations.

2. **Partial scoring mindset**: Never leave a problem blank. If you can't solve it fully, implement the O(n³) brute force and get partial credit.

3. **Implementation speed**: At IOI/camp level, you need to implement complex algorithms (HLD+segment tree, centroid+map, SAM) quickly. Practice writing them from scratch repeatedly.

4. **Clean debugging**: With 5 hours and complex problems, debugging time is precious. Use `assert()`, print intermediate states, compare with brute force on small cases.

---

## Success Benchmarks

| Level | Benchmark |
|-------|-----------|
| Platinum Entry | Can implement lazy segment tree and Dijkstra in under 20 min each |
| Platinum Mid | Can implement HLD + centroid decomposition from scratch |
| Platinum Strong | Can solve all CSES 300 problems; Codeforces rating ≥ 2500 |
| IOI Camp | Top 25 at USACO US Open; can solve 2–3 IOI subtasks on unfamiliar problems |
| IOI Team | Top 4 at camp; expected IOI bronze/silver |

---

*Primary sources: usaco.guide, ioi.org, oj.uz, cses.fi*
*Last updated: April 2026*
