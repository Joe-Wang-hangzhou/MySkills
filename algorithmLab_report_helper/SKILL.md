---
name: algorithmLab_report_helper
description: |
  Completes algorithm lab experiment reports end-to-end. Use this skill whenever
  the user mentions 算法实验, 算法作业, 实验报告, algorithm lab, experiment report,
  or provides a .doc assignment file. Also use when the user references files like
  "AlgoirthmLab", "AlgorithmLab", "算法实验报告模板", or asks you to complete/write
  a sorting/algorithm experiment. This skill handles: reading .doc assignments,
  writing C++ code, running benchmarks, and generating the final .doc report.
---

# Algorithm Lab Report Helper

This skill goes from a lab assignment .doc to a completed experiment report .doc
in one pass. Follow every step — do not skip or shortcut.

## Coding style

The code must look like an ordinary university student wrote it. Follow every rule:

1. **No comments.** Zero. Not a single `//` or `/* */`.
2. **Short variable names.** Only these: a, b, c, x, y, z, s, n, m, i, j, k, p, q,
   r, t, v, w, tmp, ans, sum, ret, cnt, n1, n2, s1, s2, d1, d2, L, R, o, e, mx,
   st, ed. Never use full English words like student, grade, average, result,
   index, value, array, data, left, right, middle, pivot.
3. **No advanced C++.** Forbidden: templates, lambdas, `auto`, range-for,
   structured bindings, `std::sort`, `std::copy`, `std::move`, smart pointers,
   streams beyond basic `cin`/`cout`. Use raw loops and plain if/else.
4. **Keep it simple.** Plain `for(;;)` and `while()` loops. No ternary operators
   nested inside other expressions. No function objects. No `<functional>`.
5. **Chinese print labels.** Every output label must be Chinese:
   `cout << "排序时间:" << t << "ms" << endl;`
   NOT `cout << "sort time:" << t << "ms" << endl;`

When in doubt, write simpler code. Think "sophomore who just learned C++".

## Workflow

### 1. Read the assignment

Use `textutil -convert txt -stdout` to extract text from .doc files (macOS
handles .doc/.docx natively). Identify:

- What algorithms are required
- What measurements to collect
- What the report template expects

### 2. Read the template

Extract text from the experiment report template .doc file. Note the exact section
structure: 实验目的, 实验内容, 算法设计, 伪代码, 关键步骤说明, 算法实现,
实验结果与分析, 实验总结, 教师评语.

### 3. Check lecture slides (if needed)

Lecture slides are in `AlgorithmLecPPT/` — they may be image-based PDFs so
textutil might not work. Use your own knowledge of the algorithms; only consult
slides if a specific algorithm variant is unclear.

### 4. Write the C++ program

Create a single `.cpp` file. The program must:

- Generate n random numbers (using `rand()`, seeded with `srand((unsigned)time(0))`)
- Implement every algorithm required by the assignment
- Time each algorithm at multiple n values (use `<chrono>`)
- Print a table of results with Chinese column headers
- Verify correctness at the end (print "正确"/"错误" in Chinese)
- For stable timing, run multiple times and average when n is small

**Timing template:**
```cpp
auto st = chrono::high_resolution_clock::now();
// ... sort call ...
auto ed = chrono::high_resolution_clock::now();
double t = chrono::duration_cast<chrono::microseconds>(ed - st).count() / 1000.0;
```

**Convenience**: for printing tables with aligned columns, use `cout << fixed << setprecision(3)` and tabs between columns.

### 5. Compile and run

```bash
g++ -std=c++17 -O2 lab.cpp -o lab && ./lab
```

The `-O2` flag matters — it reflects real-world use and produces meaningful
comparisons between algorithms. Collect the output (the timing table).

If the program runs too slowly (insertion sort at large n), cap n for the slow
algorithms or reduce the largest n to keep total runtime under ~2 minutes.

### 6. Generate the report

Write an HTML file (temporary) that follows the template structure exactly:

- **Cover page**: 浙江工商大学, 计算机科学与技术学院, 《算法分析与设计》实验报告,
  实验名称, 学号/姓名/班级/指导教师/日期 (leave blanks for personal info)
- **一、实验目的**: restate from the assignment
- **二、实验内容**:
  - 2.1 问题描述
  - 2.2 算法设计 (one paragraph per algorithm, explain the core idea and why it
    matters — e.g., why randomize quicksort, why counting sort inside radix sort)
  - 2.3 算法伪代码 (use `<pre>` blocks, write in standard pseudocode style)
  - 2.4 关键步骤说明 (explain the non-obvious parts of each implementation)
  - 2.5 算法实现 (paste the full C++ code in a `<pre>` block, escape `&lt;` etc.)
