# Contest Strategy Guide
## World-Class Competitive Programming Techniques — USACO, Codeforces, AtCoder, IOI

---

## USACO Contest Facts

- **4 contests per year**: December, January, February, US Open (April/May)
- **Duration**: 4 hours each
- **Format**: 3 problems, open book (can use internet, notes, templates)
- **Submissions**: Unlimited re-submissions within 4 hours — NO PENALTY for wrong answers
- **Feedback**: "All test cases passed" or count of failed test cases (no details on which ones fail)
- **Scoring**: Each problem worth ~333 points (total ~1000). Each problem has multiple test cases. Partial credit for solving a subset of test cases.
- **Promotion**: ~750/1000 threshold (varies per contest)
- **After contest**: All test inputs/outputs revealed. Study them.

---

## USACO Pre-Contest Checklist

### 1 Hour Before
- [ ] Test your compilation environment (template compiles, file I/O works)
- [ ] Review your standard template one more time
- [ ] Ensure `bits/stdc++.h` and `using namespace std` are ready
- [ ] Set up file I/O template: `freopen("problem.in","r",stdin); freopen("problem.out","w",stdout);`

### Right Before the Clock Starts
- [ ] Have scratch paper ready
- [ ] Have your reference sheet open (allowed during USACO)
- [ ] Browser tab open: usaco.org (for the problems)
- [ ] Clear your terminal, open IDE

---

## USACO During-Contest Strategy

### Minutes 0–15: Read All 3 Problems

**Do not skip this step.** Read all 3 problems before starting to code.

For each problem, note:
- **N constraint**: n ≤ 1000? n ≤ 100,000? n ≤ 10^6? This tells you the required algorithm complexity.
- **Time limit**: 2s or 4s?
- **What is being asked**: Output format, what counts as a valid answer

Estimate relative difficulty. USACO problems are (generally) ordered easy → hard, but not always. Sometimes problem 2 is easier than problem 1.

### Complexity → Algorithm Mapping

| Constraint | Maximum Algorithm Complexity |
|------------|---------------------------|
| n ≤ 10 | O(n!) — brute force, all permutations |
| n ≤ 20 | O(2^n * n) — bitmask DP |
| n ≤ 200 | O(n³) — Floyd-Warshall, O(n³) DP |
| n ≤ 5,000 | O(n²) — double nested loop |
| n ≤ 100,000 | O(n log n) — sorting, binary search, segment tree |
| n ≤ 1,000,000 | O(n) or O(n log log n) — linear algorithms only |

**Use this table immediately when reading constraints.** If n = 200,000, your O(n²) idea is wrong.

### Time Allocation

```
Total: 4 hours = 240 minutes

Standard (solve all 3):
  0:00 – 0:15   Read all 3 problems
  0:15 – 1:00   Problem 1 (easiest): 45 min
  1:00 – 2:15   Problem 2 (medium): 75 min
  2:15 – 3:30   Problem 3 (hardest): 75 min
  3:30 – 4:00   Buffer: debug, improve partial solutions

If stuck on problem 3:
  3:00 – 4:00   Implement partial solution for problem 3 (brute force subtasks)
                Submit brute force: may score 100–300 partial points

Never spend the full 4 hours on 1 problem.
```

### Per-Problem Workflow

1. **Understand the problem** (5–10 min)
   - Re-read carefully
   - Trace through the sample input manually on paper
   - What are edge cases? (n=1, all same values, empty cases)

2. **Design algorithm** (5–10 min, think before typing)
   - What's the time complexity required? (from constraints)
   - What algorithmic approach fits? (DP? greedy? BFS? binary search?)
   - If not sure, start with brute force and think about optimizing

3. **Code** (15–40 min depending on problem)
   - Follow your template; don't reinvent file I/O or standard includes
   - Write clean code — debugging messy code costs much more time
   - Comment major sections for your own clarity under pressure

4. **Test on sample** (5 min)
   - Must match sample output before submitting
   - If wrong, trace through logic on the sample manually

5. **Edge cases** (5 min)
   - n=1, n=2
   - All elements equal
   - Minimum/maximum possible values
   - Overflow? Use `long long` if products might exceed 2×10^9

6. **Submit** — then continue to next problem

### Partial Credit Strategy

USACO scores each test case independently. If you can't solve a problem fully:
- Implement the O(n²) brute force even if it only handles n ≤ 5000 (might score 300–600/1000 on a problem)
- Add special-case code for small inputs: `if (n <= 100) brute_force(); else fast_solution();`
- Submit the partial solution, then continue working on the full solution
- Any points from partial credit could determine whether you promote

---

## Stress Testing (Critical Technique)

Stress testing is how you find bugs when your solution passes samples but fails on judge.

