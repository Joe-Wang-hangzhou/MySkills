---
name: leetcode-note-generator
description: |
  Generate LeetCode solution notes in Markdown. This skill should be used whenever
  the user provides a LeetCode URL (leetcode.cn or leetcode.com) along with their
  solution code, and wants a structured MD note with 题意/思路/code sections.
  Also trigger when the user asks to create LeetCode notes, 力扣笔记, or 算法笔记
  from their solution, even if they don't use the exact phrase "LeetCode note."
  The skill fetches the problem description from the URL, analyzes the user's
  solution, and writes a formatted .md file to the current directory.
---

# LeetCode Note Generator

## Workflow

When invoked, follow this sequence:

1. **Fetch problem info** — Use `WebFetch` on the LeetCode URL to get the problem title and description. Parse the problem number and name from the page.

2. **Understand the solution** — Read the user's code carefully. Identify the algorithm/pattern used, the key variables, the base cases, and any non-obvious implementation choices (e.g., why `-1` instead of `0`, why `long` instead of `int`).

3. **Write the .md file** — Generate the note following the format below, then save to `./{题号}.{题目名}.md`.

## Output format

Every note must start with the title line, followed by the required sections in this order. Use `---` as a separator only when appending to an existing notes file.

```
### [**<font style="color:rgb(10, 132, 255);">题号. 题目名</font>**](原始LeetCode链接)
#### 题意
...
#### 思路
...
#### code
...
```

If there are multiple approaches, use `#### 思路 1 方法名`, `#### 思路 2 方法名` at the same level, each followed by its own `#### code 1 方法名` block.

An optional `#### 分析` section can go after (or between) approaches, at the same heading level as 思路.

## How to write each section

### 题意

A concise Chinese summary of the problem in 1-3 sentences. Include key constraints and edge cases only when they matter to the solution (e.g., "空的树也是二叉搜索树"). Don't copy the entire problem statement — distill it to what the solution actually needs to handle.

### 思路

This is the most important section. Write in a conversational, first-person-plural Chinese style (用"我们"). The goal is to capture the *thinking process*, not just describe the algorithm.

A good 思路 does these things:

- **Start from intuition.** What's the first thing that comes to mind? Why does or doesn't it work? Show the dead ends — they're as valuable as the solution.

- **Build up step by step.** Walk through how the approach emerges from the problem structure. Use concrete reasoning: "一开始队列中只有一个根节点，自己就是第一层" rather than abstract summaries.

- **Explain the WHY behind every non-obvious choice.** If the code uses `-1` where `0` seems natural, explain it. If it uses `long` instead of `int`, mention the test case that forces it. The user's own comments in the code are good hints for what needs explanation.

- **Ask and answer sub-questions.** Patterns like "如何理解递归？" or "为什么不需要考虑平衡？" followed by an answer make the note feel alive and help when reviewing later.

- **Use formatting to highlight key insights.** Bold critical concepts (`**队列**`, `**快照**`), underline pivotal observations (`<u>第一层</u>`), and use inline code for variable names (`node.val`, `l + r`).

- **For recursive solutions, teach the "black box" mental model.** Describe what the recursive function *does* (its contract), then show how to use it without knowing internals.

Keep paragraphs short. Use line breaks generously — the example file has blank lines between almost every sentence. This makes the notes scannable when reviewing.

### code

Include the user's solution code in a fenced code block with the correct language tag (`java`, `cpp`, `python`, `go`, etc.). Preserve ALL their comments — those are part of the note. Include the full struct/class definition (TreeNode, ListNode, etc.) so the code is self-contained.

If the user provided solutions in multiple languages, include each in its own code block.

### 分析 (optional)

Use this section for complexity analysis, comparison with alternative approaches, or edge-case discussion that doesn't fit in 思路. Keep it at the same heading level as 思路:

```
#### 分析
- 时间复杂度: O(n)
- 空间复杂度: O(n)
```

Only add this section when there's meaningful analysis beyond what the 思路 already covered.

## Writing principles

- **Think like the user.** The example notes show a student who's learning — they question their own understanding, leave traces of their thought process, and highlight "aha" moments. Match that voice.

- **Code comments are clues.** If the user wrote comments explaining parts of their code, those lines are the most important to unpack in the 思路.

- **File names** should follow the pattern `{题号}.{题目名}.md` using the number extracted from the LeetCode URL and the Chinese problem name.

- **WebFetch may fail** on LeetCode pages due to anti-scraping. If it does, extract whatever title/number you can from the URL, and write the 题意 from your own knowledge of the problem. The note is still valuable even if the fetch fails.
