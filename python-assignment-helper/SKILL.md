---
name: python-assignment-helper
description: >
  Solves university Python course assignments. Use whenever the user asks to complete
  Python homework problems from a university course, or when they share a .docx file
  containing numbered Python programming exercises (especially if written in Chinese).
  Handles module usage, class design, file I/O, inheritance, packages, and standard
  library exercises. Creates numbered .py files for each problem and a README.md with
  run instructions.
triggers:
  - python assignment
  - python homework
  - 实验4
  - 模块与面向对象编程
  - solve python problems
---

## Overview

This skill solves university Python programming assignments. It reads questions from
the shared docx file, creates a separate `.py` file for each numbered problem, and
writes a `README.md` explaining how to run each one.

## Workflow

1. The input is the docx file `实验4_模块与面向对象编程.docx` in the current directory.
2. Extract the questions using pandoc and identify all numbered problems (1) through (12).
3. For each problem N, create a file named `N.py` in the same directory as the docx.
4. Also create `README.md` with run instructions for every problem.
5. If `books.csv` is referenced, read it from the same directory.

## Coding Style Rules

These rules make the code look like it was written by an average university student:

- **No comments** inside the code.
- **Variable names** must be simple and short: use `a`, `b`, `c`, `x`, `y`, `z`, `s`,
  `n`, `m`, `i`, `j`, `k`, `p`, `q`, `r`, `t`, `v`, `w`, `o`, `f`, `g`, `h`,
  `tmp`, `ans`, `sum`, `ret`, `n1`, `n2`, `s1`, `s2`, `d1`, `d2`, `c1`, `c2`,
  `a1`, `a2`, `b1`, `b2`, `x1`, `x2`, `y1`, `y2`, `z1`, `z2`, `e1`, `e2`,
  `l1`, `l2`, `l3`, `v1`, `v2`, `v3`, `u1`, `u2`, `o1`, `o2`, `r1`, `r2`.
  **Never** use full English words like `student`, `grade`, `calculator`, `manager`,
  `average`, `result`, `value`, `string`, `number`, `data`, `item`, `key`, `point`.
- **No advanced or clever idioms**: don't use list comprehensions where a loop is
  clearer, don't use `map`/`filter`/`lambda` in fancy ways, don't use `*args`/`**kwargs`
  unless the problem requires it, don't use `__slots__`, metaclasses, decorators,
  context managers, or any "power user" Python pattern.
- **Keep it simple and direct**: straightforward variable names, plain loops and ifs,
  no nested clever tricks.
- The code should compile and run on a standard Python 3 environment with only
  built-in and standard library modules (no external packages needed).
- **All print output labels must be in Chinese.** When printing results, use Chinese
  characters for labels and descriptions, not English. For example:
  - `print("平均值:", avg)` not `print("avg:", avg)`
  - `print("标准差:", std)` not `print("std:", std)`
  - `print("历史记录:", h)` not `print("history:", h)`
  - `print("最高学生:", t)` not `print("top student:", t)`
  This applies to every print statement throughout the code — all visible labels
  and prompts must be in Chinese to match the Chinese-language assignment context.

## Per-Problem Guidelines

### Problem 1
time, datetime, random modules. Four sub-parts (a-d). Each sub-part is a small
task. Output clearly labeled results for each sub-part.

### Problem 2
GradeManager class with private dict `students`. Implement: `add_student`,
`remove_student`, `get_grade`, `get_average`, `get_top_student`. Test with some
sample data (at least 3 students) and print results.

### Problem 3
Calculator class. Methods: `add`, `subtract`, `multiply`, `divide`, `power`, `history`.
Store history as a list of tuples like `("add", 2, 3, 5)`. Test each operation
and show history. Handle division by zero gracefully.

### Problem 4
Function `fs(dirname, s)`. Randomly pick a .txt file from the given directory and
append string `s` to it. If no .txt files exist, create `new.txt`. Test by creating
a test directory with a few .txt files, calling fs, then verifying content.

### Problem 5
DictInfo class. Methods: `add_item(key, value)`, `get_value(key)`,
`merge_dict(dict2)`, `del_last_item()`. Test with:
`dict_info = DictInfo({'a': 1, 'b': 2, 3: 'c'})` and show all operations.

### Problem 6
Class Human with `__init__(name, age)`, `get_name()`, `do_homework()`.
Class Student inherits Human with `__init__(name, age, homework)` and overrides
`do_homework()` using super(). Test with `stu = Student('John', 20, 'Python实验')`
and print name, age, homework, call both methods.

### Problem 7
Create a **package structure** at the same level as the python files:
- `mypack/__init__.py`: define `a=2, b=1`
- `mypack/aa.py`: function `add(x, y)` that prints `x+y`
- `mypack/subpack/__init__.py`: empty
- `mypack/subpack/bb.py`: function `sub(x, y)` that prints `x-y`
- `test.py` at the same level as `mypack/`: defines `m=4, n=3` and imports
  as specified in the problem.

The problem asks for the test.py source. Create the full directory structure and
include a setup script or instructions so it can actually run.

### Problem 8
Create module `mc.py` with:
- Person class with private attributes `__name`, `__age`, `__sex`
- Student class inheriting Person, stores variable number of course scores,
  has `average()` method

Create `test.py` that imports Student and tests with 3 students having different
numbers of courses.

### Problem 9
Read `books.csv` from the same directory. Create Book class with private
`__price` and `__title`. Read CSV, create Book instances, apply 20% discount,
sort by price descending, print title and price.

### Problem 10
ZipTest class with methods:
- `create_txt(filename, content)`: creates txt with content
- `zip_file(filename)`: compresses to zip, deletes original txt
- `unzip_print(filename)`: unzips, if txt file print content

### Problem 11
MD5Tool class:
- `__init__(self, bytes_list=None)`: initializes empty hash_list, if bytes_list
  provided compute MD5 for each
- `calc_MD5(self, b)`: compute MD5 and append to hash_list
- `comp_MD5(self, b)`: compare MD5 of b with hash_list, print whether match found

Test with the two specific bytes objects provided in the problem. Include a brief
text description (2-3 sentences) at the end explaining MD5 use and limitations.

### Problem 12
HDPoints class:
- `__init__(self, points)`: points is list of sublists (high-dimensional points)
- `centerpoint(self)`: returns center point (mean of each dimension)
- `minkowski(self, x, y, p)`: p-Minkowski distance between points x and y
- `farthestpoint(self, p)`: index and distance of point farthest from center
- `farthest2points(self, p)`: indices and distance of pair farthest apart

Generate at least 50 random points with at least 5 dimensions each (uniform [0,1]).
Random p in range 0-5. Test all methods.

## Output Files

- `1.py` through `12.py` in the same directory as the docx file
- `README.md` with instructions for every problem (see format below)
- `mypack/` directory (for problem 7) created as a subdirectory
- `mc.py` and `test.py` for problem 8

## README.md Format

Each problem section in README.md must follow this exact structure:

```
## 第N题 (N.py)

**题意**：一句话简要说明本题要干什么（用最简单直白的话）。

**运行方式**：
```bash
python N.py
```

**输出说明**：描述运行后会看到什么输出，表示题目正确运行了。
```

- Use `(N)` format for problem number in the heading, e.g. `(1)`, `(2)`, etc.
- **题意** section: describe the purpose in the simplest possible terms, in Chinese.
  1-2 sentences maximum. What does this problem ask you to do? What is the goal?
- **运行方式**: give the exact command to run.
- **输出说明**: describe what output the user should expect to see.
- Leave one blank line between sections.
- For problems 7 and 8 which involve multiple files, clearly state which files to run.