**Setup**:
```
brute.cpp   — correct O(n²) or O(n³) solution (easy to write, definitely correct)
fast.cpp    — your optimized solution
gen.py      — random test case generator
```

**gen.py example**:
```python
import random, sys

n = random.randint(1, 10)
a = [random.randint(1, 100) for _ in range(n)]
print(n)
print(*a)
```

**stress.sh**:
```bash
#!/bin/bash
while true; do
    python3 gen.py > test.in
    ./brute < test.in > expected.out
    ./fast  < test.in > actual.out
    if ! diff -q expected.out actual.out > /dev/null; then
        echo "DIFFERENCE FOUND!"
        echo "Input:"
        cat test.in
        echo "Expected:"
        cat expected.out
        echo "Actual:"
        cat actual.out
        exit 1
    fi
done
```

**When to stress test**: Any time your solution gives WA (wrong answer) on the judge but passes your sample. This finds bugs in 5–15 minutes that manual testing might never find.

---

## Post-Contest Analysis

After every USACO contest, within 24–48 hours:

1. **Check all test cases**: USACO releases all inputs/outputs after the contest. Run your code on them.

2. **Read the official editorial**: usaco.org posts editorials. Even for problems you solved, read the editorial — there's often a cleaner approach.

3. **Re-implement problems you couldn't solve**: Write the correct solution from scratch using the editorial. This is where you actually learn.

4. **Categorize your errors**:
   - Was the algorithm wrong? (need more algorithm study)
   - Was the implementation buggy? (need more coding practice)
   - Did you run out of time? (need speed drills)
   - Did you misread the problem? (need more careful reading)

5. **Add to your weak areas list**: If you couldn't figure out a problem's approach, add that topic to study.

---

## Codeforces Contest Strategy

### Format
- **Div. 2**: 2 hours, 5–6 problems. A = easy (5–10 min), B = easy (10–20 min), C = medium, D = harder, E/F = hard
- **Div. 1**: 2.5 hours, 5–6 problems. All problems are hard.
- **Educational rounds**: Slower pacing, hacking not allowed, editorials published same day

### Scoring
Codeforces uses dynamic scoring — each problem starts at a maximum point value (500, 1000, 1500, 2000, 2500, 3000) and decreases as time passes. Solving faster earns more points.

**Wrong submission penalty**: Each wrong submission on a problem deducts **50 points from that problem's score** at the time you eventually solve it. This is not a time penalty — the deduction applies when (and only if) you solve the problem. If you never solve a problem, wrong submissions on it don't affect your score at all.

**Hacking** (Div. 1/2 rounds, during the 12-hour hack phase after the contest): You can submit a counterexample against another contestant's solution. A successful hack gives +100 points; a failed hack attempt costs −50 points. Hacking is entirely separate from the wrong-submission penalty above.

**Round types and their scoring rules**:
- **Div. 1/2 rounds**: Dynamic scoring + 50-pt per WA (when solved) + hacking phase
- **Educational rounds**: Fixed scoring, no hacking, no wrong-submission penalty — safe to try-and-see
- **Div. 3 / Div. 4 rounds**: Fixed scoring, no hacking
- **Global rounds**: Dynamic scoring, hacking

**Practical implication**: In Div. 1/2, do NOT spam wrong submissions. Each WA on A costs you 50 pts if you eventually solve A. But if you're confident in your solution, don't over-test — time lost is also points lost.

### Strategy
- **A + B fast** (target < 15 min combined): These are easy. Speed matters for rating.
- **C problem**: Central challenge of Div. 2. Spend 20–30 min. If stuck, skip to D.
- **D/E**: Harder problems — attempt if time allows

### Minimize Wrong Submissions
Unlike USACO, Codeforces penalizes wrong submissions (in Div. 1/2 rounds):
- Test locally with sample cases AND manually-crafted edge cases before submitting
- If 80% confident, submit — but trace through edge cases first
- In Educational rounds, the penalty rule doesn't apply — safer to submit speculatively
- Use `assert()` statements for debugging locally, remove before submission

---

## AtCoder Contest Strategy

### ABC (AtCoder Beginner Contest)
**Duration**: 100 minutes, 6–8 problems
- A/B: trivial (2–5 min each)
- C: simple implementation or algorithm (5–10 min)
- D: first real problem — medium difficulty (15–25 min) [Silver equivalent]
- E: harder — Gold equivalent (20–40 min)
- F/G: expert level [Platinum equivalent]

**Strategy**: Solve A–D as fast as possible, then attempt E. F/G require Platinum+ level skill.

### ARC (Regular Contest)
**Duration**: 100 minutes, 6 problems
- A/B: medium (Silver/Gold)
- C/D: hard (Platinum)
- E/F: expert (IOI level)

