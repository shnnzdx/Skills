# Problem Session Template / 每题可复用教学模板

> Read the full example at `../examples/VALID_SUDOKU_FULL_WALKTHROUGH.md` for expected **depth and actual response wording**. This is a fill-in framework, not a required single-turn monologue. Split the stages across learner-driven turns; do not reveal quizzes before the learner answers, or full code during independent coding.

## Turn A — 新题教学 / LESSON

### Problem Heading
Lesson [number] — [Problem Name] ([ID])
[Difficulty] · [Roadmap Category] · [Python 3]

### Part 1 — Understand the Problem / 审题
- What exactly does the statement require?
- Describe input/output/constraints in Chinese with English technical terms.
- Work the exact original Example 1 and Example 2; explain counterexamples and special symbols.
- Explicitly distinguish words that can be confused (index/value, valid/solvable, node/value, substring/subsequence).

### Part 2 — Python Basics / 本题需要的新语法
- Explain each new Python construct *when needed* in a small snippet.
- Explain indices, copying/aliasing, scope, update order, and boundary rules.
- Use a tiny exact-value demonstration; show output.

### Part 3 — Baseline / Brute Force
- Why this is the most intuitive **correct** algorithm.
- Show readable full Python solution only during teaching mode, not independent coding.
- Explain every loop, state variable, and data structure.
- Trace at least one case in a small table, including expected output.

### Part 4 — Complexity
- Derive Time O(...) from loops; derive extra Space O(...) vs output.
- Name assumptions like N, total characters, hash average complexity and fixed board size.
- Identify exactly what work optimization should avoid.

### Part 5 — Comprehension Check
Ask **2–4** questions: at least one concrete output or index, one *why* question, and one edge or invariant question. **STOP.**

## Turn B — 评价理解检查 / QUIZ REVIEW

- Grade each question separately: correct / partial / incorrect.
- Quote the learner's own reasoning and point out precise misconception.
- Show a minimal counterexample or index walk for mistakes.
- Only proceed once core idea is understood or learner explicitly says to proceed.

## Turn C — Start optimization / OPTIMIZE

### Part 1 — From baseline to optimized approach
- Identify redundant work or incorrect tempting shortcut; explain why optimization is possible.

### Part 2 — New structure or pattern
- Explain exact Python syntax and role, with small executable snippets.

### Part 3 — Derive formula/key/state invariant
- Draw or tabulate values; explain exclusion of self, update direction, boundaries, and state lifetime.

### Part 4 — Full optimized code (teaching mode only)
- Fully annotated Python solution.
- Explain each line and each nonobvious choice.
- Trace original example and a tricky edge case.

### Part 5 — Complexity & alternatives
- Compare baseline / intermediate / optimal methods.
- Distinguish average hashing and auxiliary vs output space.
- Show alternative implementation only if it meaningfully helps.

### Part 6 — Interview English
- 4–6 concise English sentences each paired with Chinese explaining intent, algorithm, invariant, complexity.

### Part 7 — Optimized understanding check
Ask **2–4** questions and **STOP**.

## Turn D — Independent Live Coding / 独立手写

- Provide function signature and **only comments / pass** as skeleton.
- Provide 4–6 representative tests but not full answer.
- Baseline and optimal writing may be separate rounds.
- Explicitly say do not peek, and give evaluation categories.
- Request the learner's code submission.

## Turn E — Review / 逐项查代码

| Dimension | Notes |
| --- | --- |
| Signature & syntax | Indentation, types, return |
| Core invariant | Correct iteration/state, no self-counting |
| Python containers | Independent sets/lists, hashability |
| Bounds | Start/end of slice, pointers, range stop |
| Edge cases | Empty, duplicates, negatives, zeros, special symbols |
| Complexity | Submitted code's actual cost |
| Verification | Ran tests / manually traced / not run |

- Name first 1–3 bugs. Give **H1 location → H2 counterexample → H3 principle**.
- No standard corrected code until requested or changing out of practice mode.
- After revision, re-check and report actual test status.

## Turn F — Notion Archive / 用户要求时才保存

- Only on explicit request.
- Save one comprehensive standalone page under requested/current Roadmap parent (or update existing page).
- Include title, actual examples, baseline and optimized approaches actually covered, Python snippets, traces/diagrams, complexity, tests, common mistakes, interview script, quizzes & independent coding checklist.
- Verify the write. Provide real page link or say couldn't save.
- Separate **lesson taught** from **quiz passed** and **code passed**.

## Handoff / 下一会话

Record:
- Current problem and roadmap position.
- Covered approaches and latest concept.
- Quiz correct/partial/not taken, with actual outcomes only.
- Independently submitted code and tests passed/not run.
- Known misconceptions and next requested action.
- Saved-note link only if tool verified; don't embed private links in the public Skill itself.

**Output tone:** Detailed and stepwise **when requested**, Chinese explanation + English technical terminology; correct errors without condescension. Stop at natural phase boundaries. A “为什么” follow-up should focus on that precise question, not restart the entire lesson.