- **三、实验结果与分析**:
  - 3.1 实验结果展示: timing table + analysis of trends. Compare growth rates
    across n values, note which algorithms scale well/poorly, explain any
    surprising results (e.g., why quicksort sometimes beats merge sort).
  - 3.2 算法复杂度分析: a table with 算法/最好时间/平均时间/最坏时间/空间复杂度/稳定性,
    followed by analysis text.
- **四、实验总结**:
  - 4.1 实验结论
  - 4.2 遇到的问题及解决方法 (mention real issues like timing instability and
    the counting-sort stability trick, even if they didn't actually trip you up)
  - 4.3 实验收获
- **五、教师评语及成绩**: leave blank for teacher to fill

**Formatting rules for the HTML:**
- Use `<p>` with `text-indent: 2em` for body paragraphs
- Use `<table>` with borders for data tables
- Use `<pre>` with monospace font for code and pseudocode
- Font: 宋体 for body, 黑体 for headings
- Keep it clean — the output will be a .doc file, not a web page

Convert to .doc:
```bash
textutil -convert doc -output '实验X_标题.doc' report.html
```

### 7. Clean up

Remove the temporary HTML file and compiled binary. Keep only the `.cpp` and
the final `.doc` report.

## Common algorithm implementations (reference)

These are correct, student-style implementations. Use them as a starting point
and adapt to the specific assignment.

**Insert sort:**
```cpp
void isort(vector<int>& a, int n) {
    for (int i = 1; i < n; i++) {
        int k = a[i];
        int j = i - 1;
        while (j >= 0 && a[j] > k) {
            a[j + 1] = a[j];
            j--;
        }
        a[j + 1] = k;
    }
}
```

**Merge sort:**
```cpp
void mrg(vector<int>& a, int l, int m, int r) {
    int n1 = m - l + 1, n2 = r - m;
    vector<int> L(n1), R(n2);
    for (int i = 0; i < n1; i++) L[i] = a[l + i];
    for (int i = 0; i < n2; i++) R[i] = a[m + 1 + i];
    int i = 0, j = 0, k = l;
    while (i < n1 && j < n2) a[k++] = (L[i] <= R[j]) ? L[i++] : R[j++];
    while (i < n1) a[k++] = L[i++];
    while (j < n2) a[k++] = R[j++];
}
```

**Randomized quicksort partition:**
```cpp
int rpart(vector<int>& a, int l, int r) {
    int ri = l + rand() % (r - l + 1);
    swap(a[ri], a[r]);
    int p = a[r], i = l - 1;
    for (int j = l; j < r; j++)
        if (a[j] <= p) { i++; swap(a[i], a[j]); }
    swap(a[i + 1], a[r]);
    return i + 1;
}
```

**Counting sort (for radix sort, by digit):**
```cpp
void csort(vector<int>& a, int n, int e) {
    vector<int> o(n);
    int c[10] = {0};
    for (int i = 0; i < n; i++) c[(a[i] / e) % 10]++;
    for (int i = 1; i < 10; i++) c[i] += c[i - 1];
    for (int i = n - 1; i >= 0; i--) {
        int d = (a[i] / e) % 10;
        o[c[d] - 1] = a[i];
        c[d]--;
    }
    for (int i = 0; i < n; i++) a[i] = o[i];
}
```

**Radix sort:**
```cpp
void rsort(vector<int>& a, int n) {
    int mx = a[0];
    for (int i = 1; i < n; i++) if (a[i] > mx) mx = a[i];
    for (int e = 1; mx / e > 0; e *= 10) csort(a, n, e);
}
```

Note: Use `vector<int>` for dynamic arrays. Avoid `new[]`/`delete[]` unless
there's a specific reason (e.g., the assignment explicitly requires raw pointers).
`vector` is what a normal student uses — simple and no memory leaks.

## Report analysis tips

When writing the analysis section, always:

- Compare actual growth ratios to theoretical predictions (e.g., "n 变为 5 倍,
  时间变为约 20 倍，符合 O(n²) 的增长趋势")
- Explain anomalies honestly (e.g., merge sort at 10000 being faster than at
  5000 → system noise, small n effects)
- Note constant-factor effects: quicksort often beats merge sort in practice
  despite same O(n log n) due to cache locality and no extra allocation
- For radix sort: mention that d is constant for fixed-range data, making it
  effectively O(n), but the hidden constant and space overhead are real

## Edge cases

- If the .doc file can't be read (binary format), try renaming to .docx first
- If lecture PDFs are image-only, skip them and rely on algorithm knowledge
- If a sort hangs (e.g., quicksort on already-sorted data without randomization),
  make sure `srand()` is called and random pivot selection is working
- For very large n (≥ 10⁵), insertion sort may take minutes — warn the user
  or cap n for that specific algorithm