### AGC (Grand Contest)
**Duration**: 110–180 minutes, 6 problems
- All problems are hard. A/B = Gold/Platinum, C/D/E/F = IOI level.
- Recommended for: Platinum+ competitors who want IOI-level challenges

---

## IOI Contest Strategy

### Format
- 2 days × 5 hours = 10 hours total
- 3 problems per day, 6 problems total
- Each problem: 100 points, 5–10 subtasks
- Partial credit: solve subtasks in increasing difficulty

### IOI Scoring Mindset

**NEVER leave a problem blank.** Even a brute force O(n³) solution scores 10–30 points on small subtasks. A blank submission scores 0.

For each problem:
1. Read all subtasks first: understand what the easy subtasks ask
2. Implement brute force: get subtask 1 (n ≤ 100 or similar) for 5–20 points
3. Look for the key structural insight for harder subtasks
4. Implement each subtask's optimized solution

### IOI Time Allocation (5 hours)

```
0:00 – 0:20   Read ALL 3 problems; read all subtasks
0:20 – 1:00   Implement brute forces for all 3 problems (subtask 1 each)
1:00 – 2:30   Deep solve on your strongest problem (target: full score)
2:30 – 4:00   Second problem: solve as many subtasks as possible
4:00 – 4:40   Third problem: maximize remaining subtask points
4:40 – 5:00   Final review, double-check submissions
```

**Day 2**: After Day 1 results, you know exactly how many points you have. Adjust strategy: if already near medal threshold, be conservative; if far below, take risks on harder subtasks.

### IOI Implementation Tips

- Use `#include <bits/stdc++.h>` but note IOI problems often use custom headers (`grader.h`, etc.)
- For interaction problems: flush output after each response: `cout << answer << "\n"; cout.flush();`
- Memory: IOI usually allows 512 MB or 1 GB. Be careful with large arrays.
- `assert()` for debugging in practice; remove for contest (asserts slow down code)
- For partial scoring: implement solution per subtask with explicit `if (n <= 100)` guards

---

## Mental Performance Under Pressure

### Before the Contest
- Sleep 8+ hours the night before
- Light meal, stay hydrated
- 30-minute review of your template and key algorithms (don't cram new material)

### During the Contest
- **When stuck**: Take a 2-minute break, breathe, re-read the problem from scratch. Often you misunderstood something.
- **When panicking**: Focus only on the current problem. Block out awareness of remaining time.
- **When approaching time limit**: Switch to partial credit mode. A brute force that scores 200/333 is better than nothing.
- **Keep a scratch paper log**: Write down your key observations and invariants. This prevents going in circles.

### After the Contest
- Don't check standings/others' solutions until you've submitted your final answers.
- If you didn't perform well: that's normal. Every top competitor has bad contests. Analyze and move on.

---

## Code Templates

### Standard C++ Template (USACO)
```cpp
#pragma GCC optimize("O2")
#include <bits/stdc++.h>
using namespace std;

using ll = long long;
using pii = pair<int,int>;
using vi = vector<int>;
using vl = vector<ll>;

const int MOD = 1e9 + 7;
const int INF = 1e9;
const ll LINF = 1e18;

void setIO(string name = "") {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    if (name.size()) {
        freopen((name+".in").c_str(), "r", stdin);
        freopen((name+".out").c_str(), "w", stdout);
    }
}

int main() {
    setIO("problem_name"); // change to actual problem name
    int n; cin >> n;
    // solution here
}
```

### Useful Macros
```cpp
#define all(v) (v).begin(), (v).end()
#define rep(i,a,b) for(int i=a; i<(b); i++)
#define pb push_back
#define fi first
#define se second
#define sz(v) ((int)(v).size())
```

### Debug Output (remove before submission)
```cpp
#define DEBUG
#ifdef DEBUG
    #define dbg(x) cerr << #x << " = " << x << "\n"
    #define dbg2(x,y) cerr << #x << " = " << x << ", " << #y << " = " << y << "\n"
#else
    #define dbg(x)
    #define dbg2(x,y)
#endif
```

---

## Contest Calendar

| Month | Recommended Contests |
|-------|---------------------|
| Every week | Codeforces rounds (participate in Div. 2 or Div. 1 based on level) |
| Every week | AtCoder ABC (Sunday evenings JST) |
| Monthly | AtCoder ARC (once or twice per month) |
| December | USACO December contest |
| January | USACO January contest |
| February | USACO February contest |
| March | AtCoder AGC (for Platinum+) |
| April/May | USACO US Open (most important!) |
| June | USACO Training Camp (if invited) |
| July | IOI (if selected for team) |

---

*Last updated: April 2026*
